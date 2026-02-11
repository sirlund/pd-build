# 02_Kernel — Source of Truth

**Do not edit TOON files here without running the audit driver first.**

This is the brain of PD-Build. Every `.toon` file defines an object's logic, states, and data provenance.

## Subfolders

### `global/`
Base tokens and primitives: colors, typography, spacing, elevation, grids. These are inherited by all objects.

### `objects/`
Component definitions. Each `.toon` file defines one object with its logic, states, and `DATA_SOURCE` label.

## Key Files

| File | Purpose |
|---|---|
| `MEMORY.md` | Compilation session log — driver used, model, kernel version, conflicts resolved |
| `CONFLICTS.md` | Active inheritance and logic collisions pending resolution |
