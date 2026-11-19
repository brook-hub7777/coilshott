# Vercel deployment and releases

[Development walkthrough](README.md)

## The checked-in deployment fix

Commit `7eb7ada` added the top-level `index.html`, `vercel.json`, and the assets the page references. The archived copies remain under `coilshot/analysis/reference-assets`. Use `git show --stat 7eb7ada` to see the actual changes and `git log -- index.html vercel.json` to find later changes to the entrypoint or routing configuration. The commit subject identifies a Vercel 404 as the reason for the change.
