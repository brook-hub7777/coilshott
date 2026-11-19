# Vercel deployment and releases

[Development walkthrough](README.md)

## The checked-in deployment fix

Commit `7eb7ada` added the top-level `index.html`, `vercel.json`, and the assets the page references. The archived copies remain under `coilshot/analysis/reference-assets`. Use `git show --stat 7eb7ada` to see the actual changes and `git log -- index.html vercel.json` to find later changes to the entrypoint or routing configuration. The commit subject identifies a Vercel 404 as the reason for the change.

## Project root

Check that the Vercel project uses the repository root as its Root Directory. That directory contains the deployment entrypoint and the `assets`, `fonts`, and `art` directories it references. Before changing the setting, preview the same layout locally and open the deployment details to identify the commit Vercel built. A correct local checkout can still yield a 404 when a deployment points at a different directory or revision.
