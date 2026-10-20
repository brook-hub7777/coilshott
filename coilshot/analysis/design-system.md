# Design System Specification: Nimbus Salvagers

## 1. Color System & Surface Hierarchy

The visual identity of Nimbus Salvagers balances a deep, military-grade aerospace navy theme with warm solar coral accents and high-contrast technical cyan/gold indicators.

### 1.1. Core Palette Tokens

```css
:root {
  /* Surface & Background Layers */
  --color-surface-base: #0e121c;         /* Deepest background shade */
  --color-surface-panel: #131824;        /* Primary card and section panel */
  --color-surface-panel-border: rgba(142, 165, 195, 0.18); /* Subtle architectural glass border */
  --color-surface-overlay: rgba(14, 18, 28, 0.72); /* Dialog and shade overlay */
  --color-surface-shade: rgba(14, 24, 38, 0.85);   /* Heavy menu shade */

  /* Primary Brand Accents (Solar & Lift) */
  --color-accent-coral: #ec947e;         /* Primary brand accent (Launch button, active tabs) */
  --color-accent-coral-glow: #e69580;    /* Hover and focus ring coral */
  --color-accent-coral-dark: #c98672;    /* Darker coral border */
  --color-accent-gold: #ffc857;          /* Critical damage, objective rewards, legendary rank */
  --color-accent-blue: #8898c1;          /* Story / campaign signature color */
  --color-accent-cyan: #8eb9c5;          /* Vanguard / cooperative signature color */
  --color-accent-purple: #c6b2e1;        /* Condensate / Mistwright signature color */

  /* Text & Foreground Hierarchy */
  --color-text-primary: #f1f2f6;       /* Crisp white for headlines and key stats */
  --color-text-secondary: #a3aec9;     /* Soft slate blue for descriptions and subheadings */
  --color-text-muted: #696f7d;         /* Dim gray for metadata separators and inactive elements */
  --color-text-accent: #ec947e;        /* Highlight coral text */

  /* Status & Feedback Indicators */
  --color-status-online: #4ade80;       /* Green beacon dot ("SYSTEM ONLINE") */
  --color-status-danger: #ef4444;       /* Enemy hostiles, low health pulse */
  --color-shield: #60a5fa;              /* Plasma shield bar */
  --color-health: #e2e8f0;              /* Core vitality bar */
}
```

---

## 2. Typography System

The application uses three Google Fonts to create a technical aerospace hierarchy:

| Role | Font Family | Weights Observed | Usage Context |
| :--- | :--- | :--- | :--- |
| **Display / Headlines** | `Orbitron, sans-serif` | 700, 800, 900 | Hero title (`RIDE THE UPDRAFT.`), section titles (`CHOOSE YOUR PATH.`, `YOUR ENGINEER.`), wave announcements (`WAVE 1 OF 3`), card headings. |
| **Technical / Monospace** | `Barlow Condensed, sans-serif` | 500, 600, 700 | Navigation buttons, eyebrows, coordinates, ammo counters (`32 / 320`), stat numbers, keybinding badges. |
| **Body / Information** | `Manrope, sans-serif` | 400, 500, 600 | Lore descriptions, dialog paragraphs, field notes, tooltips, settings subtexts. |

### Typography Scale

```css
/* Display Titles */
.hero-title {
  font-family: "Orbitron", sans-serif;
  font-size: clamp(2.5rem, 6vw, 4.25rem);
  font-weight: 800;
  line-height: 1.05;
  letter-spacing: 0.04em;
  text-transform: uppercase;
}

/* Section Headings */
.section-heading {
  font-family: "Orbitron", sans-serif;
  font-size: 2.25rem;
  font-weight: 700;
  letter-spacing: 0.05em;
  text-transform: uppercase;
}

/* Eyebrows & Badges */
.eyebrow {
  font-family: "Barlow Condensed", sans-serif;
  font-size: 0.8125rem;
  font-weight: 600;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: var(--color-accent-coral);
}

/* Body Text */
.body-copy {
  font-family: "Manrope", sans-serif;
  font-size: 0.9375rem;
  line-height: 1.6;
  color: var(--color-text-secondary);
}

/* Numeric Data & Counters */
.hud-numeric {
  font-family: "Barlow Condensed", sans-serif;
  font-size: 2rem;
  font-weight: 700;
  letter-spacing: 0.08em;
}
```

---

## 3. Elevation, Borders & Glassmorphism

The aesthetic relies on sci-fi HUD "hard-light glass" panels layered over real-time 3D environments:

```css
/* Card Container */
.panel-card {
  background: rgba(19, 24, 36, 0.78);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  border: 1px solid rgba(142, 165, 195, 0.18);
  border-radius: 12px;
  box-shadow: 0 12px 32px rgba(6, 10, 18, 0.45);
}

/* Selected Active Card */
.panel-card.selected {
  border-color: var(--color-accent-coral);
  box-shadow: 0 0 24px rgba(236, 148, 126, 0.22), inset 0 0 12px rgba(236, 148, 126, 0.08);
}

/* Modal Dialog Surface */
.modal-surface {
  background: rgba(14, 18, 28, 0.92);
  backdrop-filter: blur(24px);
  border: 1px solid rgba(162, 180, 205, 0.22);
  border-radius: 16px;
  box-shadow: 0 24px 64px rgba(0, 0, 0, 0.7);
}
```

