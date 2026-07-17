# 13 — FIELD_NOTES: The Blog Platform + 8 Launch Posts

Phase 1 (index + article + clean mode) / Phase 2 (reading modes, search, categories) · Depends on: [04-window-manager](./04-window-manager.md), [06-content-architecture](./06-content-architecture.md) · Covers §16, §32

---

## 1. Mission

FIELD_NOTES (§16.1's recommended name) is the public-thinking engine (§33 priority 7) — a technical archive that *lives on the machine*, not a bolted-on Medium clone. It must satisfy two masters at once: the fiction (dot-matrix index, DOCUMENT IDs, terminal metadata) and serious technical reading (clean typography, crawlable, printable, searchable). The three reading modes (§16.5) are how both win.

## 2. Deliverables

- Routes: `/blogs` (index), `/blogs/category/[cat]` for the five §5.1 categories, `/blogs/post/[slug]`.
- `src/components/blogs/` — `BlogIndex`, `PostLayout`, `ReadingModeBar`, `SideRail`, and the MDX component kit: `EngineeringNote`, `FailureMode`, `DecisionRecord`, `Diagram`, `BenchmarkTable`, `Glossary`, `RelatedPosts`.
- Pagefind integration: post-build indexing script + `SearchBox` (index window + `/` focus shortcut).
- Content: 8 launch posts (`content/blogs/*.mdx`) per the outlines in §3.5.
- `src/styles/print.css` completion (paper palette, doc 01 stub).

## 3. Technical design

### 3.1 Index (§16.2, §32 wireframe)

Implements §32: header `C:\ATHARVA\FIELD_NOTES`, search box, category filter chips (GPU / INFERENCE / K8S / AGENTS / PERFORMANCE), numbered rows:

```text
01  GPU UTILIZATION IS LYING TO YOU
    2026-07-12  8 MIN  [GPU]  LEVEL 3  PUBLISHED
```

Rows show the §16.2 metadata (date, read time, category, difficulty, status) from frontmatter (read_time computed, doc 06). `[OPEN] [SORT] [PRINT INDEX]` footer: sort by date/difficulty/read-time; PRINT INDEX opens the print stylesheet view. Index is a server component; filters/sort are URL params (crawlable category pages per §5.1, no client-only state).

### 3.2 Article page (§16.4)

- **Top metadata block** (§16.4): `DOCUMENT ID: FN-004 · TITLE · AUTHOR: ATHARVA CHANDWADKAR · STATUS: RELEASED · REVISION` — doc_id computed (doc 06), revision from `updated` count.
- **Body:** MDX rendered server-side with the component kit; Reading font, 68ch measure, §22.2 rules (no pixel font in body).
- **Side rail** (§16.4): CONTENTS (generated from headings, scrollspy), DIAGRAMS (jump list of Diagram instances), KEY TERMS (Glossary anchors), RELATED SYSTEMS (frontmatter `related_projects` → doc 09), DOWNLOAD AS TEXT (plaintext rendition endpoint), PRINT MODE. Rail collapses to a details-disclosure on mobile.
- Wide elements (tables, diagrams) break out of the measure to window width with `overflow-x:auto` containment.

### 3.3 Reading modes (§16.5)

`ReadingModeBar`: `[ CRT MODE ] [ CLEAN READING MODE ] [ PRINT MANUAL MODE ]`, persisted per-visitor (settingsStore), URL-overridable (`?mode=print` for sharing).

| Mode | Implementation |
|---|---|
| CRT | article inside the effect stack, phosphor tokens — but body text still Reading font at full contrast (fiction never taxes reading) |
| CLEAN | `data-display="clean"` scope: effects off, near-white-on-dark high-legibility theme; **default on mobile** (§24) and for reduced-motion first visits |
| PRINT MANUAL | paper palette (§22.1 third palette), dot-matrix headings, black diagrams (Diagram components swap to their monochrome variant), doubles as the `@media print` stylesheet |

### 3.4 Article components (§16.6) — exact renderings

- `EngineeringNote` → the §16.6 box-drawn `┌─ ENGINEERING NOTE ─┐` callout (border-drawn with CSS, not literal box characters, so it reflows).
- `FailureMode` → `WARNING:` header in `--warning`, SYMPTOM line, warning icon + role="note".
- `DecisionRecord` → `DECISION:` + REASON bullet list, styled as a stamped memo.
- `Diagram` → **ASCII fallback + interactive SVG** (§16.6): authors provide both; ASCII renders in no-JS/text-download/print contexts, SVG (optionally animated) otherwise; both share one caption/alt.
- `BenchmarkTable` → dot-matrix styled table (§16.6) with sticky header, `<caption>`, and the §14.3 label auto-attached when frontmatter marks data synthetic.
- `Glossary` → definition-list block whose terms register into the side rail.

### 3.5 The 8 launch posts (§16.2 titles ∪ §16.7 briefs) — per-post outlines

1. **GPU Utilization Is Lying to You** `[GPU] L3` — util ≠ useful throughput; SM occupancy vs `nvidia-smi`; memory- vs compute-bound; the INC-001 story as case (cross-link doc 14); how to actually measure (DCGM, profiling). Diagram: two racks at "80%" doing very different work.
2. **Prefill, Decode, and the Real Shape of LLM Latency** `[INFERENCE] L4` — phase asymmetry (compute-bound prefill, bandwidth-bound decode); TTFT vs ITL; chunked prefill; continuous batching interactions. Diagram: request waterfall (shared with doc 11 profiler).
3. **How to Size GPU Capacity for an Enterprise Inference Platform** `[CAPACITY] L4` — the doc-11 planner's math as prose: weights/KV/activations arithmetic, headroom, availability reserve, worked 70B example; ends `RUN THE PLANNER →`.
4. **What Kubernetes Does Not Understand About GPU Topology** `[KUBERNETES] L4` — device plugin's flat view; NVLink domains and fragmentation; topology-aware scheduling approaches; ties to doc 11 simulator. (FN-004 per §16.4's example.)
5. **KV Cache: The Hidden Memory Tax** `[INFERENCE] L3` — per-token cost math; context×concurrency blowup; paged KV, quantized KV, eviction; INC-002 cross-link.
6. **Building Observability for Agentic AI Systems** `[AGENTIC-AI] L3` — tracing across tools/models/retrieval/retries/human review; span taxonomy for agents; what "error" even means; Deloitte-flavored, confidentiality-safe.
7. **What I Learned Scaling a Multi-Agent Platform** `[AGENTIC-AI] L2` — 10x-analyst lessons, normalized (§16.7: no confidential detail): routing beats bigger models, human review as a system component, migration to on-prem.
8. **The Difference Between a Demo and a Production AI System** `[PERFORMANCE] L2` — reliability, evaluation, security, monitoring, cost, support; the recruiter-friendliest post, `featured: true`.

