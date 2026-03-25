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
| `vtsls` | TypeScript, JavaScript, JSX, TSX (VS Code's tsserver — path alias & monorepo support) |
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

**Buffers** (files open in memory — use these instead of tabs)

| Key | Action |
|-----|--------|
| `]b` / `[b` | Next / previous buffer |
| `<leader><leader>` | List all open buffers (fuzzy search) |
| `<leader>bd` | Delete (close) current buffer |

**Search**

| Key | Action |
|-----|--------|
| `<leader>sf` | Find files |
| `<leader>sg` | Live grep |
| `<leader>sd` | Search diagnostics |
| `<leader>s.` | Recent files |
| `<leader>/` | Fuzzy search in current buffer |

**LSP** — `gr` prefix로 통일 (Neovim 0.11 컨벤션)

| Key | Action |
|-----|--------|
| `grd` | Go to **d**efinition ← `gd` 대신 이걸 써야 함 |
| `grr` | Go to **r**eferences |
| `gri` | Go to **i**mplementation |
| `grt` | Go to **t**ype definition |
| `grn` | Re**n**ame symbol |
| `gra` | Code **a**ction |
| `gO` | Document symbols |
| `gW` | Workspace symbols |
| `grD` | Go to declaration (헤더 등) |
| `[d` / `]d` | Previous / next diagnostic |
| `<leader>th` | Toggle inlay hints (파라미터명, 타입, 반환타입 등) |

**File Tree (Neo-tree)**

| Key | Action |
|-----|--------|
| `\` | Toggle file tree (reveals current file) |

**Terminal**

| Key / Command | Action |
|---------------|--------|
| `:term` | 빌트인 터미널 열기 |
| `<Esc><Esc>` | 터미널 모드 → 노멀 모드 |
| `:term lazygit` | lazygit 실행 |

**Other**

| Key | Action |
|-----|--------|
| `<leader>f` | Format buffer |
| `<leader>q` | Open diagnostic quickfix list |
| `<C-h/j/k/l>` | Move focus between splits |

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
