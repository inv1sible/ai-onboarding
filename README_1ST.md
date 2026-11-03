# AI-onboarding — a kit for making existing projects AI-ready

This repository is a **template kit**, not a code project. It holds everything
needed to bring an existing (often legacy) coding project into an AI-ready
state: a canonical agent instruction file, a thin Claude Code adapter, a
stubbed `docs/` tree, and a self-driving onboarding playbook.

There is deliberately **no `README.md`** in this repo: a target project's
`README.md` belongs to that project, and this kit never overwrites it. This
file (`README_1ST.md`) is the kit's own entry point instead.

## What's in the kit

| File | Role | Copied into the target project? |
|---|---|---|
| `AGENTS.md` | Canonical agent entry point, as a generic template with `STUB`s | Yes — copied verbatim, then filled by the agent |
| `CLAUDE.md` | Thin Claude Code adapter (`@AGENTS.md` import + tool notes) | Yes — copied verbatim, then filled |
| `docs/` | Stubbed deep docs (ARCHITECTURE, MEMORY, TESTING, TASKS, ROADMAP, CHANGELOG) | Yes — copied verbatim, then filled |
| `_AI_ONBOARDING.md` | The onboarding playbook (audit → write → verify) | Yes — copied verbatim, then **deleted by the agent** once onboarding is done |
| `README_1ST.md` (this file) | The kit's own entry point | **No** — lives only in this repo |

## Quickstart — onboarding a target project

1. Copy `AGENTS.md`, `CLAUDE.md`, and the `docs/` directory from this repo
   into the target project's root. **Never overwrite the target's existing
   `README.md`.**
2. Copy `_AI_ONBOARDING.md` into the target root, keeping its name.
3. Open the target project with your coding agent. The onboarding block at
   the top of the copied `AGENTS.md` makes the process self-driving: the
   agent reads `_AI_ONBOARDING.md`, audits the *real* repository state,
   fills every `STUB` with verified facts, and deletes `_AI_ONBOARDING.md`
   when done.
4. Commit the result. The target is now AI-ready: one canonical entry point,
   honest deep docs, and a per-branch handover loop (TASKS/CHANGELOG).

The full reasoning — why the docs are layered this way, the audit workflow,
the design principles, lessons learned, and a reusable checklist — is in
[_AI_ONBOARDING.md](_AI_ONBOARDING.md).

## Working in this repo (the kit itself)

- `AGENTS.md`, `CLAUDE.md`, and `docs/` are **unfilled templates**. They must
  stay generic so they can be copied as-is. Do not fill their stubs with
  kit-specific content — that would break the kit.
- The onboarding block in `AGENTS.md` contains a guard: if `README_1ST.md`
  exists at the repo root, the agent treats this repo as the kit and must
  not fill stubs or delete `_AI_ONBOARDING.md`.
- Keep the kit's invariants when changing templates: one canonical entry
  point with thin adapters; stubs carry their own research instructions;
  `_AI_ONBOARDING.md` §7 (template skeleton) matches `AGENTS.md` 1:1.
- References to `README.md` inside the templates point to the *target*
  project's README — that is why the kit has none of its own.
- The kit's own change history lives in git; `docs/CHANGELOG.md` is a
  template and must not receive kit-specific entries.

## Layout

```
AI-onboarding/
├── AGENTS.md              # template — canonical agent entry point (copied)
├── CLAUDE.md              # template — Claude Code adapter (copied)
├── README_1ST.md          # this file — the kit's own entry point (not copied)
├── _AI_ONBOARDING.md      # the playbook (copied, deleted after onboarding)
└── docs/                  # template deep docs (copied, then filled)
    ├── ARCHITECTURE.md
    ├── CHANGELOG.md
    ├── MEMORY.md
    ├── ROADMAP.md
    ├── TASKS.md
    └── TESTING.md
```
