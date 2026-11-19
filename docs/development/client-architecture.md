# Browser client and state

[Development walkthrough](README.md)

## Browser entry point

Start with the repository root `index.html`. It declares the Nimbus Salvagers title, includes the shipped stylesheet, and loads the application with a module script. The HTML contains an empty `#root` element. Near the end of `assets/index-CFnmQ0aO.js`, React mounts the application into that element. In DevTools, compare View Source with the Elements panel to distinguish the initial HTML from the browser-generated UI.

## Tracing a label into the bundle

Open the application module in DevTools Sources and use the pretty-print control before searching for a visible label such as `CHOOSE YOUR PATH.` or `VIEW MISSION DETAILS`. The surrounding generated code shows the element, event handler, and data bindings. The archive under `coilshot/analysis` provides historical context. Verify its selectors, names, and values against the shipped module before using them as references.

## Application state boundaries

The root React component keeps lobby screen, selected mission, active overlay, player profile, and engine HUD in separate state values. The application uses the HUD phase to decide when to show the lobby, and the active overlay controls the dialog content. When diagnosing a UI mismatch, record the visible screen and dialog alongside the engine phase. A screen change and a mission launch update different state.

## Lobby navigation

The lobby has four views named Play, Destinations, Engineer, and Collections. Click a view and look for the corresponding `screen-play`, `screen-destinations`, `screen-engineer`, or `screen-collections` class on `.menu-shell`. View changes leave the URL intact because the selected screen lives in React state. The wordmark has the accessible name `Nimbus Salvagers home` and returns the lobby to Play.
