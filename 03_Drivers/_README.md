# 03_Drivers — Platform-Specific Output

**Generated.** Produced by `/compile` from definitions. Each driver targets a specific platform or tool.

## Planned drivers

| Driver | Output | Target |
|---|---|---|
| Cursor | `.cursorrules` | AI agent coding rules |
| CSS | `variables.css` | CSS custom properties |
| Tailwind | `config.js` | Tailwind theme |

Additional drivers (Swift, Flutter, Figma plugin) are added when a real use case demands them.

## Rules

- Never edit driver output manually — regenerate from `02_Definitions/` instead
- Drivers are the "heads" of the headless DS — definitions are the brain
