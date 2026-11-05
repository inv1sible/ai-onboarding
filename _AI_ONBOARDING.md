# AI Onboarding — Making a Legacy Project AI-Ready

A playbook for driving an existing coding project into an AI-ready state —
what to do, in what order, and the reasoning behind each step. Written to be
applied as-is to another project: no project-specific content.

**How to use it:** copy `AGENTS.md`, `docs/`, `.agents/skills/` (the
onboarding skill), and `.claude/agents/` (workflow agents) from this repo into
the target project (never overwrite its existing `README.md`), and copy this
file as `_AI_ONBOARDING.md`. The `AGENTS.md` template tells the agent to
follow this playbook on first run, fill in every stub, and then delete the
onboarding scaffolding. In pi and OpenCode, the `ai-onboarding` skill is the
native trigger for the same workflow; other harnesses that read `AGENTS.md`
(e.g. Claude Code) are driven by its onboarding block. The kit's own entry
point, `README_1ST.md`, lives only in this repo and is not copied.

---

## 1. The problem

AI coding agents fail on legacy projects for predictable reasons:

1. **No entry point.** The agent must discover conventions, commands, and
   architecture by reading dozens of files — and guesses wrong.
2. **Undocumented critical facts.** The things that bite (non-obvious storage
   layouts, config quirks, "the README is outdated") live only in people's
   heads. An agent that misses them produces plausible-but-wrong code.
3. **No feedback loop.** Without tests or a verified command, the agent cannot
   check its own work and claims success without evidence.
4. **Stale or missing docs.** Old docs actively mislead; agents (and humans)
   trust them.

The goal is not "more documentation". The goal is a **small, layered,
verifiable documentation system** that an agent can load in one context window
and that stays honest about what is true *right now*.

---

## 2. The documentation system

### 2.1 File layout & responsibilities

| File | Role | Audience | Size target |
|------|------|----------|-------------|
| `AGENTS.md` (repo root) | **Canonical agent entry point.** What the project is, critical facts, verified commands, structure, conventions, do/don't. | All AI agents (Copilot, Claude Code, …) + humans | ≤ ~250 lines |
| `.agents/skills/ai-onboarding/SKILL.md` | **Onboarding skill** — thin native trigger for pi and OpenCode; points to this playbook. Transient scaffolding, deleted at finalize. | pi / OpenCode | ≤ ~60 lines |
| `.claude/agents/` | **Workflow agents** — `analyst`, `reviewer-clean`, `reviewer-issue` for projects with an issue workflow. Persistent — not deleted at finalize. | Claude Code (native; OpenCode: port to `.opencode/agents/`) | one file per agent |
| `docs/ARCHITECTURE.md` | Full system map: topology, storage, request flow, services, integrations, config, models. | Agent deep-dives, new humans | as long as needed |
| `docs/MEMORY.md` | Conventions, naming, patterns, gotchas, do/don't — the "how we do things here" knowledge. | Agent deep-dives | as long as needed |
| `docs/TESTING.md` | The *real* test command, where tests live, what must be mocked, e2e roadmap. | Agent + CI | as long as needed |
| `docs/TASKS.md` | **Branch-scoped handover doc:** what this branch does, what's done, what carries over. | Next agent/human picking up the branch | small |
| `docs/ROADMAP.md` | General ideas & epics that outgrow any single branch. | Planning | small |
| `docs/CHANGELOG.md` | Notable changes per branch/release. | History | append-only |

Key decisions:

- **One canonical source of truth** (`AGENTS.md`). The primary harnesses
  (pi, OpenCode, Claude Code ≥ 2.1.277) all read `AGENTS.md` natively, so the
  kit ships **no adapter instruction files**. If a harness you use cannot read
  `AGENTS.md`, add a thin adapter for that harness only (e.g. `CLAUDE.md` with
  an `@AGENTS.md` import) — never a parallel source of truth.
- **UPPER_CASE `.md` filenames** for all project docs (convention recorded in
  `AGENTS.md` §6) so docs are visually distinct from code.
