# CLAUDE.md — AI Agent Protocol

## Project Context

- **Project:** PD-Build
- **Product:** The Execution Kernel of ProductDesign OS — tool-agnostic design logic (.toon) compiled into implementation artifacts by AI agents
- **Team:** Niklas Lundin
- **Started:** 2025-02-10
- **Sibling project:** [PD-Spec](https://github.com/sirlund/pd-spec) (Strategy & Intelligence layer)

## The Truth Stack

This project follows the PD-Build 4-layer architecture. The AI agent operates within these layers and must respect their rules at all times.

```
01_Sources/  →  Inputs (design briefs, bootstrap code). Read-only after placement.
02_Kernel/   →  Design logic objects (.toon), tokens, memory, conflicts. The SSoT.
03_Drivers/  →  Transformation prompts. Compile TOON into tool-specific output.
04_Dist/     →  Generated artifacts. Staging (temporary) → Production (persistent).
```

## Agent Mandates

### 1. Kernel Integrity
The `.toon` files in `02_Kernel/` are the single source of truth. Generated artifacts in `04_Dist/` are derived — if there's a conflict between a `.cursorrules` file and a `.toon` definition, the `.toon` wins. Regenerate, don't patch.

### 2. TRACED > HYPOTHESIS
Data labeled `[TRACED]` (from PD-Spec evidence) is protected. The agent cannot modify, override, or weaken `[TRACED]` logic. Data labeled `[HYPOTHESIS]` (from bootstrap or benchmarks) is experimental — the agent should flag it for validation and add comments requesting telemetry confirmation.

### 3. Propose Before Deploying
The agent generates artifacts in `04_Dist/staging/` first. Never write directly to `04_Dist/production/` or to the root `.cursorrules`. Present the diff to the Architect for review. Only after approval: move to production and copy to root.

### 4. Inheritance Respect
When a TOON object uses `EXTENDS`, the agent must validate that child rules don't contradict parent `[TRACED]` rules. If a collision is detected, write a conflict card to `02_Kernel/CONFLICTS.md` and halt until resolved.

### 5. Homer's Car Detector
If a TOON object, token, or driver rule cannot justify itself with a `DATA_SOURCE` reference (either `[TRACED]` or `[HYPOTHESIS]`), challenge its existence. Prefer the simplest Kernel that covers verified needs.

### 6. Silence is Gold
Generated `.cursorrules` and implementation guides should be concise and actionable. No decorative comments, no restating obvious patterns. Every rule must earn its place.

## TOON Format Reference

```
OBJECT: [Name]
VERSION: [semver]
EXTENDS: [Parent | none]
CONTEXT: "[Domain / Flow]"

DESC: "[One-line description]"

LOGIC:
    [behavioral rules]

STATES:
    [state_name]
        [visual tokens]
        @trigger: [condition]      # optional
        feedback:                  # optional
            icon: [icon-name]
            msg: "[message]"

DATA_SOURCE: [TRACED|HYPOTHESIS] [reference]
```

## Driver System

Drivers live in `03_Drivers/` and come in two types:

| Type | Location | Function |
|---|---|---|
| **Audit** | `03_Drivers/logic/audit.md` | Pre-build validation. Scans Kernel for conflicts, inheritance violations, missing DATA_SOURCE. |
| **Build** | `03_Drivers/logic/cursor_build.md` | Compilation. Transforms TOON objects into `.cursorrules` via `03_Drivers/templates/`. |

Both are system prompts — the AI agent reads the driver instructions and executes the transformation.

## Conflict Resolution

Conflicts are logged in `02_Kernel/CONFLICTS.md` as cards:

```
ID: CF-XXX
OBJECT: [object name] (Extends [parent])
CONFLICT: [description of the collision]
OPTIONS:
  1. OVERRIDE — Prioritize child.
  2. REJECT — Keep parent rule.
  3. MERGE — Create conditional logic.
```

Resolve with `/resolve CF-XXX --option N`.

## Folder Structure

```
├── 01_Sources/                  Inputs (read-only)
│   ├── specs/                   DESIGN_BRIEF.md from PD-Spec
│   └── bootstrap/               Legacy code, raw notes
├── 02_Kernel/                   Source of Truth
│   ├── global/                  Tokens, grids (.toon)
│   ├── objects/                 Components (.toon)
│   ├── MEMORY.md                Session log
│   └── CONFLICTS.md             Collision log
├── 03_Drivers/                  Transformation Logic
│   ├── logic/                   System prompts (audit, build)
│   └── templates/               Output blueprints
├── 04_Dist/                     Artifacts
│   ├── staging/                 Proposals (git-ignored)
│   └── production/              Approved (persistent)
├── CLAUDE.md                    This file
├── CHANGELOG.md                 Version history
└── README.md                    Project overview
```

## Git Conventions

- `04_Dist/staging/` is git-ignored (ephemeral preview area)
- `04_Dist/production/` is committed (versioned artifacts)
- When used inside a product repo, `.cursorrules` lives at the product root, not inside `pd-build/`

## Current State

> Update this section as the project evolves.

- **Version:** 0.1.0
- **Status:** Template scaffold. No TOON objects or drivers implemented yet.
- **Kernel objects:** 0
- **Drivers:** 0
- **Conflicts:** 0
