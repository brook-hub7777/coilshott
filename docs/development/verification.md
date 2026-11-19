# Verification and debugging

[Development walkthrough](README.md)

## Record the scope of a local check

Start a verification note with the Git revision from `git rev-parse --short HEAD`, the server command, the port and the browser version. The repository has no checked-in test harness. Treat the HTTP and Chromium steps in this chapter as a manual smoke routine that an automation can also run. Keep a record of skipped checks so the next person can reproduce the same scope.
