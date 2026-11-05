# AGENTS.md — <project>

Guidance for AI coding agents (and humans) working in this repository.
Read this file first. It is the single source of truth for how this project
works, how to run it, and how to write code that fits the existing patterns.

Deeper references live in [`docs/`](docs/):
- [Architecture](docs/ARCHITECTURE.md) — full map of the system
- [Memory — conventions & gotchas](docs/MEMORY.md) — patterns, naming, pitfalls, do/don't
- [Testing](docs/TESTING.md) — how to run and add tests

## Onboarding (first run only)

If `_AI_ONBOARDING.md` exists at the repo root, this project has not been
onboarded yet. Before any other work:

1. **Guard — is this the template kit itself?** If `README_1ST.md` exists at
   the repo root, this repository is the AI-onboarding kit, not an onboarding
   target. Its `AGENTS.md`, `docs/`, and `_AI_ONBOARDING.md` are generic
   templates and the playbook, meant to be *copied* into other
   projects — not filled in here. Do **not** fill the stubs with kit-specific
   content and do **not** delete `_AI_ONBOARDING.md`; it is the kit's core
   asset. Skip the steps below and work on the kit itself.
2. Read `_AI_ONBOARDING.md` in full — it is the onboarding playbook.
3. Follow it: audit the repository, then fill in every `STUB` in this file
   and in the `docs/` files it refers to, using the research instructions
   inside each stub.
4. **Delete the template traces as you fill each stub.** The stub's HTML
   comment, the placeholder lines, the status banner at the top of each
   `docs/` file, and the "Research instructions" sections are scaffolding:
   once the real content is in place, they are deleted — including the
   `<project>` placeholders in the headings. A finished doc must read as
   if written from scratch.
