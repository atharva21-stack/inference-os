# ATHARVA.RUNTIME
## Complete Portfolio Website Structure and Creative Direction

**Project type:** Personal portfolio for an AI Infrastructure / GPU Inference Engineer  
**Primary creative concept:** A mysterious 1990s computer terminal connected to a futuristic AI datacenter  
**Visual language:** CRT monitor, phosphor glow, dot-matrix graphics, scanlines, terminal windows, BIOS screens, low-resolution 3D, wireframe GPUs, industrial server hardware  
**Primary goal:** Make the visitor feel that they discovered a classified, high-performance inference system built and operated by Atharva Chandwadkar  
**Secondary goal:** Present real technical depth without sacrificing recruiter usability  
**Target audience:** AI infrastructure hiring managers, GPU systems engineers, inference engineers, platform leaders, technical recruiters, founders, and engineering executives

---

# 1. Core Creative Thesis

This portfolio should not resemble a modern SaaS landing page.

It should feel like the visitor found an old workstation in a dark laboratory, powered it on, and discovered that the machine is secretly connected to a modern AI inference cluster.

The website begins as a 1990s computer interface:

- Chunky CRT monitor framing
- Green, amber, or off-white phosphor text
- Dotted and scanlined display texture
- BIOS boot messages
- Mechanical keyboard sounds
- Low-resolution icons
- Blinking block cursor
- Window chrome inspired by Windows 3.1, early UNIX workstations, and DOS
- Pixel-art loading bars
- Occasional horizontal sync distortion
- Subtle bloom and glass curvature
- Text labels rendered as if they were printed by a dot-matrix printer

Behind the nostalgic shell, the content reveals modern systems:

- NVIDIA-inspired accelerator cards
- GPU server racks
- NVLink-style interconnect maps
- Kubernetes clusters
- Inference pipelines
- p50, p95, and p99 latency traces
- HBM memory visualizations
- Model-serving engines
- Capacity-planning simulators
- Agentic-AI workflows
- Observability and failure diagnosis

The key tension is:

> **A retro computer interface controlling a future-grade AI infrastructure system.**

That contrast makes the site memorable.

---

# 2. Portfolio Identity

## 2.1 Product Name

Primary identity:

```text
ATHARVA.RUNTIME
```

Alternative labels used within the interface:

```text
AC-90 INFERENCE WORKSTATION
ATHARVA SYSTEMS LAB
INFERENCE OPERATING SYSTEM
AC/OS
ATHARVA GPU CONTROL TERMINAL
```

Recommended hierarchy:

```text
Machine name: AC-90
Operating system: ATHARVA.RUNTIME
Current user: ATHARVA
System role: AI INFRASTRUCTURE ENGINEER
```

---

## 2.2 Core Positioning

The site should communicate this positioning repeatedly:

> I design and scale the complete path from business demand to GPU capacity to production inference.

Supporting themes:

1. **Hardware awareness**
   - GPU architecture
   - HBM constraints
   - topology
   - accelerator selection
   - interconnect behavior
   - utilization

2. **Inference depth**
   - batching
   - model serving
   - latency
   - prefill and decode
   - KV cache
   - routing
   - quantization

3. **Distributed systems**
   - Kubernetes
   - Slurm
   - autoscaling
   - queues
   - fault tolerance
   - observability
   - multi-tenancy

4. **Production engineering**
   - APIs
   - deployment pipelines
   - monitoring
   - incident analysis
   - reliability
   - capacity forecasting

5. **Business outcomes**
   - cost reduction
   - platform adoption
   - enterprise scale
   - reduced manual turnaround
   - improved latency
   - better utilization

---

# 3. Tone and Personality

The site should feel:

- technically elite
- mysterious
- calm
- intelligent
- industrial
- slightly playful
- credible
- deeply engineered

It should not feel:

- like a gamer website
- overly neon
- cyberpunk for the sake of cyberpunk
- cluttered
- unreadable
- like a fake movie “hacker” screen
- like an NVIDIA copy
- like a generic developer portfolio

The personality should be:

> A serious systems engineer with taste, humor, and command of the entire AI infrastructure stack.

---

# 4. Main Experience Narrative

The entire website follows a system boot and exploration narrative.

## Phase 1: Discovery

The visitor sees an unpowered CRT workstation.

A physical power button appears below the screen.

```text
[ POWER ]
```

When pressed:

- the screen flashes white
- a horizontal line collapses into the center
- the CRT warms up
- boot text appears
- speakers emit a subtle startup tone
- the screen stabilizes
- the system begins diagnostics

## Phase 2: Boot

Example boot log:

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

## Phase 3: Desktop

After boot, the user enters a retro desktop environment.

Desktop icons:

```text
[ ABOUT.EXE ]
[ EXPERIENCE.DIR ]
[ PROJECTS.SYS ]
[ GPU_LAB.EXE ]
[ BLOGS.TXT ]
[ INCIDENTS.LOG ]
[ RESUME.PDF ]
[ CONTACT.COM ]
```

The desktop acts as the primary navigation.

## Phase 4: Exploration

The visitor opens windows, navigates directories, inspects GPU systems, reads case studies, and interacts with simulations.

## Phase 5: Connection

The final screen behaves like an old modem or terminal session:

```text
ESTABLISHING CONNECTION...
REMOTE ENDPOINT FOUND.
SEND MESSAGE TO ATHARVA? [Y/N]
```

---

# 5. Site Architecture

## 5.1 Primary Routes

```text
/
├── /boot
├── /desktop
├── /about
├── /experience
│   ├── /deloitte
│   ├── /bank-of-america
│   ├── /10x-analyst
│   ├── /humana
│   ├── /stryke
│   └── /hex-lab
├── /projects
│   ├── /gpu-capacity-planner
│   ├── /inference-profiler
│   ├── /multi-agent-platform
│   ├── /model-serving-benchmark-lab
│   └── /cluster-topology-simulator
├── /gpu-lab
├── /incidents
├── /blogs
│   ├── /category/gpu
│   ├── /category/inference
│   ├── /category/kubernetes
│   ├── /category/agentic-ai
│   ├── /category/performance
│   └── /post/[slug]
├── /speaking
├── /resume
└── /contact
```

---