---

## 4. Keyframe Animations & Micro-Interactions

Six verified CSS animations govern the fluid feel of the interface:

```css
/* Modal and Dialog Entrance */
@keyframes dialog-in {
  0% {
    opacity: 0;
    transform: translateY(8px) scale(0.99);
  }
  100% {
    opacity: 1;
    transform: translateY(0) scale(1);
  }
}

/* Card Content Staggered Reveal */
@keyframes reveal-in {
  0% {
    opacity: 0;
    transform: translateY(13px);
  }
  100% {
    opacity: 1;
    transform: translateY(0);
  }
}

/* Active Mission Beacon Pulse */
@keyframes signal-pulse {
  0%, 100% {
    opacity: 0.65;
  }
  50% {
    opacity: 1.0;
  }
}

/* Low Shield & Health Pulse Warning */
@keyframes health-pulse {
  0%, 100% {
    opacity: 0.35;
  }
  50% {
    opacity: 0.65;
  }
}

/* World-Projected Floating Damage Numbers */
@keyframes damage-float {
  0% {
    opacity: 1;
    transform: translate(-50%, 0);
  }
  100% {
    opacity: 0;
    transform: translate(-50%, -35px);
  }
}

/* Wave & Milestone Announcement */
@keyframes announcement-in {
  0% {
    opacity: 0;
    margin-top: 10px;
  }
  100% {
    opacity: 1;
    margin-top: 0;
  }
}
```

---

## 5. Responsive Breakpoint Matrix

| Viewport Category | Width Threshold | Layout Adaptation Observed |
| :--- | :--- | :--- |
| **Desktop (Large)** | `≥ 1280px` | 3-column asymmetric layout. Left: Hero copy; Right: Tracked mission & Engineer tile; Bottom: 3-column activity card grid; Header: Full wordmark, 4 nav items, power currency, settings button. |
| **Tablet** | `768px - 1023px` | Header hides `"SYSTEM ONLINE"` and currency counter to preserve breathing room. Right tracked mission card is hidden; Engineer tile relocates to right middle; 3 activity cards scale horizontally. |
| **Mobile** | `< 768px` (e.g. 390px) | Full single-column vertical stack. Header logo and settings top row; main nav buttons wrap onto secondary row. Hero section full-width. Engineer card stacked below hero. Activity cards switch to vertical list with thumbnail left and text right. |

---

## 6. Audio Token System (Web Audio API Synthesizer)

Rather than loading remote audio files, all sound effects are generated algorithmically:

| Sound Preset | Oscillator / Filter Type | Base Frequency | Modulation / Envelopes | Gain / Duration |
| :--- | :--- | :--- | :--- | :--- |
| **Ambient Yard Drone** | 4 Sine Oscillators | `[55.0, 82.41, 110.0, 164.81] Hz` (A1, E2, A2, E3) | Detuned by `index * 2.7 Hz` | Master gain `0.12`, continuous loop |
| **Gunshot (Auto/Kinetic)** | White Noise Buffer + Sawtooth Tone | `220 Hz -> 35 Hz` | Lowpass filter at `2500 Hz` | Noise gain `0.36`, duration `150ms` |
| **Gunshot (Heavy Cannon)** | White Noise Buffer + Sawtooth Tone | `150 Hz -> 35 Hz` | Lowpass filter at `2500 Hz` | Noise gain `0.70`, duration `150ms` |
| **Hit Confirm (Normal)** | Sine Wave | `900 Hz -> 500 Hz` | Exponential pitch drop | Gain `0.13`, duration `90ms` |
| **Hit Confirm (Crit/Eye)** | Sine Wave | `1460 Hz -> 500 Hz` | Sharp high-register ping | Gain `0.13`, duration `90ms` |
| **Kill Confirmation** | Triangle Wave | `520 Hz -> 1100 Hz` | Ascending pitch sweep | Gain `0.20`, duration `160ms` |
| **Weapon Reload** | White Noise + Square Wave | `180 Hz` click | Lowpass filter at `4000 Hz` | Gain `0.045`, duration `170ms` |
| **Class Ability Trigger** | Sawtooth Wave + White Noise | `90 Hz -> 450 Hz` | Exponential frequency rise, filter at `1500 Hz` | Gain `0.18`, duration `650ms` |
| **Explosion** | White Noise + Sub-Sine Wave | `100 Hz -> 25 Hz` | Lowpass filter at `650 Hz` | Gain `0.80`, duration `800ms` |
| **UI Click / Hover** | Sine Wave | `700 Hz -> 1100 Hz` | Clean upward chirp | Gain `0.10`, duration `80ms` |
| **Mission Victory** | 4 Harmonic Sines | `[330, 415, 494, 660] Hz` (E Major) | Staggered chord arpeggiation | Gain `0.12`, duration `1.2s - 1.5s` |
