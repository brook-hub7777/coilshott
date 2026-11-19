# Browser client and state

[Development walkthrough](README.md)

## Browser entry point

Start with the repository root `index.html`. It declares the Nimbus Salvagers title, includes the shipped stylesheet, and loads the application with a module script. The HTML contains an empty `#root` element. Near the end of `assets/index-CFnmQ0aO.js`, React mounts the application into that element. In DevTools, compare View Source with the Elements panel to distinguish the initial HTML from the browser-generated UI.
