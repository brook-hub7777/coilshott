# Browser client and state

[Development walkthrough](README.md)

## Browser entry point

Start with the repository root `index.html`. It declares the Nimbus Salvagers title, includes the shipped stylesheet, and loads the application with a module script. The HTML contains an empty `#root` element. Near the end of `assets/index-CFnmQ0aO.js`, React mounts the application into that element. In DevTools, compare View Source with the Elements panel to distinguish the initial HTML from the browser-generated UI.

## Tracing a label into the bundle

Open the application module in DevTools Sources and use the pretty-print control before searching for a visible label such as `CHOOSE YOUR PATH.` or `VIEW MISSION DETAILS`. The surrounding generated code shows the element, event handler, and data bindings. The archive under `coilshot/analysis` provides historical context. Verify its selectors, names, and values against the shipped module before using them as references.
