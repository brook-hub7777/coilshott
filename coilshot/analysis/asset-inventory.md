# Asset Inventory: Nimbus Salvagers

## 1. Static Graphical Assets

| Asset Path | Format | Size / Resolution | Verified Usage | Legitimacy / Reuse Notes |
| :--- | :--- | :--- | :--- | :--- |
| **`/art/sky-yard.png`** | PNG | 2.73 MB (2560 × 1440) | Full-screen background plate for the Orbit Lobby ("The Yard"). Renders floating celestial docks, airships, sails, and clouds. | Downloaded & archived in `analysis/reference-assets/art/sky-yard.png`. Safe to optimize to WebP (~600 KB) for production rebuild. |
| **`/favicon.svg`** | SVG | 274 B | Browser tab favicon and brand mark. Airship balloon icon. | Inline SVG format; fully recreatable. Archived in `analysis/reference-assets/favicon.svg`. |

---

## 2. Typography Assets (Font Files)

All fonts are embedded and self-hosted as TrueType files:

| Font Family | Weight | File Path | Usage in Target Application |
| :--- | :--- | :--- | :--- |
| **`Barlow Condensed`** | 400 (Regular) | `fonts/font-0.ttf` (80.9 KB) | Secondary body copy, table labels |
| **`Barlow Condensed`** | 500 (Medium) | `fonts/font-1.ttf` (81.4 KB) | Nav buttons, coordinates, timestamps |
| **`Barlow Condensed`** | 600 (SemiBold) | `fonts/font-2.ttf` (86.6 KB) | Eyebrow badges, stat labels, keycap text |
| **`Barlow Condensed`** | 700 (Bold) | `fonts/font-3.ttf` (87.1 KB) | Ammo counters (`32 / 320`), score totals |
| **`Manrope`** | 400 (Regular) | `fonts/font-4.ttf` (94.9 KB) | Mission lore descriptions, field notes |
| **`Manrope`** | 500 (Medium) | `fonts/font-5.ttf` (95.0 KB) | Dialog paragraphs, subheadings |
| **`Manrope`** | 600 (SemiBold) | `fonts/font-6.ttf` (95.0 KB) | Stat tooltips, settings subtexts |
| **`Manrope`** | 700 (Bold) | `fonts/font-7.ttf` (94.9 KB) | Button labels, callout headers |
| **`Manrope`** | 800 (ExtraBold) | `fonts/font-8.ttf` (95.6 KB) | Modal action emphasis |
| **`Orbitron`** | 100-900 (Variable) | `fonts/original/display.ttf` (38.5 KB) | Primary headlines, game title, wave announcements |

*All 10 font files are archived in `analysis/reference-assets/fonts/` and can also be referenced directly via Google Fonts.*

---

## 3. Vector Icons & Schematic Blueprints (SVGs)

### 3.1. Brand & Emblem SVGs

```xml
<!-- Airship Balloon Brand Logo (viewBox 0 0 48 48) -->
<svg viewBox="0 0 48 48" fill="none" stroke="currentColor" stroke-width="1.5">
  <ellipse cx="24" cy="17" rx="17" ry="13"></ellipse>
  <path d="M7 17h34M24 4v26M14 7q-7 11 3 22M34 7q7 11-3 22M13 27l6 13m16-13-6 13M18 40h12v5H18Z"></path>
</svg>

<!-- Campaign Emblem: The Beryl Signal (viewBox 0 0 56 56) -->
<svg viewBox="0 0 56 56" fill="none" stroke="currentColor" stroke-width="1.35">
  <path d="m28 3 25 25-25 25L3 28Z" opacity=".42"></path>
  <path d="m28 11 13 8v15l-13 9-13-9V19Z"></path>
  <path d="m15 19 13 8 13-8M28 27v16M21 15l14 23M35 15 21 38"></path>
  <circle cx="28" cy="27" r="4" fill="currentColor" stroke="none"></circle>
</svg>

<!-- Vanguard Emblem: Through the Squall (viewBox 0 0 56 56) -->
<svg viewBox="0 0 56 56" fill="none" stroke="currentColor" stroke-width="1.35">
  <path d="m28 3 25 25-25 25L3 28Z" opacity=".42"></path>
  <path d="m14 17 14 7 14-7-7 22H21Z"></path>
  <path d="m17 18 11 20 11-20M28 11v13M22 12h12"></path>
  <path d="m13 35 7 8h16l7-8"></path>
</svg>

<!-- Wind Trials Emblem: Trial by Wind (viewBox 0 0 56 56) -->
<svg viewBox="0 0 56 56" fill="none" stroke="currentColor" stroke-width="1.35">
  <path d="m28 3 25 25-25 25L3 28Z" opacity=".42"></path>
  <path d="m17 14 22 25M39 14 17 39M13 13l8 1-7 7ZM43 13l-8 1 7 7ZM14 35l7 7M42 35l-7 7"></path>
  <circle cx="28" cy="27" r="12"></circle>
</svg>

<!-- Rigger Subclass Wheel Emblem (viewBox 0 0 48 48) -->
<svg viewBox="0 0 48 48" fill="none" stroke="currentColor" stroke-width="1.6">
  <circle cx="24" cy="24" r="17"></circle>
  <path d="M24 7v34M7 24h34M12 12l24 24M12 36l24-24"></path>
  <circle cx="24" cy="24" r="5" fill="currentColor"></circle>
</svg>
```

