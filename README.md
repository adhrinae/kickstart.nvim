# nvim config

Personal Neovim configuration based on [kickstart.nvim](https://github.com/nvim-lua/kickstart.nvim), optimized for TypeScript / Node.js / pnpm development.

## Prerequisites

```sh
# Core
brew install neovim ripgrep fd

# Tree-sitter CLI (for compiling parsers)
npm install -g tree-sitter-cli

# Neovim providers
npm install -g neovim
pip install pynvim
```

## Install

```sh
git clone <this-repo> ~/.config/nvim
nvim  # Lazy auto-installs all plugins on first launch
```

## Stack

### LSP

| Server | Languages |
|--------|-----------|
| `ts_ls` | TypeScript, JavaScript, JSX, TSX |
| `jsonls` | JSON, JSONC |
| `cssls` | CSS, SCSS, LESS |
| `html` | HTML |
| `lua_ls` | Lua |

### Formatter (conform.nvim)

Priority order per filetype:

- **JS/TS/JSX/TSX/JSON**: biome → prettierd → prettier (`stop_after_first`)
- **CSS/HTML/YAML/Markdown**: prettierd → prettier
- **Lua**: stylua

Biome runs when `biome.json` is found in the project. Falls back to prettierd automatically.

### Linter (nvim-lint)

- **JS/TS/JSX/TSX**: eslint_d + biomejs (each fails gracefully when its config is absent)
- **Markdown**: markdownlint

### Treesitter Parsers

`typescript`, `javascript`, `tsx`, `json`, `jsonc`, `yaml`, `css`, `html`, `bash`, `lua`, `markdown`, and more.

## Key Mappings

Leader key: `<Space>`

| Key | Action |
|-----|--------|
| `<leader>sf` | Find files |
| `<leader>sg` | Live grep |
| `<leader>sd` | Search diagnostics |
| `<leader>f` | Format buffer |
| `<leader>th` | Toggle inlay hints |
| `grn` | Rename symbol |
| `gra` | Code action |
| `grr` | Go to references |
| `grd` | Go to definition |
| `gri` | Go to implementation |
| `grt` | Go to type definition |
| `[d` / `]d` | Previous / next diagnostic |

## Structure

```
~/.config/nvim/
├── init.lua                        # Main config (single file)
└── lua/
    ├── kickstart/plugins/
    │   ├── lint.lua                # nvim-lint (eslint_d, biomejs)
    │   ├── autopairs.lua
    │   ├── debug.lua
    │   ├── gitsigns.lua
    │   ├── indent_line.lua
    │   └── neo-tree.lua
    └── custom/plugins/
        └── init.lua                # Add your own plugins here
```

## Adding Plugins

Put custom plugins in `lua/custom/plugins/init.lua` and uncomment the import in `init.lua`:

```lua
-- init.lua (near the bottom of lazy.setup)
{ import = 'custom.plugins' },
```

## Updating

```
:Lazy update     " update plugins
:MasonUpdate     " update LSP servers and tools
:TSUpdate        " update treesitter parsers
```