- **Docs live in `docs/`; the root keeps only the entry points**
  (`AGENTS.md`, `README.md`) and the agent-asset directories (`.agents/`,
  `.claude/agents/`). Easy to find, easy to ignore.
- **Align the README with reality and with the project's target language.**
  Agents read READMEs; a README that contradicts the repo or is written in a
  different language than the project targets is a constant source of
  confusion. (Legacy-language *code strings* are a separate, queued task —
  see `docs/TASKS.md`.)

### 2.2 What goes where (the layering rule)

```
AGENTS.md          → "what you MUST know before touching anything"
                     (critical facts, commands, structure, do/don't)
.agents/skills/    → "reusable procedures, loaded on demand" (SKILL.md per skill)
docs/ARCHITECTURE  → "how the system actually works" (deep reference)
docs/MEMORY        → "how we write code here" (conventions & gotchas)
docs/TESTING       → "how to prove it works"
docs/TASKS         → "what is happening on this branch right now"
docs/ROADMAP       → "what comes after"
docs/CHANGELOG     → "what changed and when"
```

`AGENTS.md` links to the deep docs instead of duplicating them. Each doc has
exactly one job; if a fact belongs in two places, one of them is a link.

---

## 3. The workflow (in order)

### Step 1 — Read-only audit of the real state

Before writing a single doc line, inspect the repository as it *is*, not as
docs claim it is:

- `git log`, `git status`, branch state
- entry points: package manifest(s), main config/settings, routing or entry
  file(s), CI config, `Dockerfile`/compose, lockfiles, `README`
- the actual source layout (services/handlers/templates or equivalent)
- runtime artifacts present in the tree (databases, state dirs, build output, …)

**This step found the discrepancies that shaped the docs:**

