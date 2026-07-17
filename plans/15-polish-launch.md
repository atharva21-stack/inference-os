# 15 — Polish & Launch: Contact, Easter Eggs, A11y/Perf/SEO, Launch Checklist

Phase 4 · Depends on: everything (final phase) · Covers §19, §21 (except §21.3 → doc 05), §25, §26, §29

---

## 1. Mission

Phase 5 of the narrative — *connection* (§4) — plus everything that makes the site shippable at the standard it preaches: a portfolio claiming infrastructure discipline must launch with audited accessibility, a met performance budget, and correct SEO (§26: "The portfolio itself should prove infrastructure discipline"). Easter eggs land last and only where they pass the §38 test.

## 2. Deliverables

- `/contact` (CONTACT.COM) + `src/app/api/contact/route.ts` (Resend).
- `src/components/easter/` — `CareerDefrag`, `Overclock`, `DotMatrixPrinter`, `FloppyDrive`, `VisitorBenchmark`; `src/app/not-found.tsx` (kernel panic).
- SEO kit: metadata factory, OG image generation (`next/og`), JSON-LD, `sitemap.ts`, `robots.ts`.
- Audit artifacts: a11y audit checklist run (§25), performance hardening pass (§26), launch checklist (§6 below).

## 3. Technical design

### 3.1 CONTACT.COM (§19)

Dial-up session flow:

1. Window opens → connection sequence (§19.1): `DIALING REMOTE ENGINEER... / HANDSHAKE ACCEPTED. / ENCRYPTION ENABLED. / CHANNEL OPEN.` (~1.5s, skippable, reduced-motion: instant; optional modem chirp — quiet, 1s, sound-gated).
2. Contact options row (§19.2): `[ EMAIL ] [ LINKEDIN ] [ GITHUB ] [ RESUME ]` — real links (email = `mailto:`, resume → doc 07).
3. Form (§19.3): `FROM / ORGANIZATION / SUBJECT / MESSAGE / [ TRANSMIT ]` → POST `/api/contact` → Resend to Atharva's inbox. Success: `PACKET SENT SUCCESSFULLY. / EXPECTED RESPONSE: HUMAN, NOT AUTOMATED.` Failure: `CARRIER LOST — RETRY OR USE [ EMAIL ]` with the mailto fallback surfaced.
4. Protection: honeypot field + minimum-fill-time check server-side (no CAPTCHA — hostile to the aesthetic and to a11y); rate limit per IP in the route handler. Labels + errors fully accessible (`aria-describedby`, error summary focus).

Fallback if Resend/env is absent at deploy: form swaps to a composed `mailto:` flow automatically (feature-detected at build via env var) — §19 ships either way.

### 3.2 Easter eggs (§21) — each with its §38 justification

| Egg | Trigger | Behavior | §38 justification |
|---|---|---|---|
| **Career Defragmenter** (§21.1) | SYSTEM TOOLS menu; `defrag` in terminal | Win-9x defrag grid animates blocks into a **real career timeline**: contiguous colored runs = roles, block clusters = projects/posts, legend links everything | It *is* a data visualization of the career — the joke renders real information |
| **Overclock** (§21.2) | button in PERFMON | 30s: animations speed up, phosphor brightens, telemetry band rises, then `THERMAL WARNING — REVERTING TO SAFE CLOCKS` auto-reverts; disabled under reduced motion | Demonstrates the telemetry system reacting end-to-end; teaches DVFS-flavored lore |
| **Dot-matrix printer** (§21.4) | `[ PRINT PROFILE ]` in ABOUT + Start menu | Printer SVG feeds paper line-by-line (tractor-feed edges), rendering a one-page profile; `SAVE PDF` downloads a real generated one-pager (react-pdf or prebuilt) | Produces the most shareable recruiter artifact on the site |
| **Floppy / SECRET_PROJECTS** (§21.5) | bezel floppy slot (doc 02) + hidden desktop icon | Insert animation → `SECRET_PROJECTS` window: experiments, abandoned prototypes, architecture sketches, reading notes (small Velite collection; honest work-in-progress framing) | Depth signal: even discards show thinking |
| **Kernel panic 404** (§21.6) | `not-found.tsx` | §21.6 verbatim: `KERNEL PANIC / REQUESTED RESOURCE NOT FOUND` + `[ RETURN TO DESKTOP ] [ OPEN SYSTEM MAP ]` (system map = sitemap page listing all routes — the 404 becomes navigation) | Recovers lost visitors with full route map |
| **Visitor benchmark** (§21.7) | SYSTEM TOOLS; `benchmark` in terminal; auto-run silently on first load to gate capabilities | Prints the §21.7 report (`WEBGL / MEMORY / MOTION SETTINGS / DISPLAY MODE`) from `capabilities.ts` — real browser APIs only, no fingerprinting (§21.7); doubles as the user-visible face of the capability gate | Transparency: shows visitors exactly what the site adapts to |

### 3.3 Accessibility audit (§25) — executed as a gate, not a hope

