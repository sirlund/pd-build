# Design Notes

Architecture decisions, concept exploration, and critical analysis captured during PD-Build's design phase. This is a living document — ideas here are evaluated, not committed.

> Origin: PD-Spec v3.0 iteration (2025-02-13), refined during PD-Build design sessions (2026-02-11). Key contributions from Gemini sessions on headless DS architecture.

---

## Core Concept: Research-Reactive Design System

The design system is not static. It reacts to research findings via the PD-Spec contract.

**Flow:**
1. PD-Spec produces: `[IG-22] Users over 50 struggle with visual hierarchy in critical flows`
2. PD-Spec ships: `[US-07] When users need to cancel a subscription [IG-22], I want destructive actions to have high contrast, so they can identify critical buttons without strain.`
3. PD-Build reads `[US-07]` and generates:
   - Token: `color.action.destructive` (high contrast red)
   - Component spec: `Button` variant `destructive` with enforced contrast ratio
   - Pattern: "Destructive actions always use `color.action.destructive` + confirmation step"

The design system updates when the research updates — not when a designer remembers to sync Figma.

### Research-Reactive Examples

| PD-Spec insight | Token/spec change | Why |
|---|---|---|
| Users are 50+ | `font.size.base: 18px`, `color.contrast.min: 4.5` | Accessibility-driven |
| App feels slow | Remove `shadow.complex`, `blur.backdrop` | Performance-driven |
| Brand feels cold | Increase `border.radius.base`, shift palette warmer | Brand-driven |
| Users can't find cancel | Add `color.action.destructive` (high contrast) | Usability-driven |
| Users multitask on mobile | `spacing.touch.min: 48px`, simplify nav | Context-driven |

Each row traces: source → `[IG-XX]` → `[US-XX]` → definition change.

---

## Beyond Tokens: Component Schemas

Tokens are values. Design decisions also include **relationships** — layouts, composition rules, interaction patterns:

```json
{
  "pattern": "DashboardShell",
  "rules": {
    "layout_strategy": "css-grid",
    "areas": ["sidebar", "header", "main"],
    "constraints": {
      "sidebar": { "width": "fixed", "value": "{size.layout.sidebar}" },
      "main": { "overflow": "scroll", "padding": "{spacing.container.inset}" }
    }
  }
}
```

The agent reads this and understands: "The sidebar has fixed width." No CSS needed. This is the path from "design tokens" to "design system as data."

**Open question:** Who produces these schemas? If the agent generates from user stories, quality depends on story quality. If the designer writes manually, the problem just moved from Figma to JSON. Likely answer: agent drafts, designer validates.

---

## Assisted Governance — Detailed Design

See [ROADMAP.md](ROADMAP.md) v1.0 for summary. Full model:

### Level 1: Compliant (Silent Execution)
Request uses existing tokens and patterns. Zero friction.

### Level 2: Deviation (Warning + Override)
Request breaks a rule. Agent warns, explains, offers alternatives, executes if human insists. Logs the deviation.

> "Make the Cancel button red."
> Agent: "The system defines secondary actions in `neutral-500`. Red implies destructive. Options: (1) Use system rule, (2) Override with reason."

### Level 3: System Mutation (Confirmation Required)
Request changes a base rule with cascade effects. Agent simulates impact, requires confirmation.

> "Change the primary color."
> Agent: "This affects 47 components and 12 patterns. Cascade impact: [list]. Confirm?"

### Why this model
- Not a police state — blocking overrides kills edge cases and experimentation
- Documents decisions — like `// TODO` comments, marks exceptions
- Educates — designers learn rules through the agent's warnings

### Concern: Copilot vs Pipeline
PD-Spec works as batch (run skill, review, run next). Assisted Governance is real-time — designer says "make it red", agent responds immediately. PD-Build may need a different agent architecture: more copilot than pipeline.

---

## Design Debt Registry — Detailed Design

See [ROADMAP.md](ROADMAP.md) v1.x for summary. When a designer overrides a system rule, PD-Build logs it:

```json
{
  "component_id": "btn_cta_marketing",
  "base_pattern": "button.primary",
  "status": "divergent",
  "overrides": { "background_color": "#FF5733" },
  "governance_log": {
    "author": "Nicolas",
    "reason": "CyberMonday campaign",
    "timestamp": "2026-02-13T10:00:00Z"
  }
}
```

### Pattern Promotion
If the same override appears 3+ times, suggest officializing it as a new token.

### Concerns
- **Debt registry risks becoming its own debt.** Without a review mechanism, it grows forever unread.
- **Pattern promotion needs critical mass.** For solo designers, override volume may never reach useful thresholds. Three identical mistakes aren't an emergent pattern — they're a repeated error.

---

## Known Limitations of Headless DS

- **Gestalt and visual context.** The agent can validate WCAG contrast but can't "feel" visual imbalance.
- **Exceptions vs. rules.** Real design breaks the grid for marketing impact. Encoding "permitted exceptions" grows complexity exponentially.
- **Representation ceiling.** A button is easy to define. A data table with filters, sorting, and empty states produces JSON too complex to read.
- **Bidirectional sync.** Reverse-engineering a manual Figma adjustment back to JSON is technically hard.

---

## Methodology-Aware Generation

PD-Build could adapt priorities based on project context:

| Project context | Priority | Defer |
|---|---|---|
| Design Sprint (5-day) | Prototype specs, UI schemas | Full tokens, QA |
| MVP launch | Tokens, component specs, QA criteria | Voice-tone, deep accessibility |
| System audit | Accessibility, QA criteria | New tokens |
| Scale / mature | Full definitions suite | Nothing |

Not a config file — designer judgment on what to generate first: `/systemize tokens` vs `/systemize all`.

---

## Open Questions

- How does the contract sync work? Manual copy, git submodule, API?
- Should PD-Build validate that every token traces back to a `[US-XX]`?
- Should `/compile` support incremental updates? (Only regenerate for changed tokens)
- How does PD-Build handle multiple platforms? (Same definitions, different drivers)
- What's the relationship between `03_Drivers/` and the codebase? (Import or copy?)

---

## Origin

Ideas emerged from:
- **Gemini v2.5:** Methodology-aware generation, extended artifact catalog, reactive DS examples
- **Gemini v3.0:** Refined skill structure, definition/implementation boundary
- **Headless DS session:** Component schemas, known limitations, Assisted Governance, Design Debt Registry, pattern promotion
- **PD-Build design sessions (2026-02-11):** Pivot from .toon to standard formats, two audiences insight, fidelity gradient, vibe coding context
