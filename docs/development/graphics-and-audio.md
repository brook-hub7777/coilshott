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

## Locate the favicon and inline interface icons

The browser tab gets its icon from the standalone `/favicon.svg` referenced in `index.html`. Interface icons and schematics are rendered within page markup. If an icon appears incorrect, identify whether the symptom is in the browser tab or in the interface. For inline SVG, inspect `viewBox`, inherited `color`, and computed fill or stroke. For the tab icon, inspect the asset response and refresh with caching disabled.

## Find the active CSS declaration

The design notes under `coilshot/analysis/design-system.md` provide a vocabulary for surfaces, type, and motion. Use the computed style of the running page to establish the actual declaration behind a color or spacing issue. Expand the computed property to find its source rule in `assets/index-Cunqe2o7.css`, then check specificity, media conditions, and inherited custom properties. Record the matching selector in any proposed change.

## Identify viewport and input media rules

The shipped stylesheet contains layout rules for widths up to 700 pixels and for 701–1000 pixels, along with separate height and coarse-pointer queries. When a layout issue appears on a phone or tablet, capture the viewport dimensions and pointer type. Toggle touch emulation independently of width to find which rule applies. Landscape and portrait at the same width can produce different results because the stylesheet also checks height.
