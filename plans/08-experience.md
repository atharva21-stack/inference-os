# 08 — EXPERIENCE.DIR: Directory Tree + Server Rack

Phase 1 · Depends on: [04-window-manager](./04-window-manager.md), [06-content-architecture](./06-content-architecture.md) · Covers §10

---

## 1. Mission

Experience is the site's proof-of-work (§33 priority 2: real systems and outcomes). The metaphor (§10.1): careers as **physical rack blades** — select a directory entry, a blade lights and slides out, the case study loads. Six roles, one rigorous template (§10.2), so a hiring manager can diff roles at a glance and an engineer can go deep on any one.

## 2. Deliverables

- Routes: `/experience` (window with tree + rack) and `/experience/[id]` for `deloitte, bank-of-america, 10x-analyst, humana, stryke, hex-lab` (§5.1).
- `src/components/experience/` — `DirectoryTree`, `ServerRack`, `RackBlade`, `CaseStudy`, plus per-role visual modules (`TraceWaterfall`, `WorkflowAnimation`, `LogicAnalyzer`).
- Content: six `content/experience/*.mdx` docs on the §28.1 schema (seeded by doc 06 with Deloitte).

## 3. Technical design

### 3.1 Split layout (§10.1)

Desktop window: left pane directory tree, right pane rack visualization; mobile: tree only, rack collapses to a thumbnail strip.

```text
C:\CAREER\EXPERIENCE          ┌────────────────────┐
├── DELOITTE_AI_FACTORY   ◀── │ ▓▓▓▓▓▓▓▓▓▓  blade 1│
├── BANK_OF_AMERICA           │ ░░░░░░░░░░  blade 2│
├── 10X_ANALYST               │ ░░░░░░░░░░  blade 3│
├── HUMANA                    │ ...        6 blades│
├── STRYKE_GPU                └────────────────────┘
└── HEX_LAB
```

- Tree entries are links to `/experience/[id]` (crawlable). Selection syncs: tree ↔ blade ↔ URL.
- **Rack rendering:** dithered **SVG rack**, not WebGL (§10.1 allows "low-poly *or dithered*"; keeps three.js off this route per master plan §2.1 — the 3D showcase belongs to GPU Lab). Blade select animation (§10.1): LED strip lights → blade translates out 12px → metadata strip (`SYSTEM / ROLE / STATE`) prints above the rack → route change loads case study below/beside. Reduced motion: instant highlight.
- Case-study content server-renders from Velite; the rack/tree is the client-side chrome around it.

### 3.2 Case-study template (§10.2) — one component, six datasets

`CaseStudy` renders §28.1 fields in the §10.2 order: `SYSTEM NAME · ROLE · DATE RANGE · LOCATION · MISSION · SCALE · ARCHITECTURE · RESPONSIBILITIES · PERFORMANCE WORK · FAILURES ENCOUNTERED · DECISIONS MADE · OUTCOMES · TECHNOLOGY STACK` — each as a labeled terminal-style section; empty optional sections are omitted, never rendered blank. `FAILURES ENCOUNTERED` cross-links to INCIDENTS.LOG entries via the schema's `incidents[]` refs (doc 14). Technology stack chips link into MEMORY.MAP skills (doc 07).

### 3.3 The six case studies (content spec — what each MDX must contain)

