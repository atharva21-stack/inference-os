# 05 — Desktop: Icons, Wallpaper, Telemetry, Screensaver

Phase 1 · Depends on: [02-crt-shell](./02-crt-shell.md), [04-window-manager](./04-window-manager.md) · Unblocks: primary navigation for all sections

---

## 1. Mission

The desktop is Phase 3 of the narrative (§4) and the site's real homepage: the moment the visitor realizes the 1994 machine is running something serious. It must read in ten seconds (icon grid = sitemap), reward a longer look (ambient wireframe GPU, live-feeling telemetry), and carry the §37 identity copy. Owns §8, §22.4 (icon system), §21.3 (screensaver), §30 (wireframe), §37 (homepage copy).

## 2. Deliverables

- `src/components/desktop/Desktop.tsx`, `DesktopIcon.tsx`, `Wallpaper.tsx`, `SystemStatus.tsx`, `Screensaver.tsx`.
- `src/stores/telemetry.ts` — decorative metrics engine feeding taskbar tray (doc 04) and SystemStatus.
- Pixel-art icon set in `public/icons/` (SVG, see §3.3).
- `src/app/(os)/desktop/page.tsx`.

## 3. Technical design

### 3.1 Layout (§8.1, §30)

Implements the §30 wireframe:

- **Top-left identity block:** `ATHARVA.RUNTIME` / `AI INFRASTRUCTURE OPERATING SYSTEM`, plus (first visit only, dismissed after any window opens) the §37 copy block: name, role, `I BUILD THE INFRASTRUCTURE THAT MAKES INTELLIGENCE RUN.`, capability list. This puts positioning first per §33 priority 1 and gives crawlers an H1.
- **Icon grid** (loose grid, §8.1), order = §33 content priority: `ABOUT.EXE · EXPERIENCE.DIR · PROJECTS.SYS · GPU_LAB.EXE · FIELD_NOTES · INCIDENTS.LOG · MEMORY.MAP · PERFMON.EXE · RESUME.PDF · CONTACT.COM` (+ hidden floppy `SECRET_PROJECTS`, wired in doc 15). Icons are `<a>`s to their routes (crawlable), double-click *or* single-Enter opens (single-click selects, per era convention; touch = single tap opens).
- **Center-lower:** rotating wireframe GPU (§30 `[ ROTATING WIREFRAME GPU ]`) — see §3.2.
- **SYSTEM STATUS strip** (§30): `GPU FABRIC: ONLINE · BLOG ARCHIVE: MOUNTED`, fed by telemetry.
- Mobile (§24): identity block condensed; icon grid becomes a 2-column launcher; wireframe GPU replaced by a static dithered image.

### 3.2 Ambient wallpaper (§8.2)

Layered, back to front — all cheap:

1. dark dotted grid (CSS gradient, part of doc 02's DotPitch family)
2. faint cluster-topology map (static SVG, 4% opacity)
3. drifting dot-matrix particles (single CSS-animated layer, ≤ 12 dots, paused when tab hidden and under reduced motion)
4. faint oscilloscope waveform (SVG `stroke-dashoffset` loop)
5. hidden coordinates/memory addresses (§8.2) in corners at 3% opacity — one of them, `0x1994:0x2026`, is a doc-15 easter-egg hook
6. **the wireframe GPU**: *not* WebGL on the desktop. It is a pre-rendered sprite sequence (24 frames, one KTX2/WebP sprite sheet ≤ 60 kB) exported from the doc-10 Blender asset, stepped at 8fps for a period-correct look. Keeps three.js out of the desktop bundle (master plan §2.1) while fulfilling §34 Phase 1's "one 3D GPU object". Static single frame under reduced motion/low power.

### 3.3 Icon system (§22.4)

SVG pixel-art (crisp at 2×; `shape-rendering: crispEdges`), 32×32 grid, beige-workstation palette accents over phosphor (§8.1), styled after Win3.1/early Mac/DOS utilities (§22.4). Custom icons required by §22.4: GPU card, server rack, memory, network, blog/document, incident, resume, contact, terminal, benchmark — plus folder, floppy, printer for docs 12/15. Labels in Display font below each icon, `.dotlabel` treatment. Each icon ships as a React component with `title` for tooltips and reuse by taskbar/start menu (doc 04) at 16×16.

### 3.4 Telemetry engine (`telemetryStore`)

```ts
interface Telemetry { gpuLoad: number; hbmUsed: number; net: 'OK'|'DEGRADED'; uptimeSec: number }
```

- Baseline: slow random walk (tick 2s, ±3 points, clamped 55–85% GPU) — alive but calm (§3 tone).
- **Reactive rules (§6.3):** window open +5 GPU; GPU Lab open → 85–95 band; running any doc-11 simulation → spike to 97 then decay; OVERCLOCK (doc 15) shifts the whole band up; idle 60s → decay toward 40%.
- Ticks pause when tab hidden (§26). Values are decorative; `SystemStatus` renders them `aria-hidden` with an sr-only line "Decorative system telemetry".

### 3.5 Screensaver (§21.3)

After 3 minutes idle (no pointer/key/scroll): bouncing `ATHARVA.RUNTIME` logotype (DVD-style, CSS transform animation) or the wireframe-GPU sprite drifting — random pick. Any input dismisses instantly. Never engages under reduced motion, on mobile, or while media/simulation is running. Implemented as a portal layer above windows, `aria-hidden`, and it *does not* steal focus.

## 4. Creative direction

- Desktop copy is sparse: the §37 block and icon labels do the talking. No taglines scattered around (§3: calm, not cluttered).
- Icon hover: 1px phosphor outline + tooltip in BIOS style (`ABOUT.EXE — 14,336 BYTES — OPERATOR PROFILE`); fake byte sizes are consistent per icon (seeded), a §38-compliant wink that rewards attention.
- Selection uses the inverted-phosphor token (doc 01); rubber-band multi-select is deliberately **out of scope** (no multi-icon operations exist — §38: remove what doesn't serve understanding).
- SYSTEM STATUS strings rotate through mounted-archive lines (`CAREER ARCHIVE: 6 SYSTEMS · FIELD_NOTES: 8 DOCUMENTS`) — real counts sourced from Velite at build time, keeping decoration truthful.

## 5. Dependencies

Doc 02 (shell/effects), doc 04 (WindowLayer renders above; taskbar consumes `telemetryStore`), doc 10 (Blender GPU asset for the sprite render — until it exists, a placeholder wireframe SVG rotates), doc 06 (Velite counts for status strings).

## 6. Acceptance criteria

- [ ] Desktop matches §30 wireframe structure at ≥ 1024px; icon order follows §33 priorities; all icons navigate correctly by mouse, touch, and keyboard (arrow keys move selection, Enter opens).
- [ ] `curl /desktop` returns H1 + §37 copy + all icon links (crawlability).
- [ ] Wireframe GPU animates at 8fps from sprite sheet ≤ 60 kB; static under reduced motion; three.js absent from desktop route chunk.
- [ ] Telemetry reacts per rules (open GPU Lab → tray GPU climbs); pauses when tab hidden; absent from a11y tree.
- [ ] Screensaver engages at 3min idle, dismisses on any input without focus loss, never engages under reduced motion/mobile.
- [ ] Wallpaper layers cost < 2ms/frame combined (performance trace while idle).
- [ ] Icon set renders crisply at DPR 1–3; taskbar reuses 16×16 variants.

## 7. Risks & fallbacks

| Risk | Fallback |
|---|---|
| Sprite-sheet GPU looks cheap vs expectations of "one 3D object" | Escalation path: mount a real R3F canvas on desktop *only* on high-capability desktops (capabilities.ts gate), keeping sprite as universal fallback — decision at Gate G1 review |
| Telemetry randomness reads as fake and undermines credibility | Tune toward fewer, slower changes; keep only interaction-driven changes if the walk still feels fake (§38 test) |
| Icon art time-sink (12+ custom pixel icons) | Ship Phase 1 with 6 core icons + 2 generic (folder/document); complete the set in Phase 2; style guide in this doc keeps them consistent |
| Idle timer conflicts with reading long content in a window | Screensaver disabled while any window is focused *and* scrolled recently; only fires on true desktop idle |
