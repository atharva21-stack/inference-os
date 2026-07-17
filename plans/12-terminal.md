# 12 — Terminal: `~` Command Line

Phase 2 · Depends on: [04-window-manager](./04-window-manager.md) · Covers §20.3

---

## 1. Mission

The terminal is the navigation style for the visitor who *is* the target audience (§20.3): infra engineers reflexively press `~` and type `help`. It must be a real, forgiving command line over the site's content graph — every destination reachable by command, nothing exclusive to it (§25: alternatives always exist). It is also connective tissue: docs 08/15 register commands here.

## 2. Deliverables

- `src/components/terminal/Terminal.tsx` (window + inline renderer), `CommandParser.ts`, `commands/` registry (one module per command), `useHistory.ts`.
- Global `~` keybinding registered in doc 04's `shortcuts.ts`; TERMINAL entry in Start menu SYSTEM TOOLS; terminal window id in the doc 04 registry (no dedicated route — the terminal opens *over* any page; its state is ephemeral by design, exempt from the URL-per-window rule as it is a tool, not content).

## 3. Technical design

### 3.1 Parser

Line → `tokenize` (quoted-string aware) → `resolve` (command registry lookup + alias map) → `execute(args, ctx)` where `ctx` exposes `router`, `windowStore`, `settingsStore`, content accessors (doc 06), and `print/printLines`. Commands are plain objects:

```ts
interface Command {
  name: string; aliases?: string[]; usage: string; summary: string
  complete?(partial: string): string[]      // tab completion
  execute(args: string[], ctx: TermCtx): TermOutput | Promise<TermOutput>
}
```

Unknown commands get a helpful failure: `UNKNOWN COMMAND 'balgs'. DID YOU MEAN 'blogs'?` (Levenshtein ≤ 2 suggestion). Errors never stack-trace; worst case `SEGMENTATION FAULT (JUST KIDDING). TYPE help.`

### 3.2 Command set (§20.3 verbatim + system verbs)

| Command | Behavior |
|---|---|
| `help [cmd]` | command table, or usage+summary for one; `help` output groups by NAVIGATION / FILES / SYSTEM / FUN |
| `whoami` | prints the §9.2 ID block |
| `ls [path]` | lists the virtual FS (below) |
| `cd <path>` | navigates: `cd experience` → `/experience`; `cd experience/deloitte` → case study (path parity with doc 08's tree) |
| `open <target>` / `run <target>` | opens window/tool: `open projects`, `run gpu_lab`, `run capacity_planner` |
| `cat <file>` | prints content: `cat blogs/latest.txt` → latest post summary + link; `cat incidents/inc-001` → incident summary |
| `inspect skills` | MEMORY.MAP band summary + link to `/skills` |
| `show metrics` | current PERFMON snapshot as text + link |
| `download resume` | triggers the doc 07 PDF download |
| `contact` | opens CONTACT.COM |
| `clear`, `exit` | clear buffer / close terminal |
| system extras | `theme green|amber|paper`, `display crt|clean`, `sound on|off` (settingsStore verbs); `history`; `uptime`; `defrag` + `overclock` + `benchmark` registered by doc 15 |

### 3.3 Virtual filesystem

A static manifest generated at build time from routes + Velite content:

```text
/                       → about.exe, experience/, projects/, gpu_lab.exe, blogs/, incidents/, resume.pdf, contact.com, skills.map, perfmon.exe
/experience             → deloitte, bank-of-america, 10x-analyst, humana, stryke, hex-lab
/projects               → the five .EXE entries
/blogs                  → post slugs (+ latest.txt symlink)
/incidents              → inc-001 … inc-006
```

Because it derives from real content, `ls blogs` is always current; `complete()` uses the same manifest for tab completion.

### 3.4 UX mechanics

Prompt `C:\ATHARVA> ` with block cursor (doc 01 token); ↑/↓ history (session-persisted); Tab completion; `Ctrl+L` clear; output is plain DOM text (selectable, screen-reader readable, `role="log"` + `aria-live="polite"` for new output). While terminal is focused, global shortcuts suspend (doc 04 rule). Mobile: terminal opens full-screen with a suggestion chip row (`help · ls · whoami · open projects`) above the keyboard since `~` doesn't exist on touch keyboards; launched from Start menu/desktop icon.

## 4. Creative direction

- Boot line on open: `ATHARVA.RUNTIME SHELL v2.6 — TYPE help TO BEGIN` — the version string matches FIELD_NOTES v2.6 (§16.2); the OS is one consistent build.
- Output prints instantly (real terminals aren't typewriters); only the MOTD types in.
- Flavor verbs kept to the fun group: `nvidia-smi` prints a period-perfect fake SMI table for the AC-90's "136 GB300 accelerators" (§7.3 callback); `sudo` → `ATHARVA IS ALREADY ROOT.`; `rm -rf /` → `PERMISSION DENIED. THIS MACHINE HAS SURVIVED WORSE.` Three jokes maximum in the registry at any time (§3: slightly playful, not a joke terminal).
- `help` is a designed artifact — aligned columns, grouped, ≤ 24 lines so it never scrolls off a small screen.

## 5. Dependencies

Doc 04 (shortcuts, window shell, focus rules), doc 06 (content manifest), docs 07–11/15 provide targets (commands land with their targets: `run capacity_planner` registers in Phase 3 alongside doc 11).

## 6. Acceptance criteria

- [ ] `~` opens the terminal over any page; Esc/`exit` closes and returns focus to the prior element.
- [ ] Every §20.3 command listed above works against real content (e.g. `cat blogs/latest.txt` reflects the actual newest post).
- [ ] Tab completion and ↑ history work; unknown commands produce a suggestion; no input can throw an unstyled error.
- [ ] `cd experience/deloitte` and clicking the doc 08 tree reach the identical URL (path parity test).
- [ ] Screen reader announces new output (`role="log"` verified with NVDA); terminal is fully usable with keyboard alone (trivially) *and* discoverable without it (Start menu, icon).
- [ ] Terminal chunk lazy-loads on first open, not in the shared bundle.
- [ ] Mobile: opens from Start menu, chip suggestions insert text, layout usable at 390px with software keyboard up.

## 7. Risks & fallbacks

| Risk | Fallback |
|---|---|
| Command registry drifts from routes/content | Virtual FS is generated from the same manifests routes use; a CI test walks every FS node and asserts the mapped route exists |
| `~` conflicts with browser/OS or non-US keyboard layouts | Also bind `Ctrl+\``; both listed in the doc 04 cheat sheet; Start-menu path always exists |
| aria-live flood on multi-line output | Batch output per command into one live-region update |
| Scope creep into a fantasy OS (pipes, scripting) | Registry is the contract: navigation + system + ≤ 3 fun verbs. Anything more requires editing this doc first (§38 test applies) |
