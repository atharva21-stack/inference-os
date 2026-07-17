# 14 — INCIDENTS.LOG · TRANSMISSIONS.LOG

Phase 2 · Depends on: [04-window-manager](./04-window-manager.md), [06-content-architecture](./06-content-architecture.md) · Covers §15, §17

---

## 1. Mission

INCIDENTS.LOG shows engineering maturity through failure analysis (§15.1) — the section senior engineers trust most, because nobody fakes a good postmortem (§33 priority 5). TRANSMISSIONS.LOG (§17) frames talks and community work as intercepted signals. They share this doc because both are log-styled archive windows over small Velite collections.

## 2. Deliverables

- Routes: `/incidents` (+ `/incidents/[id]`), `/speaking`.
- `src/components/incidents/` — `IncidentLog`, `IncidentReport`, `SeverityBadge`.
- `src/components/transmissions/` — `TransmissionLog`, `TransmissionCard`, `WaveformHeader`.
- Content: `content/incidents/inc-001…inc-006.mdx` (§28.4 schema), `content/transmissions/*.mdx` (§17.2 schema, doc 06).

## 3. Technical design

### 3.1 Incident index (§15.2)

A terminal log list, exactly the §15.2 register:

```text
INC-001  LOW GPU UTILIZATION DESPITE HIGH TRAFFIC
INC-002  KV CACHE EXHAUSTION UNDER LONG CONTEXT
INC-003  HIGH P95 DUE TO NETWORK HOP
INC-004  GPU FRAGMENTATION IN KUBERNETES
INC-005  COLD START DELAY DURING SCALE-OUT
INC-006  PARTIAL REQUEST FAILURE UNDER LOAD
```

Rows: id, severity badge (SEV text + shape, not color-only), title, system tag. Server-rendered list of links; arrow-key row navigation like doc 09's file manager.

### 3.2 Incident report page (§15.3 template)

`IncidentReport` renders the §15.3 fields in order: `INCIDENT ID · SEVERITY · SYSTEM · SYMPTOMS · SIGNALS · HYPOTHESES · INVESTIGATION · ROOT CAUSE · FIX · VALIDATION · OUTCOME · WHAT I WOULD DO DIFFERENTLY`. Structure notes:

- SYMPTOMS/SIGNALS/HYPOTHESES/FIX/VALIDATION/OUTCOME are frontmatter lists rendered as log lines; INVESTIGATION is the MDX body (narrative prose + doc 13 components — a timeline of what was checked and why, the actual demonstration of method).
- HYPOTHESES render as a checklist: ruled-out items get `✗ RULED OUT (reason)`, the confirmed path gets `✓` — showing *process*, not just answers.
- ROOT CAUSE is typographically the loudest block on the page (bordered, bright).
- WHAT I WOULD DO DIFFERENTLY is mandatory (schema-required) — humility is the section's credibility signature.
- Cross-links: `system` ties to an experience id where applicable (INC-001/003 → bank-of-america; renders `SYSTEM: BANK_OF_AMERICA_INFERENCE` linking to doc 08); related blog links (INC-001 ↔ post 1, INC-002 ↔ post 5).

### 3.3 The six incidents (content spec)

| id | Root-cause territory (writeups follow §15.3 fully) |
|---|---|
| INC-001 | §15.4 verbatim as the seed: CPU-side preprocessing + batching starving the GPU |
| INC-002 | context×concurrency KV blowup; fix: paged KV, request limits, batch control (§12.5 echo) |
| INC-003 | extra network hop/serialization in the serving path inflating p95 (BoA-flavored, §10.4) |
| INC-004 | k8s bin-packing fragmenting NVLink domains; fix: topology-aware placement (doc 11 sim tie-in) |
| INC-005 | scale-out cold starts (image pull + weight load); fix: pre-warmed pools, snapshotting |
| INC-006 | partial failures under load (timeouts mid-stream); fix: backpressure, load shedding, retry budgets |

All are normalized/anonymized; each carries `confidentiality_note` handling per §14.3 where metrics appear.

### 3.4 TRANSMISSIONS.LOG (§17)

Index of transmission cards in the §17.1 frame:

```text
TRANSMISSION 001
SOURCE: NVIDIA GTC
STATUS: DECODED
TOPIC: ENTERPRISE AI INFRASTRUCTURE
```

Card expands to the §17.2 content: event, date, session title, audience, abstract, slides/video links, key takeaways, photos. Header visual (§17.3): a CRT waveform + frequency-spectrum bar strip (CSS/SVG animation, seeded per transmission id; static under reduced motion; decorative/aria-hidden). `STATUS: SCHEDULED` styles future talks as not-yet-decoded (dimmed, no links). If video exists, it embeds lazily (click-to-load poster — no third-party JS until consent to play).

## 4. Creative direction

- Incident pages open with a brief "log retrieval" beat: `RETRIEVING INC-004 FROM ARCHIVE... OK` (300ms, reduced-motion: skipped).
- Severity chrome is restrained: SEV2 in `--warning`, no red-alert theatrics — postmortems are calm documents (§3).
- VALIDATION sections render as a test-report block (`P95 RECHECKED UNDER REPLAYED LOAD ✓`) — reinforcing that fixes were proven, not assumed.
- Transmission photos render halftone-dithered thumbnails (doc 07's pipeline) that resolve to full images on open.

## 5. Dependencies

Doc 06 (both schemas + seed INC-001), doc 04 (windows/routes), doc 13 (MDX components inside INVESTIGATION bodies), doc 08 (experience cross-link targets; doc 08's `incidents[]` refs point here — land ids before Gate G2 so both sides link).

## 6. Acceptance criteria

- [ ] `/incidents` lists all six §15.2 entries; keyboard navigation; all rows deep-link and server-render.
- [ ] Every incident page renders all twelve §15.3 fields in order; INC-001 content matches §15.4; WHAT I WOULD DO DIFFERENTLY present on all six (schema enforces).
- [ ] Hypothesis checklists show ruled-out vs confirmed states; ROOT CAUSE block visually dominant.
- [ ] Cross-links resolve both ways (experience ↔ incident, incident ↔ blog) — build-validated refs.
- [ ] `/speaking` renders transmission cards with all populated §17.2 fields; SCHEDULED items dimmed without dead links; video embeds are click-to-load.
- [ ] Waveform header is aria-hidden, static under reduced motion, < 1ms/frame.
- [ ] Both windows usable at 390px; axe passes.

## 7. Risks & fallbacks

| Risk | Fallback |
|---|---|
| Six rigorous postmortems is a heavy writing lift | Floor is 3 at Gate G2 (INC-001/002/004 — the strongest teaching trio); log list shows only published entries so the section never looks half-filled |
| Anonymization weakens specificity | Normalize numbers but keep mechanisms exact — the §15.3 template's value is in reasoning chains, which need no confidential figures; `confidentiality_note` explains the normalization |
| Few real talks to list | TRANSMISSIONS renders gracefully with 1–2 entries; include podcast/panel/internal-talk formats (§17 doesn't require conferences); never pad with fake events |
| Incident content overlaps blog posts 1/5 | By design — incident = the event record, post = the general lesson; cross-links make the pairing a feature; content reviews check they don't duplicate paragraphs |
