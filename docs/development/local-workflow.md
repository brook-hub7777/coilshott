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

## Find the analysis archive

Open `coilshot/analysis/README.md` for the technical report and its index. Its neighboring inventory files describe the observed application, and `coilshot/analysis/screenshots/` stores the recorded visual evidence. The separate `coilshot/analysis/reference-assets/` tree contains a runnable HTML and asset snapshot. Use that distinction when tracing a file. A screenshot records one observed state, while a matching HTML or bundle path identifies executable content that the local helper can serve.

## Recognize the proposed reconstruction roadmap

The analysis folder includes `implementation-plan.md`, which proposes a source scaffold, build tools, and reconstruction milestones. Read those sections as design proposals and check the tracked files before using any proposed command or path. The available local launcher is `coilshot/server.js`. The root app already contains checked-in browser output. Use `git ls-files` to verify whether a proposed source file, manifest, or configuration actually exists in the checkout.

## Follow startup through the HTML

Open `index.html` to find the page title, icon link, Orbitron face declaration, and root mount element. The stylesheet is `/assets/index-Cunqe2o7.css`. The module element loads `/assets/index-CFnmQ0aO.js` after the document declares `#root`. This short entry document gives you concrete starting points when a browser can reach the server and the expected interface has yet to appear. The nested and archived HTML copies have the same bootstrap at the baseline.

## Follow the shipped JavaScript files

The application bundle `assets/index-CFnmQ0aO.js` imports `./three-CoWDQTm9.js` relative to its own location. Both filenames are part of the delivered module graph. Keep the HTML module reference and the relative import in mind when checking the asset directory. For example, moving the entry bundle to a different directory changes where the browser resolves its rendering dependency. These tracked files contain generated, compressed browser code.

## Find the files that supply the page appearance

The stylesheet resolves image and font URLs relative to its own path. For example, `../art/sky-yard.png` resolves to `/art/sky-yard.png` when the stylesheet is served from `/assets/`. The HTML also declares the display font directly and loads `/favicon.svg`. These files are part of the committed static payload. If the shell appears with an unexpected typeface or background, use the browser Network panel to check the resolved resource URLs.

## Check which local copies match

The baseline root and archived web payloads have matching content for counterpart files, and all three checked-in `index.html` files are identical. Run `cmp index.html coilshot/analysis/reference-assets/index.html` to verify the entry pages and use `cmp` on a specific bundle when tracking a change. Identical bytes describe the baseline only. The two trees are separate paths, so a later edit to one tree can change which content a serving workflow picks up.

## Verify Node.js before starting the helper

Run `node --version` from the checkout to confirm that Node.js is on your path. Cloud onboarding validated the launcher with Node.js v24.22.1. The repository supplies no Node version pin, so this version records the tested environment. The helper uses the built-in `http`, `fs`, and `path` modules. A Node installation that runs the helper provides the server-side runtime needed to serve the tracked web assets.

## Use the prebuilt application directly

The checkout ships browser-ready HTML, JavaScript, CSS, fonts, and artwork. It includes no package manifest, package-manager lockfile, or build task. Once Node.js and a browser are available, begin with the checked-in helper. There are no declared packages to install and no compiled output to generate during onboarding. Keep the distinction between bundled dependencies and local tools in mind when you inspect the project or reproduce the launch on another machine.

## Choose a browser that supports the game

Nimbus Salvagers renders a three-dimensional scene in the browser and expects WebGL 2 support. Audio uses the browser’s Web Audio API, while mouse input during play can request pointer lock. Start in a current browser with WebGL available. If the app reports a graphics initialization failure, check browser acceleration and WebGL support before changing the static server. A successful HTML response only confirms that the browser reached the server.

## Start Nimbus Salvagers locally

From the repository root, run `node coilshot/server.js`. Keep that terminal open while the foreground server runs. Use the local address printed by the server, which defaults to `http://localhost:3000/`. Start with the root pathname and look for the Nimbus Salvagers lobby. Using the HTTP server gives the root-relative asset URLs and JavaScript module imports an origin from which to resolve.

## Select an available port

The server reads `PORT` and uses `3000` by default. When that port is already in use, run `PORT=3001 node coilshot/server.js` from the repository root and use port 3001 for subsequent requests. The helper binds to `0.0.0.0` on the configured port. Choose a valid TCP port. The server parses the environment variable as an integer and leaves listen errors to Node.