# 6. Global Shell

## 6.1 CRT Monitor Frame

The entire website is presented inside a large CRT monitor shell.

### Frame elements

- thick charcoal plastic bezel
- subtle scratches and wear
- molded brand label: `AC-90`
- physical power LED
- brightness knob
- contrast knob
- volume slider
- ventilation slots
- sticker labels:
  - `CUDA INSIDE`
  - `INFERENCE READY`
  - `DO NOT POWER OFF DURING KERNEL EXECUTION`
- optional floppy disk slot as a decorative element
- amber or green power indicator

### Screen effects

- subtle convex curvature
- scanlines
- dot pitch texture
- phosphor glow
- mild chromatic aberration near edges
- occasional flicker
- bloom around bright text
- random tiny dust particles
- slight rolling line during transitions

### Accessibility control

A visible toggle must disable CRT effects:

```text
DISPLAY MODE:
[ CRT ] [ CLEAN ]
```

The clean mode removes:

- scanlines
- flicker
- distortion
- chromatic aberration
- sound effects

---

## 6.2 Window System

All content opens in movable retro windows.

Window chrome includes:

```text
┌─ ABOUT.EXE ────────────────────────────── [–][□][×]
│
│
└────────────────────────────────────────────────────
```

Window rules:

- users can drag windows
- active window has brighter title bar
- windows can minimize to a bottom taskbar
- windows can overlap
- double-clicking title bar maximizes
- mobile uses stacked full-screen windows
- keyboard shortcuts should work
- each window has a URL route for sharing and SEO

---

## 6.3 Persistent Status Bar

Bottom taskbar:

```text
[ START ]  ABOUT.EXE  PROJECTS.SYS  BLOGS.TXT      GPU:78%  NET:OK  17:42
```

Status indicators:

- GPU load
- HBM usage
- network state
- system uptime
- current route
- local time
- sound enabled
- display mode

The metrics may be decorative but should react to page interactions.

---

# 7. Homepage / Boot Sequence

## 7.1 Unpowered State

The first frame is almost black.

Only visible elements:

- CRT outline
- red or amber power LED
- power button
- tiny text:

```text
AC-90 HIGH PERFORMANCE WORKSTATION
```

No navigation is shown before powering on.

---

## 7.2 Boot Sequence Timing

Recommended timeline:

```text
0.0s  User presses power
0.1s  Mechanical click
0.2s  White CRT flash
0.4s  Horizontal scan line
0.8s  BIOS logo
1.2s  Hardware diagnostics
2.5s  GPU topology detection
3.5s  Career archive mounted
4.2s  Blog archive mounted
5.0s  Prompt: PRESS ENTER
```

Allow a `SKIP BOOT` button after one second.

Returning users should load directly into the desktop unless they select:

```text
REPLAY BOOT
```

---

## 7.3 Boot Easter Eggs

Random boot messages:

```text
DETECTING 136 GB300 ACCELERATORS...
NO BOTTLENECKS FOUND. SUSPICIOUS.
LOADING WARPS 0–31...
CHECKING P95 LATENCY...
MOUNTING /CAREER/DELOITTE...
CONNECTING TO HBM CHANNEL 7...
KERNEL PANIC AVOIDED.
```

Use sparingly.

---

# 8. Desktop Home Screen

## 8.1 Desktop Layout

Top left:

```text
ATHARVA.RUNTIME
AI INFRASTRUCTURE OPERATING SYSTEM
```

Desktop icons are arranged in a loose grid.

Recommended icon appearance:

- 16-bit or 32-bit pixel art
- beige workstation colors
- tiny GPU cards
- server racks
- folders
- terminal windows
- oscilloscope traces
- floppy disk icons
- paper documents

## 8.2 Ambient Background

The desktop wallpaper is a dark dotted grid with a faint rotating wireframe accelerator in the background.

Possible background layers:

1. low-resolution GPU wireframe
2. cluster topology map
3. drifting dot-matrix particles
4. faint oscilloscope waveform
5. hidden coordinates and memory addresses

---

# 9. About Section — ABOUT.EXE

## 9.1 Purpose

Introduce Atharva quickly and clearly.

The visitor should understand within 10 seconds:

- who he is
- what he specializes in
- what scale he has worked at
- why he is differentiated

## 9.2 Visual Structure

The window is split into two panes.

### Left pane

A monochrome, dithered portrait or silhouette.

Visual treatments:

- black-and-white halftone
- ASCII portrait
- dot-matrix portrait
- thermal camera style
- wireframe face
- ID badge aesthetic

### Right pane

```text
NAME: ATHARVA CHANDWADKAR
ROLE: AI INFRASTRUCTURE ENGINEER
LOCATION: CHICAGO, IL
STATUS: AVAILABLE FOR HIGH-IMPACT SYSTEMS WORK

SPECIALIZATION:
GPU INFERENCE
AI INFRASTRUCTURE
KUBERNETES
CAPACITY PLANNING
MODEL SERVING
PERFORMANCE ENGINEERING
```

## 9.3 Hero Copy

Recommended primary message:

> I build and scale the systems between an AI model and the real world.

Supporting paragraph:

> My work spans GPU capacity planning, Kubernetes scheduling, inference optimization, model serving, observability, agentic AI platforms, and production reliability. I focus on making expensive infrastructure faster, more predictable, and easier to operate.

## 9.4 Key Metrics

Display as vintage system counters:

```text
PLATFORM SCALE........ $10M+
CLIENT ENGAGEMENTS.... 6+
TURNAROUND REDUCTION.. ~1 WEEK → <1 HOUR
GPU STACK............. NVIDIA / KUBERNETES / vLLM / TENSORRT-LLM
FOCUS.................. PERFORMANCE + RELIABILITY + CAPACITY
```

## 9.5 Positioning Diagram

A simple pipeline:

```text
BUSINESS DEMAND
      ↓
WORKLOAD SIZING
      ↓
GPU CAPACITY
      ↓
SCHEDULING
      ↓
MODEL SERVING
      ↓
OBSERVABILITY
      ↓
BUSINESS OUTCOME
```

Caption:

> I work across the entire inference path, not only the model endpoint.

---

# 10. Experience Section — EXPERIENCE.DIR

