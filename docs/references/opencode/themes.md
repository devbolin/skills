---
source: https://opencode.ai/docs/themes/
retrieved: 2026-05-12
type: archived
---

# Themes

Select a built-in theme or define your own.

With OpenCode you can select from one of several built-in themes, use a theme that adapts to your terminal theme, or define your own custom theme.

By default, OpenCode uses our own `opencode` theme.

---

## Terminal requirements

For themes to display correctly with their full color palette, your terminal must support **truecolor** (24-bit color).

---

## Built-in themes

| Name | Description |
|---|---|
| `system` | Adapts to your terminal's background color |
| `tokyonight` | Based on the Tokyonight theme |
| `everforest` | Based on the Everforest theme |
| `ayu` | Based on the Ayu dark theme |
| `catppuccin` | Based on the Catppuccin theme |
| `catppuccin-macchiato` | Based on the Catppuccin theme |
| `gruvbox` | Based on the Gruvbox theme |
| `kanagawa` | Based on the Kanagawa theme |
| `nord` | Based on the Nord theme |
| `matrix` | Hacker-style green on black theme |
| `one-dark` | Based on the Atom One Dark theme |

And more, we are constantly adding new themes.

---

## System theme

The `system` theme automatically adapts to your terminal's color scheme.

---

## Using a theme

You can select a theme with the `/theme` command or specify it in `tui.json`:

```json
{
  "$schema": "https://opencode.ai/tui.json",
  "theme": "tokyonight"
}
```

---

## Custom themes

OpenCode supports a flexible JSON-based theme system.

### Hierarchy

Themes are loaded from multiple directories (later overrides earlier):
1. Built-in themes
2. User config directory (`~/.config/opencode/themes/*.json`)
3. Project root directory (`.opencode/themes/*.json`)
4. Current working directory (`./.opencode/themes/*.json`)

### Creating a theme

Create a JSON file in one of the theme directories:

```
mkdir -p ~/.config/opencode/themes
```

### JSON format

Themes use a flexible JSON format with support for:
- **Hex colors**: `"#ffffff"`
- **ANSI colors**: `3` (0-255)
- **Color references**: `"primary"` or custom definitions
- **Dark/light variants**: `{"dark": "#000", "light": "#fff"}`
- **No color**: `"none"` - Uses the terminal's default color
