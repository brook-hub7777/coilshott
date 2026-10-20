# NIMBUS SALVAGERS: Technical Specification & Reverse Engineering Report

> **Target URL:** `https://vwqqgcdvyw7og.ok.kimi.link/`  
> **Inspection Date:** 2026-10-19  
> **Inspector:** Senior Web Reverse Engineer (REA Suite)  
> **Target Title:** Nimbus Salvagers  
> **Target Type:** Web-based Destiny 2-inspired Sci-Fi Action FPS & Interactive 3D Orbit Lobby Application  

---

## 1. Executive Summary

**Nimbus Salvagers** is a production-grade, highly polished single-page web application combining an ambient sci-fi game lobby ("The Yard") with a full real-time 3D first-person shooter (FPS) combat mission ("The Beryl Signal", "Through the Squall", and "Trial by Wind") built using **React 18**, **Three.js (r170+)**, **Tailwind CSS**, and the **Web Audio API**.

The application renders a dual-mode experience:
1. **The Orbit/Lobby Interface (`menu-shell`):**
   - Immersive 3D background with celestial airships, dynamic clouds, wind particles, and day-night lighting.
   - Four primary navigation views: **PLAY** (Hero banner, mission tracking, character badge, quick-launch adventure cards), **DESTINATIONS** (Starchart / planetary orbital selector), **ENGINEER** (Interactive 3D character preview, subclass customization, stat bars, weapon loadouts), and **COLLECTIONS** (Loot codex, blueprint schematics, discovery tracking).
   - Three full interactive overlay modals: **Settings** (Audio, Mouse Sensitivity, Graphics Quality, Reduced Motion, Invert Y), **Field Guide / Controls** (Complete WASD/mouse action mapping and gameplay lore), and **Mission Briefing** (Detailed lore, location coordinates, difficulty, rewards).
2. **The 3D Tactical Combat Engine (`y4` Game Engine):**
   - Full client-side Three.js FPS renderer with Pointer Lock API support, smooth WASD movement, sprint, jump gravity, weapon recoil, raycasted hit detection, enemy AI (Vane Walkers and Rotor Marshal boss), wave progression, and post-processing bloom/tone mapping.
   - Built-in dynamic procedural synthesizer (`SoundEngine`) that synthesizes all audio in real-time (drones, firing, hits, crits, reloads, abilities, explosions, UI chirps) without requiring any external audio MP3 files.

All findings, DOM layouts, design tokens, data structures, and runtime mechanics have been verified via headless Chromium CDP inspection, visual artifact verification, and bundle deconstruction.

---

## 2. Target Overview & Tech Stack Breakdown

| Layer | Implementation Observed | Re-implementation Choice |
| :--- | :--- | :--- |
| **Framework** | React 18 (production module bundle `index-CFnmQ0aO.js`) | React 18+ / Vite + TypeScript |
| **3D Engine** | Three.js (r170+, bundled as `three-CoWDQTm9.js`) | Three.js / `@types/three` |
| **Post-Processing** | Three.js `EffectComposer`, `RenderPass`, `OutputPass` | Three.js Postprocessing |
| **Styling** | Tailwind CSS + Custom CSS Variables (`index-Cunqe2o7.css`) | Tailwind CSS + CSS Modules / Theme |
| **Typography** | `Barlow Condensed` (400-700), `Manrope` (400-800), `Orbitron` (Display) | Google Fonts / Self-hosted TTF |
| **Icons** | Phosphor Icons (`@phosphor-icons/react` / SVG duotone) + Custom SVGs | `@phosphor-icons/react` + inline SVGs |
| **Audio** | Pure procedural Web Audio API synthesis (`AudioContext`) | Web Audio API class (`SoundEngine`) |
| **Persistence** | Browser `localStorage` (key: `nimbus-engineer-v1`) | `localStorage` API |
| **Artwork** | High-res background plate `sky-yard.png` (2.7 MB, 2560x1440) | Optimized WebP/PNG asset |

---

## 3. Deliverables Directory Map

This technical specification is structured across dedicated Markdown documents in this directory:

1. [**Route Inventory (`route-inventory.md`)**](./route-inventory.md):  
   Comprehensive inventory of all client-side view states, screen identifiers, modal overlays, URL parameter handling, and history behavior.
2. [**Component Inventory (`component-inventory.md`)**](./component-inventory.md):  
   Complete component hierarchy, props interface definitions, state bindings, and DOM structures for both the UI shell and the 3D game canvas.
3. [**Design System Specification (`design-system.md`)**](./design-system.md):  
   Design tokens, color palettes, typography scales, layout grids, spacing rules, border radii, shadows, animations, and keyframes.
4. [**Interactions & Behavior Specification (`interactions-and-behavior.md`)**](./interactions-and-behavior.md):  
   Detailed behavioral specifications for mouse, keyboard, touch, modal lifecycle, 3D camera controls, weapon mechanics, wave triggers, and audio triggers.
5. [**Asset Inventory (`asset-inventory.md`)**](./asset-inventory.md):  
   Catalog of all static imagery, TTF fonts, SVG icons and schematics, procedural 3D geometries, procedural textures, and procedural sound presets.
6. [**Unknowns & Limitations (`unknowns-and-limitations.md`)**](./unknowns-and-limitations.md):  
   Rigorous boundary documentation distinguishing verified CDP observations from inferred candidates, edge cases, and hardware dependencies.
7. [**Prioritized Implementation Plan (`implementation-plan.md`)**](./implementation-plan.md):  
   Phased, milestone-driven roadmap to rebuild the entire application from scratch with zero functional or visual regressions.

---

## 4. Visual Evidence Archive

All captured evidence screenshots are preserved in [`analysis/screenshots/`](./screenshots/):
- **Desktop Play Screen:** [`desktop-play.png`](./screenshots/desktop-play.png)
- **Tablet Responsive View (768px):** [`tablet-play.png`](./screenshots/tablet-play.png)
- **Mobile Responsive View (390px):** [`mobile-play.png`](./screenshots/mobile-play.png)
- **Destinations Screen:** [`desktop-destinations.png`](./screenshots/desktop-destinations.png)
- **Engineer Customization Screen:** [`desktop-engineer.png`](./screenshots/desktop-engineer.png)
- **Collections Screen:** [`desktop-collections.png`](./screenshots/desktop-collections.png)
- **Settings Modal:** [`modal-settings.png`](./screenshots/modal-settings.png)
- **Controls & Field Guide Modal:** [`modal-controls.png`](./screenshots/modal-controls.png)
- **Tactical Gameplay HUD (3D FPS):** [`gameplay-hud.png`](./screenshots/gameplay-hud.png)
- **In-Game Pause Menu:** [`gameplay-pause.png`](./screenshots/gameplay-pause.png)
