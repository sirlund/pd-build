# CLAUDE.md — AI Agent Protocol

## Project Context

- **Project:** PD-Build
- **Product:** The Execution Kernel of ProductDesign OS — validated design decisions compiled into a headless design system by AI agents
- **Author:** Niklas Lundin
- **Sibling project:** [PD-Spec](https://github.com/sirlund/pd-spec) (Strategy & Intelligence layer)
- **Status:** Design specification phase. Not yet in development.

## The 3-Layer Stack

```
01_Contract/     →  Input from PD-Spec. Read-only after placement.
02_Definitions/  →  Headless DS core. Tokens, component specs, patterns. The SSoT.
03_Drivers/      →  Platform-specific output. Generated from definitions.
```

Information flows in one direction: Contract → Definitions → Drivers.

## Agent Mandates

### 1. Definitions Integrity
`02_Definitions/` is the single source of truth. Driver output in `03_Drivers/` is derived. If there's a conflict between a `.cursorrules` file and a definition, the definition wins. Regenerate, don't patch.

### 2. No Hallucination
Every token and rule must reference a PD-Spec insight (`[IG-XX]`). The agent cannot invent design decisions. If a definition can't trace back to validated research, it doesn't exist.

### 3. Propose Before Deploying
The agent generates artifacts for review before applying. Present the diff to the Architect for approval. Never write directly to production drivers.

### 4. Two Audiences
Definitions serve both compilers (need values) and AI agents (need values + reasoning). Always include the *why* alongside the *what*: `color.action.destructive: #CC0000 — high contrast for users 50+ [IG-22]`.

### 5. Homer's Car Detector
If a definition, token, or pattern can't justify itself with a data reference, challenge its existence. Prefer the simplest set of definitions that covers verified needs.

### 6. Silence is Gold
Generated artifacts should be concise and actionable. No decorative comments, no restating obvious patterns. Every rule earns its place.

## Definition Formats

| File | Format | Purpose |
|---|---|---|
| `tokens.json` | W3C Design Tokens standard | Semantic values: colors, typography, spacing, elevation |
| `components.yaml` | Functional specs | Component states, variants, accessibility requirements |
| `patterns.md` | Composition rules | Layout rules, interaction patterns, flow constraints |

## Skills (planned)

| Skill | Reads | Writes |
|---|---|---|
| `/systemize` | `01_Contract/` | `02_Definitions/` |
| `/compile` | `02_Definitions/` + stack config | `03_Drivers/` |

Both skills follow PD-Spec's pattern: Phase 0 integrity check, propose before writing, append to session log.

## Contract with PD-Spec

- **v0.1:** Reads PD-Spec Work layer directly (`SYSTEM_MAP.md` + `INSIGHTS_GRAPH.md`)
- **v0.2+:** Reads formal `[US-XX]` user stories (once PD-Spec BL-03 ships)

## Folder Structure

```
├── .claude/skills/            Skills (future: /systemize, /compile)
├── 01_Contract/               Input from PD-Spec (read-only)
├── 02_Definitions/            Headless DS definitions (SSoT)
├── 03_Drivers/                Platform-specific output (generated)
├── docs/
│   ├── ROADMAP.md             Versioned build plan
│   ├── BACKLOG.md             Future ideas
│   └── DESIGN_NOTES.md        Architecture decisions and concept exploration
├── CLAUDE.md                  This file
├── CHANGELOG.md               Version history
└── README.md                  Project overview
```

## Current State

> Update this section as the project evolves.

- **Version:** 0.2.0
- **Status:** Design specification. Architecture and philosophy defined. No skills or definitions implemented.
- **Definitions:** 0
- **Drivers:** 0
- **Skills:** 0
