# Changelog

## [0.2.0] — 2026-02-13

Architecture definition session. Consolidated inputs from PD-Spec design notes, parallel Claude session on pd-spec, and design decisions into a definitive vision. PD-Build is now fully defined as philosophy and architecture — development starts after PD-Spec is validated with real projects.

**Next step:** Implement `/systemize` skill (v0.1: read PD-Spec Work layer → generate `tokens.json`). See `docs/ROADMAP.md`.

### Changed
- **Architecture pivot** — From 4-layer (.toon format) to 3-layer (standard formats)
  - `01_Sources/` → `01_Contract/` (input from PD-Spec)
  - `02_Kernel/` → `02_Definitions/` (tokens.json, components.yaml, patterns.md)
  - `03_Drivers/` — kept, now for generated platform-specific output
  - `04_Dist/` — removed (drivers ARE the output layer)
- **Format pivot** — From custom `.toon` DSL to W3C Design Tokens (JSON) + YAML + Markdown
- **README rewrite** — Consolidated vision: vibe coding context, two audiences (compilers + AI agents), headless DS, fidelity gradient (discovery/production)
- **CLAUDE.md rewrite** — Updated agent mandates for new architecture, added "Two Audiences" and "No Hallucination" mandates
- **Documentation restructure** — `docs/ROADMAP.md`, `docs/BACKLOG.md`, `docs/DESIGN_NOTES.md`

### Removed
- `.toon` format and all references to it
- `IMPLEMENTATION.md` — replaced by `docs/ROADMAP.md`
- `02_Kernel/MEMORY.md` and `CONFLICTS.md` templates
- `04_Dist/` layer (staging/production pattern)

## [0.1.0] — 2025-02-10

### Added
- **Template scaffold** — 4-layer folder structure (Sources, Kernel, Drivers, Dist)
- **README.md** — Architecture documentation, TOON format reference, pipeline description
- **CLAUDE.md** — AI agent protocol with 6 mandates
- **IMPLEMENTATION.md** — Phased build plan
- **Layer guides** (`_README.md`) in all 4 layers
- **MEMORY.md** and **CONFLICTS.md** templates in `02_Kernel/`
- **Git policy** — `04_Dist/staging/` ignored, `04_Dist/production/` persisted
