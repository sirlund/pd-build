# PD-Build

The **Execution Kernel** of [ProductDesign OS](https://product-os.design). Transforms validated design decisions into a headless design system that AI agents consume to generate coherent output — whether a throwaway prototype or a production PR.

> PD-Build lives inside your product repo — like Storybook. This repo is the template you copy into your project.

## Why This Exists

Designers are increasingly "vibe coding" — prompting LLMs to generate interfaces directly. The results are fast but chaotic: random Tailwind classes, no design consistency, no connection to research or business decisions.

AI tools are great at generating UI. But without validated design definitions, every prompt starts from zero. The LLM picks its own colors, spacing, and patterns. Three screens later, nothing looks like it belongs together.

**PD-Build is not a design system library. It's the engine that guarantees visual coherence when AI agents generate output.** It takes the strategic decisions validated in [PD-Spec](https://github.com/sirlund/pd-spec) and compiles them into definitions that any tool — Cursor, Claude, Figma, or whatever comes next — can consume.

### Two Audiences for Definitions

| Audience | What it needs | Format |
|---|---|---|
| **Compilers** (Style Dictionary, plugins) | Values: `color.primary: #0055FF` | `tokens.json` (W3C standard) |
| **AI agents** (Cursor, Claude Code) | Values + context + rules: "Destructive actions use high contrast because users are 50+ `[IG-22]`. Always pair with confirmation step." | `.cursorrules`, system prompts, markdown with reasoning |

An LLM that reads `#CC0000` produces generic code. An LLM that reads `#CC0000 — high contrast for users who struggle with visual hierarchy [IG-22]` produces code that respects the design intent.

## Architecture

### The 3-Layer Stack

```
01_Contract/     →  Input. Design decisions imported from PD-Spec. Read-only.
02_Definitions/  →  The brain. Tokens, component specs, patterns. The SSoT.
03_Drivers/      →  Output. Platform-specific artifacts, generated from definitions.
```

Information flows in one direction: Contract → Definitions → Drivers. The same 3-layer pattern as PD-Spec (Sources → Work → Outputs), applied to implementation.

### Headless Design System

The Design System is **data**, not files. Like a headless CMS separates content from presentation, a headless DS separates design *intent* from design *implementation*.

```
Traditional DS:    Figma file → dev reads it → writes CSS → hopes it matches
Headless DS:       definitions → compiler → CSS / Tailwind / Swift / Figma plugin
```

`02_Definitions/` describes *what* the design system is — colors, spacing, component states, interaction patterns — in tool-agnostic formats. `03_Drivers/` gives it a "head": compiled into platform-specific code.

**Why headless is mandatory for AI agents:** An AI doesn't "see" pixels — it processes data structures and logic. If the DS lives in Figma, it's opaque to the agent. If it lives in JSON with semantic reasoning, it's actionable. A headless DS is **Infrastructure as Code applied to design**.

### Multiple Rendering Layers, One Core

```
                        ┌→ Cursor (.cursorrules) → prototype / production PR
                        │
02_Definitions/  ───────┼→ CSS / Tailwind → web production
(tokens + specs +       │
 patterns + WHY)        ├→ Figma plugin → design review
                        │
                        └→ Any future tool
```

If the core is solid, every rendering layer works. If it's not, no amount of plugins will fix it.

### Discovery and Production: Same Definitions, Different Fidelity

The definitions are the same. What changes is enforcement:

| | Discovery | Production |
|---|---|---|
| **Purpose** | Prototype, explore, validate flows | Real features, PRs, production code |
| **Stack context** | Not needed | Required (framework, lang, architecture) |
| **Consumer** | Designer + AI agent prototyping | AI agent writing production code |
| **Enforcement** | Guidance — tokens applied freely | Law — tokens + coding rules enforced |

A designer vibe-coding a prototype in Cursor uses the same tokens as the dev writing the final component. The difference is rigor, not the source of truth.

## Definitions

PD-Build produces three core definition files:

| Output | Format | What it defines |
|---|---|---|
| `tokens.json` | W3C Design Tokens standard | Colors, typography, spacing, elevation — semantic, not implementation |
| `components.yaml` | Functional specs | Component states, variants, accessibility requirements — not code |
| `patterns.md` | Composition rules | "Forms always use inline validation", "Modals require explicit dismiss" |

These are **definitions**, not implementations. They describe *what* the design system is. Downstream tools (Style Dictionary, Cursor, Figma plugins) consume them to generate *how*.

## Connection to PD-Spec

PD-Build is the second layer of **ProductDesign OS (PD-OS)** — a framework that separates strategic decision-making from technical execution.

```
PD-Spec (strategy)                    PD-Build (execution)
research → insights → conflicts →    contract → definitions → drivers
system map → decisions          →    → tokens.json, components.yaml, patterns.md
```

**PD-Spec** processes research and business objectives into validated decisions. **PD-Build** transforms those decisions into a headless design system that AI agents use to generate coherent output.

The contract between them evolves with maturity:
- **Today:** PD-Build reads PD-Spec's Work layer directly (`SYSTEM_MAP.md` + `INSIGHTS_GRAPH.md`)
- **Future:** Formal contract via `[US-XX]` user stories (once PD-Spec implements `/ship user-stories`)

Every definition in PD-Build traces back to a PD-Spec insight (`[IG-XX]`). If a token can't justify itself with a research reference, it doesn't belong.

## Design Principles

### No Hallucination
Every token and rule references a real source from PD-Spec. The agent cannot invent design decisions.

### Propose Before Deploying
The agent generates artifacts for review before applying them. Never writes directly to production.

### Homer's Car Detector
If a definition can't justify itself with a data reference, challenge its existence. Prefer the simplest set of definitions that covers verified needs.

### Silence is Gold
Generated artifacts should be concise and actionable. No decorative comments, no restating obvious patterns. Every rule earns its place.

## Boundaries

**Not a component library.** It defines that `Button` must have a `destructive` variant with `color.action.destructive`. It does NOT write `Button.tsx`. The dev team writes the component; PD-Build gives them the spec.

**Not a Figma plugin.** It outputs data that a Figma plugin *could* consume, but it doesn't manipulate Figma files directly.

**Not a testing framework.** It tells you what to test, not how to test it.

**Not a substitute for design judgment.** The designer makes the final call on aesthetics. PD-Build is the starting point, not the final word.

## Repo Structure

```
├── .claude/skills/            Skills (future: /systemize, /compile)
├── 01_Contract/               Input from PD-Spec (read-only)
├── 02_Definitions/            Headless DS definitions (SSoT)
│   ├── tokens.json            W3C design tokens
│   ├── components.yaml        Functional component specs
│   └── patterns.md            Composition rules
├── 03_Drivers/                Platform-specific output (generated)
├── docs/
│   ├── ROADMAP.md             Versioned build plan
│   ├── BACKLOG.md             Future ideas
│   └── DESIGN_NOTES.md        Detailed design notes and architecture decisions
├── CLAUDE.md                  AI agent protocol
├── CHANGELOG.md               Version history
└── README.md                  This file
```

## Current State

> **Status:** Design specification phase. PD-Build is fully defined as architecture and philosophy but not yet implemented. Development begins after PD-Spec is validated with real projects.

See [docs/ROADMAP.md](docs/ROADMAP.md) for the versioned build plan.

## License

[MIT](LICENSE)
