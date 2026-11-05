---
name: ai-onboarding
description: "First-time onboarding of a repository for AI coding agents. Use when `_AI_ONBOARDING.md` exists at the repo root (the project has not been onboarded yet). Audits the real repo state, fills every stub in AGENTS.md and docs/ with verified facts, removes all template traces, then removes the onboarding scaffolding. Do not use in the template kit itself (marked by README_1ST.md)."
---

# AI Onboarding

You are driving an existing (often legacy) project into an AI-ready state.

## Guard — do not onboard the template kit

If `README_1ST.md` exists at the repo root, this is the AI-onboarding kit
itself, not an onboarding target. **Stop.** Do not fill any stub and do not
delete `_AI_ONBOARDING.md`.

## Workflow

The single source of truth is `_AI_ONBOARDING.md` at the repo root. Read it
in full and follow it exactly — this skill is a thin trigger; if it and the
playbook disagree, the playbook wins.

1. **Audit (read-only).** Git state, CI config, entry points, config files,
   runtime artifacts. Derive the 5–8 critical facts from evidence, never
   from existing docs.
2. **Fill.** Every stub in `AGENTS.md` and `docs/` with
   verified facts. Commands must be actually verified — CI config is the
   authority for the test command.
3. **Clean up template traces as you fill.** Delete the stub comments,
   `STUB:` placeholder lines, `STATUS: STUB` banners, and "Research
   instructions" sections. A finished doc must read as if written from
   scratch.
4. **Finalize.**
   - `grep -rn -e 'STATUS: STUB' -e '<!-- STUB' -e 'STUB:' -e '^- STUB'
     -e '| STUB' -e 'Onboarding (first run only)' AGENTS.md docs/`
     returns nothing.
   - Stale path references grepped out; secret scan over all new docs;
     every "done" task re-verified against repo state.
   - Record the onboarding in `docs/CHANGELOG.md`.
   - Only then delete the onboarding scaffolding: `_AI_ONBOARDING.md`,
     this skill (`.agents/skills/ai-onboarding/`), and the "Onboarding
     (first run only)" section of `AGENTS.md`, in the same commit.

The result is a small, layered, honest doc system: `AGENTS.md` as the single
canonical entry point, deep docs in `docs/`, and a per-branch handover loop
(`TASKS.md` / `CHANGELOG.md`).
