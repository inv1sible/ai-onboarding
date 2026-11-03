# MEMORY.md — Conventions, patterns & gotchas

> **STATUS: STUB — template.** This file captures "how we write code here".
> Research the repository and replace every `STUB` below with verified
> facts. Derive every rule from the existing code, not from preference.
> **Delete this banner once the file is filled in.**

## Research instructions (delete this section once filled in)

1. **Language & tone** — what language are user-facing strings, comments,
   and docs in? Is there a legacy language still present (queue its
   translation in `TASKS.md`)?
2. **Naming** — sample 10–20 identifiers per layer (files, functions,
   routes, templates, assets) and extract the *actual* convention.
3. **Layering patterns** — how does a handler talk to a service? What do
   services return (dicts with an ok/error envelope? exceptions? data
   classes)?
4. **Data-access rules** — which layer may touch storage directly? Are
   there helpers that must be used instead of raw access?
5. **External-system helpers** — is there a canonical helper for each
   external integration (SSH, HTTP APIs, brokers)? What is its contract?
6. **Background jobs** — how are tasks defined and scheduled?
7. **Frontend patterns** — how are pages structured, how is JS organized,
   how are API calls made?
8. **Config access** — how is config read, and is there a rule about when
   (e.g. inside functions, not at import time)?
9. **Gotchas** — the short "don't" list: mistakes that are easy to make and
   hard to find.
10. **Do / Don't** — the positive contract and the explicitly rejected
    anti-patterns.

## 1. Language & tone

STUB: doc/code/user-facing language; legacy-language status.

## 2. Naming

STUB: conventions per layer (files, identifiers, routes, docs).

## 3. Layering patterns

STUB: handler → service flow; return contracts.

## 4. Data-access rules

STUB: who may touch storage; required helpers.

## 5. External-system helpers

STUB: canonical helper per integration + its contract.

## 6. Background jobs

STUB: how tasks are defined and scheduled.

## 7. Frontend patterns

STUB: page structure, JS organization, API-call pattern.

## 8. Config access

STUB: how and when config is read.

## 9. Gotchas (short list)

STUB: the "don't" list.

## 10. Do / Don't

STUB: the contract in two columns.
