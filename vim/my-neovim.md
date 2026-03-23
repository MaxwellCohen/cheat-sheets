# Neovim Cheat Sheet (LazyVim)

## Key Legend

| Symbol    | Meaning          |
| --------- | ---------------- |
| `⌃`       | Control (^)      |
| `⌥`       | Option/Alt (⌥)   |
| `⇧`       | Shift (⇧)        |
| `⎋`       | Escape           |
| `⇥`       | Tab              |
| `␣`       | Space            |
| `<SPACE>` | Leader key (␣)   |
| `\ `      | Local leader key |

## Leader Key

- `<SPACE>` — global leader
- `\` — local leader (filetype-specific)

---

## Movement

| Key         | Action                                 |
| ----------- | -------------------------------------- |
| `j` / `k`   | Down / Up (wraps across wrapped lines) |
| `gj` / `gk` | Down / Up (visual line only)           |
| `⌃h`        | Go to left window                      |
| `⌃j`        | Go to lower window                     |
| `⌃k`        | Go to upper window                     |
| `⌃l`        | Go to right window                     |
| `⌃⇧↑`       | Increase window height                 |
| `⌃⇧↓`       | Decrease window height                 |
| `⌃⇧←`       | Decrease window width                  |
| `⌃⇧→`       | Increase window width                  |
| `⌥j`        | Move line down                         |
| `⌥k`        | Move line up                           |
| `⇧h`        | Previous buffer                        |
| `⇧l`        | Next buffer                            |

---

## Buffers

| Key            | Action                   |
| -------------- | ------------------------ |
| `⇧h`           | Previous buffer          |
| `⇧l`           | Next buffer              |
| `[b`           | Previous buffer          |
| `]b`           | Next buffer              |
| `<SPACE>bb`    | Switch to other buffer   |
| `` <SPACE>` `` | Switch to other buffer   |
| `<SPACE>bd`    | Delete buffer            |
| `<SPACE>bo`    | Delete other buffers     |
| `<SPACE>bD`    | Delete buffer and window |

---

## Windows & Splits

| Key         | Action               |
| ----------- | -------------------- |
| `<SPACE>-`  | Split window below   |
| `<SPACE>\|` | Split window right   |
| `<SPACE>wd` | Delete window        |
| `<SPACE>wm` | Zoom / unzoom window |
| `<SPACE>uz` | Zen mode             |

---

## Tabs

| Key                 | Action           |
| ------------------- | ---------------- |
| `<SPACE><TAB>l`     | Last tab         |
| `<SPACE><TAB>o`     | Close other tabs |
| `<SPACE><TAB>f`     | First tab        |
| `<SPACE><TAB><TAB>` | New tab          |
| `<SPACE><TAB>]`     | Next tab         |
| `<SPACE><TAB>d`     | Close tab        |
| `<SPACE><TAB>[`     | Previous tab     |

---

## Files (fzf-lua)

| Key              | Action                |
| ---------------- | --------------------- |
| `<SPACE>,`       | Switch buffer         |
| `<SPACE>:`       | Command history       |
| `<SPACE><SPACE>` | Find files (root dir) |
| `<SPACE>fb`      | Buffers               |
| `<SPACE>fB`      | Buffers (all)         |
| `<SPACE>fc`      | Find config file      |
| `<SPACE>ff`      | Find files (root dir) |
| `<SPACE>fF`      | Find files (cwd)      |
| `<SPACE>fg`      | Find git files        |
| `<SPACE>fr`      | Recent files          |
| `<SPACE>fR`      | Recent files (cwd)    |

---

## Search & Grep (fzf-lua)

| Key           | Action                            |
| ------------- | --------------------------------- |
| `<SPACE>/`    | Live grep (root dir)              |
| `<SPACE>sg`   | Grep (root dir)                   |
| `<SPACE>sG`   | Grep (cwd)                        |
| `<SPACE>sw`   | Grep word under cursor (root dir) |
| `<SPACE>sW`   | Grep word under cursor (cwd)      |
| `ss` (visual) | Grep selection (root dir)         |
| `sS` (visual) | Grep selection (cwd)              |
| `<SPACE>s"`   | Registers                         |
| `<SPACE>s/`   | Search history                    |
| `<SPACE>sa`   | Auto commands                     |
| `<SPACE>sb`   | Buffer lines                      |
| `<SPACE>sc`   | Command history                   |
| `<SPACE>sC`   | Commands                          |
| `<SPACE>sd`   | Diagnostics (workspace)           |
| `<SPACE>sD`   | Diagnostics (buffer)              |
| `<SPACE>sh`   | Help tags                         |
| `<SPACE>sH`   | Highlight groups                  |
| `<SPACE>sj`   | Jumplist                          |
| `<SPACE>sk`   | Key maps                          |
| `<SPACE>sl`   | Location list                     |
| `<SPACE>sM`   | Man pages                         |
| `<SPACE>sm`   | Jump to mark                      |
| `<SPACE>sR`   | Resume last picker                |
| `<SPACE>sq`   | Quickfix list                     |
| `<SPACE>ss`   | LSP document symbols              |
| `<SPACE>sS`   | LSP workspace symbols             |
| `<SPACE>st`   | Todo comments                     |
| `<SPACE>sT`   | Todo / Fix / Fixme                |
| `<SPACE>uC`   | Colorschemes with preview         |

