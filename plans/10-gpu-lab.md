# 10 — GPU_LAB.EXE: The 3D Centerpiece

Phase 2 (single solid-mode object in Phase 1, per §34) · Depends on: [04-window-manager](./04-window-manager.md) · Covers §12, §23, §31

---

## 1. Mission

GPU Lab is the visual centerpiece (§12.1): a genuine 3D accelerator card, explorable in six render modes, that proves the machine metaphor with real hardware geometry — while every annotation routes back to Atharva's actual work (§38). This doc also owns the **entire 3D asset pipeline** (§23) that other docs borrow from (desktop sprite in doc 05, topology nodes in doc 11).

## 2. Deliverables

- Route `/gpu-lab` (GPU_LAB.EXE window, maximized by default).
- `src/components/gpu/` — `GPULabCanvas`, `GPUModel`, `ViewModeBar`, `Hotspots`, `GPUAnnotations`, `LabFallback`.
- Assets: `public/models/ac90-accelerator.glb` (+ LOD variant), `public/models/rack.glb`, `topology-node.glb`, KTX2 texture set; Blender sources under `assets-src/` (repo) with an export script.
- `src/lib/three/` — shared R3F setup (loader with Draco/meshopt/KTX2 transcoders, capability-gated renderer settings).

## 3. Technical design

### 3.1 Asset pipeline (§23)

- Authoring: Blender; export via `assets-src/export.py` → glTF → `gltf-transform` CLI pass (draco/meshopt + KTX2 + prune/weld) — deterministic, run in CI so a re-export can't silently bloat.
- **Budgets:** accelerator ≤ 40k tris / ≤ 1.5 MB GLB / ≤ 2 MB textures total; LOD-low ≤ 8k tris for mobile; rack + topology node ≤ 10k tris each. Style per §23.2: low-poly industrial realism, worn surfaces, dithered textures, dark metallic, **no vendor logo replication** — silkscreen text uses ATHARVA SYSTEMS LAB part numbers (`ASL-90 XR-136`).
- **Mesh separation (§23.3)** — named nodes, the contract between Blender and code: `SHROUD, COOLING_PLATE, PCB, COMPUTE_DIE, HBM_STACK_0…HBM_STACK_5, POWER_DELIVERY, INTERCONNECT, PCIE_EDGE, CONNECTORS`. A Vitest asset test loads the GLB in node and asserts all names exist — renames fail CI, not runtime.
- §23.1's full asset list is scheduled: items 2,3,5,6,7,8,9 (accelerator + rack + topology) here; CRT monitor/keyboard/printer/floppy are **not modeled in 3D** — they exist as SVG (docs 02, 15); revisit only if a phase-4 flourish budget appears.

### 3.2 Scene architecture

```text
/gpu-lab route (next/dynamic, ssr:false for canvas; SSR renders LabFallback text + poster)
└── GPULabCanvas (R3F)
    ├── <AccleratorModel/>   gltf via useGLTF, meshes addressable by §23.3 names
    ├── <Rig/>               orbit (drei OrbitControls, damped, zoom-clamped)
    ├── <Effects/>           the ONE place WebGL postprocessing is allowed (master plan §2): scanline/dither composite pass, capability-gated
    └── <HotspotAnchors/>    Html/occlude anchors on named meshes
```

- View-mode switching is **material/visibility state, not scene swaps**: one model, six material sets, so mode changes are instant.
- Render loop uses `frameloop="demand"` + invalidate on interaction — idle lab costs ~0 GPU (§26 pause-in-background comes free).
- Capability gate (`capabilities.ts`, doc 01): high → full model + effects pass; medium → LOD-low, no post; low/no-WebGL/reduced-data → `LabFallback`: pre-rendered turntable images (from the doc-05 sprite pipeline) + full annotation text. All content reachable without WebGL (§25: 3D duplicated in text).

### 3.3 Six render modes (§12.3)

| Mode | Implementation |
|---|---|
| `SOLID` | low-poly PBR-lite (KTX2 baked AO), industrial materials |
| `WIREFRAME` | barycentric-wireframe shader material in phosphor green (1990s CAD terminal look) — not three.js `wireframe:true` (too noisy on tris) |
| `THERMAL` | dithered gradient shader (amber→red→white per §12.3) driven by per-mesh heat scalars; COMPUTE_DIE hottest, animates subtly with `telemetryStore` |
| `MEMORY MAP` | HBM_STACK_* emissive glow + animated traffic particles along PCB trace splines (prebaked curve data in the GLB extras) |
| `EXECUTION VIEW` | COMPUTE_DIE overlay texture: SM/warp grid cells illuminating in scheduling-wave patterns (shader time-based; honest simplification, labeled "SCHEMATIC") |
| `EXPLODED` | per-mesh target offsets stored in GLB extras; spring-animated separation along Y; annotations get leader lines |

