# Vim Beginner Tips

A quick-reference guide for getting comfortable with Vim.

## 1. Modes — the core concept

Vim has different modes, and almost all confusion for beginners comes from not knowing which mode you're in.

- **Normal mode** (default) — for navigation and commands. Press `Esc` to get here from anywhere.
- **Insert mode** — for typing text. Enter with `i`, `a`, `o`, etc.
- **Visual mode** — for selecting text. Enter with `v`, `V`, or `Ctrl-v`.
- **Command-line mode** — for commands like save/quit. Enter with `:`.

> **Golden rule:** If you're not sure what mode you're in, hit `Esc`. You're always safe in Normal mode.

## 2. Exiting Vim (the classic struggle)

| Command | Action |
|---|---|
| `:w` | Save (write) |
| `:q` | Quit |
| `:wq` or `:x` | Save and quit |
| `:q!` | Quit without saving |
| `ZZ` | Save and quit (Normal mode shortcut) |

## 3. Basic movement (Normal mode)

- `h` `j` `k` `l` — left, down, up, right
- `w` — jump to start of next word
- `b` — jump to start of previous word
- `e` — jump to end of word
- `0` — start of line
- `^` — first non-blank character of line
- `$` — end of line
- `gg` — go to top of file
- `G` — go to bottom of file
- num G - this can be used to jump to that line (num)
- `:n` — go to line number `n`

## 4. Entering Insert mode

- `i` — insert before cursor
- `a` — insert after cursor
- `I` — insert at start of line
- `A` — insert at end of line
- `o` — open a new line below and insert
- `O` — open a new line above and insert

## 5. Editing basics

- `x` — delete character under cursor
- `dd` — delete (cut) current line
- `yy` — yank (copy) current line
- `p` — paste after cursor / below line
- `P` — paste before cursor / above line
- `u` — undo
- `Ctrl-r` — redo
- `.` — repeat last change (very powerful!)

## 6. Combining commands (operator + motion)

Vim commands often follow the pattern **operator + motion**:

- `dw` — delete to end of word
- `d$` — delete to end of line
- `d0` — delete to start of line
- `dG` — delete to end of file
- `yw` — yank a word
- `ciw` — change inner word (delete word, enter Insert mode)
- `di"` — delete inside quotes
- `2dd` — delete 2 lines (numbers work as multipliers)

## 7. Search and replace

- `/pattern` — search forward for `pattern`
- `?pattern` — search backward
- `n` — repeat search in same direction
- `N` — repeat search in opposite direction
- `:%s/old/new/g` — replace all occurrences of `old` with `new` in the file
- `:%s/old/new/gc` — same, but confirm each replacement

## 8. Visual mode basics

- `v` — start character-wise selection
- `V` — start line-wise selection
- `Ctrl-v` — start block (column) selection
- After selecting: `d` to delete, `y` to yank, `>` or `<` to indent/outdent

## 9. Useful quality-of-life tips

- `set number` in your config (or `:set number`) shows line numbers.
- `:noh` clears search highlighting.
- `Ctrl-o` / `Ctrl-i` jump backward/forward through your cursor history.
- Use `%` to jump between matching brackets/parens.
- Combine counts with motions/commands, e.g. `5j` moves down 5 lines, `3dd` deletes 3 lines.

## 10. Practice

- Run `vimtutor` in your terminal — it's a built-in interactive tutorial and the best starting point.
- Don't try to learn everything at once. Master movement and `i`/`Esc`/`:wq` first, then add commands gradually.
- Resist the urge to use arrow keys — stick to `hjkl` until it becomes muscle memory.

---
*Tip: Keep this file open in a split (`:vs tips.md`) while you practice in another buffer.*


## moving the lines

---

In the normal mode to cut and past the whole line use the command below


:m 1  -> for absolute motion this will cut the line to line 1
:m .-2/+2 -> this is relative using . we can move the line +- two steps above or below
