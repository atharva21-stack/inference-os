# 03 — Boot Sequence: Power-On, BIOS, Diagnostics

Phase 1 · Depends on: [01-foundation](./01-foundation.md), [02-crt-shell](./02-crt-shell.md) · Unblocks: [05-desktop](./05-desktop.md) (narratively; desktop is what boot lands on)

---

## 1. Mission

The boot sequence is Phase 1–2 of the experience narrative (§4): *discovery* of an unpowered machine, then *boot* into ATHARVA.RUNTIME. It is the site's first impression and its biggest conversion risk — it must be skippable within one second (§7.2) and must never gate returning visitors or crawlers away from content (§25, §29).

## 2. Deliverables

- `src/app/page.tsx` — the `/` route: unpowered state → boot → redirect/transition to `/desktop` (route amendment 3, master plan §3: no separate `/boot` route).
- `src/components/boot/PowerButton.tsx`, `BootScreen.tsx`, `BiosLog.tsx`, `SkipBoot.tsx`.
- `src/stores/boot.ts` — boot state machine.
- GSAP timeline module `src/components/boot/bootTimeline.ts` (GSAP dynamically imported on this route only).
- Sounds: `public/sounds/power-click.mp3`, `startup-tone.mp3` (played only if sound enabled — which is off by default, so first-visit boot is silent).

## 3. Technical design

### 3.1 State machine (`bootStore`)

```ts
type BootPhase = 'unpowered' | 'flash' | 'bios' | 'diagnostics' | 'ready' | 'desktop'
interface BootStore {
  phase: BootPhase
  hasBootedBefore: boolean      // localStorage: 'ac90:booted'
  powerOn(): void; skip(): void; enterDesktop(): void; replayBoot(): void
}
```

- **First visit:** `/` shows `unpowered`. POWER → timeline below → `ready` (PRESS ENTER) → `desktop` (client navigation to `/desktop`).
- **Returning visitor (§7.2):** `hasBootedBefore` → `/` shows a 300ms CRT warm-up shimmer then goes straight to `/desktop`. `REPLAY BOOT` lives in the Start menu (doc 04) and calls `replayBoot()`.
- **Crawlers/no-JS:** `/` server-renders the §37 homepage copy (name, role, brand line, capability list, `[ ENTER SYSTEM ]` link to `/desktop`) *underneath* the boot layer — boot is progressive enhancement, content is never JS-gated (§29).
- **Deep links** (`/about` etc.) never see boot at all.

### 3.2 Unpowered state (§7.1)

Near-black screen inside the CRT shell. Visible: CRT outline, amber LED (from doc 02 bezel), `[ POWER ]` button below the screen, and tiny text `AC-90 HIGH PERFORMANCE WORKSTATION`. No navigation shown (§7.1) — but an sr-only skip link ("Skip intro, go to desktop") is first in tab order (§25), and the DisplayModeToggle is reachable (doc 02 §3.3).

### 3.3 Timeline (§7.2) — one GSAP timeline, labels matching the brief exactly

```text
0.0s  power press        → mechanical click (0.1s), settingsStore-gated
0.2s  white CRT flash    → full-screen white, 80ms, then horizontal line collapse
0.4s  horizontal scan line
0.8s  BIOS logo          → "AC-90 SYSTEM BIOS 4.6 / COPYRIGHT 1994–2026 ATHARVA SYSTEMS LAB"
1.0s  [ SKIP BOOT ] appears (§7.2: after one second)
1.2s  hardware diagnostics — §7.2 boot log lines type in, dot-leader style
2.5s  GPU topology detection line
3.5s  CAREER ARCHIVE...... MOUNTED
4.2s  BLOG INDEX.......... MOUNTED
5.0s  PRESS ENTER TO LAUNCH INFERENCE OS  (blinking block cursor)
```

The full log is the §7 boot log verbatim:

