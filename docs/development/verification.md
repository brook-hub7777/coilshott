# Verification and debugging

[Development walkthrough](README.md)

## Record the scope of a local check

Start a verification note with the Git revision from `git rev-parse --short HEAD`, the server command, the port and the browser version. The repository has no checked-in test harness. Treat the HTTP and Chromium steps in this chapter as a manual smoke routine that an automation can also run. Keep a record of skipped checks so the next person can reproduce the same scope.

## Confirm the entry point responds

Request `/` on the running loopback server and confirm HTTP 200 and a `Content-Type` containing `text/html`. Read the response body and look for `<title>Nimbus Salvagers</title>` and `<div id="root"></div>`. The title and mount point confirm that the server returned the expected page before any browser code ran. Keep the server console available so a failed request can be compared with its startup output.

## Check the files that bootstrap the app

Take the stylesheet and module URLs from the HTML that the server actually returned. Request each one and verify its status, body and `Content-Type`. The stylesheet needs `text/css` and each JavaScript module needs a JavaScript MIME type. Include the Three.js module imported by the entry bundle. A successful status alone can hide an HTML fallback, so inspect the header before investigating a blank canvas.

## Confirm a missing file is reported

Ask the included Node server for a distinct file name that is absent from the checkout, such as `/assets/smoke-check-missing.js`. Its missing-file branch should return HTTP 404 and a plain-text body. This gives you a control when diagnosing a broken module URL in the browser. Capture the status using an HTTP client before applying this expectation to any other server, since the rule comes from `coilshot/server.js`.

## Match response bytes to the selected copy

Fetch the same path from both local server modes and compare the responses with the intended files. For example, compare `/art/sky-yard.png` with the tracked image from each server's document root. Use `cmp` or compute file digests on both files. This check can expose a stale copy when both servers return HTTP 200. Compare the assets affected by an edit before drawing a browser-level conclusion.

## Listen for failures before opening the page

Attach Playwright listeners for `pageerror`, `requestfailed` and `response` before calling `page.goto`. Keep the error message, failed URL and any HTTP response status of 400 or above in the run output. A transport-level failure appears through `requestfailed`, while a completed 404 response arrives through `response`. On a failing smoke run, check the earliest relevant failure before changing a selector.

## Wait for an observable ready state

After navigating to `/`, use a browser assertion that retries until the heading `RIDE THE UPDRAFT.` becomes visible. Then wait for the canvas and the navigation controls that the next action needs. An HTML response or a page load event confirms an earlier stage of startup. Capture the visible page and the current URL if the interface assertion times out.

## Configure a headless WebGL test browser

The local smoke run used system Chromium with `--use-gl=angle`, `--use-angle=swiftshader` and `--enable-unsafe-swiftshader`. Keep those flags scoped to the disposable local test browser used to run this project. Record the Chromium version with the result because the available graphics backend varies with the host. Use a regular hardware-accelerated browser for checks that depend on normal graphics performance.

## Start each independent run in a clean context

Use a new Playwright browser context for each independent scenario. This isolates saved settings and progression from earlier runs. Record the viewport and the local origin, including its port, in failures. A reload inside one context is useful when checking persistence, while closing the context gives the next scenario a clean player profile. Prefer disposable test state when changing settings or launching missions.

## Check the visible result of graphics startup

Confirm that the WebGL canvas and the lobby heading are visible together. A canvas element can exist before the renderer is working. If `A MOMENT BETWEEN WORLDS.` appears, save the first browser error and note the active Chromium graphics flags. Keep that failure distinct from a missing asset response so the next check targets the browser capability or the resource that actually failed.

## Use an accessible navigation label

In the shipped bundle the main navigation has a button with accessible name `destinations` in lowercase. Select that role and name, click it, then wait for the `CHOOSE YOUR PATH.` heading. Verify that the Play heading has left the screen. This assertion checks the actual view transition and stays independent of generated CSS class names and visual capitalization.

## Exercise the Settings overlay

Click the button named `Open settings` and assert that the `MAKE IT YOURS.` heading appears. For a slightly broader check, also wait for the `Master volume` slider and the `Graphics quality` combobox. Click `DONE` and confirm the Settings heading is hidden. This covers the dialog's entry point, visible content and dismissal using accessible names confirmed in the shipped interface.

## Open the controls reference from the lobby

From the lobby, activate the footer button whose accessible name contains `CONTROLS` and wait for `KNOW YOUR LIFT.`. Verify that the panel shows the control reference, then dismiss it with `READY, ENGINEER` and check that the lobby is visible again. This is a short independent check for the footer help flow, and it can run before requesting browser pointer lock.

## Observe the transition into a mission

On Play, wait for the `LAUNCH MISSION` button and activate it with a real browser click. Assert that `RIDE THE UPDRAFT.` disappears and look for the in-mission HUD or pause controls. The initial gesture matters to browser input and audio policies. Record which in-mission view appeared so automation can account for pointer-lock behavior on the particular browser and host.

## Account for pointer-lock behavior during pause

After mission launch, check whether `Return to the Yard` is already visible. The test browser may enter pause when it cannot retain pointer lock. If the mission is active, press `p` and wait for that control to appear. This confirms access to the pause menu. Use a headed browser with working pointer lock if the task also needs to verify mouse capture and resumption.