## 10.1 Core Metaphor

Experience is shown as a directory tree connected to a server rack.

Left side:

```text
C:\CAREER\EXPERIENCE
├── DELOITTE_AI_FACTORY
├── BANK_OF_AMERICA
├── 10X_ANALYST
├── HUMANA
├── STRYKE_GPU
└── HEX_LAB
```

Right side:

A low-poly or dithered 3D server rack.

Each experience is represented by a physical rack blade.

When selected:

- the blade lights up
- it slides out
- the screen displays metadata
- the detailed case study loads

---

## 10.2 Experience Detail Template

Every role should use the same structure.

```text
SYSTEM NAME
ROLE
DATE RANGE
LOCATION
MISSION
SCALE
ARCHITECTURE
RESPONSIBILITIES
PERFORMANCE WORK
FAILURES ENCOUNTERED
DECISIONS MADE
OUTCOMES
TECHNOLOGY STACK
```

## 10.3 Deloitte AI Factory

### Header

```text
SYSTEM: DELOITTE_AI_FACTORY
ROLE: AI INFRASTRUCTURE ENGINEER / CONSULTANT
STATE: PRODUCTION
```

### Mission

Build and scale enterprise AI systems across multiple client engagements.

### Key narrative

- Built production agentic and inference systems
- Worked across orchestration, GPU infrastructure, observability, deployment, and reliability
- Translated client demand into platform requirements
- Helped scale a $10M+ hybrid AI platform
- Supported multiple regulated enterprise engagements

### Technical modules

```text
[ MODEL SERVING ]
[ GPU CAPACITY ]
[ KUBERNETES ]
[ OBSERVABILITY ]
[ AGENTIC AI ]
[ PLATFORM RELIABILITY ]
```

### Visual treatment

A rack blade with six blinking submodules.

---

## 10.4 Bank of America Inference Platform

### Mission

Scale and optimize an inference platform used by multiple client teams.

### Story structure

```text
PROBLEM
Requests were experiencing high latency and drops under load.

INVESTIGATION
- Kubernetes resource profiling
- application CPU and memory profiling
- network hop analysis
- API tracing
- model-serving path inspection
- p50 and p95 latency review

INTERVENTIONS
- optimized critical request paths
- improved resource allocation
- reduced unnecessary network and serialization overhead
- implemented deeper observability
- created reusable POCs and deployment patterns

OUTCOME
More stable inference behavior, improved latency, and broader platform usability.
```

### Visual

A trace waterfall styled like an old oscilloscope.

---

## 10.5 10X Analyst

### Mission

Automate repetitive consulting workflows using a multi-agent platform.

### Key numbers

```text
ADOPTION........ 6 CLIENT ENGAGEMENTS
BEFORE.......... APPROX. 1 WEEK
AFTER........... UNDER 1 HOUR
ARCHITECTURE.... MULTI-AGENT + RETRIEVAL + MODEL ROUTING
MIGRATION....... AWS → ON-PREMISES NVIDIA STACK
```

### Interaction

A dot-matrix workflow animates from:

```text
MANUAL ANALYST WORK
```

to:

```text
AGENT 01 → AGENT 02 → REVIEW → OUTPUT
```

---

## 10.6 HEX Lab

### Focus

GPU performance, distributed training, quantization, memory behavior, and kernel analysis.

### Metrics

```text
DISTRIBUTED TRAINING SPEEDUP.... 3×
L2 HIT RATE..................... 58% → 81%
TENSORRT INT8 SPEEDUP........... 40%
POWER REDUCTION................. 12%
```

### Visual

A retro logic analyzer display with animated cache-hit lines.

---

# 11. Projects Section — PROJECTS.SYS

## 11.1 Core Metaphor

Projects are executable system files.

```text
C:\SYSTEMS\PROJECTS
├── GPU_CAPACITY_PLANNER.EXE
├── INFERENCE_PROFILER.EXE
├── MULTI_AGENT_PLATFORM.EXE
├── MODEL_SERVING_BENCHMARK.EXE
└── TOPOLOGY_SIMULATOR.EXE
```

Each project can be:

- inspected
- launched
- benchmarked
- opened in GitHub
- viewed as an architecture diagram

---

## 11.2 Project Card Format

```text
FILE NAME
VERSION
STATUS
PURPOSE
INPUTS
OUTPUTS
ARCHITECTURE
METRICS
FAILURE MODES
TECH STACK
SOURCE
DEMO
```

---

## 11.3 GPU Capacity Planner

### Concept

An interactive tool for estimating GPU infrastructure requirements.

### Inputs

```text
MODEL SIZE
PARAMETER COUNT
PRECISION
CONTEXT LENGTH
CONCURRENCY
TOKENS PER SECOND
REPLICA COUNT
AVAILABILITY TARGET
ACCELERATOR CLASS
```

### Outputs

```text
RECOMMENDED GPU
GPU COUNT
ESTIMATED HBM REQUIREMENT
PARALLELISM STRATEGY
EXPECTED THROUGHPUT
HEADROOM
FAILURE RESERVE
ESTIMATED COST
```

### Retro interface

A green-screen form with blinking input fields.

Example:

```text
ENTER MODEL PARAMETERS

MODEL............. LLAMA-70B
PRECISION......... INT4
CONTEXT........... 8192
CONCURRENT USERS.. 40
SLA................ 99.9%

[ RUN CAPACITY MODEL ]
```

### Result visualization

- dot-matrix rack map
- memory bars
- node allocation
- utilization heatmap
- topology warning messages

---

## 11.4 Inference Profiler

### Purpose

Show how an inference request spends time.

### Metrics

```text
TOKENIZATION
QUEUE WAIT
PREFILL
DECODE
NETWORK
POST-PROCESSING
TIME TO FIRST TOKEN
INTER-TOKEN LATENCY
GPU UTILIZATION
KV CACHE UTILIZATION
```

### Simulation modes

```text
[ CPU BOTTLENECK ]
[ NETWORK BOTTLENECK ]
[ HBM PRESSURE ]
[ POOR BATCHING ]
[ KV CACHE EXHAUSTION ]
[ REPLICA IMBALANCE ]
```

Each mode changes the CRT graphs and prints a diagnosis.

---

