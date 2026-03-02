# Neovim Config Reference — mbergo

> A complete guide to **your** Neovim setup, built on [kickstart.nvim](https://github.com/nvim-lua/kickstart.nvim).
> Everything in this file reflects what is actually configured in `~/.config/nvim/`.

---

## Table of Contents

1. [Reading Key Notation](#1-reading-key-notation)
2. [The Leader Key](#2-the-leader-key)
3. [Modes](#3-modes)
4. [Survival — the absolute minimum](#4-survival--the-absolute-minimum)
5. [Moving Around](#5-moving-around)
6. [Editing Text](#6-editing-text)
7. [Selecting Text (Visual Mode)](#7-selecting-text-visual-mode)
8. [Working with Files & Buffers](#8-working-with-files--buffers)
9. [Windows and Splits](#9-windows-and-splits)
10. [Search and Replace](#10-search-and-replace)
11. [Command-line Mode](#11-command-line-mode)
12. [Plugin — Telescope (fuzzy finder)](#12-plugin--telescope-fuzzy-finder)
13. [Plugin — LSP (language intelligence)](#13-plugin--lsp-language-intelligence)
14. [Plugin — blink.cmp (autocomplete)](#14-plugin--blinkcmp-autocomplete)
15. [Plugin — Gitsigns (git in the gutter)](#15-plugin--gitsigns-git-in-the-gutter)
16. [Plugin — mini.ai (text objects)](#16-plugin--miniai-text-objects)
17. [Plugin — mini.surround (brackets & quotes)](#17-plugin--minisurround-brackets--quotes)
18. [Plugin — todo-comments](#18-plugin--todo-comments)
19. [Plugin — Claude Code (claudecode.nvim)](#19-plugin--claude-code-claudecodenvim)
20. [Plugin — lazy.nvim (plugin manager)](#20-plugin--lazynvim-plugin-manager)
21. [Plugin — Mason (LSP installer)](#21-plugin--mason-lsp-installer)
22. [Plugin — conform.nvim (formatting)](#22-plugin--conformnvim-formatting)
23. [Diagnostics (errors & warnings)](#23-diagnostics-errors--warnings)
24. [Useful Built-in Commands](#24-useful-built-in-commands)
25. [Installed Plugins Summary](#25-installed-plugins-summary)

---

## 1. Reading Key Notation

Neovim uses a compact notation for key combinations. You will see it everywhere in docs and config files.

| Notation | Meaning | Example |
|---|---|---|
| `<leader>` | Your leader key (see next section) | `<leader>sf` = Space then s then f |
| `<CR>` | Enter / Return key | |
| `<Esc>` | Escape key | |
| `<C-x>` | Ctrl held, then press x | `<C-w>` = Ctrl+w |
| `<S-x>` | Shift held, then press x | `<S-v>` = Shift+v |
| `<A-x>` or `<M-x>` | Alt held, then press x | |
| `<Tab>` | Tab key | |
| `<Space>` | Spacebar | |
| `<BS>` | Backspace | |
| `<Up>` `<Down>` `<Left>` `<Right>` | Arrow keys | |

### Sequences vs. Combinations

This is the most confusing part for beginners. There are **two different things**:

**`<C-w><C-h>` — a sequence of two combinations**
- You press Ctrl+w, **release**, then press Ctrl+h.
- These are two separate key chords pressed one after another.
- Neovim waits for the second chord after the first one.

**`<C-w>h` — Ctrl+w then just h**
- Press Ctrl+w, release, then press the plain letter `h`.
- Same idea — a sequence, not holding both at once.

**`<C-wh>` — does NOT exist**
- You cannot hold Ctrl and press two letters at the same time in most terminals.

So when you see `<C-w><C-h>` or `<C-w>h`, always read it as: **press the first chord, release, then the second**.

---

## 2. The Leader Key

The **leader key** is a configurable prefix that unlocks a whole namespace of custom shortcuts without conflicting with built-in Vim keys.

In this config it is set to **Space**:
```lua
vim.g.mapleader = ' '
```

**How it works:**
- Press `Space` (release it), then type the rest of the key sequence.
- Example: `<leader>sf` means: press Space, then `s`, then `f` → opens file search.
- There is a short timeout (~300ms). If you type slowly, it may not register.

**Tip:** The `which-key` plugin is installed. If you press Space and wait, a popup will show you all available continuations — you never need to memorize them all.

---

## 3. Modes

Neovim is a **modal editor**. The mode you are in changes what keys do.

| Mode | How to enter | What it does |
|---|---|---|
| **Normal** | `<Esc>` from anywhere | Navigate, run commands. Default mode. |
| **Insert** | `i` `a` `o` `I` `A` `O` | Type text like a regular editor. |
| **Visual** | `v` | Select characters. |
| **Visual Line** | `V` (Shift+v) | Select whole lines. |
| **Visual Block** | `<C-v>` | Select a rectangle of text. |
| **Command** | `:` | Type ex-commands like `:w`, `:q`. |
| **Terminal** | `:terminal` or `<leader>ac` | Shell inside Neovim. Press `<Esc><Esc>` to escape. |
| **Replace** | `R` | Overwrite characters as you type. |

The current mode is always shown in the statusline (bottom of screen).

---

## 4. Survival — the absolute minimum

If you are stuck and need to get out:

| Key | What it does |
|---|---|
| `<Esc>` | Go back to Normal mode (press it whenever lost) |
| `:q` + `<CR>` | Quit (fails if unsaved changes) |
| `:q!` + `<CR>` | Quit and discard all changes |
| `:w` + `<CR>` | Save (write) the file |
| `:wq` + `<CR>` | Save and quit |
| `:wa` + `<CR>` | Save all open files |
| `u` | Undo |
| `<C-r>` | Redo |

---

## 5. Moving Around

All of these work in **Normal mode**.

### Basic Motion

| Key | Movement |
|---|---|
| `h` | Left one character |
| `l` | Right one character |
| `j` | Down one line |
| `k` | Up one line |
| `w` | Forward to start of next **word** |
| `b` | Backward to start of previous **word** |
| `e` | Forward to **end** of current word |
| `W` `B` `E` | Same as above but skip punctuation (WORD = space-delimited) |
| `0` | Start of line |
| `^` | First non-blank character of line |
| `$` | End of line |
| `gg` | Top of file |
| `G` | Bottom of file |
| `5G` | Go to line 5 (or `:5` in command mode) |
| `{` `}` | Jump to previous/next empty line (paragraph) |
| `(` `)` | Jump by sentence |
| `%` | Jump to matching bracket / parenthesis / brace |
| `<C-d>` | Scroll half-page down |
| `<C-u>` | Scroll half-page up |
| `<C-f>` | Scroll full page down |
| `<C-b>` | Scroll full page up |
| `zz` | Center current line in the screen |
| `zt` | Bring current line to top |
| `zb` | Bring current line to bottom |

### Counts

Every motion can be prefixed with a count:
- `5j` — move down 5 lines
- `3w` — forward 3 words
- `10G` — go to line 10

### Jumping (with the jump list)

| Key | Action |
|---|---|
| `<C-o>` | Go back to previous location (jump list) |
| `<C-i>` or `<Tab>` | Go forward in jump list |
| `''` (two apostrophes) | Jump to the position before the last jump |
| `m{a-z}` | Set a mark at cursor position (e.g., `ma`) |
| `` `a `` | Jump to mark `a` |

### Search-based navigation

| Key | Action |
|---|---|
| `f{char}` | Jump forward to next occurrence of `char` on line |
| `F{char}` | Jump backward to `char` on line |
| `t{char}` | Jump to just before `char` (forward) |
| `T{char}` | Jump to just after `char` (backward) |
| `;` | Repeat last `f`/`F`/`t`/`T` forward |
| `,` | Repeat last `f`/`F`/`t`/`T` backward |

---

## 6. Editing Text

### Entering Insert Mode

| Key | Where cursor goes |
|---|---|
| `i` | Before cursor |
| `a` | After cursor |
| `I` | Beginning of line |
| `A` | End of line |
| `o` | New line below and enter Insert |
| `O` | New line above and enter Insert |
| `s` | Delete character under cursor and enter Insert |
| `S` | Delete whole line and enter Insert |
| `C` | Delete from cursor to end of line and enter Insert |

### Operators (Normal mode — combine with motions)

Operators follow the pattern: **operator + motion** = action on the text the motion covers.

| Operator | Action |
|---|---|
| `d` | Delete (cut) |
| `y` | Yank (copy) |
| `c` | Change (delete then enter Insert) |
| `>` | Indent right |
| `<` | Indent left |
| `=` | Auto-indent |
| `g~` | Toggle case |
| `gu` | Make lowercase |
| `gU` | Make uppercase |

**Examples:**

| Command | What it does |
|---|---|
| `dw` | Delete forward one word |
| `d$` | Delete to end of line |
| `dd` | Delete entire line |
| `yy` | Yank (copy) entire line |
| `yw` | Yank one word |
| `cc` | Change entire line |
| `cw` | Change word |
| `ci"` | Change text Inside quotes |
| `ca{` | Change text Around curly braces (including braces) |
| `>G` | Indent from current line to end of file |
| `==` | Auto-indent current line |
| `gg=G` | Auto-indent entire file |

### Pasting and Registers

| Key | Action |
|---|---|
| `p` | Paste after cursor |
| `P` | Paste before cursor |
| `"ayy` | Yank line into register `a` |
| `"ap` | Paste from register `a` |
| `"+y` | Yank to system clipboard |
| `"+p` | Paste from system clipboard |

> **Note:** This config syncs the clipboard automatically, so regular `y`/`p` also works with your OS clipboard.

### Deleting in Insert Mode

| Key | Action |
|---|---|
| `<C-h>` | Delete character before cursor (like Backspace) |
| `<C-w>` | Delete word before cursor |
| `<C-u>` | Delete to start of line |

### Other Editing Commands

| Key | Action |
|---|---|
| `x` | Delete character under cursor |
| `X` | Delete character before cursor |
| `r{char}` | Replace single character under cursor with `char` |
| `R` | Enter Replace mode (overwrite) |
| `J` | Join current line with the line below |
| `.` | Repeat last change — extremely useful |
| `~` | Toggle case of character under cursor |
| `<C-a>` | Increment number under cursor |
| `<C-x>` | Decrement number under cursor |

---

## 7. Selecting Text (Visual Mode)

Enter visual mode with `v`, `V`, or `<C-v>`, then move to extend selection, then apply an operator.

| Key | Action |
|---|---|
| `v` | Start character-wise visual selection |
| `V` | Start line-wise visual selection |
| `<C-v>` | Start block-wise (rectangle) visual selection |
| `o` | Go to other end of selection |
| `gv` | Re-select last visual selection |

**In Visual mode, apply operators:**
- `d` delete selection
- `y` yank selection
- `c` change selection
- `>` indent right
- `<` indent left
- `~` toggle case
- `u` make lowercase
- `U` make uppercase

**Visual Block (`<C-v>`) tricks:**
- Select a column, then `I` to insert at the start of each line in the block.
- Select a column, then `d` to delete that column everywhere.

---

## 8. Working with Files & Buffers

In Neovim, a **buffer** is a loaded file. A **window** is a viewport into a buffer. A **tab** is a collection of windows.

### Opening Files

| Command | Action |
|---|---|
| `:e filename` | Open a file (`:e` = edit) |
| `:e .` | Open file explorer in current directory |
| `<leader>sf` | Fuzzy-find and open a file (Telescope) |
| `<leader>s.` | Open a recently-opened file (Telescope) |

### Buffers

| Key / Command | Action |
|---|---|
| `<leader><leader>` | List all open buffers and switch (Telescope) |
| `:bn` | Next buffer |
| `:bp` | Previous buffer |
| `:bd` | Delete (close) current buffer |
| `:ls` | List all open buffers |

### Saving & Quitting

| Command | Action |
|---|---|
| `:w` | Save current file |
| `:w filename` | Save as new filename |
| `:wa` | Save all buffers |
| `:q` | Quit window |
| `:qa` | Quit all windows |
| `:q!` | Quit without saving |
| `:wqa` | Save all and quit |
| `ZZ` | Save and quit (Normal mode shortcut for `:wq`) |
| `ZQ` | Quit without saving (Normal mode shortcut for `:q!`) |

---

## 9. Windows and Splits

### Creating Splits

| Command | Action |
|---|---|
| `<C-w>s` or `:sp` | Split horizontally (same file) |
| `<C-w>v` or `:vsp` | Split vertically (same file) |
| `:sp filename` | Horizontal split with a different file |
| `:vsp filename` | Vertical split with a different file |

### Navigating Between Windows

These are configured in this setup:

| Key | Action |
|---|---|
| `<C-h>` | Move focus to the **left** window |
| `<C-l>` | Move focus to the **right** window |
| `<C-j>` | Move focus to the **lower** window |
| `<C-k>` | Move focus to the **upper** window |

Standard Vim equivalents (also work):

| Key | Action |
|---|---|
| `<C-w>w` | Cycle to next window |
| `<C-w>h/j/k/l` | Move to window in direction |
| `<C-w>p` | Go to previous (last focused) window |

### Resizing Windows

| Key | Action |
|---|---|
| `<C-w>=` | Make all windows equal size |
| `<C-w>>` | Increase width |
| `<C-w><` | Decrease width |
| `<C-w>+` | Increase height |
| `<C-w>-` | Decrease height |
| `<C-w>_` | Maximize height |
| `<C-w>\|` | Maximize width |

### Moving Windows Around

| Key | Action |
|---|---|
| `<C-w>H` | Move window to far left |
| `<C-w>J` | Move window to far bottom |
| `<C-w>K` | Move window to far top |
| `<C-w>L` | Move window to far right |
| `<C-w>T` | Move window to its own tab |

### Closing Windows

| Key | Action |
|---|---|
| `<C-w>c` or `:q` | Close current window |
| `<C-w>o` or `:only` | Close all windows except current |

### Tabs

| Command | Action |
|---|---|
| `:tabnew` | Open a new tab |
| `:tabnext` or `gt` | Go to next tab |
| `:tabprev` or `gT` | Go to previous tab |
| `:tabclose` | Close current tab |
| `{n}gt` | Go to tab number n |

---

## 10. Search and Replace

### Searching

| Key | Action |
|---|---|
| `/pattern` | Search forward for pattern |
| `?pattern` | Search backward for pattern |
| `n` | Next match (same direction) |
| `N` | Previous match (opposite direction) |
| `*` | Search forward for word under cursor |
| `#` | Search backward for word under cursor |
| `<Esc>` | Clear search highlights (configured in this setup) |

### Search/Replace (Substitution)

| Command | Action |
|---|---|
| `:%s/old/new/g` | Replace all occurrences in file |
| `:%s/old/new/gc` | Replace all, confirm each one |
| `:s/old/new/g` | Replace on current line only |
| `:'<,'>s/old/new/g` | Replace in visual selection (auto-filled when in Visual) |

**Flags:**
- `g` = all occurrences on the line (without it: only first)
- `c` = confirm each replacement
- `i` = case-insensitive
- `I` = case-sensitive (override ignorecase setting)

### Live Preview

In this config `inccommand = 'split'` is set — as you type a `:s/` command you see matches highlighted in the file and a small preview split.

### In-buffer Fuzzy Search

| Key | Action |
|---|---|
| `<leader>/` | Fuzzy search within current buffer (Telescope) |

---

## 11. Command-line Mode

Press `:` to enter. Press `<Esc>` or `<C-c>` to cancel.

| Key | Action |
|---|---|
| `<Tab>` | Autocomplete command or filename |
| `<C-n>` / `<C-p>` | Next/previous autocomplete suggestion |
| `<Up>` / `<Down>` | Navigate command history |
| `<C-r><C-w>` | Insert word under cursor into command line |
| `q:` | Open command history window (edit and re-run) |
| `q/` | Open search history window |

---

## 12. Plugin — Telescope (fuzzy finder)

Telescope opens a floating search window. Type to filter, use arrows or `<C-n>`/`<C-p>` to navigate, `<CR>` to open.

**Inside any Telescope window:**
- `<C-/>` (Insert mode) or `?` (Normal mode) — show all available keymaps for that picker.
- `<Esc>` or `<C-c>` — close Telescope.
- `<C-x>` — open in horizontal split.
- `<C-v>` — open in vertical split.
- `<C-t>` — open in new tab.
- `<C-u>` / `<C-d>` — scroll preview up/down.

### File & Buffer Search

| Key | Action |
|---|---|
| `<leader>sf` | **S**earch **F**iles — find any file in project |
| `<leader>s.` | Search **recent** files |
| `<leader><leader>` | Find and switch between open **buffers** |
| `<leader>sn` | Search **N**eovim config files |

### Text Search

| Key | Action |
|---|---|
| `<leader>sg` | **S**earch by **G**rep — live grep across all files |
| `<leader>sw` | Search current **W**ord under cursor in all files |
| `<leader>s/` | Search across only currently open files |
| `<leader>/` | Fuzzy search inside current buffer |

### Meta / Help

| Key | Action |
|---|---|
| `<leader>sh` | Search **H**elp documentation |
| `<leader>sk` | Search **K**eymaps |
| `<leader>ss` | Search/Select Telescope builtins |
| `<leader>sc` | Search **C**ommands |
| `<leader>sd` | Search **D**iagnostics |
| `<leader>sr` | **R**esume last Telescope search |

---

## 13. Plugin — LSP (language intelligence)

LSP = Language Server Protocol. When you open a code file, a language server analyzes it and provides intelligence. These keys only work in a buffer with an LSP attached (lua, etc.).

### Navigation

| Key | Action |
|---|---|
| `grd` | **G**oto **D**efinition — jump to where a function/variable is defined |
| `grr` | **G**oto **R**eferences — find all places this is used |
| `gri` | **G**oto **I**mplementation |
| `grt` | **G**oto **T**ype definition |
| `grD` | **G**oto **D**eclaration (e.g., header file in C) |
| `gO` | Open Document Symbols — all symbols in current file |
| `gW` | Open Workspace Symbols — all symbols in project |
| `<C-t>` | Jump back after a goto (built-in jump list) |

### Actions

| Key | Action |
|---|---|
| `grn` | **R**e**n**ame symbol — renames across all files |
| `gra` | Code **A**ction — fix suggestion (also works in visual selection) |
| `K` | Hover documentation — show docs for symbol under cursor |
| `<leader>f` | **F**ormat current buffer (conform.nvim) |
| `<leader>th` | **T**oggle inlay **H**ints |

### Language Servers Installed

Currently active: **lua_ls** (Lua language server)

To add more, edit the `servers` table in `init.lua` (around line 595). Examples:
```lua
servers = {
  gopls = {},          -- Go
  pyright = {},        -- Python
  rust_analyzer = {},  -- Rust
  ts_ls = {},          -- TypeScript/JavaScript
  clangd = {},         -- C/C++
}
```

---

## 14. Plugin — blink.cmp (autocomplete)

Autocomplete appears automatically as you type in Insert mode.

| Key | Action |
|---|---|
| `<C-Space>` | Open completion menu (or show docs if open) |
| `<C-n>` or `<Down>` | Select next item |
| `<C-p>` or `<Up>` | Select previous item |
| `<C-y>` | **Accept** the selected completion |
| `<C-e>` | Hide/dismiss the completion menu |
| `<C-k>` | Toggle **signature help** (shows function arguments) |
| `<Tab>` / `<S-Tab>` | Move right/left through snippet placeholders |

---

## 15. Plugin — Gitsigns (git in the gutter)

Shows `+` `~` `_` signs in the left gutter for added/changed/deleted lines. Works in any file inside a git repository.

| Key | Action |
|---|---|
| `]h` | Next git hunk (change) |
| `[h` | Previous git hunk |
| `<leader>hs` | **H**unk **S**tage — stage this change |
| `<leader>hr` | **H**unk **R**eset — discard this change |
| `<leader>hS` | Stage entire file |
| `<leader>hR` | Reset entire file |
| `<leader>hp` | **H**unk **P**review — float showing the diff |
| `<leader>hb` | **H**unk **B**lame — show git blame for line |
| `<leader>hd` | **H**unk **D**iff — diff this file |
| `<leader>hD` | Diff against last commit |
| `<leader>tb` | **T**oggle **B**lame line |
| `<leader>tD` | **T**oggle deleted (show removed lines inline) |

**In visual mode:**

| Key | Action |
|---|---|
| `<leader>hs` | Stage selected lines |
| `<leader>hr` | Reset selected lines |

---

## 16. Plugin — mini.ai (text objects)

Enhances Vim's built-in `i` (inner) and `a` (around) text objects with more options. Use with operators like `d`, `c`, `y`, `v`.

### Built-in text objects (still work)

| Object | Covers |
|---|---|
| `iw` / `aw` | inner/around **word** |
| `is` / `as` | inner/around **sentence** |
| `ip` / `ap` | inner/around **paragraph** |
| `i"` / `a"` | inner/around **double quotes** |
| `i'` / `a'` | inner/around **single quotes** |
| `` i` `` / `` a` `` | inner/around **backticks** |
| `i(` / `a(` | inner/around **parentheses** |
| `i[` / `a[` | inner/around **brackets** |
| `i{` / `a{` | inner/around **curly braces** |
| `i<` / `a<` | inner/around **angle brackets** |
| `it` / `at` | inner/around **HTML tag** |

### Added by mini.ai (next/last variant)

Add `n` (next) or `l` (last) to target the next or previous object, not just the one the cursor is in:

| Example | Meaning |
|---|---|
| `cin"` | **C**hange **I**nside **N**ext quotes |
| `yil'` | **Y**ank **I**nside **L**ast single quotes |
| `van)` | **V**isually select **A**round **N**ext parentheses |

---

## 17. Plugin — mini.surround (brackets & quotes)

Manage surrounding characters: add, delete, or replace brackets, quotes, etc.

| Key | Action | Example |
|---|---|---|
| `saiw)` | **S**urround **A**dd **I**nner **W**ord with `)` | `word` → `(word)` |
| `saiw"` | Surround word with `"` | `word` → `"word"` |
| `sd'` | **S**urround **D**elete `'` | `'hello'` → `hello` |
| `sr)"` | **S**urround **R**eplace `)` with `"` | `(hello)` → `"hello"` |
| `sf(` | **S**urround **F**ind `(` — jump to next surrounding `(` | |
| `sF(` | **S**urround **F**ind `(` backwards | |
| `sh(` | **S**urround **H**ighlight `(` — visually select surrounding | |

**Supported surrounds:** `(` `)` `[` `]` `{` `}` `<` `>` `"` `'` `` ` `` and HTML tags with `t`.

---

## 18. Plugin — todo-comments

Highlights special comment keywords in code. Use Telescope to search them.

| Keyword | Color | Use for |
|---|---|---|
| `TODO:` | Blue | Things to do |
| `FIXME:` / `BUG:` | Red | Known bugs |
| `HACK:` | Yellow | Temporary workarounds |
| `WARN:` / `WARNING:` | Yellow | Caution |
| `PERF:` / `OPTIM:` | Purple | Performance issues |
| `NOTE:` / `INFO:` | Green | General notes |
| `TEST:` | Cyan | Test-related notes |

**Commands:**

| Command | Action |
|---|---|
| `:TodoTelescope` | Search all TODOs in project with Telescope |
| `:TodoQuickFix` | Open all TODOs in quickfix list |
| `:TodoLocList` | Open all TODOs in location list |
| `]t` | Jump to next TODO comment |
| `[t` | Jump to previous TODO comment |

---

## 19. Plugin — Claude Code (claudecode.nvim)

Integrates the Claude Code CLI (`claude`) directly into Neovim as a terminal panel. Requires `claude` to be installed and running.

### Keymaps

| Key | Mode | Action |
|---|---|---|
| `<leader>ac` | Normal | **A**I **C**laude — Toggle Claude terminal (show/hide) |
| `<leader>af` | Normal | **A**I **F**ocus — Smart focus/toggle (focus if open, open if closed) |
| `<leader>as` | Visual | **A**I **S**end — Send selected text to Claude |
| `<leader>aa` | Normal | **A**I **A**dd — Add current file to Claude's context |

### Commands

| Command | Action |
|---|---|
| `:ClaudeCode` | Toggle the Claude terminal window |
| `:ClaudeCodeFocus` | Focus the Claude terminal (open if closed) |
| `:ClaudeCodeSend` | Send visual selection to Claude |
| `:ClaudeCodeAdd <file>` | Add a file to Claude's context |
| `:ClaudeCodeDiffAccept` | Accept Claude's proposed changes |
| `:ClaudeCodeDiffDeny` | Reject Claude's proposed changes |

### Diff Workflow

When Claude proposes code changes, they appear as a native Neovim diff:
- `:w` — accept the change
- `:q` — reject the change
- You can also edit the diff before accepting it.

### Terminal Escape

Inside the Claude terminal, press `<Esc><Esc>` to go back to Normal mode in Neovim (without sending Esc to Claude).

---

## 20. Plugin — lazy.nvim (plugin manager)

| Command | Action |
|---|---|
| `:Lazy` | Open the plugin manager UI |
| `:Lazy update` | Update all plugins |
| `:Lazy sync` | Install missing + update + clean |
| `:Lazy install` | Install any missing plugins |
| `:Lazy clean` | Remove unused plugins |
| `:Lazy check` | Check for updates without installing |
| `:Lazy log` | Show recent plugin changes |
| `:Lazy profile` | Show startup time per plugin |

**Inside the `:Lazy` UI:** press `?` for help.

---

## 21. Plugin — Mason (LSP installer)

Mason installs and manages language servers, formatters, and linters.

| Command | Action |
|---|---|
| `:Mason` | Open the Mason UI |
| `:MasonInstall lua-language-server` | Install a specific tool |
| `:MasonUninstall lua-language-server` | Uninstall a tool |
| `:MasonUpdate` | Update all installed tools |

**Inside the `:Mason` UI:** press `g?` for help, `i` to install, `X` to uninstall.

---

## 22. Plugin — conform.nvim (formatting)

Automatically formats files on save. Also available manually.

| Key / Command | Action |
|---|---|
| `<leader>f` | **F**ormat current buffer |
| `:ConformInfo` | Show which formatter is active for current file |

**Currently configured formatters:**
- Lua → `stylua`

---

## 23. Diagnostics (errors & warnings)

When an LSP is active, errors and warnings appear inline and in the gutter.

| Key | Action |
|---|---|
| `[d` | Jump to **previous** diagnostic |
| `]d` | Jump to **next** diagnostic |
| `<leader>q` | Open all diagnostics in **quickfix** list |
| `<leader>sd` | Search diagnostics with Telescope |

**Quickfix list navigation (after `<leader>q`):**

| Command | Action |
|---|---|
| `:cn` or `:cnext` | Next item in quickfix |
| `:cp` or `:cprev` | Previous item in quickfix |
| `:cc {n}` | Jump to item number n |
| `:cclose` | Close quickfix window |

---

## 24. Useful Built-in Commands

### Help System

| Command | Action |
|---|---|
| `:help` | Open the Neovim manual |
| `:help {topic}` | Search help for a topic (e.g., `:help motion`) |
| `:help vim-diff` | Differences between Vim and Neovim |
| `<leader>sh` | Search help with Telescope (easier) |
| `K` | In Normal mode over a vim keyword, open its help |

### Misc

| Command | Action |
|---|---|
| `:checkhealth` | Run Neovim health checks (use this to debug issues) |
| `:checkhealth lazy` | Health check for lazy.nvim |
| `:messages` | Show past messages/errors |
| `:verbose map <key>` | Find out where a keybinding is defined |
| `:map` | List all current keybindings |
| `:set {option}?` | Show the current value of an option |
| `:source %` | Reload the current file as Lua/Vimscript |
| `:Tutor` | Built-in interactive Vim tutorial (~30 min, very worth it) |

### Macros

| Key | Action |
|---|---|
| `q{a-z}` | Start recording a macro into register letter |
| `q` | Stop recording |
| `@{a-z}` | Play back macro |
| `@@` | Repeat last macro |
| `5@a` | Run macro `a` five times |

---

## 25. Installed Plugins Summary

| Plugin | Purpose |
|---|---|
| `NMAC427/guess-indent.nvim` | Auto-detects indentation style per file |
| `lewis6991/gitsigns.nvim` | Git change indicators in the gutter |
| `folke/which-key.nvim` | Shows pending keybinds in a popup |
| `nvim-telescope/telescope.nvim` | Fuzzy finder for files, text, and more |
| `nvim-telescope/telescope-fzf-native.nvim` | Faster fuzzy sorting for Telescope |
| `nvim-telescope/telescope-ui-select.nvim` | Uses Telescope for code actions picker |
| `neovim/nvim-lspconfig` | LSP client configuration |
| `mason-org/mason.nvim` | LSP/formatter/linter installer UI |
| `WhoIsSethDaniel/mason-tool-installer.nvim` | Auto-installs tools configured in init.lua |
| `j-hui/fidget.nvim` | LSP progress spinner (bottom right) |
| `saghen/blink.cmp` | Autocomplete engine |
| `L3MON4D3/LuaSnip` | Snippet engine (used by blink.cmp) |
| `stevearc/conform.nvim` | Code formatter (runs stylua, prettier, etc.) |
| `folke/tokyonight.nvim` | Color scheme (tokyonight-night) |
| `folke/todo-comments.nvim` | Highlights TODO/FIXME/HACK/etc. in comments |
| `nvim-mini/mini.nvim` | Collection: mini.ai, mini.surround, mini.statusline |
| `nvim-treesitter/nvim-treesitter` | Syntax highlighting and code parsing |
| `folke/snacks.nvim` | Terminal provider (used by claudecode) |
| `coder/claudecode.nvim` | Claude Code CLI integration |

---

## Quick Reference Card

```
MODES:  Normal(Esc)  Insert(i/a/o)  Visual(v/V/C-v)  Command(:)

MOVE:   h j k l      w b e W B E     0 ^ $     gg G     { }
        f/t{char}    <C-d> <C-u>     <C-o> <C-i>  (jump list)

EDIT:   i a o I A O s S C    (enter insert)
        d y c > < =          (operators — combine with motion)
        dd yy cc             (line shorthand)
        . u <C-r>            (repeat, undo, redo)
        x r{c} J             (delete char, replace, join)

LEADER (Space):
  sf=files  sg=grep  s.=recent  <space>=buffers  sh=help
  /=buffer-search  f=format  q=quickfix
  ac=Claude  af=Claude-focus  as=send-sel  aa=add-file

LSP:    grd=def  grr=refs  grn=rename  gra=action  K=hover

SPLIT:  <C-w>s/v  <C-h/j/k/l>=navigate  <C-w>=equal

SURROUND: sa{motion}{char}  sd{char}  sr{old}{new}
```
