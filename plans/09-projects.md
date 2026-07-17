# 09 — PROJECTS.SYS: Executable Files

Phase 1 · Depends on: [04-window-manager](./04-window-manager.md), [06-content-architecture](./06-content-architecture.md) · Covers §11.1–§11.2 (the interactive tools *themselves* are [doc 11](./11-interactive-tools.md))

---

## 1. Mission

Projects are presented as executable system files (§11.1) — things the machine can *run*, not screenshots in a grid. This doc builds the PROJECTS.SYS directory window, the project card format (§11.2), and the five project detail pages. The separation from doc 11 is deliberate: **this doc ships the readable case-study pages (Phase 1); doc 11 ships the live simulations they launch (Phase 3)** — so the section is valuable long before the tools exist.

## 2. Deliverables

- Routes: `/projects` and `/projects/[id]` for `gpu-capacity-planner, inference-profiler, multi-agent-platform, model-serving-benchmark-lab, cluster-topology-simulator` (§5.1).
- `src/components/projects/` — `ProjectDirectory`, `ProjectCard`, `ProjectDetail`, `LaunchButton`.
- Content: five `content/projects/*.mdx` on the §28.2 schema.

## 3. Technical design

### 3.1 Directory window (§11.1)

```text
C:\SYSTEMS\PROJECTS
├── GPU_CAPACITY_PLANNER.EXE      v2.1  PRODUCTION
├── INFERENCE_PROFILER.EXE        v1.4  PRODUCTION
├── MULTI_AGENT_PLATFORM.EXE      v3.0  PRODUCTION
├── MODEL_SERVING_BENCHMARK.EXE   v1.2  BETA
└── TOPOLOGY_SIMULATOR.EXE        v0.9  BETA
```

A file-listing window: name, version, status from frontmatter; row selection opens a **file-inspector footer** (summary + actions) — the §11.1 verbs map to actions:

| §11.1 verb | Action |
|---|---|
| inspected | open `/projects/[id]` detail page |
| launched | `RUN` → the doc-11 tool (route from frontmatter `demo`); disabled with tooltip `MODULE NOT YET LINKED` until Phase 3 |
| benchmarked | jump-link to the detail page's METRICS section |
| opened in GitHub | `repository` frontmatter link |
| viewed as architecture diagram | jump-link to the ARCHITECTURE diagram on the detail page |

Rows are real links (crawlable); the window supports arrow-key row navigation like a DOS file manager.

### 3.2 Detail page = §11.2 card format

`ProjectDetail` renders the §11.2 fields in order: `FILE NAME · VERSION · STATUS · PURPOSE · INPUTS · OUTPUTS · ARCHITECTURE · METRICS · FAILURE MODES · TECH STACK · SOURCE · DEMO`. ARCHITECTURE is an MDX slot expecting a diagram component (ASCII fallback + SVG, the doc 13 `Diagram` component reused); METRICS uses the dot-matrix benchmark table (doc 13 component); FAILURE MODES renders as WARNING-styled callouts — projects show their sharp edges, which is the credibility play (§33 priority 5).

### 3.3 The five projects (content spec)

| id | Focus of the writeup | DEMO target (Phase 3) |
|---|---|---|
| `gpu-capacity-planner` | Sizing methodology: §11.3 inputs/outputs tables, the math's assumptions (weights + KV cache + activation memory, headroom, failure reserve), why naive GPU-count division fails | doc 11 planner |
| `inference-profiler` | Anatomy of request time (§11.4 metric list), TTFT vs ITL, why prefill/decode split matters | doc 11 profiler |
| `multi-agent-platform` | The 10x-analyst platform generalized (§10.5 cross-link): agent orchestration, retrieval, model routing, AWS→on-prem migration; failure modes of agent systems | none — links to `/experience/10x-analyst` and blog 06/07 instead of a tool |
| `model-serving-benchmark-lab` | Engine comparison methodology (§11.5): what TTFT/TPOT/P50/P95/tokens-per-sec/HBM/queue-depth each reveal; synthetic-data labeling policy | doc 11 benchmark lab |
| `cluster-topology-simulator` | Topology-aware vs naive scheduling (§11.6), NVLink/IB path costs, fragmentation | doc 11 simulator |

Every metrics block that is illustrative carries the §14.3 confidentiality label (enforced by doc 06's test).

## 4. Creative direction

- File rows behave like a real file manager: `.EXE` files get the benchmark icon, statuses colorized (PRODUCTION green, BETA amber — plus a text label, never color-only per §25).
- `RUN` on a launchable project plays a 600ms "loading executable" strip (`LOADING GPU_CAPACITY_PLANNER.EXE ████████░░`) before navigating — the §1 pixel-art loading bar, reduced-motion: instant.
- Detail pages open with a file-header block styled as an EXE properties dialog (`SIZE: 48,128 BYTES · CREATED: 2025 · CRC OK`).
- The disabled RUN tooltip (`MODULE NOT YET LINKED — SCHEDULED PHASE 3`) turns the phased rollout into lore instead of a broken link.

## 5. Dependencies

Doc 04 (window/routes), doc 06 (schema, `demo`/`repository` frontmatter, seed project), doc 13 (Diagram + benchmark-table components — if 13 lands later, detail pages use plain `<pre>` ASCII until then), doc 11 (RUN targets, Phase 3).

## 6. Acceptance criteria

- [ ] `/projects` lists all five with version/status from frontmatter; keyboard row navigation works; every row deep-links.
- [ ] Each detail page renders all populated §11.2 sections in order; `curl` returns full text.
- [ ] All five §3.3 writeups exist with real architecture prose and labeled metrics; `multi-agent-platform` cross-links to the 10x-analyst case study.
- [ ] RUN buttons: disabled-with-tooltip pre-Phase-3; after doc 11 lands, each navigates to its tool (frontmatter-driven, no hardcoded routes).
- [ ] GitHub links open in new tab with `rel=noopener`; missing `repository` hides SOURCE row rather than rendering a dead link.
- [ ] FAILURE MODES render as warning callouts and are present for all five projects (a project with no failure modes listed fails review, not the build).

## 7. Risks & fallbacks

| Risk | Fallback |
|---|---|
| Detail pages feel thin before tools exist | The §3.3 writeups are specified as methodology essays, not tool manuals — they stand alone; RUN is explicitly framed as scheduled |
| Some projects have no public repo | SOURCE row hidden; DEMO/writeup carry the weight — never link private repos |
| Version/status theater reads as fake | Versions map to real content revisions (bump on meaningful edits, noted in frontmatter `updated`); statuses reflect actual tool maturity per phase |
| Component dependency on doc 13 inverts phase order | Both fallbacks specified (plain `<pre>`); alternatively extract `Diagram`/`BenchmarkTable` into `src/components/charts/` in Phase 1 and let doc 13 consume them — decide at Gate G1 |
