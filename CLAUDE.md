# CLAUDE.md

Neovim config based on kickstart.nvim. Single-file design: almost everything lives in `init.lua`.

## Structure

- `init.lua` — all options, keymaps, and plugin specs
- `lua/kickstart/plugins/` — optional kickstart plugins (lint, debug, autopairs, etc.)
- `lua/custom/plugins/` — user-added plugins (currently empty, uncomment the import in init.lua to activate)

## Adding an LSP Server

1. Add to the `servers` table in `init.lua` (around `local servers = {}`):
   ```lua
   gopls = {},
   ```
2. Mason auto-installs it via `mason-tool-installer`. No separate `ensure_installed` entry needed for LSP servers.

## Adding a Mason Tool (formatter/linter)

Add to `vim.list_extend(ensure_installed, {...})` in init.lua:
```lua
vim.list_extend(ensure_installed, {
  'prettierd',
  'your-new-tool',
})
```

## Adding a Formatter

Edit `formatters_by_ft` in the conform.nvim opts block:
```lua
rust = { 'rustfmt' },
```

## Adding a Linter

Edit `lint.linters_by_ft` in `lua/kickstart/plugins/lint.lua`:
```lua
python = { 'flake8' },
```

## Adding a Treesitter Parser

Add to the `parsers` list in the nvim-treesitter config block:
```lua
local parsers = { ..., 'python' }
```

## Formatter Priority (JS/TS)

biome → prettierd → prettier, `stop_after_first = true`.
Biome is used when `biome.json` exists in the project root; otherwise prettierd takes over.

## Linter Strategy (JS/TS)

eslint_d and biomejs are both registered. Each fails silently when its config file isn't found, so the right one activates automatically per project.

## Initial Setup

Clone 후 최초 1회 실행:
```sh
# 1. 시스템 의존성
brew install neovim ripgrep fd

# 2. Neovim providers & Tree-sitter CLI
npm install -g neovim tree-sitter-cli
pip install pynvim

# 3. Mason이 관리하지 않는 글로벌 도구
npm install -g markdownlint-cli

# 4. Neovim 실행 (플러그인 자동 설치)
nvim
```

Perl and Ruby providers are intentionally disabled (`vim.g.loaded_perl_provider = 0`, `vim.g.loaded_ruby_provider = 0`).

## Nerd Font

`vim.g.have_nerd_font = false` by default. Set to `true` in init.lua if using a Nerd Font in your terminal.
