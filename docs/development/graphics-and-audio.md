# Graphics, assets, and audio

[Development walkthrough](README.md)

## Trace scene rendering through the browser

The shipped client sets up a renderer, scene, camera, and postprocessing composer for the game canvas. Place a DevTools breakpoint where the renderer or composer draws a frame to trace the call stack back to the game loop. Check the current view and canvas dimensions at the breakpoint. The rendering module is bundled with the app, so this trace starts with the delivered browser files.

## Map the lobby's visual layers

The lobby combines browser-rendered controls, CSS backdrops, and a game canvas. Select a visible card or heading in the Elements panel and inspect its ancestors, pseudo-elements, and stacking order. Then locate the canvas and inspect its rectangle. This lets you narrow down a dark or empty area to a hidden overlay, a background image that failed to load, or a canvas that failed to paint.

## Record the shipped sky backdrop

The stylesheet references `art/sky-yard.png` from multiple backgrounds, including the mission briefing. The tracked PNG is 1536 × 1024 and 2,733,562 bytes. The archived inventory lists a different resolution, so use a file inspector on the actual PNG when measuring changes. Compare the computed `background-size` and `background-position` across the lobby and briefing before filing an image-crop issue.