## 11.5 Model Serving Benchmark Lab

### Features

- compare vLLM, TensorRT-LLM, Triton, and NIM-style serving approaches
- choose model size
- choose precision
- select concurrency
- inspect throughput and latency tradeoffs
- show representative benchmark results
- clearly label synthetic or reconstructed data

### UI concept

A split-screen benchmark console:

```text
ENGINE A: vLLM
ENGINE B: TensorRT-LLM

TTFT
TPOT
P50
P95
TOKENS/SEC
HBM
QUEUE DEPTH
```

---

## 11.6 Cluster Topology Simulator

### Purpose

Demonstrate topology-aware scheduling.

### Interactions

- drag workloads onto nodes
- see NVLink and InfiniBand paths
- view fragmentation
- create failures
- observe rescheduling
- compare naive scheduling against topology-aware scheduling

### Visual style

Low-resolution 3D rack nodes rendered as wireframes inside the CRT.

---

# 12. GPU Lab — GPU_LAB.EXE

## 12.1 Purpose

This is the visual centerpiece.

It should prove that the site contains actual 3D hardware objects while preserving the 1990s computer style.

## 12.2 Main Object

A 3D data-center accelerator card inspired by modern NVIDIA hardware, but not copied directly.

It should include:

- metallic shroud
- compute die
- HBM stacks
- power delivery components
- PCIe connector
- interconnect bridge
- cooling plate
- board traces
- labels rendered as technical annotations

## 12.3 Rendering Style

The user can switch modes:

```text
[ SOLID ]
[ WIREFRAME ]
[ THERMAL ]
[ MEMORY MAP ]
[ EXECUTION VIEW ]
[ EXPLODED VIEW ]
```

### Solid

A low-poly, industrial accelerator model.

### Wireframe

Green vector lines like a 1990s CAD terminal.

### Thermal

Amber, red, and white heat zones rendered with a dithered palette.

### Memory Map

HBM modules glow while memory traffic animates.

### Execution View

Threads and blocks illuminate across the die.

### Exploded View

The card separates into layers.

---

## 12.4 Interactive Hotspots

### Compute Die

Reveals:

- CUDA
- Triton
- tensor operations
- kernel optimization
- inference execution
- profiling

### HBM

Reveals:

- model memory
- KV cache
- context length
- quantization
- batching
- concurrency limits

### Interconnect

Reveals:

- NVLink
- NVSwitch
- InfiniBand
- tensor parallelism
- multi-node inference
- topology-aware scheduling

### Power and Cooling

Reveals:

- utilization
- efficiency
- power-aware tuning
- DVFS
- sustained performance
- thermal constraints

### PCIe and Host Interface

Reveals:

- CPU preprocessing
- serialization
- network transfer
- data pipelines
- host-device bottlenecks

---

## 12.5 GPU Annotation Mode

When a component is selected, technical annotations appear:

```text
COMPONENT: HBM STACK 03
ROLE: HIGH-BANDWIDTH MODEL AND KV CACHE STORAGE
RISK: CAPACITY EXHAUSTION UNDER LONG-CONTEXT CONCURRENCY
ENGINEERING RESPONSE:
- QUANTIZATION
- PAGED KV CACHE
- BATCH CONTROL
- REPLICA SIZING
- REQUEST LIMITS
```

---

# 13. Skills Section — MEMORY.MAP

## 13.1 Core Metaphor

Skills are organized by memory hierarchy.

```text
REGISTER FILE
SHARED MEMORY
HBM
HOST MEMORY
NETWORK FABRIC
CONTROL PLANE
```

## 13.2 Register File

Deepest hands-on skills:

```text
PYTHON
CUDA C++
TRITON
KUBERNETES
vLLM
TENSORRT-LLM
NVIDIA TRITON
FASTAPI
LINUX
```

## 13.3 Shared Memory

Closely integrated tools:

```text
LANGGRAPH
NEMO AGENT TOOLKIT
LANGCHAIN
PROMETHEUS
OPENTELEMETRY
SPLUNK
REDIS
POSTGRESQL
PGVECTOR
```

## 13.4 HBM

System-level expertise:

```text
DISTRIBUTED INFERENCE
GPU CAPACITY PLANNING
MODEL PARALLELISM
AUTOSCALING
CONCURRENCY MODELING
QUANTIZATION
KV CACHE MANAGEMENT
LATENCY ENGINEERING
```

## 13.5 Host Memory

Broader platform stack:

```text
AWS
AZURE
GCP
TERRAFORM
GITHUB ACTIONS
DOCKER
SECURITY
ENTERPRISE ARCHITECTURE
CLIENT DELIVERY
```

## 13.6 Network Fabric

```text
NVLINK
NVSWITCH
INFINIBAND
RDMA
PCIE
LOAD BALANCING
REQUEST ROUTING
SERVICE MESH
```

## 13.7 Interaction

Hovering a skill displays:

```text
USED AT
PROJECTS
DEPTH
RELATED METRICS
ARCHITECTURAL ROLE
```

Do not use fake percentage bars.

---

# 14. Performance Section — PERFMON.EXE

## 14.1 Visual Inspiration

Combine:

- DOS performance monitor
- oscilloscope
- early UNIX system monitor
- vector line charts
- dot-matrix printer output

## 14.2 Main Metrics

```text
P50 LATENCY
P95 LATENCY
P99 LATENCY
TIME TO FIRST TOKEN
TOKENS PER SECOND
GPU UTILIZATION
HBM UTILIZATION
QUEUE DEPTH
ERROR RATE
REPLICA HEALTH
```

## 14.3 Before and After Mode

The visitor can press:

```text
[ APPLY ATHARVA OPTIMIZATION ]
```

The system changes from unstable to optimized.

Before:

```text
GPU UTILIZATION.... 31%
CPU UTILIZATION.... 92%
P95 LATENCY........ 2.8s
QUEUE DEPTH......... RISING
ERROR RATE.......... 3.2%
```

After:

```text
GPU UTILIZATION.... 78%
CPU UTILIZATION.... 61%
P95 LATENCY........ 1.1s
QUEUE DEPTH......... STABLE
ERROR RATE.......... 0.4%
```

Only use real numbers where defensible. Recreated examples must be marked:

