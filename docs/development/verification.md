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
