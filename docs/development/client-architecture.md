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

## Dialog lifecycle and focus

Settings, Field Guide, Mission Briefing, and collection details share the native `dialog` component. The bundle calls `showModal()` when it opens and records the previously focused element. Closing the component calls `close()` and attempts to restore that focus. Open Settings with the keyboard, dismiss it with Escape, and check that focus returns to the settings button. The visible Close dialog button uses the same dismissal callback.

## Field Guide entry points

The lobby footer offers a Controls button. The question mark shortcut also opens the Field Guide while the engine is in the menu phase with no modal open. During a paused mission, use the Controls & Field Guide button on the pause panel. All of these paths lead to the same Know Your Lift dialog, which includes keyboard bindings and fallback instructions for mouse capture and touch.

## Settings controls and persistence

Open Settings using the top-bar gear or the Settings button in the pause panel. Look Sensitivity ranges from 0.25 to 2.50 in 0.05 increments, and Graphics Quality offers High and Performance. Reduce Motion and Invert Vertical Look use checkboxes. Changing a control updates the profile immediately. The Done button closes the panel, so inspect the live state while the dialog is still open if a change appears delayed.

## Loading status and error alert

The root component renders a status called Checking Weather until the engine signals readiness. The launch controls are disabled until that signal arrives. The error overlay uses the heading A Moment Between Worlds and exposes an alert with the reported reason. Its Reconnect button reloads the page. If startup appears stalled, use Elements or Accessibility to identify which of those states the browser is showing.

## Runtime phases and result screens

The engine begins in `menu` and moves through `playing` or `paused` during a mission. It reports `won` or `lost` on completion. React also receives a separate mission result object. That object supplies the summary and causes the result overlay to replace the HUD. When investigating the end of a mission, inspect the phase and visible result together to locate which part of the transition has occurred.

## Pause and return to the lobby

During play, `P` or Escape requests a pause. The pause panel says The Lift Can Wait and offers Resume Mission and Return to the Yard. The engine also pauses active play when the document becomes hidden or pointer lock is lost after capture. Record whether the window was focused and whether pointer lock was active when diagnosing an unexpected pause.

## Browser state inspection getters

Read `window.__NIMBUS_STATE__` in the console to see engine telemetry such as position, phase, health, ammo, subclass, and weapon. `window.__REVISION_STATE__` returns a JSON copy of the HUD state held by React. Read each property again when checking a transition. The getter returns a snapshot, so retaining the previous object is useful for comparing values across a UI action.
