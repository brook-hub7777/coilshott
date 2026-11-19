# Graphics, assets, and audio

[Development walkthrough](README.md)

## Trace scene rendering through the browser

The shipped client sets up a renderer, scene, camera, and postprocessing composer for the game canvas. Place a DevTools breakpoint where the renderer or composer draws a frame to trace the call stack back to the game loop. Check the current view and canvas dimensions at the breakpoint. The rendering module is bundled with the app, so this trace starts with the delivered browser files.

## Map the lobby's visual layers

The lobby combines browser-rendered controls, CSS backdrops, and a game canvas. Select a visible card or heading in the Elements panel and inspect its ancestors, pseudo-elements, and stacking order. Then locate the canvas and inspect its rectangle. This lets you narrow down a dark or empty area to a hidden overlay, a background image that failed to load, or a canvas that failed to paint.

## Record the shipped sky backdrop

The stylesheet references `art/sky-yard.png` from multiple backgrounds, including the mission briefing. The tracked PNG is 1536 × 1024 and 2,733,562 bytes. The archived inventory lists a different resolution, so use a file inspector on the actual PNG when measuring changes. Compare the computed `background-size` and `background-position` across the lobby and briefing before filing an image-crop issue.

## Map the bundled interface font weights

The stylesheet maps `fonts/font-0.ttf` through `fonts/font-3.ttf` to Barlow Condensed weights 400–700. It maps `fonts/font-4.ttf` through `fonts/font-8.ttf` to Manrope weights 400–800. Each rule uses `font-display: swap`. When the width of a label changes during page load, compare its computed weight, the rendered font, and the corresponding font request. Wait for `document.fonts.ready` before capturing a settled screenshot.

## Identify both Orbitron declarations

`index.html` declares Orbitron with the absolute URL `/fonts/original/display.ttf`. The shipped stylesheet declares the same variable face through `../fonts/original/display.ttf`, resolved relative to the stylesheet. Both specify weights 100–900. When diagnosing a display-heading font issue, compare the two resolved URLs in the Network panel and inspect the rendered font on the heading. A server mounted at a subpath may resolve those URLs differently.
