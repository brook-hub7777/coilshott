# Vercel deployment and releases

[Development walkthrough](README.md)

## The checked-in deployment fix

Commit `7eb7ada` added the top-level `index.html`, `vercel.json`, and the assets the page references. The archived copies remain under `coilshot/analysis/reference-assets`. Use `git show --stat 7eb7ada` to see the actual changes and `git log -- index.html vercel.json` to find later changes to the entrypoint or routing configuration. The commit subject identifies a Vercel 404 as the reason for the change.

## Project root

Check that the Vercel project uses the repository root as its Root Directory. That directory contains the deployment entrypoint and the `assets`, `fonts`, and `art` directories it references. Before changing the setting, preview the same layout locally and open the deployment details to identify the commit Vercel built. A correct local checkout can still yield a 404 when a deployment points at a different directory or revision.

## Static build settings

The repository stores the files Vercel needs for a static deployment and contains no package manifest or generated build script to run. Review any existing install, build, or output directory overrides in the Vercel project settings. A custom output directory must contain the deployed `index.html` and its assets. Record the observed settings when debugging, since dashboard overrides are outside the Git diff.

## Routing configuration

The checked-in `vercel.json` contains a rewrite from `/(.*)` to `/index.html`. A rewrite serves content under the requested address. To check its effect, request the homepage, a real JavaScript asset, and a path without a corresponding file in a preview. Record the status and content type for each request. This shows which responses are the app shell and which carry actual assets under the deployed routing rules.

## Asset paths in a deployment

The deployment entrypoint uses root-relative URLs for the module, stylesheet, favicon, and display font. Changing the Vercel root or introducing a URL prefix affects those requests. Review HTML and nested asset references when a release changes the directory layout. In a preview, start at the intended public entrypoint and check the browser Network panel for requested paths that differ from the deployed file paths.

## Hashed filenames

The entrypoint currently names `assets/index-CFnmQ0aO.js` and `assets/index-Cunqe2o7.css`. The JavaScript loads `three-CoWDQTm9.js` from the same assets directory. If a future source build changes these names, review the generated HTML and the matching bundles in one change. A preview should load the module graph in a fresh browser session to catch stale references while the previous deployment is still available.

## Preview revision selection

When the Vercel Git integration is active, a branch update can produce a preview. Open the preview’s deployment details and compare the revision to the Git commit being reviewed. Use that specific preview for browser checks. The production domain can continue serving a different revision during review, so include the preview URL and its commit when reporting a release issue.

## Production branch selection

Confirm the configured production branch in the Vercel project before treating a push as a release. The GitHub repository uses `main` as its primary branch. Deployment settings determine which revision reaches the production domain. After an update, open the Vercel deployment list and record the production revision, the deployment status, and the domain that points to it.