---

## LSP (fzf-lua)

| Key  | Action                |
| ---- | --------------------- |
| `gd` | Go to definition      |
| `gr` | References            |
| `gI` | Go to implementation  |
| `gy` | Go to type definition |

---

## Git (fzf-lua + lazygit)

| Key         | Action                       |
| ----------- | ---------------------------- |
| `<SPACE>gg` | Lazygit (root dir)           |
| `<SPACE>gG` | Lazygit (cwd)                |
| `<SPACE>gL` | Git log (cwd)                |
| `<SPACE>gl` | Git log (root dir)           |
| `<SPACE>gb` | Git blame line               |
| `<SPACE>gf` | Git current file history     |
| `<SPACE>gB` | Git browse (open in browser) |
| `<SPACE>gY` | Git browse (copy URL)        |
| `<SPACE>gc` | Git commits                  |
| `<SPACE>gs` | Git status                   |
| `<SPACE>gS` | Git stash                    |

---

## Refactoring (refactoring.nvim)

| Key         | Action                   |
| ----------- | ------------------------ |
| `<SPACE>rs` | Select refactor          |
| `<SPACE>rf` | Extract function         |
| `<SPACE>rF` | Extract function to file |
| `<SPACE>rx` | Extract variable         |
| `<SPACE>ri` | Inline variable          |
| `<SPACE>rb` | Extract block            |
| `<SPACE>rP` | Debug print (below)      |
| `<SPACE>rp` | Debug print variable     |
| `<SPACE>rc` | Debug cleanup            |

---

## Diagnostics

| Key         | Action                      |
| ----------- | --------------------------- |
| `<SPACE>cd` | Line diagnostics (floating) |
| `]d` / `[d` | Next / previous diagnostic  |
| `]e` / `[e` | Next / previous error       |
| `]w` / `[w` | Next / previous warning     |

---

## Quickfix & Location

| Key         | Action                        |
| ----------- | ----------------------------- |
| `<SPACE>xq` | Toggle quickfix list          |
| `<SPACE>xl` | Toggle location list          |
| `[q` / `]q` | Previous / next quickfix item |

---

## Terminal

| Key         | Action                    |
| ----------- | ------------------------- |
| `<SPACE>fT` | Terminal (cwd)            |
| `<SPACE>ft` | Terminal (root dir)       |
| `⌃/`        | Focus terminal (root dir) |

---

## Toggles

| Key         | Action                              |
| ----------- | ----------------------------------- |
| `<SPACE>ub` | Toggle dark/light background        |
| `<SPACE>uB` | Toggle background (inverse)         |
| `<SPACE>us` | Toggle spelling                     |
| `<SPACE>uw` | Toggle wrap                         |
| `<SPACE>uL` | Toggle relative numbers             |
| `<SPACE>ul` | Toggle line numbers                 |
| `<SPACE>uc` | Toggle conceal level                |
| `<SPACE>uA` | Toggle tabline                      |
| `<SPACE>ua` | Toggle animations                   |
| `<SPACE>uD` | Toggle dimming                      |
| `<SPACE>uT` | Toggle Treesitter                   |
| `<SPACE>ug` | Toggle indent guides                |
| `<SPACE>uh` | Toggle inlay hints                  |
| `<SPACE>uf` | Toggle auto format                  |
| `<SPACE>uF` | Toggle format (inverse)             |
| `<SPACE>ur` | Redraw / clear search / diff update |

---

## General

| Key           | Action                                |
| ------------- | ------------------------------------- |
| `⎋`           | Escape (also clears search highlight) |
| `⌃s`          | Save file                             |
| `<SPACE>fn`   | New file                              |
| `<SPACE>K`    | Keywordprg (look up word)             |
| `<SPACE>ui`   | Inspect position (highlight groups)   |
| `<SPACE>uI`   | Inspect syntax tree                   |
| `<SPACE>l`    | Open Lazy plugin manager              |
| `<SPACE>L`    | LazyVim changelog                     |
| `<SPACE>qq`   | Quit all                              |
| `<SPACE>dp`   | Toggle profiler                       |
| `<SPACE>dpp`  | Toggle profiler                       |
| `<SPACE>dph`  | Toggle profiler highlights            |
| `\ ` + `r`    | Run Lua code (in Lua files)           |

---

## Indentation & Comments

| Key                | Action                               |
| ------------------ | ------------------------------------ |
| `<` / `>` (visual) | Indent left / right (stay in visual) |
| `gco`              | Add comment below                    |
| `gcO`              | Add comment above                    |
| `gcc`              | Toggle line comment                  |
