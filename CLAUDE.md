# <project> — Claude Code

This project uses **`AGENTS.md`** as its canonical agent instruction file.
Read and follow it for all project context, commands, architecture, conventions,
and testing guidance:

@AGENTS.md

## Working Agreements

- Inspect repository state before editing; code, tests, Git state, and current decisions are authoritative.
- Prefer the smallest solution that satisfies the requirement.
- Preserve user work and avoid unrelated refactors.
- Define acceptance criteria for non-trivial tasks.
- Never claim completion without observed evidence.
- A retry must add evidence or change the plan.
- Treat edit, commit, push, deploy, production changes, and other external actions as separate authorization boundaries.
- Keep durable memory concise, factual, and free of transcript dumps.

For project-specific behavior, prefer the repository's `AGENTS.md` contract. Modern
Claude Code can read `AGENTS.md`; when a project also uses `CLAUDE.md`, use an
`@AGENTS.md` import (as at the top of this file) to keep one portable source of truth.

## Claude-specific notes

<!-- STUB: 3–6 tool-specific notes that do not belong in AGENTS.md: the
     project's user-facing language, the verified test command (if it differs
     from what the README claims), the single most important gotcha, and any
     Claude Code behavior worth pinning. If a fact belongs in AGENTS.md, add
     it there — this file stays a thin adapter. -->

- <user-facing language rule.>
- <verified test command, if it surprises.>
- <top gotcha, one line.>