| id | Header (§10.3–10.6 where given) | Distinct visual module | Key content |
|---|---|---|---|
| `deloitte` | `SYSTEM: DELOITTE_AI_FACTORY · ROLE: AI INFRASTRUCTURE ENGINEER / CONSULTANT · STATE: PRODUCTION` | Blade with **six blinking submodules** (§10.3): MODEL SERVING, GPU CAPACITY, KUBERNETES, OBSERVABILITY, AGENTIC AI, PLATFORM RELIABILITY — each submodule expands a paragraph | §10.3 narrative: production agentic + inference systems; $10M+ hybrid platform; regulated enterprise engagements; demand→platform-requirements translation. `confidentiality_note` required |
| `bank-of-america` | Inference platform scaling | **TraceWaterfall** — request trace styled as an old oscilloscope (§10.4), before/after overlay | §10.4 PROBLEM→INVESTIGATION (k8s resource profiling, CPU/mem profiling, network hop analysis, API tracing, serving-path inspection, p50/p95 review) → INTERVENTIONS → OUTCOME structure verbatim. `confidentiality_note` required |
| `10x-analyst` | Multi-agent consulting automation | **WorkflowAnimation** — dot-matrix morph `MANUAL ANALYST WORK` → `AGENT 01 → AGENT 02 → REVIEW → OUTPUT` (§10.5) | §10.5 numbers: ADOPTION 6 engagements; BEFORE ~1 week → AFTER <1 hour; multi-agent + retrieval + model routing; AWS → on-prem NVIDIA migration |
| `humana` | (brief gives no detail) | Reuses TraceWaterfall or plain template | Author from resume source material on the §10.2 template; mark reconstructed metrics per §14.3 |
| `stryke` | (brief gives no detail) | Plain template + rack blade | Same — author from real material; GPU angle per company name |
| `hex-lab` | GPU performance research (§10.6) | **LogicAnalyzer** — retro logic-analyzer chart with animated cache-hit lines (§10.6) | §10.6 metrics verbatim: distributed training speedup 3×; L2 hit rate 58%→81%; TensorRT INT8 +40%; power −12% |

Visual modules are small self-contained SVG/CSS components (`src/components/experience/visuals/`), lazy-mounted when scrolled into view, static-rendered under reduced motion, and each carries a text table equivalent (§25: no info only-in-graphics).

## 4. Creative direction

- Blade faceplates carry era-correct engraving: role dates as serial numbers (`S/N 2023-08 → PRESENT`), a barcode, and a `PROPERTY OF ATHARVA SYSTEMS LAB` stamp.
- Section labels print with the dot-leader treatment; OUTCOMES sections end with a `RESULT VERIFIED ✓` stamp only where metrics are real (unlabeled-confidential) — the stamp is *withheld* on normalized data, quietly reinforcing honesty (§14.3, §3 credible).
- The tree uses genuine DOS path affordances: typing a path in doc 12's terminal (`cd experience/deloitte`) opens the same route — one metaphor, two navigations.
- Humor budget: exactly one line per case study (e.g. HEX_LAB: `CACHE HIT RATE IMPROVED. MORALE FOLLOWED.`).

## 5. Dependencies

Doc 04 (window/routes), doc 06 (schema, seed, incident refs → doc 14 ids must exist before `FAILURES ENCOUNTERED` links go live; until then plain text), doc 07 (skill chips link target), doc 12 (terminal path parity — registered there).

## 6. Acceptance criteria

- [ ] `/experience` shows tree + rack; selecting any of 6 entries by mouse/keyboard lights the blade and navigates; URL back/forward walks selections.
- [ ] All six `/experience/[id]` pages render every populated §10.2 section in order; `curl` returns full text (SSR).
- [ ] Deloitte/BoA/10x/HEX content matches §10.3–10.6 specifics above (spot-check strings: "$10M+", "6 CLIENT ENGAGEMENTS", "58% → 81%").
- [ ] Each visual module has a text-equivalent table; animations respect reduced motion; modules lazy-load (no cost on `/experience` index).
- [ ] Regulated-client docs carry `confidentiality_note`; normalized metrics show the §14.3 label (doc 06 test extends to these files).
- [ ] Tech-stack chips deep-link to `/skills` with the right cell focused.
- [ ] Mobile: tree-first layout usable at 390px; rack thumbnail strip optional but non-blocking.

## 7. Risks & fallbacks

| Risk | Fallback |
|---|---|
| Humana/Stryke lack public-safe detail | Template renders gracefully with fewer sections; both can ship as "compact" case studies (MISSION/RESPONSIBILITIES/OUTCOMES/STACK only) — better short and true than padded |
| SVG rack looks flat next to GPU Lab's real 3D | Acceptable by design (rack = navigation chrome, lab = showcase); if Gate G2 review disagrees, reuse doc 10's rack GLB in a shared lazy canvas — asset already planned (§23.1 item 3) |
| Six bespoke visual modules blow the Phase 1 budget | Priority order: TraceWaterfall, WorkflowAnimation, LogicAnalyzer (brief-specified) ship Phase 1; submodule-expander for Deloitte can ship as static list; all others plain template |
| Blade animation jank on tablets | Animation is transform/opacity only; below 60fps threshold in testing, tablet gets the reduced (highlight-only) variant |