| Discrepancy found | Where it landed |
|---|---|
| README documents a test command that does not exist; CI uses a different one | `AGENTS.md` §2, `docs/TESTING.md` §1 |
| Compose file defines a service the app never actually uses (default config differs) | `AGENTS.md` §2 |
| A class of data lives outside the primary database — invisible from any single file | `AGENTS.md` §2, `docs/ARCHITECTURE.md` |
| A file the project needs (e.g. an initial migration) was missing from the repo | restored the file; noted in `docs/CHANGELOG.md` |
| A checked-in config file holds a real secret | `AGENTS.md` §8 (don't add real secrets) |

**Lesson:** the highest-value content in the whole system is the *critical
facts* section — and it can only be written from evidence, not from existing
docs.

### Step 2 — Write `AGENTS.md` (the entry point)

Structure used (numbered, stable — agents reference these sections; the
stubbed template in §7 maps 1:1 to them):

1. **What is the project** — one paragraph, domains, stack, language.
2. **Critical facts (read before writing any code)** — the 5–8 things that
   will most likely bite an agent. Numbered. Each one is a *verified* fact.
3. **Commands** — run, test, migrate. Only commands that were actually
   verified (CI file is the source of truth for the test command).
4. **Project structure** — annotated tree, one line per directory.
5. **Architecture essentials** — short, with links into `docs/ARCHITECTURE.md`.
6. **Code conventions & patterns** — naming, config access, where new
   features land (an ordered landing list).
7. **Testing** — real command, what to mock, current state ("no tests yet").
8. **Gotchas & pitfalls** — the "don't" list.
9. **Do / Don't** — the contract in two columns.
10. **Further reading** — links to the `docs/` tree.

Also included at the top: **general agentic development principles**
(think before coding, clarify before implementing, keep solutions simple,
surgical changes, evidence before DONE, retry discipline, security/authority
boundaries, durable knowledge). These are project-agnostic and transfer
directly into any onboarding skill.

### Step 3 — Write the deep docs

- `docs/ARCHITECTURE.md` — full map: runtime topology, storage layout,
  request flow, layering patterns, external integrations, background jobs,
  frontend, config, data models. Mark anything explicitly stubbed.
- `docs/MEMORY.md` — conventions: language & tone, naming, layering
  patterns, data-access rules, external-system helper contracts, background
  jobs, frontend patterns, config access, the short gotcha list, do/don't.
- `docs/TESTING.md` — Part A: the real test command, where tests live,
  **what must be mocked** (external services that don't exist in CI),
  checklist for adding a test. Part B: any planned end-to-end test rig.
- `docs/TASKS.md` — the handover doc (see Step 5).
- `docs/ROADMAP.md` — epics, migrated from any legacy TODO file (which was
  then deleted — one source of truth).
- `docs/CHANGELOG.md` — starts with this branch; earlier history not
  backfilled (honest about its own scope).

In every file you fill (including `AGENTS.md`), filled stubs
leave no trace: the `STUB:` placeholder lines, the `STATUS: STUB` banners,
and the "Research instructions" sections are scaffolding — delete them once
the real content is in place.

### Step 4 — Tool adapters (only if a harness requires one)

The kit ships no adapter files. Add one only for a harness that cannot read
`AGENTS.md` (e.g. `CLAUDE.md` for an older Claude Code):

```markdown
@AGENTS.md            ← import the canonical file
+ a few harness-specific notes (language, real test command, top gotcha)
```

Nothing else. If a fact is missing, it gets added to `AGENTS.md`, not to the
adapter.

### Step 5 — Establish the handover & maintenance loop

- `docs/TASKS.md` is **branch-scoped**: done tasks may be deleted on merge;
  open tasks carry over to the next branch. This file is how one agent
  (or human) hands work to the next without a transcript dump.
- `docs/CHANGELOG.md` gets an entry per branch (Added/Changed/Fixed/Removed/
  Moved).
- `docs/ROADMAP.md` absorbs anything that outgrows the current branch.
- The rule: **repository state is authoritative; docs and handoffs are
  context, not truth.** When they disagree, fix the doc.

### Step 6 — Verify & finalize

Before committing:

- every "done" task re-checked against the actual repo state
- stale path references grepped out (see Lesson 1 below)
- template traces removed: `grep -rn -e 'STATUS: STUB' -e '<!-- STUB'
  -e 'STUB:' -e '^- STUB' AGENTS.md docs/` returns nothing, and a
  final read of each file shows no leftover placeholders (`<project>`,
  `<fact>`, stub cells in tables, …)
- onboarding scaffolding removed in the same commit (`_AI_ONBOARDING.md`
  and `.agents/skills/ai-onboarding/`)
- secret scan over all new docs
- rename fidelity checked (`git diff` shows `R100` for pure moves)
- commit + push, MR opened

---

## 4. Design principles (the transferable part)

1. **One canonical entry point, adapters only when required.** `AGENTS.md`
   is the source of truth; pi, OpenCode, and Claude Code read it natively, so
   no adapter files are shipped. A harness that cannot read `AGENTS.md` gets
   a thin import (e.g. `CLAUDE.md` with `@AGENTS.md`), never a parallel
   source of truth.
2. **Critical facts first.** The single highest-value section is a short,
   numbered list of verified "things that will bite you". Write it from
   evidence (code, CI, git history), not from old docs.
3. **AGENTS.md is a map, not a manual.** ≤ ~250 lines. Deep content lives in
   `docs/` and is linked. The entry point must fit in one context window.
4. **Every doc has one job.** Architecture ≠ conventions ≠ testing ≠ tasks ≠
   roadmap ≠ changelog. If a fact appears in two docs, one becomes a link.
5. **Commands must be verified.** Only document commands that were actually
   run or are provably what CI runs. An outdated command is worse than none.
6. **Be honest about state.** "No tests exist yet", "<subsystem> is a stub",
   "this changelog starts with branch X" — agents plan better around stated
   reality than around implied completeness.
7. **The handover doc is per-branch.** `TASKS.md` answers "what is happening
   right now and what's left". It is the anti-transcript: short, structured,
   deletable on merge.
8. **Docs are code.** They get reviewed, committed, and fixed in the same MR
   as the work they describe. Stale docs are a defect.
9. **Language consistency.** Pick one language for docs and new user-facing
   strings; translate legacy text incrementally (queue it in TASKS/ROADMAP,
   don't boil the ocean).
10. **Security boundary in the docs.** Explicitly state where secrets live
    and the rule: never add new real secrets to the repo.

---

## 5. Lessons learned during onboarding

1. **Stale references survive consolidation.** `docs/CHANGELOG.md` still
   referenced an intermediate `docs/ai/` directory after the docs were moved
   to `docs/`. Caught only by a final grep for old paths. → *Always grep for
   old paths after a reorganization; include it in the finalize checklist.*
2. **"Done" needs evidence.** Every task marked done in `TASKS.md` was
   re-verified against the repo before the commit (rename fidelity via
   `git diff`, migration content vs. git history, secret scan). → *The
   finalize step is a verification step, not a formality.*
3. **The repo can be missing its own history.** A file the project needs
   (e.g. an initial migration) had been deleted in an old commit and the app
   ran on a pre-existing database, so nobody noticed. Restoring it was a
   one-file fix that made fresh environments (Docker, CI) actually work. →
   *During onboarding, check that a clean clone can bootstrap: migrations,
   sample config, install steps.*
4. **READMEs lie first.** The README's test command pointed to a module that
   doesn't exist. The CI file was the truth. → *Treat CI config as the
   authority for commands; translate/fix the README in the same branch.*
5. **Non-obvious storage is the archetype of a critical fact.** "A class of
   data lives outside the primary database" is invisible from any single file
   — you only see it by reading the services. Facts like this are exactly
   what belongs in `AGENTS.md` §2, because an agent that misses them will
   write subtly broken queries.
6. **Keep the entry point boring.** No marketing, no history, no TODOs in
   `AGENTS.md`. It is a reference document. History goes to CHANGELOG, TODOs
   to TASKS/ROADMAP.

---

## 6. Reusable onboarding checklist (for other projects)

Use this to transform an existing project. Estimated effort: half a day to a
day for a medium project, most of it in steps 1–2.

### Phase A — Audit (read-only, no edits)
- [ ] `git log --oneline -30`, `git status`, branches, CI config
- [ ] Read entry points: package manifest(s), settings/config, main server
      file, CI file, Dockerfile/compose, README
- [ ] Map the real structure (tree of source dirs, one line each)
- [ ] Identify the 5–8 **critical facts** (non-obvious storage, config
      quirks, outdated docs, secret locations, "X is a stub")
- [ ] Verify the real run/build/test commands (run them or prove them from CI)
- [ ] Check clean-clone bootstrap: migrations/lockfiles, sample config,
      install steps

### Phase B — Write the system
- [ ] Copy the templates from this repo (`AGENTS.md`, `docs/`,
      `.agents/skills/` — the onboarding skill, `.claude/agents/` — workflow
      agents, and this file as `_AI_ONBOARDING.md`) as the starting point —
      never overwrite the target project's existing `README.md` (the kit's
      own entry point, `README_1ST.md`, is not copied)
- [ ] `AGENTS.md` at repo root using the §3/Step-2 structure (10 sections)
- [ ] `docs/ARCHITECTURE.md` — system map with short code snippets
- [ ] `docs/MEMORY.md` — conventions, naming, patterns, gotchas, do/don't
- [ ] `docs/TESTING.md` — real test command, what to mock, how to add a test
- [ ] `docs/TASKS.md` — current branch scope, done/open tables
- [ ] `docs/ROADMAP.md` — epics & backlog (migrate any legacy TODO file,
      then delete the legacy file)
- [ ] `docs/CHANGELOG.md` — starts with the onboarding branch
- [ ] Tool adapter(s): **none by default** (pi, OpenCode, and Claude Code
      read `AGENTS.md` natively); add a thin import only for a harness that
      cannot read it
- [ ] Translate/fix README if it contradicts reality or uses another language

### Phase C — Verify & finalize
- [ ] Grep for stale paths/references after any reorganization
- [ ] Grep for template traces — `grep -rn -e 'STATUS: STUB' -e '<!-- STUB'
      -e 'STUB:' -e '^- STUB' AGENTS.md docs/` returns nothing;
      no leftover placeholders in any filled file
- [ ] Secret scan over all new docs
- [ ] Re-verify every "done" task against repo state
- [ ] Commit docs in their own branch/MR (docs-only = low-risk review)
- [ ] Record the onboarding itself in `docs/CHANGELOG.md`
- [ ] **Delete the onboarding scaffolding: `_AI_ONBOARDING.md` and
      `.agents/skills/ai-onboarding/`** — the onboarding is done; the doc
      system is now self-sufficient

### Ongoing maintenance (the loop that keeps it working)
- [ ] Every branch: update `docs/TASKS.md` (done → delete, open → carry over)
- [ ] Every branch: `docs/CHANGELOG.md` entry
- [ ] Every branch that changes architecture/conventions: update
      `ARCHITECTURE.md` / `MEMORY.md` in the same MR
- [ ] When a doc and the repo disagree: fix the doc, same MR
- [ ] Keep `AGENTS.md` ≤ ~250 lines; push overflow into `docs/`

---

## 7. Template skeleton for `AGENTS.md`

The `AGENTS.md` in this repo is this skeleton with the stubs in place —
copy it into the target project and fill in each stub. The onboarding block
at the top is what makes the kit self-driving: it tells the agent to follow
`_AI_ONBOARDING.md`, fill the stubs, and then delete the onboarding
scaffolding.

```markdown
# AGENTS.md — <project>

Guidance for AI coding agents (and humans). Read this file first.
Deeper references live in `docs/`: ARCHITECTURE, MEMORY, TESTING, TASKS,
ROADMAP, CHANGELOG.

## Onboarding (first run only)
(if _AI_ONBOARDING.md exists: follow it, fill every stub it refers to,
record the onboarding in docs/CHANGELOG.md, then delete the onboarding
scaffolding: _AI_ONBOARDING.md and .agents/skills/ai-onboarding/)

## General agentic development principles
(think before coding / clarify before implementing / keep it simple /
surgical changes / verifiable goals / evidence before DONE / retry
discipline / security boundaries / durable knowledge)

## 1. What is <project>?
(STUB: one paragraph — purpose, domains, stack, language)

## 2. Critical facts (read before writing any code)
(STUB: 5–8 numbered, verified items — the things that will bite)

## 3. Commands
(STUB: run / build / test — verified only)

## 4. Project structure
(STUB: annotated tree, one line per entry)

## 5. Architecture essentials
(STUB: 3–7 bullets; link to docs/ARCHITECTURE.md)

## 6. Code conventions & patterns
(STUB: naming, config access, where new features land — step list)

## 7. Testing
(STUB: real command, what to mock, current state)

## 8. Gotchas & pitfalls
(STUB: the "don't" list, secrets, generated artifacts)

## 9. Do / Don't
(STUB: the contract in two columns)

## 10. Further reading
(links into docs/)
```

---

## 8. What NOT to do

- Don't dump the whole codebase summary into `AGENTS.md` — it becomes
  unreadable by humans and stale within a week.
- Don't create per-tool instruction files — not with divergent content, and
  not at all for harnesses that read `AGENTS.md` natively (pi, OpenCode,
  Claude Code).
- Don't duplicate skills into per-harness directories (`.claude/skills/`,
  `.pi/skills/`, …) — the canonical skills location is `.agents/skills/`.
- Don't document unverified commands "from memory".
- Don't backfill history you don't have (changelog starts now, honestly).
- Don't use chat transcripts as project memory — distill into TASKS/CHANGELOG.
- Don't skip the audit: docs written from old docs inherit their lies.