5. When every stub is filled and verified and all template traces are
   removed (the playbook's finalize step includes the final check),
   record the onboarding in `docs/CHANGELOG.md`, then delete the onboarding
   scaffolding: `_AI_ONBOARDING.md` and, if present,
   `.agents/skills/ai-onboarding/` (the onboarding skill).

Do not delete `_AI_ONBOARDING.md` before the stubs are filled — and never in
the kit's own repo (see the guard above).

---

## General Agentic Development Principles

### Think before coding
- Inspect current repository state, relevant code, tests, Git status, and existing decisions before changing anything.
- State important assumptions and surface ambiguity when it materially changes the result.
- Repository state is authoritative; memory and handoffs are context, not truth.

### Clarify before implementing
- When a request is ambiguous, underspecified, or has more than one reasonable
  interpretation, **ask the user before implementing** — do not guess and proceed.
- Prefer a short, concrete question (with the options you see) over silently
  picking one path. Only proceed on your own when the intent is unambiguous.
- Never "talk to yourself" through a decision that the user should make.

### Keep solutions simple
- Implement the smallest solution that satisfies the requested behavior.
- Avoid speculative features, abstractions, dependencies, and unrelated refactoring.
- Prefer existing repository conventions and mechanisms.

### Make surgical changes
- Touch only what the task requires.
- Preserve existing user changes.
- Respect explicit non-goals and intentionally rejected behavior.

### Work toward verifiable goals
- For non-trivial work, identify the outcome and acceptance criteria before editing.
- Run the smallest relevant check first, then broader required checks.
- Read command output; do not infer success without evidence.

### Evidence before DONE
- Never claim completion without observed evidence.
- Match the claim to the evidence: changed != tested != built != artifact-verified != deployed != live/device-verified.
- Report checks performed, skipped verification, failures, and remaining risk honestly.

### Failure and retry discipline
- Do not repeat an unchanged failed action blindly.
- Record the observation, change the hypothesis or method, then retry only when the retry can add evidence.
- Bound retries and stop with a concrete blocker when the limit is reached.

**A retry must add evidence or change the plan.**

### Security and authority boundaries
- Investigation does not imply permission to modify.
- Local editing does not imply permission to commit, push, deploy, publish, alter production, or perform external actions.
- Never expose secrets, credentials, personal data, or sensitive content in code, logs, reports, handoffs, or durable memory.
- Avoid destructive Git/filesystem operations unless explicitly required and authorized.

### Durable knowledge
- Persist only non-obvious decisions, recurring gotchas, verified commands, and genuinely current state.
- Keep one clear source of truth for current state.
- Do not use transcript dumps as project memory.
- Mark obsolete plans/runbooks clearly as historical.

---

## 1. What is <project>?

<!-- STUB: one paragraph. Purpose of the project, the domains/features it
     covers, the tech stack (language, framework, key libraries), where it
     runs, and which external systems it integrates with. State the
     project's user-facing language and any legacy-language migration still
     in progress. -->

---

## 2. Critical facts (read before writing any code)

<!-- STUB: 5–8 numbered, verified facts that will most likely bite an agent:
     non-obvious storage layouts, config quirks, "the README is outdated",
     subsystems that are stubs, missing tests, where secrets live. Verify
     every item against the repo (code, CI config, git history) — never copy
     it from existing docs. -->

1. **<fact>.** <what it is, why it matters, what to do instead.>
2. **<fact>.** <...>

---

## 3. Commands

<!-- STUB: only commands that were actually verified — run them, or prove
     them from the CI config. An outdated command is worse than no command. -->

### Run
```bash
# <verified run command>
```

### Build (if applicable)
```bash
# <verified build command>
```

### Test (CI-verified)
```bash
# <verified test command>
```

See [Testing](docs/TESTING.md) for how to add tests and what to mock.

---

## 4. Project structure

<!-- STUB: annotated tree of the repo, one line per entry an agent needs to
     know. Mark stubs, generated artifacts, and untracked runtime state. -->

```
<project>/
├── AGENTS.md                  # ← you are here (agent entry point)
├── .agents/skills/            # shared skills (canonical location; native in pi + OpenCode)
├── .claude/agents/            # Claude Code subagents, if the issue-workflow trio is used
├── docs/                      # project docs (UPPER_CASE .md files)
│   ├── ARCHITECTURE.md        #   full system map
│   ├── MEMORY.md              #   conventions, patterns, gotchas
│   ├── TESTING.md             #   how to run & add tests
│   ├── TASKS.md               #   current branch tasks (handover)
│   ├── ROADMAP.md             #   general ideas & epics
│   └── CHANGELOG.md           #   notable changes per branch/release
├── <source dir>/              # <one line: what lives here>
├── <config file>              # <one line: what it configures>
└── <CI file>                  # <one line: what it runs, where>
```

---

## 5. Architecture essentials

(Deep version in [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md).)

<!-- STUB: 3–7 bullets on how the system works at a glance: request flow,
     layering (e.g. routes/handlers/services/models), background jobs,
     external integrations, and anything explicitly stubbed. One line each;
     deep detail belongs in docs/ARCHITECTURE.md. -->

- **<layer or pattern>.** <one line.>

---

## 6. Code conventions & patterns

(Full detail in [docs/MEMORY.md](docs/MEMORY.md).)

<!-- STUB: naming rules, how config is read, how to talk to external
     systems, and the ordered list of places a new feature must touch (keep
     them in sync). -->

- **Naming**: <rules for code identifiers and docs.>
- **Config access**: <how and where config is read.>
- **New feature landing spots** (keep these in sync):
  1. <step>
  2. <step>

---

## 7. Testing

<!-- STUB: the real test command, where tests live, the current state (be
     honest — "no tests exist yet" if true), what must be mocked (external
     services that don't exist in CI), and any planned end-to-end approach. -->

- Real command: `<verified test command>`.
- Tests live in: <location, naming convention>.
- Current state: <honest statement>.
- Must be mocked in tests: <external deps>.
- Full guide: [docs/TESTING.md](docs/TESTING.md).

---

## 8. Gotchas & pitfalls

<!-- STUB: the concrete "don't" list — mistakes agents make in this repo.
     Include where secrets live (and the rule: never add new real secrets to
     the repo) and which files are generated runtime state that must not be
     clobbered or committed. -->

- **Don't** <pitfall>.
- <secret / generated-artifact note.>

---

## 9. Do / Don't

<!-- STUB: the contract in two columns. Do: the patterns to follow.
     Don't: the anti-patterns and explicitly rejected behavior. -->

**Do**
- <pattern to follow.>

**Don't**
- <anti-pattern.>

---

## 10. Further reading

- [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) — full system map.
- [docs/MEMORY.md](docs/MEMORY.md) — patterns, naming, gotchas, do/don't.
- [docs/TESTING.md](docs/TESTING.md) — running & writing tests, what to mock.
- [README.md](README.md) — project overview.
- [docs/ROADMAP.md](docs/ROADMAP.md) — general ideas & epics.
- [docs/CHANGELOG.md](docs/CHANGELOG.md) — notable changes per branch/release.
- [docs/TASKS.md](docs/TASKS.md) — current branch tasks (handover).
