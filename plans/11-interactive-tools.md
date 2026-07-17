# 11 — Interactive Tools: Four Simulations + PERFMON.EXE

Phase 3 · Depends on: [04-window-manager](./04-window-manager.md), [09-projects](./09-projects.md), [10-gpu-lab](./10-gpu-lab.md) (topology assets) · Covers §11.3–§11.6, §14

---

## 1. Mission

Five interactive surfaces where the visitor *operates* the infrastructure instead of reading about it — the strongest possible proof of §35's standard. Four simulations (§11.3–11.6) launched from PROJECTS.SYS, plus **PERFMON.EXE** (§14), the live-styled dashboard with the `[ APPLY ATHARVA OPTIMIZATION ]` before/after move. The governing rule: **the math must be defensible** (§14.3) — every model is either real engineering arithmetic or clearly labeled schematic behavior. A recruiter playing for 30 seconds gets a story; a GPU engineer probing for 10 minutes must not catch a lie.

## 2. Deliverables

- Routes: tools mount at their project routes (`/projects/[id]` gains a `RUN` state; heavy code via `next/dynamic`), PERFMON at `/perfmon` (route amendment, master plan §3).
- `src/components/tools/` — `CapacityPlanner`, `InferenceProfiler`, `BenchmarkLab`, `TopologySim`, `Perfmon`.
- `src/lib/sizing-math.ts` — pure, unit-tested sizing/latency arithmetic shared by planner, profiler, and benchmark lab.
- `src/components/charts/` — retro chart primitives: `ScopeChart` (oscilloscope line), `BarBank` (memory bars), `RackMap` (dot-matrix rack/heatmap), `WaterfallChart` — shared with docs 08/13.
- Datasets: `content/tools/*.json` — accelerator specs table, representative benchmark data (labeled per §14.3).

## 3. Technical design

### 3.0 Shared architecture

Every tool = **pure model function + retro view**: `model(inputs) → results` in `sizing-math.ts` (deterministic, unit-tested, no React), views render results through the chart primitives. All tools: keyboard operable, results also rendered as plain text tables (§25), state encoded in URL query for shareable configurations, and a `[ COPY REPORT ]` action emitting a plaintext dot-matrix report.

### 3.1 GPU Capacity Planner (§11.3) — the flagship, real math

- **Inputs (§11.3):** model size/parameter count, precision (FP16/FP8/INT8/INT4), context length, concurrency, target tokens/sec, replica count, availability target, accelerator class (spec table: HBM GB, bandwidth, dense TFLOPs per class — generic classes `ASL-A100-LIKE`, `ASL-H100-LIKE`, `ASL-B200-LIKE`, real published specs, no vendor branding).
- **Math (documented in-code and in the project writeup, doc 09):**
  - weights GB = params × bytes/param (precision) × 1.05 overhead
  - KV cache/token = 2 × layers × kv_heads × head_dim × bytes; total KV = concurrency × context × per-token
  - HBM need = weights (÷ TP degree) + KV + activations + runtime overhead → TP/PP strategy chosen to fit; fragmentation headroom 15%
  - throughput estimate: memory-bandwidth-bound decode model (tokens/sec ≈ bandwidth / bytes-touched-per-token, batching-scaled) — clearly labeled as roofline estimate
  - availability: N+1/N+2 failure reserve from availability target; cost from $/GPU-hr table
- **Outputs (§11.3):** recommended GPU, count, HBM requirement, parallelism strategy, expected throughput, headroom, failure reserve, cost — rendered as the §11.3 result set: `RackMap` allocation, `BarBank` memory bars, utilization heatmap, and topology warnings (`WARNING: TP=8 SPANS NVLINK DOMAIN`) .
- Green-screen form per §11.3's example (blinking field cursors, `[ RUN CAPACITY MODEL ]`).

### 3.2 Inference Profiler (§11.4)

Interactive request-anatomy explorer: a `WaterfallChart` of TOKENIZATION → QUEUE WAIT → PREFILL → DECODE → NETWORK → POST-PROCESSING with derived TTFT/inter-token latency, GPU util, KV util readouts. Six failure-mode buttons (§11.4) — CPU BOTTLENECK, NETWORK BOTTLENECK, HBM PRESSURE, POOR BATCHING, KV CACHE EXHAUSTION, REPLICA IMBALANCE — each reshapes the waterfall via parameterized model presets and prints a **diagnosis paragraph** (the actual teaching payload: symptom → signal → root cause → fix, mirroring the §15.3 incident structure). Scope-styled charts; labeled `SCHEMATIC MODEL — SHAPES REPRESENTATIVE`.

### 3.3 Model Serving Benchmark Lab (§11.5)

