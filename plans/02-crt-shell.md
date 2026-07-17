# 02 — CRT Shell: Monitor Frame + Screen Effects

Phase 0 · Depends on: [01-foundation](./01-foundation.md) · Unblocks: [03-boot-sequence](./03-boot-sequence.md), [04-window-manager](./04-window-manager.md)

---

## 1. Mission

The AC-90 CRT monitor is the physical body of the whole experience (§1, §6.1): every route renders *inside* it. This doc builds the bezel, the screen-effect stack, and the CRT/CLEAN display-mode toggle. It is the single place CRT visual effects are implemented — no other subsystem may add its own scanlines. Per the locked stack (master plan §2), everything here is **CSS/SVG-first**; no WebGL.

## 2. Deliverables

- `src/components/crt/CRTFrame.tsx` — bezel + screen viewport wrapper used by the root layout.
- `src/components/crt/Scanlines.tsx`, `DotPitch.tsx`, `ScreenGlitch.tsx`, `PhosphorBloom.tsx`, `Curvature.tsx` — independent effect layers.
- `src/components/crt/DisplayModeToggle.tsx` — the visible `[ CRT ] [ CLEAN ]` control (§6.1).
- `src/styles/crt.css` — all effect keyframes/filters (file stubbed in doc 01).
- Bezel art assets in `public/images/bezel/` (SVG stickers, knobs, vents).

## 3. Technical design

### 3.1 Layer architecture

The shell is a fixed-position stack; page content renders in the `screen` layer, effects are non-interactive overlays:

```text
CRTFrame
├── .bezel            (z 0)  plastic frame, knobs, LED, stickers, vents — outside the screen
└── .screen           (z 1)  position:relative; overflow:hidden; border-radius per curvature
    ├── {children}    (z 1)  the actual app — desktop, windows, boot
    └── .fx           (z 2)  pointer-events:none; aria-hidden
        ├── DotPitch      repeating radial-gradient background-image (§22.3, CSS option)
        ├── Scanlines     repeating-linear-gradient, 3px period, ≤ 0.12 opacity
        ├── PhosphorBloom subtle drop-shadow via CSS filter on a cloned glow layer — cheap version: text-shadow token applied by content, not an overlay
        ├── Curvature     inset box-shadow vignette + SVG feDisplacementMap filter (desktop only)
        └── ScreenGlitch  transform/opacity keyframes for flicker + rolling sync line (§6.1), triggered by route transitions
```

Effect budget: overlays are static background-images (composited once); the only *animated* layers are ScreenGlitch (opacity/transform only — compositor-friendly) and an occasional flicker (`opacity 0.98↔1`, ≤ 1×/45s, disabled under reduced motion). Chromatic aberration near edges (§6.1) is a fixed SVG filter on the vignette, not per-frame work.

### 3.2 Bezel (§6.1)

Charcoal plastic frame with molded `AC-90` label, power LED (amber when off, green after boot — state read from `bootStore`), decorative brightness/contrast knobs, volume slider (the **real** sound toggle — clicking it toggles `settingsStore.soundEnabled`), ventilation slots, floppy slot (easter-egg trigger surface for §21.5, wired in `plans/15`), and stickers:

```text
CUDA INSIDE · INFERENCE READY · DO NOT POWER OFF DURING KERNEL EXECUTION
```

Bezel is pure SVG/CSS (no raster) so it scales crisply; subtle scratches via an SVG noise filter at 3% opacity.

### 3.3 Display mode (§6.1)

`settingsStore.displayMode: 'crt' | 'clean'` → `<html data-display="clean">` removes, per §6.1: scanlines, flicker, distortion/curvature, chromatic aberration, and sound effects (sound wrapper checks displayMode too). DotPitch drops to 0 opacity; the bezel *remains* (it is identity, not noise) but its effects (LED glow pulse) freeze. CLEAN is also auto-selected by: `prefers-reduced-motion` (first visit), `prefers-contrast: more`, and the low-power verdict from `capabilities.ts`.

The toggle itself is always visible in the taskbar tray (doc 04) **and** reachable pre-boot on the unpowered screen (doc 03), keyboard-focusable, with `aria-pressed`.

### 3.4 Responsive frame scaling (§24, shell portion — window behavior is doc 04)

| Breakpoint | Frame |
|---|---|
| ≥ 1024px | Full bezel, curvature on, screen ~4:3 letterboxed inside viewport |
| 640–1023px | Slimmer bezel, curvature off, screen fills width |
| < 640px | Bezel collapses to a thin top strip labeled `AC-90 POCKET TERMINAL` (§24) + status LED; all screen, no curvature |

## 4. Creative direction

- Dot pitch (§22.3) must stay *subordinate to readability*: opacity ≤ 0.08 over text areas; verified against WCAG contrast in acceptance criteria.
- Rolling sync line (§6.1) plays only on route transitions and boot — a 1px bright line sweeping down over 400ms. Never during reading.
- Dust particles (§6.1): ≤ 6 static 1px dots at 4% opacity baked into the DotPitch gradient — free.
- The volume slider making sound *physical* (you reach for the monitor to change volume) is the kind of concept-connected detail §38 asks for.
- LED microcopy on hover: `POWER: SOFT-OFF NOT SUPPORTED SINCE 1994`.

## 5. Dependencies

- Doc 01 tokens (`--bg`, `--text-bright`, cursor/focus tokens) and `settingsStore`.
- `bootStore` (doc 03) for LED state — until doc 03 lands, LED reads a stub `powered: true`.

## 6. Acceptance criteria

- [ ] Every route renders inside CRTFrame; effects never intercept pointer events or appear in the a11y tree (`aria-hidden`, verified by axe).
- [ ] `[ CRT ] [ CLEAN ]` toggle removes exactly the §6.1 list; choice persists across reload; reachable and operable by keyboard from any page.
- [ ] Body text over the effect stack keeps ≥ 4.5:1 contrast (≥ 7:1 in high-contrast mode), measured with effects at max opacity.
- [ ] Scrolling a long blog post inside the shell shows no dropped frames from effect layers (DevTools: no non-composited animations; paint count stable).
- [ ] Reduced-motion: flicker, glitch, rolling line, and LED pulse are all inert.
- [ ] The three breakpoint behaviors above verified on real device emulation; no horizontal scroll at any width ≥ 320px.
- [ ] Volume slider toggles `soundEnabled`; state reflected in taskbar tray icon.

## 7. Risks & fallbacks

| Risk | Fallback |
|---|---|
| SVG feDisplacementMap curvature is slow on low-end GPUs | Curvature is already desktop-only and independently toggleable; drop to border-radius + vignette only (looks 90% as good) |
| Effects legibility complaints from real users | CLEAN mode is the designed answer; additionally expose per-effect opacity const in one place (`crt.css` custom properties) for fast global tuning |
| Letterboxed 4:3 screen wastes space on ultrawide monitors | Cap bezel width at 1600px and allow 16:10 screen ratio ≥ 1440px — bezel proportions flex, content column stays centered |
| Dot pitch moirés with device pixel ratio | Size gradient period in device pixels via `min(2px, 0.125rem)` and test at DPR 1/1.5/2/3; worst case, disable DotPitch below DPR 1.5 |
