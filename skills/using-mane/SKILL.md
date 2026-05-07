---
name: using-mane
description: |
  Use this skill when using or integrating Mane, the pure-Go terminal code
  editor, including files/tabs, LSP actions, gotreesitter syntax highlighting,
  web mode, Monaco companion UI, MCP editor tools, or AI-visible editing.
---

# Using Mane

## When to reach for Mane

Use Mane when a terminal-native code editor or AI-visible editing surface is needed: local coding in a TUI, LSP actions, multi-tab editing, web-accessible modes, or MCP tools that expose editor buffers and syntax resources to agents.

Use Canopy for repository-wide code intelligence. Use Mane for editing buffers and editor actions.

## What Mane is

Mane is a terminal text editor and code workspace in pure Go. It uses gotreesitter for syntax highlighting and incremental parsing, exposes LSP-powered actions, supports TUI-in-browser and Monaco companion modes, and provides MCP editor tools/resources.

## Quick start

```bash
go install github.com/odvcencio/mane@latest
mane .
mane path/to/file.go
```

Useful modes:

```bash
mane -web :8080 .
mane -webui :8080 .
mane -mcp unix:/tmp/mane.sock .
```

Key actions include `Ctrl+S` save, `Ctrl+P` open file, `Ctrl+F` find, `Ctrl+Shift+P` command palette, `F12` definition, `Shift+F12` references, `F2` rename, and `Ctrl+.` code actions.

## When NOT to use Mane

Do not use Mane as a library for parsing or structural analysis; use gotreesitter or Canopy directly. Do not expect the Monaco `-webui` companion surface to have full parity with the terminal UI.

Do not use MCP write tools unless the user has asked for edits and you understand which buffer is active.

## Going deeper

- Public source: [odvcencio/mane](https://github.com/odvcencio/mane)
- README "Features": Read for supported editor capabilities.
- README "MCP extensions": Read before exposing editor tools to agents.
- `.mane-lsp.json`, `$XDG_CONFIG_HOME/mane/lsp.json`, and `MANE_LSP_CONFIG`: Use for LSP server overrides.