Split-screen console comparing two of {vLLM, TensorRT-LLM, Triton, NIM-style}: pick model size, precision, concurrency → both panels show TTFT, TPOT, P50, P95, tokens/sec, HBM, queue depth from the representative dataset (curves interpolated from published/reconstructed benchmarks; every figure carries the §14.3 label per §11.5's own requirement). The point is **trade-off literacy**: a `TRADE-OFF READOUT` line summarizes each comparison (`ENGINE B WINS THROUGHPUT AT HIGH CONCURRENCY; ENGINE A HOLDS LOWER TTFT AT LOW LOAD`).

### 3.4 Cluster Topology Simulator (§11.6)

- Grid of rack nodes (2D dot-matrix view default; capability-gated wireframe 3D using doc 10's `topology-node.glb` on desktop).
- Interactions (§11.6): drag workloads (TP groups of 2/4/8) onto nodes; NVLink (intra-node) vs InfiniBand (inter-node) paths draw with cost badges; fragmentation meter; `INJECT FAILURE` kills a node → watch rescheduling; toggle `NAIVE` vs `TOPOLOGY-AWARE` scheduler and compare placement quality scores.
- Scheduler logic is a real greedy bin-packer with topology cost function (~150 LOC, unit-tested) — simplified, honest, and explained in an inline `HOW THIS MODEL WORKS` disclosure.
- Keyboard path: select workload → arrow to node → Enter to place (drag parity, §25).

### 3.5 PERFMON.EXE (§14)

DOS-perfmon × oscilloscope dashboard (§14.1): scope charts + counters for the §14.2 metric set (P50/P95/P99, TTFT, tokens/sec, GPU util, HBM util, queue depth, error rate, replica health), animated from a scripted scenario generator (not random — shaped degradation patterns). The centerpiece interaction (§14.3):

```text
[ APPLY ATHARVA OPTIMIZATION ]
BEFORE: GPU 31% · CPU 92% · P95 2.8s · QUEUE RISING  · ERR 3.2%
AFTER:  GPU 78% · CPU 61% · P95 1.1s · QUEUE STABLE  · ERR 0.4%
```

Transition animates over ~3s (queue drains, percentiles settle); a `WHAT CHANGED` panel lists the interventions (mirroring §10.4's BoA story — cross-linked). Permanent footer: `REPRESENTATIVE WORKLOAD — NORMALIZED FOR CONFIDENTIALITY`. PERFMON is also the OVERCLOCK easter egg's host surface (doc 15).

## 4. Creative direction

- All five tools share the "instrument" chassis: bezel-less inner panel, scope-green traces, Terminal font readouts, `▮▯` LED banks. Charts follow dataviz-with-dot-matrix restraint — max 2 series per scope, labeled directly (no legends where avoidable).
- Results print progressively line-by-line like a report coming off a printer (150ms/line, reduced-motion: instant).
- Wrong/extreme inputs produce era-styled errors that still teach: `CONFIG REJECTED: 70B @ FP16 EXCEEDS SINGLE ASL-A100-LIKE HBM (140GB > 80GB). SUGGEST TP=2 OR INT8.`
- Each tool ends with `FIELD NOTES ON THIS TOPIC →` linking the matching blog post (planner→post 03, profiler→02, KV modes→05… map kept in frontmatter).

## 5. Dependencies

Doc 09 (launch surfaces + writeups), doc 10 (topology GLB, R3F setup), doc 06 (tools datasets as a Velite JSON collection), doc 13 (linked posts), doc 05 (`telemetryStore` spikes while simulations run). Chart primitives here are consumed by docs 08/13 — build `charts/` first within this doc's execution.

## 6. Acceptance criteria

- [ ] `sizing-math.ts` unit tests pass for ≥ 6 worked examples, including: Llama-70B/INT4/8k ctx/40 users/99.9% (the §11.3 example) → plausible single-node TP recommendation; 70B/FP16 → correctly requires multi-GPU; long-context KV blowup detected.
- [ ] Every synthetic figure on screen has the §14.3 label within the same panel (manual sweep + doc 06 test where content-driven).
- [ ] All six profiler failure modes produce visibly distinct waterfalls + correct diagnosis text.
- [ ] Topology sim: naive vs topology-aware produces measurably different placement scores on the same seed; failure injection reschedules; full keyboard parity.
- [ ] PERFMON before/after matches §14.3's numbers; WHAT CHANGED links to the BoA case study.
- [ ] Tool state round-trips through URL params; COPY REPORT emits valid plaintext.
- [ ] Each tool is a route-level dynamic chunk; `/projects` index unaffected (bundle check); all tools usable at 390px width.

## 7. Risks & fallbacks

| Risk | Fallback |
|---|---|
| Sizing math disputed by a domain expert | Every formula cited in the doc-09 writeup with assumptions; an `ASSUMPTIONS` disclosure in-tool; wrong constants are data fixes (spec table), not code changes |
| Phase 3 scope blowout (5 tools) | Priority: Planner → PERFMON → Profiler → Benchmark Lab → Topology Sim. §34 puts tools last for a reason; each ships independently behind its RUN button |
| Benchmark data goes stale as engines evolve | Dataset JSON carries `as_of` date rendered in the panel (`DATA VINTAGE: 2026-05`); refresh is a data PR |
| 3D topology view doubles the sim's cost | 2D dot-matrix view is the *primary* deliverable and fully sufficient; 3D is a capability-gated garnish |
| Drag interactions poor on touch | Tap-to-select-tap-to-place is the touch model (same code path as keyboard parity) |