(§16.2's remaining listed titles — vLLM vs TensorRT-LLM, multi-tenant cluster design — are the post-launch queue, noted in frontmatter drafts.)

### 3.6 Search (Pagefind)

Post-build: `pagefind --site .next/…static output` in the build script; index ships as static assets. `SearchBox` in the blog index (and `/` keyboard shortcut) queries client-side; results grouped by category with metadata rows in index style. Drafts excluded (doc 06). Zero backend.

## 4. Creative direction

- The archive frame: index footer reads `ARCHIVE INTEGRITY: OK · 8 DOCUMENTS · LAST WRITE 2026-07-12` (real values).
- Difficulty (`LEVEL 3`) renders as stacked chevrons + text — never color-only (§25).
- DOWNLOAD AS TEXT produces a genuinely nice `.txt` (76-col wrapped, ASCII diagrams, header block) — a period-perfect artifact people will actually share.
- Post endings: `EOF · FN-00N · [NEXT DOCUMENT →]` instead of a modern "next post" card.

## 5. Dependencies

Doc 06 (schema, computed fields, drafts), doc 04 (window shell; article window opens maximized), doc 01 (fonts, prose classes, print.css stub), doc 11 (planner/profiler links from posts 2/3), doc 14 (incident cross-links from posts 1/5). `Diagram`/`BenchmarkTable` live in `charts/` shared with docs 09/11 (see doc 09 §7 decision).

## 6. Acceptance criteria

- [ ] Index matches §32 structure; filter/sort via URL params; category routes render server-side; PRINT INDEX produces a clean paper listing.
- [ ] Article page shows the §16.4 metadata block and side rail with all six rail items functional; `curl` returns complete article text.
- [ ] All three reading modes work and persist; mobile defaults to CLEAN; PRINT MANUAL and browser-print output match (visual diff of print preview).
- [ ] All six MDX components render per spec; every Diagram has ASCII+SVG+alt; BenchmarkTable auto-labels synthetic data.
- [ ] All 8 posts published, each covering its §3.5 outline, with working cross-links, computed read times, and correct doc_ids.
- [ ] Pagefind: searching "KV cache" returns posts 2/3/5; drafts absent; search works on the deployed static build.
- [ ] Lighthouse on a post: perf ≥ 95, a11y ≥ 95, zero CLS from fonts/diagrams; readable with JS disabled (§25).

## 7. Risks & fallbacks

| Risk | Fallback |
|---|---|
| 8 full posts is a large writing lift | Launch floor is 4 (posts 1, 2, 3, 8 — breadth across audience levels); remaining 4 marked `draft` and land within two weeks post-launch; index shows only published so nothing looks missing |
| Pagefind vs Next output-dir friction | Pagefind indexes the *deployed* HTML via its Node API in a `postbuild` step; proven pattern — worst case, ship a static JSON index + minisearch (~2 kB) since content volume is tiny |
| Dual-authoring ASCII+SVG diagrams is slow | An `ascii-only` Diagram variant is legal for launch; SVG upgrades are content PRs |
| Print stylesheet drift | PRINT MANUAL mode *is* the print stylesheet (same CSS layer), so it's exercised by every mode-switch QA pass, not just at print time |
