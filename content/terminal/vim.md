---
Title: Vim Cheatsheet
Date: 2022-11-03
Modified: 2024-01-08
Tags: terminal, vim
Summary: Cheatsheet for vim/nvim
---

## Normal Mode:
| Action | Key |
|-|-|
| Cancel Action | <kbd>Ctrl</kbd>+<kbd>c</kbd> |
| Move by character | <kbd>h</kbd>; <kbd>j</kbd>; <kbd>k</kbd>; <kbd>l</kbd> |
| Move to start of word | <kbd>w</kbd>; <kbd>b</kbd> |
| Move to start of WORD | <kbd>W</kbd>; <kbd>B</kbd> |
| Move to end of word | <kbd>e</kbd>; <kbd>ge</kbd> |
| Move to end of WORD | <kbd>E</kbd>; <kbd>gE</kbd> |
| Find char (forward, backward) | <kbd>f</kbd>+{character}; <kbd>F</kbd>+{character} |
| Find until (forward; backward) | <kbd>t</kbd>+{character}; <kbd>T</kbd>+{character} |
| Find char loop (next, prev) | <kbd>;</kbd>; <kbd>,</kbd> |
| Go to first char (Home) | <kbd>0</kbd> |
| Go to first non-blank char | <kbd>^</kbd> |
| Go to end of line (End) | <kbd>$</kbd> |
| Go to end non-blank char | <kbd>g</kbd>,<kbd>_</kbd> |

## Neovim keybinds

- **Leader**: <space>
- **Netrw** (`:Ex`) : `<>e`
- 

**Telescope**: extendable fuzzy finder
- Find files: `<>fj` (in current dir?)
- Find files in git project: `^p`
- Find from grep string: `<>fs`

**Treesitter**: language parser provider

**undotree**: a tree of undo histories

**lsp-zero**: 

**mason**: more language servers?