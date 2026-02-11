# Implementation Plan

Phased build plan for PD-Build. Each phase produces a testable, usable increment.

---

## Phase 1: Standard Library — Base Objects

**Goal:** Define the foundational TOON objects that all components inherit from.

### Deliverables
- `02_Kernel/global/tokens.toon` — Semantic design tokens (colors, typography, spacing, elevation, borders). Intention-based naming: `action-primary`, `surface-elevated`, `text-main`.
- `02_Kernel/objects/BaseInput.toon` — Root object for all input components. Defines common states (default, active, disabled, error), validation patterns, and accessibility rules.
- `02_Kernel/objects/BaseButton.toon` — Root object for all button components. Defines interaction states, loading behavior, and throttle rules.
- `02_Kernel/objects/BaseModal.toon` — Root object for overlays. Defines focus trap, escape behavior, backdrop rules.

### Decisions to make
- Token naming convention: flat (`color-action-primary`) vs nested (`color.action.primary`)?
- State inheritance: does a child inherit ALL parent states or only explicitly referenced ones?
- How to represent responsive behavior in TOON (breakpoints, layout shifts)?

---

## Phase 2: Audit Driver — The Logic Linter

**Goal:** Write the first driver that validates Kernel integrity before compilation.

### Deliverables
- `03_Drivers/logic/audit.md` — System prompt that instructs the AI agent to:
  1. Scan all `.toon` files in `02_Kernel/`
  2. Validate every object has a `DATA_SOURCE` label
  3. Check `EXTENDS` chains for contradictions (child overriding parent `[TRACED]` rules)
  4. Detect duplicate logic across objects
  5. Write conflict cards to `02_Kernel/CONFLICTS.md` if issues found
  6. Report "clean" if no issues — authorize build

### Testing
- Create a deliberately conflicting object (child overrides `[TRACED]` parent rule)
- Run audit driver → verify it catches the conflict and writes the card
- Resolve with `/resolve` → verify pipeline continues

---

## Phase 3: Cursor Build Driver — First Compilation

**Goal:** Write the driver that transforms TOON objects into `.cursorrules`.

### Deliverables
- `03_Drivers/logic/cursor_build.md` — System prompt that instructs the AI agent to:
  1. Read all `.toon` files from `02_Kernel/global/` and `02_Kernel/objects/`
  2. For each LOGIC rule, generate a corresponding `.cursorrules` entry
  3. For each STATE with `@trigger`, generate a validation rule
  4. For `[TRACED]` rules, add a comment: `# [TRACED] — Do not modify without PD-Spec update`
  5. For `[HYPOTHESIS]` rules, add a comment: `# [HYPOTHESIS] — Validate with telemetry`
  6. Write output to `04_Dist/staging/.cursorrules`
- `03_Drivers/templates/cursorrules.md` — Template defining the structure of the output file (sections, comment style, rule format)

### Testing
- Run cursor_build on the Standard Library from Phase 1
- Review generated `.cursorrules` in staging
- Approve → verify it moves to production
- Copy to a test project root → verify Cursor respects the rules

---

## Phase 4: End-to-End Test — FinancialInput

**Goal:** Validate the full pipeline with a real component from brief to PR.

### Deliverables
- `01_Sources/specs/DESIGN_BRIEF_financial_input.md` — Sample brief with all 4 contract blocks
- `02_Kernel/objects/FinancialInput.toon` — Extends BaseInput, currency-specific logic, real-time validation
- Generated `.cursorrules` with FinancialInput rules

### The Test
1. Place the sample brief in `01_Sources/specs/`
2. Model the FinancialInput in TOON (with `EXTENDS: BaseInput`)
3. Run audit driver → should pass (or surface expected conflicts)
4. Run cursor_build driver → generates `.cursorrules` in staging
5. Review and approve → moves to production
6. Copy `pd-build/` into a test webapp repo
7. Copy `.cursorrules` to the webapp root
8. Ask Cursor to implement FinancialInput → verify it follows the rules

### Success Criteria
- Cursor generates a component that respects the Kernel's states and validation logic
- The `[TRACED]` comments appear in the generated code context
- No manual intervention needed between brief and working component (beyond review gates)

---

## Phase 5: Memory & Session Continuity

**Goal:** Implement the compilation memory system.

### Deliverables
- Define MEMORY.md entry format (driver, model, temperature, kernel version, changes, status)
- Drivers auto-append to MEMORY.md after each compilation
- Session start reads MEMORY.md and reports Kernel state

---

## Phase 6: Integration — PD-Spec Connection

**Goal:** Test the full PD-OS loop (research → decisions → brief → kernel → code).

### Deliverables
- Run PD-Spec on a real project to generate a `DESIGN_BRIEF.md`
- Copy to PD-Build's `01_Sources/specs/`
- Model TOON objects from the brief
- Compile to `.cursorrules`
- Build the component with Cursor
- Verify end-to-end traceability: `[IG-XX]` in PD-Spec → `[TRACED]` in TOON → comment in `.cursorrules` → enforced in code

---

## Priority Order

| Phase | Dependency | Effort |
|---|---|---|
| Phase 1: Standard Library | None | Medium — requires design decisions |
| Phase 2: Audit Driver | Phase 1 (needs objects to audit) | Low — single prompt file |
| Phase 3: Cursor Build Driver | Phase 1 + 2 | Medium — prompt + template + testing |
| Phase 4: End-to-End Test | Phase 1 + 2 + 3 | Low — applies existing pipeline |
| Phase 5: Memory | Phase 3 (needs compilation sessions to log) | Low |
| Phase 6: PD-Spec Integration | Phase 4 + working PD-Spec project | Medium — cross-repo coordination |
