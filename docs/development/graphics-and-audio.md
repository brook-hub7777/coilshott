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

## Explain both reduced-motion controls

The stylesheet has a `prefers-reduced-motion` media rule. The in-game Reduce motion setting is also read by the bundled scene update, menu camera, walking motion, and visual item movement. Test the browser media preference and the in-game switch individually when an animation persists. Record whether the visible motion comes from CSS on an element or from frames drawn to the canvas.

## Diagnose WebGL startup failures

The client displays the heading `A MOMENT BETWEEN WORLDS.` when its 3D initialization fails. The recovery button reloads the page. Capture the message and browser console output before retrying. In a separate console evaluation, check whether a fresh canvas can obtain a `webgl2` context. Compare a working browser profile with the failing one and record the browser version and graphics acceleration state.

## Explain how graphics quality affects rendering

The Graphics quality setting changes the renderer's pixel-ratio cap and shadow behavior. The bundled renderer caps high quality at 1.5 and uses a cap of 1 for other settings, with a separate 0.65 cap on detected software rendering. Low quality and detected software rendering use the direct render path. Compare presets at the same viewport and browser zoom to keep pixel count from obscuring the change you are measuring.

## Trace canvas resizing and camera projection

The client uses a `ResizeObserver` and reads the canvas element's client width and height. Its resize handler updates the renderer, postprocessing composer, and camera aspect, then refreshes the projection matrix. For a stretched or clipped scene, inspect both the element rectangle and its drawing-buffer dimensions after a resize. Record whether the menu chrome, the 3D scene, or both change when the viewport is resized.

## Locate textures generated in the browser

Some scene textures originate in JavaScript. The shipped module creates a 256 × 256 canvas, writes pixel data into it, and also uses smaller canvases and gradients for generated effects. For a texture problem, first identify whether the object belongs to the static lobby artwork or the 3D scene. A generated texture has no standalone PNG request, so confirm its creation through the browser call stack or scene initialization output.

## Trace activity thumbnails to canvas captures

The client generates activity previews by moving the existing camera to preset positions, rendering frames, and capturing JPEG data URLs with `canvas.toDataURL`. It saves and restores the camera position and orientation during the process. Inspect a missing thumbnail's image source and the surrounding console output. An image beginning with `data:image/jpeg` is already embedded in the page and has no separate file to locate in `art/`.

## Profile slow frames by browser and scene

Use the Performance panel to record a short interval in the lobby and a second interval after entering a mission. The bundle can schedule frames differently on detected software rendering, so record the renderer type, dimensions, and graphics preset with both captures. Inspect long scripting tasks and rendering work separately. A trace with clean initial requests can still reveal expensive repeated work after the page has finished loading.

## Trace the Web Audio signal path

The shipped module constructs an `AudioContext` and connects a master `GainNode` to the destination. Sound generation routes through that shared master. Begin a silent-audio investigation with an explicit click in the lobby and inspect the audio context state or browser media diagnostics. If all generated sounds are quiet, inspect the shared gain and tab output before tracing each individual sound.

## Document audio activation by user gesture

The bundled sound engine attempts to resume its audio context when that context is suspended. Lobby interactions, mission launch, and mission resume call into audio startup. A fresh page can load the 3D scene before it has an allowed audio gesture. Reproduce sound reports from a new tab and record the first interaction that triggered sound, along with any browser autoplay warnings.

## Trace game volume to the master gain

The default volume in the shipped bundle is 0.35. `setVolume` updates the master gain through `setTargetAtTime`, so changes ramp instead of jumping immediately. For a volume issue, compare the Settings slider, the Sound control, and the browser tab mute state. After any change, wait for the gain to settle and trigger the same interaction again before comparing loudness.

## Locate synthesized transient sounds

The sound engine creates short tones with oscillators and constructs short noise samples in audio buffers. Its noise path creates a low-pass filter and uses a gain envelope. The tracked site contains no separate audio recordings for these sounds. When a single cue is missing, reproduce the event that should generate it and inspect whether it reaches its audio helper before looking for a missing asset.
