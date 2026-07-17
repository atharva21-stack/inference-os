# 01 — Foundation: Scaffold, Design Tokens, Typography, CI

Phase 0 · Depends on: [00-master-plan](./00-master-plan.md) · Unblocks: every other doc

---

## 1. Mission

Stand up the repository so that every later subsystem drops into a working, tested, token-driven codebase. The foundation carries the *invisible* half of the creative thesis (§1): a site that claims infrastructure discipline must itself be disciplined — strict types, enforced budgets, deterministic builds. Nothing user-facing ships from this doc except the design tokens and fonts everything else is painted with (§22.1, §22.2).

## 2. Deliverables

- Next.js 15 App Router project (TypeScript strict, React 19) at repo root, laid out per master plan §4 (refines `plan.md` §27).
- `src/styles/tokens.css` — all three §22.1 palettes as CSS custom properties + semantic aliases.
- `src/styles/crt.css` — empty shells for effect layers (filled by `plans/02`).
- Three self-hosted fonts wired through `next/font/local` with metric fallbacks.
- Zustand store skeletons: `src/stores/settings.ts` (displayMode, palette, soundEnabled, reducedMotion, highContrast — persisted to `localStorage`), `src/stores/boot.ts`, `src/stores/windows.ts`, `src/stores/telemetry.ts` (shapes defined in docs 03/04/05; created empty-but-typed here).
- Tooling: pnpm, ESLint (typescript-eslint strict + jsx-a11y), Prettier, Vitest + Testing Library, Playwright (smoke), GitHub Actions CI, Lighthouse CI config (budgets from master plan §2.1).
- `src/lib/capabilities.ts` — WebGL/memory/reduced-motion detection used by §21.7 and every 3D fallback decision.

## 3. Technical design

### 3.1 Scaffold commands (deterministic)

```bash
pnpm create next-app@latest . --ts --app --tailwind --eslint --src-dir --import-alias "@/*"
pnpm add zustand framer-motion
pnpm add -D vitest @testing-library/react @testing-library/jest-dom jsdom \
  @playwright/test prettier eslint-plugin-jsx-a11y @lhci/cli velite
# three/gsap/drei are added in the docs that use them — NEVER here (bundle rule, master plan §2.1)
```

`tsconfig.json`: `"strict": true`, `"noUncheckedIndexedAccess": true`, `"exactOptionalPropertyTypes": true`.

### 3.2 Design tokens (§22.1)

`tokens.css` defines raw palettes and a semantic layer; components use **only** semantic names so palette switching is a one-attribute change (`<html data-palette="green|amber|paper">`, set by `settingsStore`):

```css
:root, [data-palette="green"] {
  --bg: #050805; --panel: #0A0F0A;
  --text: #A8FF9E; --text-bright: #D7FFD1; --text-dim: #5F8A59;
  --border: #2B4A2A; --warning: #F1D56A; --error: #FF8B75;
}
[data-palette="amber"] {
  --bg: #0B0803; --text: #FFB74D; --text-bright: #FFE0A3; --text-dim: #9B6A2E;
  /* panel/border derived: panel = bg lightened 4%, border = text-dim at 40% */
}
[data-palette="paper"] { /* Print-Manual + high-contrast base */
  --bg: #E7E1CF; --text: #1A1A18; --border: #636056; --accent: #2E4A31;
}
```

Rules (from §22.1): green is the primary mode; amber appears only in diagnostics/warnings contexts; paper is the Print-Manual reading mode (`plans/13`) and print stylesheet. High-contrast mode (§25) is a token override layer (`[data-contrast="high"]`) that raises text/bg contrast to ≥ 7:1, not a fourth palette.

Tailwind v4 maps semantic variables to utilities via `@theme` so `text-[--text-bright]`-style escapes are unnecessary: `--color-phosphor: var(--text);` etc.

### 3.3 Typography (§22.2)

| Slot | Font (self-hosted, open-license) | Usage | Guard |
|---|---|---|---|
| Display | **Silkscreen** (pixel) | icon labels, window titles, big headings | never for body text > 2 lines (§22.2 rule) |
| Terminal | **IBM Plex Mono** | commands, metrics, logs, metadata, boot text | |
| Reading | **IBM Plex Sans** | long-form blog/case-study body | min 16px body, 1.6 line-height (§25) |

All via `next/font/local` with `adjustFontFallback` to protect CLS. A `prose-crt` and `prose-clean` typography pair (Tailwind plugin config) is defined here and consumed by `plans/13`.

### 3.4 CI pipeline (GitHub Actions)

```text
on: push/PR → jobs:
  check:  pnpm lint && pnpm typecheck && pnpm test (vitest)
  build:  pnpm build (includes velite) → upload .next stats; fail if shared First Load JS > 130 kB
  e2e:    playwright smoke (boot → desktop, once plans/03–05 exist; starts as "/" renders)
  lhci:   Lighthouse CI against `next start`, asserts master plan §2.1 budgets (warn Phase 0–2, error Phase 3+)
```

## 4. Creative direction

Foundation-level creative constraints that later docs inherit:

- The **block cursor** (§1) is a token: `--cursor: 0.6em` steps-blink animation defined in `tokens.css`, reused by terminal, boot, and forms.
- Focus states (§25) are themed: 2px `--text-bright` outline + 1px offset — visible on phosphor backgrounds, never `outline: none`.
- Dot-matrix flavor in chrome text via `letter-spacing: 0.08em; text-transform: uppercase` utility class `.dotlabel` (§1 "printed by a dot-matrix printer").
- Selection color: inverted phosphor (`::selection { background: var(--text); color: var(--bg); }`).

## 5. Dependencies

None (first doc executed). Consumed by all others. `velite` is installed here but configured in [06-content-architecture](./06-content-architecture.md).

## 6. Acceptance criteria

- [ ] `pnpm build` and all CI jobs green on a clean clone.
- [ ] `document.documentElement.dataset.palette = 'amber'` restyles the whole page with no re-render flash; `paper` likewise.
- [ ] All three fonts load self-hosted (zero external font requests); CLS from font swap = 0 in Lighthouse.
- [ ] `settingsStore` persists displayMode/sound/palette across reload; respects `prefers-reduced-motion` and `prefers-contrast` as initial values.
- [ ] Shared First Load JS ≤ 130 kB and contains no three.js/GSAP (verified by `next build` output + a CI grep of chunk manifests).
- [ ] ESLint jsx-a11y ruleset active and passing.
- [ ] `capabilities.ts` correctly reports WebGL availability, device memory (when exposed), and reduced-motion in unit tests with mocked environments.

## 7. Risks & fallbacks

| Risk | Fallback |
|---|---|
| Tailwind v4 + Next 15 integration issue at build time | Pin to latest Tailwind v3 with identical token layer — tokens live in plain CSS so nothing else changes |
| Pixel font renders poorly at small sizes on low-DPI screens | Use it ≥ 14px only; fall back to Terminal font below that (a CSS clamp handles it) |
| React 19 breaking a dependency | All deps chosen (zustand, framer-motion, velite) are React-19-compatible as of 2026; if one breaks, it is isolated behind our own wrapper modules and swappable |
| 130 kB shared-JS ceiling too tight once Framer Motion lands | Motion imports use `LazyMotion`/`domAnimation` feature slice (~30 kB); if still over, raise ceiling to 150 kB in this doc *and* master plan together |
