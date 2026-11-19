# Repository and local workflow

[Development walkthrough](README.md)

## Read this walkthrough as a retrospective

This walkthrough documents the Nimbus Salvagers checkout as it exists today. The chapters cover repository orientation and local setup, the browser application, its interfaces and assets, validation, and deployment. The entries explain how the checked-in pieces fit together and how to work with them. Treat the sequence as a reading order. Use the original Git commits when you need evidence about when files entered the repository.

## Identify the checkout and the application

The GitHub repository is `brook-hub7777/coilshott`. Its nested tooling directory is spelled `coilshot` with one final t. Nimbus Salvagers is the title shown in the HTML and the name used by the README. In the cloud checkout, `/workspace/coilshott` is the repository root. Run `git rev-parse --show-toplevel` if you entered through a different folder, then resolve all repository-relative paths in this guide from that root.

## Find the commits that established the layout

Use `git show --stat 8db9a0a` to locate the upload that added the server, nested HTML, and analysis directory. Use `git show --stat 7eb7ada` to see the later addition of the root HTML, static assets, and Vercel configuration. The earlier layout remains visible alongside those additions. The root README gained its live-site pointer in `1bfcd40`. These commits provide a traceable explanation for the two sets of files.

## Locate the root web payload

The root web payload starts at `index.html`. The three tracked files under `assets/` supply the application module, rendering module, and stylesheet. The `art/` directory holds the sky-yard image, `fonts/` holds local typefaces, and `favicon.svg` supplies the browser icon. Keep these paths together when inspecting the root page. The HTML points to its assets with paths that start at the web server root.
