# Graphics, assets, and audio

[Development walkthrough](README.md)

## Trace scene rendering through the browser

The shipped client sets up a renderer, scene, camera, and postprocessing composer for the game canvas. Place a DevTools breakpoint where the renderer or composer draws a frame to trace the call stack back to the game loop. Check the current view and canvas dimensions at the breakpoint. The rendering module is bundled with the app, so this trace starts with the delivered browser files.
