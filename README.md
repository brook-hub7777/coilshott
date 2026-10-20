# Nimbus Salvagers

> A high-fidelity, Destiny 2-inspired browser sci-fi action FPS and interactive 3D orbit lobby built with **React 18**, **Three.js (r170+)**, **Tailwind CSS**, and the **Web Audio API**.

---

## ⚡ Overview

**Nimbus Salvagers** pairs an atmospheric sci-fi game lobby (*The Yard*) with a real-time client-side 3D first-person tactical combat engine. Players can customize engineer subclasses, inspect schematics, and launch tactical combat drops with zero external plugins or audio asset downloads.

### Key Features
* 🛸 **3D Orbit Lobby Shell:** Dynamic celestial airship backdrop, day/night lighting, and multi-view navigation (*Play, Destinations, Engineer, Collections*).
* 🎯 **3D Combat Engine:** Smooth WASD movement, Pointer Lock controls, sprint, jump physics, raycast hit detection, weapon recoil, and wave-based enemy AI.
* 🔊 **Procedural Web Audio Synthesizer:** Real-time algorithmic synthesis for all weapons, hits, explosions, abilities, and ambient drones—no external `.mp3` files needed.
* 🛠️ **Deep Loadout System:** Subclass stats, weapon switching, and persistent gear discovery tracking via `localStorage`.

### Tech Stack
* **Frontend:** React 18, Tailwind CSS
* **Graphics:** Three.js (r170+), WebGL2, EffectComposer
* **Audio:** Native Web Audio API (`SoundEngine`)
* **Icons & UI:** Phosphor Icons, Custom SVGs
