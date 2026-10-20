# Route Inventory: Nimbus Salvagers

## 1. Architectural Routing Paradigm

The target application utilizes a high-performance **State-Driven Single-Page Application (SPA)** architecture. Navigation does not trigger standard multi-page browser reloads or URL fragment shifts (`hashchange`); instead, the entire interface is orchestrated via synchronized React state machines combined with a continuous WebGL canvas layer.

The visual output is governed by two orthogonal state vectors:
1. **Application Phase (`p.phase`):** Transitions the canvas and UI between the Orbit Lobby (`"menu"`), Active Combat (`"game"`), Mission Pause (`"paused"`), Mission Victory (`"complete"`), and Mission Defeat (`"gameover"`).
2. **Lobby Screen (`screen`):** When in `"menu"` phase, controls which HUD view overlays the 3D scene (`"play"`, `"destinations"`, `"engineer"`, `"collections"`).
3. **Modal Overlay Route (`activeModal`):** Overlays full-screen dialogs (`"settings"`, `"help"`, `"briefing"`, `"collection:<item_id>"`).

---

## 2. Screen & View Inventory

| View Identifier | Route Trigger | Canonical Label | Visual Focus & Key Content | Underlying 3D Camera / Scene State |
| :--- | :--- | :--- | :--- | :--- |
| **`screen-play`** | Header Nav `PLAY` / Wordmark Logo | **Play (Yard Hub)** | Hero title banner, tracked mission card, engineer subclass tile, 3 horizontal activity selection cards, power counter, footer rank bar. | Orbit camera: `landscape` view looking towards the floating sky docks and clouds. |
| **`screen-destinations`** | Header Nav `DESTINATIONS` / "VIEW DESTINATIONS →" | **Destinations** | Left side circular orbital graphic of Cirrus Reach with orbital nodes; right side list of 3 missions (Campaign, Vanguard, Wind Trials) with difficulty & duration. | Orbit camera: `landscape` view with reduced overlay shade. |
| **`screen-engineer`** | Header Nav `ENGINEER` / Engineer Tile | **Engineer** | Left side subclass selector (Rigger, Aerostat, Mistwright), ability cards, stat bars; center 3D interactive engineer model; right side weapon loadout (Coil Nine, Rainbell, Longdraft) and armor. | Orbit camera: `engineer` view focusing directly on the 3D player model at center. |
| **`screen-collections`** | Header Nav `COLLECTIONS` | **Collections** | 6-item schematic grid displaying weapons, armor, emblems, and mastery seals with unlock criteria and discovery counters (`02 / 06 DISCOVERED`). | Orbit camera: `landscape` view with dark backdrop. |
| **`gameplay-combat`** | "LAUNCH MISSION" or "READY TO DEPLOY" button | **Tactical Mission** | Full 3D FPS Canvas. Radar minimap, health/shield bar, wave banners, reticle, 3D weapon held in hand, ability cooldowns, ammo counters, hit numbers. | FPS First-Person Camera (FOV 58), mouse pointer lock enabled, real-time physics and WASD movement. |
| **`gameplay-pause`** | `ESC` or `P` key during active combat | **Mission Pause** | Frosted blurred game overlay with airship icon, "THE LIFT CAN WAIT", Resume, Settings, Field Guide, and "Return to the Yard" buttons. | Freezes simulation loop (`paused` flag), releases pointer lock. |
| **`gameplay-complete`** | Defeating Wave 3 / Boss | **Mission Victory** | Mission debrief modal: Total score, kills, elapsed time, spool tokens earned, and unlocked collection reward. | Post-combat cinematic angle, victory audio fanfare. |
| **`gameplay-gameover`** | Player Health reaching 0 | **Defeat / Fall** | Fall banner: "THE LIFT SUBSIDES", mission statistics, "TRY AGAIN" button and "RETURN TO THE YARD" button. | Camera tilt to ground, combat desaturation. |

---

## 3. Modal Overlay Routes

Modals are managed via the `U` state variable (`activeModal`) and are rendered in the top-level React tree using the `<Ms>` dialog container:

### 3.1. Settings Modal (`activeModal = "settings"`)
- **Trigger:** Topbar Gear Icon button (`button.icon-button[aria-label="Open settings"]`) or In-Game Pause Menu "SETTINGS" button.
- **Title:** `MAKE IT YOURS.` | **Eyebrow:** `SETTINGS`
- **Subtext:** `"Tune your field equipment. Changes apply immediately."`
- **Parameters:**
  - Master Volume Slider (`0% - 100%`, default `35%`)
  - Look Sensitivity Slider (`0.20x - 3.00x`, default `1.00x`)
  - Graphics Quality Dropdown (`"low"`, `"medium"`, `"high"`, default `"high"`)
  - Reduce Motion Checkbox (boolean, default `false`)
  - Invert Vertical Look Checkbox (boolean, default `false`)
- **Dismissal:** Top-right `✕` button or bottom `"DONE ✓"` button.

### 3.2. Controls & Field Guide Modal (`activeModal = "help"`)
- **Trigger:** Footer button (`button.control-link[text="[?] CONTROLS"]`) or pressing `?` key on keyboard while in menu.
- **Title:** `KNOW YOUR LIFT.` | **Eyebrow:** `ENGINEER FIELD GUIDE`
- **Content:** Two-column action-to-key matrix (Move, Look, Fire, ADS, Sprint, Jump, Reload, Class Ability, Grenade, Super, Switch Weapons, Pause) + Sailfin field notes.
- **Dismissal:** Top-right `✕` button or bottom `"READY, ENGINEER →"` button.

### 3.3. Mission Briefing Modal (`activeModal = "briefing"`)
- **Trigger:** "VIEW MISSION DETAILS ↗" button on tracked mission card.
- **Title:** Mission Title in uppercase (e.g. `THE BERYL SIGNAL`) | **Eyebrow:** `${label} / MISSION BRIEFING`
- **Content:** Real-time rendered activity thumbnail, lore description, location string, duration & team size, objective description, reward preview.
- **Actions:** `"LAUNCH MISSION →"` button.

### 3.4. Item Detail Modal (`activeModal = "collection:<id>"`)
- **Trigger:** Clicking any of the 6 collection cards on the Collections screen.
- **Title:** Item Name in uppercase (e.g. `COIL NINE`) | **Eyebrow:** `${rarity} / ${type}`
- **Content:** Vector schematic graphic, lore description, discovery status, and an `"EQUIP WEAPON"` button if discovered.

---

## 4. State Persistence & URL Synchronization

- **Storage Key:** `nimbus-engineer-v1` in `window.localStorage`.
- **Payload Schema:**
  ```json
  {
    "engineer": "rigger",
    "weapon": "auto",
    "spoolTokens": 2500,
    "xp": 0,
    "kills": 0,
    "victories": 0,
    "bestScore": 0,
    "unlocked": ["coil-nine", "pathfinder"],
    "completedModes": [],
    "settings": {
      "volume": 0.35,
      "sensitivity": 1.0,
      "quality": "high",
      "reducedMotion": false,
      "invertY": false
    }
  }
  ```
- **Error Recovery Route:** If WebGL fails to initialize, an alert overlay replaces the UI:
  - **Title:** `"A MOMENT BETWEEN WORLDS."`
  - **Action:** `"RECONNECT ↻"` (`window.location.reload()`).