```text
REPRESENTATIVE WORKLOAD — NORMALIZED FOR CONFIDENTIALITY
```

---

# 15. Incidents Section — INCIDENTS.LOG

## 15.1 Purpose

Show engineering maturity through real failure analysis.

## 15.2 Interface

A terminal log list:

```text
INC-001  LOW GPU UTILIZATION DESPITE HIGH TRAFFIC
INC-002  KV CACHE EXHAUSTION UNDER LONG CONTEXT
INC-003  HIGH P95 DUE TO NETWORK HOP
INC-004  GPU FRAGMENTATION IN KUBERNETES
INC-005  COLD START DELAY DURING SCALE-OUT
INC-006  PARTIAL REQUEST FAILURE UNDER LOAD
```

## 15.3 Incident Template

```text
INCIDENT ID
SEVERITY
SYSTEM
SYMPTOMS
SIGNALS
HYPOTHESES
INVESTIGATION
ROOT CAUSE
FIX
VALIDATION
OUTCOME
WHAT I WOULD DO DIFFERENTLY
```

## 15.4 Example Incident

```text
INC-001
TITLE: LOW GPU UTILIZATION DESPITE HIGH TRAFFIC

SYMPTOMS
- REQUEST VOLUME HIGH
- GPU UTILIZATION LOW
- CPU UTILIZATION HIGH
- QUEUE DEPTH INCREASING

INVESTIGATION
- PROFILED TOKENIZATION
- INSPECTED SERIALIZATION
- REVIEWED BATCHING
- TRACED NETWORK AND APPLICATION SPANS
- COMPARED CPU PREPARATION TO GPU EXECUTION

ROOT CAUSE
CPU-SIDE PREPROCESSING AND REQUEST BATCHING PREVENTED THE GPU FROM RECEIVING EFFICIENT WORK UNITS.

FIX
- MOVED PREPROCESSING OFF CRITICAL PATH
- INCREASED BATCH EFFICIENCY
- ADJUSTED WORKER CONCURRENCY
- REDUCED SERIALIZATION OVERHEAD

OUTCOME
- HIGHER GPU UTILIZATION
- LOWER QUEUE WAIT
- LOWER P95 LATENCY
- MORE PREDICTABLE THROUGHPUT
```

---

# 16. Blog Section — BLOGS.TXT

## 16.1 Importance

The blog should not feel like a detached modern publication.

It should appear as a technical archive stored on the machine.

Primary label:

```text
C:\ATHARVA\BLOG_ARCHIVE
```

Alternative names:

```text
FIELD_NOTES
ENGINEERING_JOURNAL
GPU_MEMOS
SYSTEM_LOGBOOK
TECHNICAL_TRANSMISSIONS
```

Recommended final name:

```text
FIELD_NOTES
```

---

## 16.2 Blog Index Screen

```text
FIELD_NOTES v2.6

[01] WHY GPU UTILIZATION CAN BE LOW UNDER HIGH TRAFFIC
[02] PREFILL, DECODE, AND THE REAL SHAPE OF LLM LATENCY
[03] HOW TO SIZE GPU CAPACITY FOR AN ENTERPRISE INFERENCE PLATFORM
[04] WHAT KUBERNETES DOES NOT UNDERSTAND ABOUT GPU TOPOLOGY
[05] KV CACHE: THE HIDDEN MEMORY TAX
[06] BUILDING OBSERVABILITY FOR AGENTIC AI SYSTEMS
[07] vLLM VS TENSORRT-LLM: THINKING IN TRADE-OFFS
[08] HOW I WOULD DESIGN A MULTI-TENANT INFERENCE CLUSTER
```

Each item includes:

```text
DATE
READ TIME
CATEGORY
DIFFICULTY
STATUS
```

Example:

```text
2026-07-12  8 MIN  [INFERENCE]  LEVEL 3  PUBLISHED
```

---

## 16.3 Blog Categories

```text
GPU ARCHITECTURE
INFERENCE SYSTEMS
KUBERNETES
CAPACITY PLANNING
PERFORMANCE ENGINEERING
AGENTIC AI
OBSERVABILITY
DISTRIBUTED SYSTEMS
CAREER AND LEARNING
```

---

## 16.4 Blog Article Layout

The blog article should look like a technical manual.

Top metadata:

```text
DOCUMENT ID: FN-004
TITLE: WHAT KUBERNETES DOES NOT UNDERSTAND ABOUT GPU TOPOLOGY
AUTHOR: ATHARVA CHANDWADKAR
STATUS: RELEASED
REVISION: 1.2
```

Main body:

- clean readable typography
- optional CRT effects
- wide diagrams
- code blocks
- callout boxes
- benchmark tables
- glossary
- related posts

Side rail:

```text
CONTENTS
DIAGRAMS
KEY TERMS
RELATED SYSTEMS
DOWNLOAD AS TEXT
PRINT MODE
```

---

## 16.5 Blog Reading Modes

```text
[ CRT MODE ]
[ CLEAN READING MODE ]
[ PRINT MANUAL MODE ]
```

### CRT Mode

Full scanlines and terminal aesthetic.

### Clean Reading Mode

Modern readable article page.

### Print Manual Mode

Off-white technical manual with dot-matrix headings and black diagrams.

---

## 16.6 Blog Article Components

### Technical Callout

```text
┌─ ENGINEERING NOTE ───────────────────────┐
│ GPU utilization is not the same as useful
│ throughput. A saturated GPU can still be
│ serving the wrong workload mix.
└───────────────────────────────────────────┘
```

### Failure Mode

```text
WARNING: KV CACHE PRESSURE
SYMPTOM: REQUESTS FAIL ONLY AT HIGH CONCURRENCY
```

### Decision Record

```text
DECISION: SEPARATE SMALL AND LARGE MODEL POOLS
REASON:
- INDEPENDENT AUTOSCALING
- REDUCED HEAD-OF-LINE BLOCKING
- BETTER COST CONTROL
```

### Diagram

ASCII fallback plus interactive SVG.

### Benchmark

Dot-matrix styled table.

---

## 16.7 Recommended Initial Blog Posts

### 1. GPU Utilization Is Lying to You

Explain why high or low utilization does not directly equal efficiency.

### 2. Prefill vs Decode

