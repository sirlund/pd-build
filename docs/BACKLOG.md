# Backlog

Future ideas and deferred concepts. Items here are NOT planned — they're candidates for future evaluation.

---

## BL-001: Configurable Critical Analysis Level

**Context:** Homer's Car Detector is a core principle during PD-OS construction. For end users of PD-Build, the level of critical analysis the agent applies could be configurable — from "just build what I ask" to "challenge every decision without a DATA_SOURCE."

**Why deferred:** Requires Assisted Governance (v1.0+) to be meaningful. Without governance levels, there's no mechanism to modulate agent behavior.

**Origin:** PD-Build design session 2026-02-13.

---

## BL-002: Standalone Mode (PD-Build without PD-Spec)

**Context:** PD-Build currently assumes PD-Spec as upstream. But a senior UX designer with their own research, conclusions, and deliverables should be able to use PD-Build directly — without adopting PD-Spec. Their research is equally valid; it's just in a different format (Notion, Miro, Google Docs, PDFs, their own structured docs).

The problem isn't input quality — it's input **structure**. PD-Spec output comes pre-structured. Own research needs a normalization step.

**Proposed approach:** `/kickoff` conversational onboarding with a minimum viable contract gate.

The skill asks questions, normalizes answers into a contract in `01_Contract/`, and validates a minimum before proceeding. The gate exists because **PD-Build's value is the WHY, not just the WHAT**. Without reasoning, an AI agent reads hex codes and produces generic output — PD-Build adds nothing over Style Dictionary.

**Minimum Viable Contract (required to proceed):**
- **Who** — Who are the users? (target, constraints, context)
- **What** — What is being built? (product/feature domain)
- **Why** — Why these design decisions? (the reasoning the AI agent needs)

If the user brings a `tokens.json` but can't explain why those values exist, PD-Build should not proceed. Not because the input is invalid — because the output won't be useful. An agent with hex codes and no context produces the same result as an agent with nothing.

**Input sources (all valid, different structure levels):**
- PD-Spec output (pre-structured, reads directly)
- Own research docs, persona maps, journey maps, briefs (`/kickoff` normalizes)
- Existing design system, brand guides (`/kickoff` normalizes + extracts reasoning)

**Why deferred:** Core pipeline (PD-Spec → PD-Build) must work first. Standalone mode adds input parsing complexity that isn't justified until the base flow is validated.

**Implications if implemented:**
- PD-Spec becomes "recommended but optional" upstream
- PD-Build becomes independently adoptable
- The minimum viable contract gate enforces that PD-Build always has enough context to generate useful definitions for AI agents

**Origin:** PD-Build design session 2026-02-13.
