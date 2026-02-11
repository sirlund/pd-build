# 03_Drivers — Transformation Logic

Drivers are system prompts that tell the AI agent how to transform TOON objects into tool-specific artifacts.

## Subfolders

### `logic/`
System prompts for the AI agent:
- `audit.md` — Pre-build validation. Scans Kernel for conflicts, missing DATA_SOURCE, inheritance violations.
- `cursor_build.md` — Compilation. Reads TOON objects + templates, generates `.cursorrules`.

### `templates/`
Output blueprints that define the structure of generated artifacts:
- `.cursorrules` template — base structure for Cursor rules
- Additional templates as drivers are added (CSS tokens, Pencil prompts, etc.)