Explain how the two phases affect latency, memory, and scheduling.

### 3. How to Size an Inference Cluster

Cover model size, precision, context, concurrency, headroom, and availability.

### 4. KV Cache Is Infrastructure

Explain memory pressure, batching, context length, and eviction.

### 5. Topology-Aware Scheduling

Explain NVLink, NVSwitch, InfiniBand, and why placement matters.

### 6. Observability for Agentic AI

Explain tracing across tools, models, retrieval, retries, and human review.

### 7. What I Learned Scaling a Multi-Agent Platform

Frame lessons without disclosing confidential client details.

### 8. The Difference Between a Demo and a Production AI System

Cover reliability, evaluation, security, monitoring, cost, and support.

---

# 17. Speaking Section — TRANSMISSIONS.LOG

## 17.1 Concept

Talks and events are represented as intercepted transmissions.

```text
TRANSMISSION 001
SOURCE: NVIDIA GTC
STATUS: DECODED
TOPIC: ENTERPRISE AI INFRASTRUCTURE
```

## 17.2 Content

- event
- date
- session title
- audience
- abstract
- slides
- video
- key takeaways
- photos

## 17.3 Visual

A CRT waveform and audio-frequency spectrum.

---

# 18. Resume Section — RESUME.PDF

## 18.1 Interface

The resume opens in a retro document viewer.

Buttons:

```text
[ VIEW ]
[ DOWNLOAD ]
[ PRINT ]
[ COPY TEXT ]
```

## 18.2 Resume Views

```text
EXECUTIVE VIEW
ENGINEERING VIEW
GPU / INFERENCE VIEW
```

These can point to tailored resume versions if available.

---

# 19. Contact Section — CONTACT.COM

## 19.1 Core Concept

A dial-up or remote terminal connection.

Boot message:

```text
DIALING REMOTE ENGINEER...
HANDSHAKE ACCEPTED.
ENCRYPTION ENABLED.
CHANNEL OPEN.
```

## 19.2 Contact Options

```text
[ EMAIL ]
[ LINKEDIN ]
[ GITHUB ]
[ RESUME ]
```

## 19.3 Contact Form

```text
FROM:
ORGANIZATION:
SUBJECT:
MESSAGE:

[ TRANSMIT ]
```

On submission:

```text
PACKET SENT SUCCESSFULLY.
EXPECTED RESPONSE: HUMAN, NOT AUTOMATED.
```

---

# 20. Navigation Model

The site should support four navigation styles.

## 20.1 Desktop Icons

Best for exploration.

## 20.2 Start Menu

```text
PROGRAMS
DOCUMENTS
SYSTEM TOOLS
FIELD NOTES
CONTACT
SHUT DOWN
```

## 20.3 Command Line

Users can press `~` to open a terminal.

Commands:

```bash
help
whoami
ls
cd experience
open projects
run gpu_lab
cat blogs/latest.txt
inspect skills
show metrics
download resume
contact
clear
exit
```

## 20.4 Keyboard Shortcuts

```text
ALT + A   ABOUT
ALT + E   EXPERIENCE
ALT + P   PROJECTS
ALT + G   GPU LAB
ALT + B   BLOGS
ALT + R   RESUME
ALT + C   CONTACT
ESC       CLOSE ACTIVE WINDOW
```

---

# 21. Out-of-the-Box Interactions

## 21.1 Defragment Career

A hidden system utility:

```text
CAREER DEFRAGMENTER
```

It organizes experiences, projects, and skills into a visual timeline.

## 21.2 Overclock Mode

A button labeled:

```text
[ OVERCLOCK ]
```

When activated:

- animations speed up
- GPU glow increases
- metrics rise
- the site warns about thermals

This is decorative and optional.

## 21.3 Screen Saver

After inactivity, a wireframe GPU or bouncing `ATHARVA.RUNTIME` logo moves around the CRT.

## 21.4 Dot-Matrix Printer

Visitors can “print” a one-page profile.

Animation:

- printer paper feeds out
- text appears line by line
- output downloads as PDF

## 21.5 Floppy Disk Easter Egg

A floppy disk icon opens:

```text
SECRET_PROJECTS
```

It may contain:

- experiments
- abandoned prototypes
- architecture sketches
- reading notes
- small code demos

## 21.6 Kernel Panic Page

The 404 page is a tasteful kernel panic.

```text
KERNEL PANIC
REQUESTED RESOURCE NOT FOUND

RECOVERY OPTIONS:
[ RETURN TO DESKTOP ]
[ OPEN SYSTEM MAP ]
```

## 21.7 Visitor Benchmark

A playful performance check:

```text
BENCHMARKING VISITOR DEVICE...
WEBGL.............. AVAILABLE
MEMORY............. SUFFICIENT
MOTION SETTINGS.... DETECTED
DISPLAY MODE....... AUTO
```

Use only real browser capabilities and avoid invasive fingerprinting.

---

# 22. Visual Design System

## 22.1 Color Palettes

### Green Phosphor

```text
BACKGROUND        #050805
PANEL             #0A0F0A
PRIMARY TEXT      #A8FF9E
BRIGHT TEXT       #D7FFD1
DIM TEXT          #5F8A59
BORDER            #2B4A2A
WARNING           #F1D56A
ERROR             #FF8B75
```

### Amber Terminal

```text
BACKGROUND        #0B0803
PRIMARY TEXT      #FFB74D
BRIGHT TEXT       #FFE0A3
DIM TEXT          #9B6A2E
```

### Paper Manual

```text
BACKGROUND        #E7E1CF
TEXT              #1A1A18
BORDER            #636056
ACCENT            #2E4A31
```

Use green as the primary mode. Amber can appear in diagnostics and warnings.

---

## 22.2 Typography

Recommended categories:

### Display

Pixel or bitmap-style display font for labels and headings.

### Terminal

Monospaced font for commands, metrics, logs, and metadata.

### Reading

Highly readable sans-serif or serif for long-form blog content.

Do not use pixel fonts for body paragraphs longer than two lines.

---

## 22.3 Dot-Matrix Texture

The screen should contain a subtle dot pitch.

Implementation options:

- CSS radial-gradient repeating background
- low-opacity texture overlay
- shader-based RGB mask
- screen-space postprocessing

