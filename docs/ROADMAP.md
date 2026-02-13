# Roadmap

Versioned build plan for PD-Build. Each version produces a testable, usable increment.

> **Prerequisite:** PD-Spec must be validated with real projects before PD-Build development begins.

---

## v0.1 — Tokens from Research (Discovery mode)

**Goal:** Read PD-Spec's Work layer → generate `tokens.json`.

- **Input:** `SYSTEM_MAP.md` + `INSIGHTS_GRAPH.md` (direct read, no formal contract yet)
- **Output:** `tokens.json` (W3C Design Tokens standard)
- **Skill:** `/systemize` — reads contract, extracts design definitions
- **Validation test:** Does an LLM produce more coherent output when it reads these tokens vs. without them?

### Why tokens first
Tokens are the atomic unit. If the pipeline can't produce useful tokens from validated research, nothing built on top will work. Smallest possible test of the core idea.

---

## v0.2 — Components and Patterns

**Goal:** Add `components.yaml` and `patterns.md`. Migrate to formal contract.

- **Input:** `[US-XX]` user stories (requires PD-Spec BL-03: `/ship user-stories`)
- **Output:** `tokens.json` + `components.yaml` + `patterns.md`
- **Validation test:** Are the definitions useful to both LLMs and developers? Does a dev read `components.yaml` and know what to build?

---

## v0.3 — First Driver (Production mode begins)

**Goal:** Add `/compile` for one platform. Definitions become implementation.

- **Input:** `02_Definitions/` + stack configuration
- **Output:** `.cursorrules` or CSS custom properties (pick one platform)
- **Skill:** `/compile` — reads definitions + stack context, generates platform-specific drivers
- **Validation test:** Does the generated `.cursorrules` actually influence Cursor's output in a real codebase?

---

## v1.0 — Assisted Governance

**Goal:** The agent understands rules and helps enforce them with nuance.

Three levels of intervention:
1. **Compliant** — request uses existing tokens. Silent execution.
2. **Deviation** — request breaks a rule. Agent warns, explains, offers alternatives, but allows override with reason.
3. **System Mutation** — request changes a base rule that cascades. Agent simulates impact, requires confirmation.

Not a police state. More like a senior dev reviewing your PR.

---

## v1.x — Design Debt and Pattern Promotion

**Goal:** Track overrides, surface emergent patterns from usage data.

- Design Debt Registry: log deviations with reasons for periodic cleanup
- Pattern Promotion: when the same override appears 3+ times, suggest officializing it as a new token or pattern

**Prerequisite:** Enough real usage data to make pattern detection meaningful. For solo designers, the volume may never reach useful thresholds — validate before building.

---

## Dependencies

| Version | Depends on |
|---|---|
| v0.1 | PD-Spec validated with real project |
| v0.2 | v0.1 + PD-Spec BL-03 (`/ship user-stories`) |
| v0.3 | v0.2 |
| v1.0 | v0.3 + enough usage to justify governance |
| v1.x | v1.0 + team-scale usage data |