Process: automated (axe-core in Playwright across all routes × CRT/CLEAN × mobile/desktop) + manual passes: keyboard-only full-site walkthrough, NVDA + VoiceOver script (boot → desktop → each window → one tool → contact submit), 200% zoom, forced-colors mode. The §25 checklist becomes the audit spreadsheet — every item gets pass/fail + issue link; launch requires all pass. Key already-designed guarantees to verify end-to-end: skip boot; reduced-motion coverage; clean mode; high-contrast tokens; 3D text equivalents; sound optional.

### 3.4 Performance hardening (§26)

- Lighthouse CI budgets flip from warn→error (doc 01 pipeline) on: `/desktop`, a post, `/experience/deloitte`, `/gpu-lab`, `/projects/gpu-capacity-planner`.
- Bundle audit: verify three/gsap/tool chunks are route-scoped (`next build` manifest diff against Phase 0 baseline); shared JS ≤ budget.
- Asset audit: GLB/KTX2/sprite budgets (doc 10), fonts subset, sounds ≤ 40 kB total.
- Runtime audit: background-tab pause (telemetry, R3F demand loop, wallpaper), long-task scan on boot and window-open paths (INP < 200ms).
- Throttled-device pass: Moto-G-class phone, 4× CPU throttle — boot skippable, desktop interactive < 3s.

### 3.5 SEO & sharing (§29)

- Metadata factory: every route exports title/description per §29's patterns (`Atharva Chandwadkar — AI Infrastructure and GPU Inference Engineer` root); canonical URLs; full sitemap/robots.
- **OG images**: `next/og` template rendering each page's title as a phosphor CRT frame screenshot-style card — the aesthetic *is* the share card; per-post OG for blogs (title + doc_id + category).
- JSON-LD: `Person` (root, with sameAs links), `Article` per post, `BreadcrumbList` on nested routes.
- Crawlability regression suite: Playwright-with-JS-off asserts §37 copy at `/`, article text at posts, case-study text at experience routes (locks in docs 03/08/13 guarantees).
- Plausible wired with custom events: boot completed/skipped, window opens, tool runs, resume downloads, contact submits — the launch success metrics.

## 4. Creative direction

- Contact success beat is the emotional payoff of the whole fiction — after TRANSMIT: brief carrier tone (sound-gated), then §19.3's `EXPECTED RESPONSE: HUMAN, NOT AUTOMATED.` sits alone on screen for a beat. No confetti. (§3: calm.)
- Kernel panic page is *tasteful* (§21.6): monospace dump, one dry line (`GURU MEDITATION #404.1994`), immediate recovery paths — an error page that makes people screenshot it.
- Easter eggs are discoverable but never interruptive: no toasts advertising them; the terminal's `help` FUN group and SYSTEM TOOLS menu are the only signposts.
- OG cards are the one place CRT effects render as *raster* — bake scanlines into the template so shares look like photographs of the machine.

## 5. Dependencies

All prior docs (this phase audits and decorates the finished system). Hard requirements: doc 02 (floppy slot surface, effect toggles), doc 05 (telemetry for overclock; hidden icon), doc 07 (profile data for printer), doc 11 (PERFMON hosts overclock), doc 12 (terminal verbs `defrag/overclock/benchmark`), doc 01 (capabilities, Lighthouse CI, env plumbing for Resend).

## 6. Acceptance criteria (doubles as the launch checklist)

- [ ] Contact: happy path delivers email (staging-verified); failure path surfaces mailto; honeypot+rate-limit block scripted spam; form fully accessible.
- [ ] All six easter eggs work per §3.2 table; overclock auto-reverts; 404 recovery paths work; system map lists every route; visitor benchmark shows only real capabilities.
- [ ] A11y audit spreadsheet: every §25 item pass; axe zero critical/serious across the route × mode matrix; manual SR script completes.
- [ ] Lighthouse ≥ 95 perf/a11y/SEO on all five gate routes; §26 budget met on throttled mobile; budgets enforced as CI errors.
- [ ] Every route: correct title/description/canonical/OG image (visual spot-check of 5 share cards in Slack/X preview debuggers); JSON-LD validates; sitemap complete.
- [ ] JS-off crawlability suite green.
- [ ] Plausible events firing (verified in dashboard); no third-party requests before consent-gated video plays.
- [ ] DNS/domain live on Vercel prod; preview→prod promotion checklist run; `plan.md` §34 Phase 1–3 features all demoable end-to-end.

## 7. Risks & fallbacks

| Risk | Fallback |
|---|---|
| Resend account/env not ready at launch | Designed-in: build-time swap to mailto flow; form UI identical |
| Easter-egg scope eats the audit time | Audits are the gate; eggs are strictly post-audit ordering: 404 + benchmark (already needed) → printer → defrag → overclock → floppy. Any egg can slip to post-launch without blocking |
| OG generation cold-start latency on Vercel | Static-generate OG images at build for all known routes; dynamic only for future posts |
| Late a11y findings force design changes | Mitigated by per-doc a11y acceptance criteria upstream — the audit should confirm, not discover; genuine discoveries get CLEAN-mode-first fixes (the escape hatch every effect already has) |
| Perf regression discovered at gate | Feature-flag ladder documented per doc (effects → sprite → static) allows shipping with subsystems degraded rather than delaying launch |