The effect must remain subtle enough to preserve readability.

---

## 22.4 Icon System

Icons should resemble:

- Windows 3.1
- early Macintosh
- DOS utilities
- workstation software
- oscilloscope symbols
- industrial control panels

Create custom icons for:

- GPU
- server rack
- memory
- network
- blog
- incident
- resume
- contact
- terminal
- benchmark

---

# 23. 3D Asset Requirements

## 23.1 Main Assets

```text
1. CRT MONITOR
2. DATA-CENTER GPU CARD
3. SERVER RACK
4. GPU SERVER NODE
5. HBM MODULE
6. INTERCONNECT BRIDGE
7. COMPUTE DIE
8. PCIe CONNECTOR
9. CLUSTER TOPOLOGY NODES
10. DOT-MATRIX PRINTER
11. FLOPPY DISK
12. KEYBOARD
```

## 23.2 Asset Style

- low polygon count
- industrial realism
- slightly worn surfaces
- no direct vendor logo replication
- dark metallic materials
- dithered textures
- optimized glTF or GLB
- separate meshes for interactive components

## 23.3 GPU Model Layers

The GPU asset should have separate selectable meshes:

```text
SHROUD
COOLING PLATE
PCB
COMPUTE DIE
HBM STACK 0–5
POWER DELIVERY
INTERCONNECT
PCIe EDGE
CONNECTORS
```

---

# 24. Responsive Behavior

## Desktop

Full CRT frame and movable windows.

## Tablet

CRT frame retained, but windows snap to columns.

## Mobile

The website becomes a handheld terminal.

Mobile shell:

```text
AC-90 POCKET TERMINAL
```

Features:

- full-screen app windows
- bottom navigation
- reduced 3D complexity
- no drag behavior
- optional horizontal “card insertion” transitions
- clean reading mode enabled by default for blogs

---

# 25. Accessibility

Mandatory requirements:

- full keyboard navigation
- skip boot option
- reduced-motion support
- clean display mode
- high-contrast mode
- proper semantic HTML
- visible focus states
- descriptive labels
- no critical information encoded only through color
- all 3D interactions duplicated in text form
- readable font sizes
- blog content accessible without JavaScript-heavy effects
- no required sound
- sound disabled by default or clearly controllable

---

# 26. Performance Requirements

The portfolio itself should prove infrastructure discipline.

Targets:

```text
LCP............... < 2.5s
INP............... < 200ms
CLS............... < 0.1
INITIAL JS........ MINIMIZED
3D ASSETS......... LAZY LOADED
TEXT CONTENT...... SERVER RENDERED
BLOGS............. STATICALLY GENERATED
```

Performance techniques:

- compressed glTF
- Draco or Meshopt
- KTX2 textures
- progressive loading
- low-resolution fallback assets
- render only visible scenes
- pause animations in background tabs
- use CSS for scanlines instead of heavy postprocessing where possible
- provide static fallback for low-power devices

---

# 27. Technical Stack

## Recommended

```text
FRAMEWORK           NEXT.JS
LANGUAGE            TYPESCRIPT
3D                  REACT THREE FIBER
THREE HELPERS       DREI
ANIMATION           GSAP + FRAMER MOTION
STYLING             TAILWIND OR CSS MODULES
CONTENT             MDX
BLOG                CONTENTLAYER OR VELOCITY
CMS OPTIONAL        SANITY / CONTENTFUL
SEARCH              PAGEFIND OR ALGOLIA
ANALYTICS           PLAUSIBLE OR POSTHOG
DEPLOYMENT          VERCEL
ASSETS              BLENDER → GLB
```

## Suggested Project Structure

```text
src/
├── app/
│   ├── page.tsx
│   ├── boot/
│   ├── desktop/
│   ├── about/
│   ├── experience/
│   ├── projects/
│   ├── gpu-lab/
│   ├── incidents/
│   ├── blogs/
│   ├── resume/
│   └── contact/
├── components/
│   ├── crt/
│   │   ├── CRTFrame.tsx
│   │   ├── Scanlines.tsx
│   │   ├── DotPitch.tsx
│   │   ├── ScreenGlitch.tsx
│   │   └── DisplayModeToggle.tsx
│   ├── desktop/
│   │   ├── Desktop.tsx
│   │   ├── DesktopIcon.tsx
│   │   ├── StartMenu.tsx
│   │   ├── Taskbar.tsx
│   │   └── WindowManager.tsx
│   ├── windows/
│   │   ├── RetroWindow.tsx
│   │   ├── TitleBar.tsx
│   │   └── Dialog.tsx
│   ├── terminal/
│   │   ├── Terminal.tsx
│   │   ├── CommandParser.ts
│   │   └── commands/
│   ├── gpu/
│   │   ├── GPUModel.tsx
│   │   ├── ExplodedView.tsx
│   │   ├── ThermalView.tsx
│   │   ├── MemoryView.tsx
│   │   └── GPUAnnotations.tsx
│   ├── charts/
│   ├── blogs/
│   └── accessibility/
├── content/
│   ├── experience/
│   ├── projects/
│   ├── incidents/
│   └── blogs/
├── public/
│   ├── models/
│   ├── textures/
│   ├── sounds/
│   ├── icons/
│   └── images/
└── styles/
```

---

# 28. Content Model

## 28.1 Experience Content Schema

```yaml
id:
company:
role:
date_start:
date_end:
location:
summary:
mission:
scale:
responsibilities:
decisions:
architecture:
performance_work:
incidents:
outcomes:
metrics:
technologies:
confidentiality_note:
```

## 28.2 Project Content Schema

```yaml
id:
title:
version:
status:
summary:
problem:
users:
inputs:
outputs:
architecture:
technical_decisions:
tradeoffs:
metrics:
failure_modes:
stack:
repository:
demo:
screenshots:
```

## 28.3 Blog Content Schema

```yaml
title:
slug:
date:
updated:
category:
tags:
difficulty:
read_time:
summary:
hero_diagram:
featured:
draft:
related_projects:
related_posts:
```

## 28.4 Incident Content Schema

```yaml
id:
title:
severity:
system:
symptoms:
signals:
hypotheses:
root_cause:
fix:
validation:
outcome:
lessons:
```

