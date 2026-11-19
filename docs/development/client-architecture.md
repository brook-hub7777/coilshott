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

## Navigation and selection attributes

In DevTools Accessibility, find the landmark named Main navigation and inspect the buttons inside it. The shipped buttons have lowercase accessible names, and the active button receives `aria-current="page"`. Activity cards and Engineer selection buttons use `aria-pressed` for their selected state. These attributes expose the current UI choice even when styling, letter case, or decorative icons make the visual selection harder to identify.

## Shared mission selection

Play activity cards and Destinations rows update the same mission selection. Choose Through the Squall in Play, check the tracked mission title, then open Destinations and find the selected Vanguard row. The three mission IDs are `story`, `strike`, and `windtrials`, with `story` selected when the application loads. The Destinations launch button incorporates the selected activity label, so its text updates with the selection.

## Mission briefing data

On Play, select a mission and open View Mission Details. The briefing reads the selected mission definition to show the title, synopsis, location, duration, team, and objective. It also looks up a collection item by the mission reward ID to display the reward name. When tracing a briefing whose objective or reward looks wrong, compare the mission and collection records before investigating the dialog layout.

## Engineer subclass selection

The Engineer view offers Rigger, Aerostat, and Mistwright. The selected subclass drives the description, Mobility, Resilience, Recovery, and ability details in the panel. Clicking a subclass updates the profile and calls the engine’s subclass setter. To follow that change, begin at the selected button, inspect `aria-pressed`, and watch the profile’s `engineer` field and the engine snapshot update.

## Weapon names and IDs

The loadout presents Coil Nine, Rainbell, and Longdraft as names players can recognize. Their internal weapon IDs are `auto`, `handcannon`, and `scout`. Engineer writes the selected weapon ID to the player profile and applies it to the engine. The Collections schematics use their own IDs such as `coil-nine`. When comparing loadout and collection data, use this mapping to relate the two sets of identifiers.

## Collections discovery

The Collections view marks items discovered when their IDs appear in the profile’s `unlocked` list. Both discovered and undiscovered cards open details. In a fresh profile, open Coil Nine and Rainbell to compare the status text. The details offer Equip Weapon for discovered weapons and give the discovery condition for locked items. The archive’s screenshots can help locate the cards during this comparison.