Mode bar per §31 wireframe: `VIEW: [SOLID] [WIRE] [THERMAL] [MEMORY] [EXECUTION] [EXPLODED]` — radio group, keyboard 1–6, URL param (`/gpu-lab?view=thermal`) for sharing.

### 3.4 Hotspots + annotation mode (§12.4–12.5, §31)

Five hotspot groups → §12.4's reveal lists, mapped to meshes: COMPUTE_DIE; HBM (any HBM_STACK_*); INTERCONNECT; POWER_DELIVERY+COOLING_PLATE; PCIE_EDGE+CONNECTORS. Click/tap/focus (hotspots are DOM buttons overlaid via drei `Html`, so keyboard/screen-reader native) opens the annotation panel — the §31 lower panel — using the §12.5 format:

```text
COMPONENT: HBM STACK 03
ROLE: HIGH-BANDWIDTH MODEL AND KV CACHE STORAGE
RISK: CAPACITY EXHAUSTION UNDER LONG-CONTEXT CONCURRENCY
ENGINEERING RESPONSE: QUANTIZATION · PAGED KV CACHE · BATCH CONTROL · REPLICA SIZING · REQUEST LIMITS
```

Every panel ends with an **ATHARVA'S WORK** block (§31) linking to the relevant experience/project/blog routes — the lab is a navigation hub in disguise. Annotation content lives in `content/gpu-lab/hotspots.yaml` (added to doc 06 config), not hardcoded.

## 4. Creative direction

- Camera intro: card boots like hardware — lights sweep across HBM stacks, then settles (2s, skipped under reduced motion).
- Selected meshes get a phosphor selection box + corner brackets (CAD style), never a modern glow ring.
- Exploded view stacks a small parts-list legend (`10 COMPONENTS · 6 HBM · 1 DIE`) like an IKEA-from-1994 diagram.
- Sound (if enabled): a soft relay click on mode change; no ambient hum (fatiguing, §3 calm).
- The §12.2 requirement list (shroud, die, HBM, power, PCIe, bridge, cooling, traces, technical-annotation labels) is the Blender modeling checklist, silkscreened labels included.

## 5. Dependencies

Doc 01 (capabilities, tokens), doc 04 (window; lab requests maximized default), doc 06 (hotspot YAML collection), exports feed doc 05 (sprite) and doc 11 (topology nodes). Content links require docs 08/09/13 routes.

## 6. Acceptance criteria

- [ ] GLB passes the mesh-name CI test; budgets met (CI prints sizes; fails over budget).
- [ ] All six modes switch instantly (< 100ms perceived), keyboard-selectable, URL-shareable; visuals match §12.3 descriptions.
- [ ] All five hotspot groups open §12.5-format annotations with working ATHARVA'S WORK links; fully operable with keyboard and screen reader (axe + manual NVDA pass).
- [ ] 60fps orbit on mid-range desktop; ≥ 30fps LOD-low on a 2022 Android phone; idle GPU usage ≈ 0 (demand frameloop verified).
- [ ] No-WebGL environment shows LabFallback with turntable images and 100% of annotation text.
- [ ] three.js chunk loads only on lab entry (build output check); route poster prevents CLS.
- [ ] No NVIDIA marks/logos anywhere on the asset (§23.2).

## 7. Risks & fallbacks

| Risk | Fallback |
|---|---|
| Blender authoring is the schedule's long pole | Phase 1 only needs SOLID mode of a blockout-quality model (§34); detail/silkscreen passes land per mode through Phase 2. Worst case: license a CC0 base mesh and re-texture to spec |
| Custom shaders (wireframe/thermal/dither) exceed capacity | drei + three stock materials approximate every mode (emissive for memory, vertexColors for thermal, `wireframe:true` despite noise) — ugly-but-working is the floor, shaders are the polish |
| `Html` hotspot overlay jitters during orbit | Switch to raycast-driven DOM panel outside the canvas (fixed position, no per-frame transforms) — a11y actually improves |
| KTX2 transcoder wasm adds startup weight | Loaded lazily with the lab chunk; if still heavy, drop KTX2 for this asset (small textures) and keep it for doc 11 |
| Mobile thermal throttling tanks fps | Demand frameloop already helps; add auto-degrade: 3 slow frames → drop to LOD-low → then to static fallback, with a `THERMAL LIMIT REACHED — REDUCING CLOCKS` toast (on-theme) |
