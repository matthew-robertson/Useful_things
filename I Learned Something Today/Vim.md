## File Navigation
1. `:tabnew /file/path` will open a (possibly new) file at the given path, as a new tab.
1. `:tabn` `:tabp`, `:tabm [tab#]` willwitch between tabs.
1. `:n` and co will switch between files.
1. CtrlP is a godsend. Use `<c-t>` to open the found file in a new tab

## Windows
1. `ctrl+w v` will vertically split, opening a new window, duplicating the current file.
1. `ctrl+w s` will horizontally slit.
1. `:vsp /file/path` will vertically split, opening the given file in the new window.
1. `:sp /file/path` will horizontally split.
1. `ctrl+w [hjkl]` to navigate between splits/windows.

## In-File Navigation
1. `/` will search, using a regex for the next occurrence of what you search for. `n` afterwards will go to the next occurrence.
1. `f` will find the next occurrence of the character you type afterwards.
1. `t` will "move 'till" the character instead, moving the cursor to the character before the one you search for.
1. `T` and `F` behave like their lowercase versions, but backwards.
1. `%` will jump to the matching brace/bracket.
1. `ctrl+y` and `ctrl+e` will expose one more line above and below the current frame, respectively. Saves you `j`ing all the way across the screen.
1. `w` will jump to the start of the next word. EG: "lo**r**em ipsum" -> "lorem **i**psum".
1. `e` will jump to the end of the current word (or the end of the next one). EG: "lo**r**em ipsum" -> "lore**m** ipsum" -> "lorem ipsu**m**".
1. `b` is like `e`, but to the start.
1. `W`, `E`, and `B` (and probably others) are like lowercase, but they only recognise whitespace as a wordbreak. 
1. `$` will jump to the end of the current line.
1. `*` is a shortcut that searchs for the word under the cursor.

## Entering Insert Mode
1. `o` and `O` open a new line below and above the current line, respectively
1. `a` inserts after the current character, `i` inserts before it.
1. `c` can be used to "change" chunks of text. EX: `ci"` will change things between the next two quotations, or `cf:` will change from the cursor to the next colon.
1. `r` can be used to replace a single character.

## Text Manipulation
1. `Y` will yank/copy an entire line.
1. `y[movement]` will yank based on the movement. E.G.: `yiw` will yank the word you're in.
1. `p` will paste, `P` will insert a new line first.
1. `ctrl+t` and `ctrl+d`, used in insert mode, will indent and unindent the current line respectively.
1. `>` in normal mode will indent. `>>` will indent the current line, `>}` will indent the paragraph.
1. `v` will enter visual mode, useful-ish for selecting blocks to indent, text to yank, or whatever,.
