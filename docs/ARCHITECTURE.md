# ARCHITECTURE.md — Full system map

> **STATUS: STUB — template.** Research the repository and replace every
> `STUB` below with verified facts. Repository state is authoritative —
> never copy from old docs or the README.

## Research instructions (delete this section once filled in)

Before writing, investigate and verify:

1. **Runtime topology** — read the Dockerfile/compose, CI config, and entry
   points. Which processes/containers run, on which ports, in what order?
2. **Storage layout** — find *every* place data is persisted: primary
   database, secondary files, caches, state dirs. Pay special attention to
   data that lives *outside* the primary database — that is the most common
   critical fact.
3. **Request flow** — trace one representative request from entry point to
   response: routing → handler → service → storage/external call.
4. **Layering** — which directories hold which layer (routes/handlers,
   services/business logic, models, frontend)? What are the contracts
   between layers (return types, envelopes, error handling)?
5. **External integrations** — every API, SSH target, message broker, or
   third-party service the code talks to, and how (helper module, SDK).
6. **Background jobs** — task queue, schedules, what runs periodically.
7. **Frontend** — framework (or absence of one), build step (or absence),
   how pages/templates are structured.
8. **Config** — where config lives, how it is loaded, when it is read
   (import time vs. call time).
9. **Data models** — the core entities and their relationships.

In the filled-in version, mark anything that is installed but unused, or
explicitly a stub, as **STUB** so agents don't build on it.

## 1. Runtime topology

STUB: processes/containers, ports, startup order.

## 2. Storage layout

STUB: every persistence target; call out data that lives outside the
primary database.

## 3. Request flow

STUB: one representative request, end to end.

## 4. Layering & contracts

STUB: layers, directory responsibilities, inter-layer contracts.

## 5. External integrations

STUB: each external system, protocol, and the helper module that wraps it.

## 6. Background jobs

STUB: queue, schedules, periodic tasks.

## 7. Frontend

STUB: stack, build step (or absence), page/template structure.

## 8. Configuration

STUB: config files, loader, and when config is read.

## 9. Data models

STUB: core entities and their relationships.
