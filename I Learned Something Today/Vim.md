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
1.`/` will search, using a regex for the next occurrence of what you search for. `n` afterwards will go to the next occurrence.
1.`f` will find the next occurrence of the character you selected.
1. `%` will jump to the matching brace/bracket.

## Entering Insert Mode
1. `o` and `O` open a new line below and above the current line, respectively
1. `a` inserts after the current character, `i` inserts before it.
1. `c` can be used to "change" chunks of text. EX: `ci"` will change things between the next two quotations, or `cf:` will change from the cursor to the next colon.
1. `r` can be used to replace a single character.

## Text Manipulation
1. `Y` will yank/copy and entire line.
1. `p` will paste, `P` will insert a new line first.