```text
AC-90 SYSTEM BIOS 4.6
COPYRIGHT 1994–2026 ATHARVA SYSTEMS LAB

CPU................. ONLINE
GPU FABRIC.......... DETECTED
HBM CHANNELS........ STABLE
KUBERNETES CONTROL.. CONNECTED
MODEL SERVERS....... READY
TELEMETRY BUS....... ACTIVE
CAREER ARCHIVE...... MOUNTED
BLOG INDEX.......... MOUNTED
PORTFOLIO KERNEL.... LOADED

USER: ATHARVA
ROLE: AI INFRASTRUCTURE ENGINEER

PRESS ENTER TO LAUNCH INFERENCE OS
```

`ENTER`, click/tap anywhere, or `SKIP BOOT` all resolve to `enterDesktop()`. Reduced motion: timeline collapses to a single 300ms fade with the complete log rendered statically (no typewriter).

### 3.4 Easter-egg lines (§7.3)

One (never more — "use sparingly") random line from the §7.3 pool is spliced into the diagnostics block per boot:

```text
DETECTING 136 GB300 ACCELERATORS... · NO BOTTLENECKS FOUND. SUSPICIOUS. ·
LOADING WARPS 0–31... · CHECKING P95 LATENCY... · MOUNTING /CAREER/DELOITTE... ·
CONNECTING TO HBM CHANNEL 7... · KERNEL PANIC AVOIDED.
```

## 4. Creative direction

- The typewriter reveal is per-line (not per-character) for log lines — real BIOSes print lines, not letters; only `PRESS ENTER` gets the blinking block cursor (token from doc 01).
- Dot leaders (`CPU.................`) are rendered with a CSS dotted-leader technique, not hand-typed periods, so alignment survives font metrics.
- White flash respects photosensitivity: ≤ 80ms, no strobe repetition, and skipped entirely under reduced motion (WCAG 2.3.1-safe).
- The screen stabilization wobble after the flash reuses doc 02's ScreenGlitch — boot owns no bespoke effects.
- BIOS logo is text-art, not an image: `ATHARVA SYSTEMS LAB` in the Display font with a double-line box border.

## 5. Dependencies

Doc 01 (tokens, settingsStore, fonts), doc 02 (shell, LED state, ScreenGlitch). Desktop route (doc 05) must exist to land on — until it does, `enterDesktop()` targets a placeholder `/desktop`.

## 6. Acceptance criteria

- [ ] First visit: full sequence matches §7.2 timings within ±100ms; ENTER/click/skip all work; `SKIP BOOT` visible at 1.0s.
- [ ] Second visit: `/` reaches `/desktop` in < 500ms with no boot; `REPLAY BOOT` (Start menu) replays it.
- [ ] `curl`/JS-disabled fetch of `/` returns §37 homepage copy and a crawlable link to `/desktop` (view-source check + Playwright with JS off).
- [ ] Reduced motion: no flash, no typewriter, single fade, full log readable.
- [ ] Sound: silent by default; with sound enabled, click + startup tone play once, duck correctly if user skips mid-tone.
- [ ] Keyboard-only: tab reaches skip link → power button; Enter powers on; Enter again enters desktop; focus lands on first desktop icon (handoff contract with doc 05).
- [ ] Exactly one §7.3 easter-egg line per boot, randomized.
- [ ] GSAP loads only on `/` (verify route-level chunk in build output).

## 7. Risks & fallbacks

| Risk | Fallback |
|---|---|
| Boot feels like an obstacle in usability tests | Timings are all in one const table in `bootTimeline.ts` — compress total to 3s without code changes; worst case, auto-skip after first `ready` |
| GSAP + React 19 strict-mode double-invoke replays timeline | Timeline creation guarded in `useEffect` with cleanup `kill()`; covered by a Playwright assertion that log lines render exactly once |
| localStorage unavailable (private mode) | `hasBootedBefore` falls back to sessionStorage → in-memory; worst case visitor sees boot each visit — acceptable, skippable |
| SEO tools penalize the near-black first paint | The SSR copy layer *is* the first contentful paint (visually hidden but rendered); verified in Lighthouse "content painted" audit — if flagged, render the §37 copy dimly visible on the unpowered screen instead of sr-only |
