## File Navigation
1. `:tabnew /file/path` will open a (possibly new) file at the given path, as a new tab.
1. `:tabn` `:tabp`, `:tabm [tab#]` will switch between tabs.
1. `:n` and co will switch between files.
1. CtrlP is a godsend. Use `<c-t>` to open the found file in a new tab
1. `:e /file/path` will activate a new buffer at the given filepath.
1. `:bd` will close the active buffer. Adding a number afterwards will close a specific buffer. You can also use tab completion and do it by name.
1. `:bp`, `:bn` switch between buffers.
1. `:buffers` will list all buffers currently in memory.
1. `ls` is a shortcut for `:buffers`. Use it instead.
1. `:b` will go to the specific buffer, using a buffer number or a partial filename
1. `:b#` will go to the alternate buffer (probably the previous one)

## Windows
1. `ctrl+w v` will vertically split, opening a new window, duplicating the current file.
1. `ctrl+w s` will horizontally split.
1. `:vsp /file/path` will vertically split, opening the given file in the new window.
1. `:sp /file/path` will horizontally split.
1. `ctrl+w [hjkl]` to navigate between splits/windows.
1. Splits can be re-equalised (after changing the terminal size or whatever) using `Ctrl+w =`.
1. If you need to open a bunch of files as splits quickly, use something like `:args path/to/*.py | vertical all`. 
1. An alternative is apparently `:vert sf path/to/*.py`.

## In-File Navigation
1. Don't move a character at a time, it's dumb and slow. Use word motions, searches, and similar to move around.
1. `/` will search using a regex for the next occurrence of what you search for. `n` afterwards will go to the next occurrence.
1. `?` works like `/`, but backwards.
1. `{` and `}` will move to the start of the next, or end of the current, paragraph/code block.
1. `ctrl+f` and `ctrl+b` will  move forward/backward one screenful.
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
1. `0` will jump to the start of the current line, and `^` jumps to the first character in the line.'
1. `*` is a shortcut that searchs for the word under the cursor.
1. `19gg` will jump to line 19.
1. `:19` and `:19G` will do the same.
1. `G` will jump to the end of the file.
1. `gg` will jump to the start of the file.
1. `` `. `` jumps to the last edited line. Much nicer than `u` + `ctrl+r`.jj
1. `g` makes for a weird prefix for commands. `g;` will jump backwards through to lines you've edited.
1. Stop moving around in insert mode, you dummy. All the tools are in normal mode, so use them.
1. `zz` redraws the vim window so your cursor is in the middle of the screen, which is cool as heck. 

## Entering Insert Mode
1. `o` and `O` open a new line below and above the current line, respectively
1. `a` inserts after the current character, `i` inserts before it.
1. `A` will insert at the end of the line. `I` inserts at the start of it. Whaaaaa
1. `c` can be used to "change" chunks of text. EX: `ci"` will change things between the next two quotations, or `cf:` will change from the cursor to the next colon.
1. `r` can be used to replace a single character.
1. `s` is similar to `r`. It deletes the current character and puts you into insert mode.

## Text Manipulation
1. `Y` will yank/copy an entire line.
1. `y[movement]` will yank based on the movement. E.G.: `yiw` will yank the word you're in.
1. `*` refers to the system register, so `"*y` will yank into your actual clipboard.
1. `:r` is used to copy a bunch of text into the current buffer. `:r!ls` will copy the result of ls, one per line, into the page at the cursor.
1. `p` will paste, `P` will insert a new line first.
1. `ctrl+t` and `ctrl+d`, used in insert mode, will indent and un-indent the current line respectively.
1. `>` in normal mode will indent. `>>` will indent the current line, `>}` will indent the paragraph.
1. `v` will enter visual mode, useful-ish for selecting blocks to indent, text to yank, or whatever,.
1. `:81,91y` will yank between lines 81 and 91.
1. Search and replace is done with `:%s/old/new/`, adding a `g` to the end makes it global, `c` makes it interactive.
1. Search and replace can be made global by default using `set gdefault`, which then inverts the use of `g` in searches. [Vim Annoyances](https://sanctum.geek.nz/arabesque/vim-annoyances/)
1. `ctrl+n` will bring up the autocomplete word suggestions when in insert mode.
1. Manipulate buffers by prefixing a command with `"a` (or some other buffer name). `"aY` `"aP" Will yank the current line into the `a` buffer, then paste it. This gives you access to multiple clipboards.
1. When you yank something, it goes into the `0` buffer, unlike on deletion/change/whatever. You can imagine how this is useful.
1. `:sort` with some kind of selection (visual works well), will sort the selected lines alphabetically.

## General Editing
1. `u` will undo a change
1. `ctrl+r` will redo a change.
1. If you're holding down a key, there's probably a better tool to use.
1. `.` will repeat the previous command.
1. `:set spell` lets you enable spell checking, using the system dictionary.
1. `:syntax enable` will turn on syntax highlighting, but I don't really know how it does anything.
1. `:set encoding=utf-8` will set the encoding for the file, to handle unicode. Neat stuff.
1. `smartcase` lets your patterns only care about case if at least one char is uppercase.
1. `ignorecase` completely ignores case in searches, which can be handy.
1. Tips for managing a `.vimrc` across many different machines are [here](https://sanctum.geek.nz/arabesque/gracefully-degrading-vimrc/)
1. Converting long `.vimrc` files into `.vim` directories, some useful config tips in [here](https://vimways.org/2018/from-vimrc-to-vim/) too.
1. Vim supports syntax folding, neat stuff.
1. Like everything else in Vim, macros are just text. They're stored in the same buffers you yank/paste from, they're just interpreted as commands instead of characters to print.
1. [The leader key](https://medium.com/usevim/vim-101-what-is-the-leader-key-f2f5c1fa610f) is a customizable modifier key, essentially: Vim doesn't map anything to it, unlike pretty much everything else. By default it's `\`, but you can change it using `let mapleader=","` or whatever you want to set it to. It also doesn't need to be held. Once pressed, Vim will wait ~1 second for a command's input.
1. `map` and `noremap` do basically the same thing, but `noremap` is non-recursive/non-expanding. a handful of characters can be prefixed onto either to specify the mode the mapping should apply to `i` - insert mode, `n` - normal mode, etc...
1. In command mode, `!` indicates the start of an external command. It will pop you out into the terminal to see the result of running it. Handy stuff for all the searching you do.
1. `gf` will open the file path you have selected apparently, and `gx` will open a link in a browser? At least, it's supposed to.
1. The `n` flag on the substitute command will tell you how many occurences of a given pattern are in a buffer. ex: `:%s/pattern//gn`
1. Want to search for something you've yanked/got in a buffer? `/ctrl+r{buffer}` will do the job. This was super useful for building macros when translating countries.
1. `:reg` will list all the named buffers, and what they contain.
1. `q{a-z}` will record to the chosen buffer. `@{a-z}` will execute the selected buffer as a macro. You can execute macros in other macros, so it's useful to build things up in pieces.
