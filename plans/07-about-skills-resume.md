# 07 — Identity Windows: ABOUT.EXE · MEMORY.MAP · RESUME.PDF

Phase 1 · Depends on: [04-window-manager](./04-window-manager.md), [06-content-architecture](./06-content-architecture.md) · Covers §9, §13, §18

---

## 1. Mission

These three windows answer the recruiter's first three questions — *who is he* (§9), *what does he actually know* (§13), *can I have the document* (§18) — inside the machine metaphor. ABOUT.EXE must communicate who/what/scale/differentiation within 10 seconds (§9.1); MEMORY.MAP proves depth without gimmicks ("Do not use fake percentage bars", §13.7); RESUME.PDF is the friction-free exit for people who just need a file.

## 2. Deliverables

- Routes/windows: `/about` (ABOUT.EXE), `/skills` (MEMORY.MAP — route amendment, master plan §3), `/resume` (RESUME.PDF).
- `src/components/about/` — `AboutWindow`, `DitheredPortrait`, `MetricCounters`, `PositioningPipeline`.
- `src/components/skills/` — `MemoryMap`, `SkillDetail`.
- `src/components/resume/` — `ResumeViewer`.
- Content: skills dataset `content/skills.yaml` (single-file Velite collection added to doc 06's config); portrait assets; three resume PDFs in `public/resume/`.

## 3. Technical design

### 3.1 ABOUT.EXE (§9)

Two-pane window (§9.2); panes stack on mobile.

- **Left pane — portrait:** one source photo processed at build time (sharp script) into a 1-bit **halftone/dither** treatment (§9.2 options; halftone chosen as most readable of the six). Rendered `<img>` with real alt text; a hover/focus easter interaction cycles to the ASCII variant (pre-generated text, not runtime).
- **Right pane — ID record:** the §9.2 block verbatim (NAME/ROLE/LOCATION/STATUS + SPECIALIZATION list), dot-leader formatted, sourced from a small `content/about.mdx`.
- **Hero copy (§9.3):** "I build and scale the systems between an AI model and the real world." + supporting paragraph, in Reading font (this is body text — §22.2 rule).
- **Metric counters (§9.4):** `PLATFORM SCALE $10M+ · CLIENT ENGAGEMENTS 6+ · TURNAROUND ~1 WEEK → <1 HOUR · GPU STACK … · FOCUS …` as vintage odometer counters — count-up animates on first view (skipped under reduced motion; values always present in DOM for crawlers).
- **Positioning pipeline (§9.5):** the 7-stage BUSINESS DEMAND→BUSINESS OUTCOME vertical pipeline as inline SVG with the §9.5 caption; each stage subtly pulses in sequence (CSS). Stages are links: GPU CAPACITY→`/projects/gpu-capacity-planner`, SCHEDULING→`/blogs/post/topology-aware-scheduling`, OBSERVABILITY→`/incidents` — the diagram doubles as a depth-map of the site.

### 3.2 MEMORY.MAP (§13)

Skills organized as the memory hierarchy — the site's cleverest honest metaphor: proximity to the die = depth of hands-on skill.

```text
REGISTER FILE   (deepest)   PYTHON · CUDA C++ · TRITON · KUBERNETES · vLLM · TENSORRT-LLM · NVIDIA TRITON · FASTAPI · LINUX
SHARED MEMORY               LANGGRAPH · NEMO AGENT TOOLKIT · LANGCHAIN · PROMETHEUS · OPENTELEMETRY · SPLUNK · REDIS · POSTGRESQL · PGVECTOR
HBM                         DISTRIBUTED INFERENCE · GPU CAPACITY PLANNING · MODEL PARALLELISM · AUTOSCALING · CONCURRENCY MODELING · QUANTIZATION · KV CACHE MGMT · LATENCY ENGINEERING
HOST MEMORY                 AWS · AZURE · GCP · TERRAFORM · GITHUB ACTIONS · DOCKER · SECURITY · ENTERPRISE ARCH · CLIENT DELIVERY
NETWORK FABRIC              NVLINK · NVSWITCH · INFINIBAND · RDMA · PCIE · LOAD BALANCING · REQUEST ROUTING · SERVICE MESH
```

- Layout: horizontal memory-bank bands (widest/brightest at top = REGISTER FILE) like a die floor plan; each skill is a chip cell. (`CONTROL PLANE` from §13.1's list is the band header row itself — Kubernetes/Slurm-level orchestration skills live in REGISTER FILE/HBM per §13.2/13.4.)
- **Hover/focus detail (§13.7):** a side panel (not tooltip — touch/keyboard friendly) shows `USED AT / PROJECTS / DEPTH / RELATED METRICS / ARCHITECTURAL ROLE`, data from `skills.yaml`:

```yaml
- id: vllm
  band: register-file
  used_at: [deloitte, bank-of-america]     # experience refs, build-validated
  projects: [model-serving-benchmark-lab]  # project refs
  depth: "Production serving, batching & KV tuning"
  related_metrics: ["p95 −60% on serving path"]
  architectural_role: "Serving engine layer"
```

- `USED AT`/`PROJECTS` entries are links into docs 08/09 content. **No percentage bars, no star ratings** (§13.7) — depth is expressed by band position + prose + linked evidence.
- Fully keyboard navigable: bands are lists, cells are buttons, detail panel is `aria-live`-announced.

### 3.3 RESUME.PDF (§18)

Retro document viewer window:

- Toolbar `[ VIEW ] [ DOWNLOAD ] [ PRINT ] [ COPY TEXT ]` (§18.1). VIEW shows an HTML rendition (server-rendered from `content/resume/*.mdx` — not an embedded PDF, so it's fast, searchable, and styleable); DOWNLOAD serves the matching PDF; PRINT applies doc 01's `print.css` (paper palette); COPY TEXT copies a plaintext rendition with a `COPIED TO CLIPBOARD BUFFER` toast.
- **Three views (§18.2):** tab strip `EXECUTIVE VIEW / ENGINEERING VIEW / GPU-INFERENCE VIEW` — three MDX variants + three tailored PDFs. Until tailored PDFs exist, all three tabs render distinct HTML emphasis but DOWNLOAD serves the single master PDF (labeled as such).

## 4. Creative direction

- ABOUT's ID-record pane styles as a laminated machine-room badge: border, corner punch-hole, `CLEARANCE: PUBLIC` stamp — mysterious-but-credible (§3).
- MEMORY.MAP band headers carry real-world capacity captions (`REGISTER FILE — SMALL, FAST, EXPENSIVE. LIKE EXPERT ATTENTION.`) — one line of wit each, no more (§3 "slightly playful").
- Counters tick with a faint relay click when sound is on (single shared sample, ≤ 5 plays).
- RESUME viewer chrome mimics a 90s shareware PDF reader: page ruler, `1 OF 1` pager, zoom buttons that actually adjust the HTML text size (a11y win disguised as skeuomorphism).

## 5. Dependencies

Doc 04 (window shell/routing), doc 06 (experience/project refs that `skills.yaml` and metrics link into — build-time validation), doc 01 (fonts/tokens, print.css). ABOUT's pipeline links require docs 08/09/13/14 routes to exist by Gate G1.

## 6. Acceptance criteria

- [ ] ABOUT.EXE: 10-second test — window at default size shows portrait, ID record, hero line, and ≥ 3 metrics without scrolling at 1280×800.
- [ ] Counters animate once, never on reduced motion; values in initial HTML (`curl /about` contains `$10M+`).
- [ ] Pipeline stage links navigate to the mapped routes; SVG has an accessible text alternative listing all stages.
- [ ] MEMORY.MAP: every §13.2–13.6 skill present in its band; hover, focus, and tap all open the detail panel with all five §13.7 fields; zero percentage bars anywhere; every `used_at`/`projects` link resolves (build-time check).
- [ ] RESUME: all four toolbar actions work; three views switch; PRINT preview shows paper palette, no CRT effects, no window chrome; PDF downloads with correct filename `atharva-chandwadkar-resume.pdf`.
- [ ] All three windows pass keyboard-only walkthrough and axe checks; portraits have meaningful alt text.

## 7. Risks & fallbacks

| Risk | Fallback |
|---|---|
| Dithered portrait reads as evasive/low-effort to recruiters | Treatment slider ships behind the CLEAN display mode: clean mode shows the normal photo, CRT mode shows halftone — both from the same source |
| Tailored resume PDFs not ready at launch | Designed-in fallback already specified: three HTML views + one labeled master PDF |
| MEMORY.MAP band layout cramped on mobile | Bands become an accordion (one open at a time); detail panel becomes bottom sheet |
| Skill-link web creates maintenance drag | All refs validated at build time (doc 06 machinery) — a broken link is a failed build, not a stale page |