### 3.2. Weapon Schematic Blueprint (`Hs` Component)

```xml
<!-- Weapon Blueprint Schematic (viewBox 0 0 400 150) -->
<svg viewBox="0 0 400 150" fill="none" stroke="currentColor">
  <path d="M30 75h340M65 20v110M330 20v110" opacity="0.3"></path>
  <rect x="80" y="42" width="228" height="66" rx="12" stroke-width="2"></rect>
  <!-- 6 chamber pulse bars -->
  <path d="M104 51v48" stroke-width="7" opacity="0.9"></path>
  <path d="M135 51v48" stroke-width="7" opacity="0.45"></path>
  <path d="M166 51v48" stroke-width="7" opacity="0.9"></path>
  <path d="M197 51v48" stroke-width="7" opacity="0.45"></path>
  <path d="M228 51v48" stroke-width="7" opacity="0.9"></path>
  <path d="M259 51v48" stroke-width="7" opacity="0.45"></path>
  <circle cx="57" cy="75" r="20" stroke-width="3"></circle>
  <circle cx="337" cy="75" r="17" stroke-width="3"></circle>
  <path d="M117 26h148M117 123h148" stroke-dasharray="3 9"></path>
  <text x="200" y="20" text-anchor="middle" fill="currentColor" stroke="none" font-size="10" letter-spacing="3">
    PRESSURE CHAMBER / CONTINUOUS
  </text>
</svg>
```

---

## 4. Procedural 3D & WebGL Asset Generator

The 3D environment requires **no binary `.gltf` or `.glb` files** to be downloaded over the network; everything is generated procedurally on startup:

1. **Procedural Ceramic Material (`w1`):**  
   Dynamically draws a procedural high-anisotropy noise and line pattern to a 256×256 HTML5 canvas, converting it to a Three.js `CanvasTexture` repeated across stone pillars, archways, and ruins.
2. **Atmospheric Sky Dome (`d4`):**  
   Spherical dome with custom vertex and fragment shaders creating a smooth vertical gradient between horizon mist (`#9298a7`) and celestial deep blue (`#30384b`).
3. **Instanced Grass & Dust Particles:**  
   Uses `THREE.InstancedMesh` with 1,500 instances of simple grass tufts and 400 floating wind motes animated by a periodic sine wind vector.
4. **Procedural First-Person Weapon:**  
   Assembled hierarchically using `RoundedBoxGeometry` (`V1`), small cylinder emitters, emissive optic sight meshes, and a dynamic point light for muzzle discharge.

---

## 5. Procedural Web Audio Sound Engine (`SoundEngine`)

The target loads **zero `.mp3` or `.wav` files**. All audio is synthesized through the Web Audio API:

- **Ambient Yard Drone:** 4-voice sine drone cluster `[55.0, 82.41, 110.0, 164.81] Hz`.
- **Guns & Explosions:** Exponentially decaying lowpass-filtered white noise buffers combined with downward frequency-ramped oscillator drops.
- **Feedback & Hits:** High-frequency pure sine chirps and triangle waves.
- **Victory:** Ascending 4-note E-major chord arpeggio.
