# Unknowns, Limitations & Architectural Boundaries

## 1. Verified Observations vs. Inferred Candidates

To adhere to senior reverse-engineering integrity, this section explicitly separates **verified CDP observations** (grounded in DOM, screenshots, and decompiled code) from **inferred candidates** and architectural boundaries.

| Feature / Domain | Verified Ground-Truth Observation | Architectural Boundary & Implementation Reality |
| :--- | :--- | :--- |
| **Multiplayer / Matchmaking** | UI displays `"AI FIRETEAM"` for Vanguard and `"VS. BOTS"` for Wind Trials. Public network requests show zero WebSocket (`ws://`, `wss://`) or WebRTC connections. | **100% Client-Side Simulation:** Allies and enemies are simulated entirely inside the Three.js frame loop (`y4.allies`, `y4.enemies`). There is no remote multiplayer server. |
| **User Persistence & Cloud Sync** | No session cookies, JWT bearer headers, or remote sync endpoints observed during inspection. | **Local-First Storage:** All player unlocks, XP, score records, tokens, and settings are serialized into `window.localStorage.getItem("nimbus-engineer-v1")`. |
| **Audio Asset Pipeline** | No `.mp3`, `.wav`, or `.ogg` requests in network capture or bundle strings. | **Pure Algorithmic Synthesis:** All sound effects and ambient drones are synthesized on the fly via `AudioContext` oscillators, noise buffers, and biquad filters. |
| **Activity Previews** | Activity cards display real-time graphical thumbnails of the stone ruins. | **Live Render Targets:** Upon initial boot, the Three.js engine positions cameras at each destination, captures an offscreen frame, and populates `previews[modeId]` with a data URL. |
| **3D Asset Pipeline** | No remote `.gltf`, `.glb`, or `.bin` files fetched. | **Procedural Geometry Assembly:** Character, weapons, environment, and enemies are built from Three.js primitives (`RoundedBoxGeometry`, cylinders, rings, instanced foliage). |

---

## 2. Technical Limitations & Edge Cases

### 2.1. Browser & Hardware Acceleration Requirements
- **WebGL 2.0:** Requires support for `EXT_color_buffer_float` and hardware instancing.
- **Graceful Degradation:** If WebGL fails to initialize (e.g. SwiftShader or virtualization), the application catches the initialization exception and renders the dedicated error overlay:
  ```html
  <div class="error-overlay" role="alert">
    <h1>A MOMENT BETWEEN WORLDS.</h1>
    <p>This browser couldn't open the 3D world. Enable hardware acceleration or use a WebGL 2 browser, then reload.</p>
    <button class="launch-button">RECONNECT ↻</button>
  </div>
  ```

### 2.2. Web Audio Autoplay Policy
- Modern browsers suspend `AudioContext` until an initial user gesture.
- The application wraps `audio.start()` in user interaction events (`onClick`, `onKeyDown`). The ambient drone and UI sounds activate seamlessly on the first interaction.

### 2.3. Pointer Lock Constraints
- The FPS combat mode relies on `canvas.requestPointerLock()`.
- If pointer lock is denied or exited (by pressing `ESC`), the game immediately pauses and renders the pause menu (`"THE LIFT CAN WAIT"`).
- Fallback controls: When mouse lock is inactive, the game accepts click-and-drag or arrow keys for turning.

---

## 3. Scope Exclusions (Non-Reconstruction Items)

- **Kimi Hosting Watermark:** The target URL is hosted on `ok.kimi.link` and injects two third-party scripts:
  - `https://www.kimi.com/sdk-seed.js`
  - `https://statics.kimi.ai/sdk/publish.DaygGu2u.min.js`
  These scripts render a floating AI attribution badge (`div.kimi-brand-container`) in the bottom-right corner. This is platform wrapper overhead and is intentionally excluded from the target application rebuild.