---

# 29. SEO and Sharing

Even though the website is highly interactive, every page should have:

- semantic title
- description
- Open Graph image
- canonical URL
- structured data
- static text content
- shareable deep link
- crawlable blog body

Example title:

```text
Atharva Chandwadkar — AI Infrastructure and GPU Inference Engineer
```

Example description:

```text
Portfolio of Atharva Chandwadkar, an AI infrastructure engineer specializing in GPU capacity planning, Kubernetes, model serving, inference optimization, and production AI systems.
```

---

# 30. Homepage Wireframe

```text
┌──────────────────────────── AC-90 CRT ────────────────────────────┐
│                                                                  │
│  ATHARVA.RUNTIME                                   17:42          │
│                                                                  │
│  ┌──────────┐  ┌──────────────┐  ┌────────────┐                   │
│  │ ABOUT.EXE│  │EXPERIENCE.DIR│  │PROJECTS.SYS│                   │
│  └──────────┘  └──────────────┘  └────────────┘                   │
│                                                                  │
│  ┌──────────┐  ┌──────────────┐  ┌────────────┐                   │
│  │GPU_LAB   │  │FIELD_NOTES   │  │INCIDENTS   │                   │
│  └──────────┘  └──────────────┘  └────────────┘                   │
│                                                                  │
│              [ ROTATING WIREFRAME GPU ]                           │
│                                                                  │
│  SYSTEM STATUS                                                   │
│  GPU FABRIC: ONLINE   BLOG ARCHIVE: MOUNTED                       │
│                                                                  │
├──────────────────────────────────────────────────────────────────┤
│ [START]  ABOUT  PROJECTS  BLOGS          GPU 78%  NET OK  17:42   │
└──────────────────────────────────────────────────────────────────┘
```

---

# 31. GPU Lab Wireframe

```text
┌─ GPU_LAB.EXE ───────────────────────────────────────── [–][□][×] ┐
│ VIEW: [SOLID] [WIRE] [THERMAL] [MEMORY] [EXPLODED]               │
│                                                                  │
│         ┌──────────────────────────────────────┐                  │
│         │                                      │                  │
│         │          3D ACCELERATOR CARD         │                  │
│         │                                      │                  │
│         └──────────────────────────────────────┘                  │
│                                                                  │
│ COMPONENT: HBM STACK 03                                           │
│ ROLE: MODEL WEIGHTS + KV CACHE                                    │
│                                                                  │
│ CAPACITY PRESSURE SOURCES                                         │
│ [CONTEXT] [CONCURRENCY] [PRECISION] [BATCH SIZE]                  │
│                                                                  │
│ ATHARVA'S WORK                                                    │
│ - GPU MEMORY SIZING                                               │
│ - QUANTIZATION                                                    │
│ - CAPACITY HEADROOM                                               │
│ - REQUEST CONTROL                                                 │
└──────────────────────────────────────────────────────────────────┘
```

---

# 32. Blog Index Wireframe

```text
┌─ FIELD_NOTES ───────────────────────────────────────── [–][□][×] ┐
│ C:\ATHARVA\FIELD_NOTES                                            │
│                                                                  │
│ SEARCH: [_________________________]                                │
│ FILTER: GPU  INFERENCE  K8S  AGENTS  PERFORMANCE                  │
│                                                                  │
│ 01  GPU UTILIZATION IS LYING TO YOU                               │
│     2026-07-12  8 MIN  [GPU]  LEVEL 3                             │
│                                                                  │
│ 02  PREFILL, DECODE, AND THE REAL SHAPE OF LATENCY                │
│     2026-07-06  10 MIN [INFERENCE] LEVEL 4                        │
│                                                                  │
│ 03  HOW TO SIZE AN ENTERPRISE INFERENCE CLUSTER                   │
│     2026-06-28  12 MIN [CAPACITY] LEVEL 4                         │
│                                                                  │
│ [OPEN] [SORT] [PRINT INDEX]                                       │
└──────────────────────────────────────────────────────────────────┘
```

---

# 33. Content Priorities

The site should prioritize content in this order:

1. Clear positioning
2. Real systems and outcomes
3. GPU and inference depth
4. Architecture decisions
5. Failure analysis
6. Projects and interactive demos
7. Blogs and public thinking
8. Speaking and community
9. Contact

The site must never allow visual design to hide the actual engineering work.

---

# 34. MVP Scope

## Phase 1

- CRT frame
- boot sequence
- desktop
- About
- Experience
- Projects
- Blogs
- Resume
- Contact
- clean-mode toggle
- one 3D GPU object

## Phase 2

- movable windows
- command-line navigation
- GPU exploded view
- server rack experience view
- incident logs
- performance dashboard
- blog search

## Phase 3

- capacity planner
- inference profiler
- cluster topology simulator
- printer animation
- screen saver
- easter eggs

---

# 35. Final Creative Standard

The finished website should make a visitor think:

> This person does not merely know AI tools. He understands the machines, schedulers, serving engines, memory limits, performance traces, and operational systems that make AI work in production.

The experience should feel like:

```text
A 1994 workstation
running a 2026 AI datacenter
operated by an engineer
who understands every layer.
```

That is the central idea.

---

# 36. Final One-Line Brand Statement

Recommended:

> **I build the infrastructure that makes intelligence run.**

Alternatives:

> **From GPU capacity to production inference.**

> **Engineering the systems behind intelligent applications.**

> **Making expensive GPUs earn their keep.**

> **Where models meet hardware, scheduling, and scale.**

---

# 37. Final Homepage Copy

```text
ATHARVA CHANDWADKAR
AI INFRASTRUCTURE ENGINEER

I BUILD THE INFRASTRUCTURE THAT MAKES INTELLIGENCE RUN.

GPU CAPACITY
MODEL SERVING
KUBERNETES
INFERENCE OPTIMIZATION
OBSERVABILITY
PRODUCTION RELIABILITY

[ ENTER SYSTEM ]
```

---

# 38. Build Principle

Every visual feature must answer one question:

> Does this help the visitor understand Atharva’s engineering depth, systems thinking, or impact?

If not, remove it.

The site should be unusual because the concept is deeply connected to the work—not because it contains random effects.
