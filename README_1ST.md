# AI-onboarding — a kit for making existing projects AI-ready

This repository is a **template kit**, not a code project. It holds everything
needed to bring an existing (often legacy) coding project into an AI-ready
state: a canonical agent instruction file, a shared skills location, a
stubbed `docs/` tree, and a self-driving onboarding playbook.

There is deliberately **no `README.md`** in this repo: a target project's
`README.md` belongs to that project, and this kit never overwrites it. This
file (`README_1ST.md`) is the kit's own entry point instead.

## What's in the kit

| File | Role | Copied into the target project? |
|---|---|---|
| `AGENTS.md` | Canonical agent entry point, as a generic template with `STUB`s | Yes — copied verbatim, then filled by the agent |
| `docs/` | Stubbed deep docs (ARCHITECTURE, MEMORY, TESTING, TASKS, ROADMAP, CHANGELOG) | Yes — copied verbatim, then filled |
| `_AI_ONBOARDING.md` | The onboarding playbook (audit → write → verify) | Yes — copied verbatim, then **deleted by the agent** once onboarding is done |
| `.agents/skills/ai-onboarding/` | Onboarding skill — thin native trigger for pi + OpenCode (other harnesses via the `AGENTS.md` onboarding block), points at the playbook | Yes — copied verbatim, then **deleted by the agent** with the playbook |
| `.claude/agents/` | Workflow agents — `analyst`, `reviewer-clean`, `reviewer-issue` for projects with an issue workflow. Persistent. | Yes — copied verbatim, **persistent** (Claude Code-native; OpenCode does not discover `.claude/agents` — port to `.opencode/agents/` if needed) |
| `README_1ST.md` (this file) | The kit's own entry point | **No** — lives only in this repo |

## Quickstart — onboarding a target project

1. Copy `AGENTS.md`, the `docs/` directory, `.agents/` (skills), and
   `.claude/agents/` (workflow agents) from this repo into the target
   project's root. **Never overwrite the target's existing `README.md`.**
2. Copy `_AI_ONBOARDING.md` into the target root, keeping its name.
3. Open the target project with your coding agent. The onboarding block at
   the top of the copied `AGENTS.md` — or the `ai-onboarding` skill in
   pi and OpenCode — makes the process self-driving: the agent
   reads `_AI_ONBOARDING.md`, audits the *real* repository state, fills
   every `STUB` with verified facts, and deletes the onboarding scaffolding
   (playbook + skill) when done.
4. Commit the result. The target is now AI-ready: one canonical entry point,
   honest deep docs, and a per-branch handover loop (TASKS/CHANGELOG). In
   projects with an issue workflow, the `.claude/agents/` trio provides the
   analyze → review loop (`analyst`, `reviewer-clean`, `reviewer-issue`).

The full reasoning — why the docs are layered this way, the audit workflow,
the design principles, lessons learned, and a reusable checklist — is in
[_AI_ONBOARDING.md](_AI_ONBOARDING.md).

## Harness support

Verified November 2026 against the tools the kit targets:

| Harness | Reads `AGENTS.md` | Discovers `.agents/skills/` |
|---|---|---|
| pi (0.78.1) | native — global + project | native — global + project |
| OpenCode (V2) | native — the only instruction file it reads | native — project |
| Claude Code (≥ 2.1.277) | native | no — it discovers `.claude/skills/` instead |

Consequences for the kit:

- **No adapter files are shipped.** All three harnesses read `AGENTS.md`;
  the onboarding block in it drives the self-driving workflow regardless of
  harness.
- **Project skills in `.agents/skills/` are invisible to Claude Code.** If a
  project needs skills under Claude Code too, that is a project decision —
  the kit never creates per-harness skill copies or symlinks.
- **The `.claude/agents/` trio is Claude Code-only** (agent frontmatter is
  not portable across harnesses). For OpenCode, port to
  `.opencode/agents/` if the issue workflow is used there.

## Working in this repo (the kit itself)

- `AGENTS.md` and `docs/` are **unfilled templates**. They must stay
  generic so they can be copied as-is. Do not fill their stubs with
  kit-specific content — that would break the kit.
- The onboarding block in `AGENTS.md` contains a guard: if `README_1ST.md`
  exists at the repo root, the agent treats this repo as the kit and must
  not fill stubs or delete `_AI_ONBOARDING.md`.
- `.agents/skills/ai-onboarding/SKILL.md` is a **thin trigger**: it carries
  the guard and points at the playbook. Never duplicate playbook workflow
  into it — the playbook wins on any disagreement.
- `.claude/agents/` holds the issue-workflow agents (`analyst`,
  `reviewer-clean`, `reviewer-issue`). They are workflow content, not kit
  templates: their references to the issue workflow (glab, `docs/issues/`,
  the collector issue, …) are intentional — do not "fix" them without the
  owner.
- Keep the kit's invariants when changing templates: one canonical entry
  point (`AGENTS.md`; pi, OpenCode, and Claude Code read it natively — no
  adapter files shipped); skills live in `.agents/skills/`; stubs carry
  their own research instructions; `_AI_ONBOARDING.md` §7 (template
  skeleton) matches `AGENTS.md` 1:1.
- References to `README.md` inside the templates point to the *target*
  project's README — that is why the kit has none of its own.
- The kit's own change history lives in git; `docs/CHANGELOG.md` is a
  template and must not receive kit-specific entries.

## Layout

```
AI-onboarding/
├── AGENTS.md              # template — canonical agent entry point (copied)
├── README_1ST.md          # this file — the kit's own entry point (not copied)
├── _AI_ONBOARDING.md      # the playbook (copied, deleted after onboarding)
├── .agents/
│   └── skills/
│       └── ai-onboarding/
│           └── SKILL.md       # onboarding skill (copied, deleted after onboarding)
├── .claude/
│   └── agents/
│       ├── analyst.md         # agent: analyst (copied, persistent; Claude Code)
│       ├── reviewer.md        # agent: reviewer-clean (copied, persistent; Claude Code)
│       └── reviewer-issue.md  # agent: reviewer-issue (copied, persistent; Claude Code)
└── docs/                  # template deep docs (copied, then filled)
    ├── ARCHITECTURE.md
    ├── CHANGELOG.md
    ├── MEMORY.md
    ├── ROADMAP.md
    ├── TASKS.md
    └── TESTING.md
```
