Impression:
Look, I didn't think I was *good* at using the shell to get things done, but I thought I at least knew a little bit. This has made it clear I hadn't even scratched the surface: after passing the - extremely meagre - stuff I already knew, more or less every page had something on it that I was mad I hadn't learned previously. Written in a way that's pleasant to read, as well.

Quotes/Annotations:
1. "You can connect these two commands because ls writes to stdout and less can read from stdin. Use a pipe to send the output of ls to the input of less: `$ ls -l /bin | less`. This combined command displays the directory’s contents one screenful at a time. The vertical bar (`|`) between the commands is the Linux pipe symbol. It connects the first command’s stdout to the next command’s stdin. Any command line containing pipes is called a pipeline."
1. "Commands generally are not aware that they’re part of a pipeline. `ls` believes it’s writing to the display, when in fact its output has been redirected to `less`. And `less` believes it’s reading from the keyboard when it’s actually reading the output of `ls`."
1. "The wc command prints the number of lines, words, and characters in
a file:
    ```
    $ wc animals.txt
    7 51 325 animals.txt
    ```
    `wc` reports that the file `animals.txt` has 7 lines, 51 words, and 325 characters. If you count the characters by eye, including spaces and tabs, you’ll find only 318 characters, but `wc` also includes the invisible newline character that ends each line."
1. "The options `-l`, `-w`, and `-c` instruct `wc` to print only the number of lines, words, and characters, respectively: `$ wc -l animals.txt`"
1. 'Let’s use `ls` to list the contents of the current directory and pipe them to `wc` to count lines. This pipeline answers the question, “How many files are visible in my current directory?”'
    ```
    ls -1 | wc -l
    4
    ```
1. "Unlike virtually every other Linux command, `ls` is aware of whether `stdout` is the screen or whether it’s been redirected (to a pipe or otherwise). The reason is user-friendliness. When `stdout` is the screen, `ls` arranges its output in multiple columns for convenient reading:
    ```
    $ ls /bin
    bash dir kmod networkctl red tar
    bsd-csh dmesg less nisdomainname rm tempfile
    ⋮
    ```
    When stdout is redirected, however, `ls` produces a single column."
1. "Force `ls` to print a single column with the `-1` option, or force multiple columns with the `-C` option."
1. "The `cut` command prints one or more columns from a file."
1. "`cut` provides two ways to define what a “column” is. The first is to cut by field (`-f`), when the input consists of strings (fields) each separated by a single tab character... You can also `cut` multiple fields, either by separating their field numbers with commas:
    ```
    $ cut -f1,3 animals.txt | head -n3
    python 2010
    snail 2005
    alpaca 2012
    ```
    or by numeric range: `$ cut -f2-4 animals.txt | head -n3` ... The second way to define a “column” for cut is by character position, using the `-c` option. Print the first three characters from each line of the file, which you can specify either with commas (1,2,3) or as a range (1-3): `$ cut -c1-3 animals.txt`"
1. 'Then pipe the results to `cut` again, using the option `-d` (meaning “delimiter”) to change the separator character to a comma instead of a tab, to isolate the authors’ last names: `$ cut -f4 animals.txt | cut -d, -f1`'
1. '[Grep] can also print lines that don’t match a given string, with the `-v` option. Notice the lines containing “Nutshell” are absent: `$ grep -v Nutshell animals.txt`'
1. "Finally, count lines with `wc`, and you have your answer, produced by a four-command pipeline—`/usr/lib` contains 145 subdirectories: 
    ```
    $ ls -l /usr/lib | cut -c1 | grep d | wc -l
    145
    ```
1. "The `sort` command reorders the lines of a file into ascending order (the default sort can order the lines alphabetically (the default) or numerically (with the `-n` option)"
1. "`sort` and `head` are powerful partners when working with numeric data, one value per line. You can print the maximum value by piping the data to: `... | sort -nr | head -n1` and print the minimum value with: ``... | sort -n | head -n1`"
1. "The `-w` option instructs grep to match full words only, not partial words"
1. "The uniq command detects repeated, adjacent lines in a file. By default, it removes the repeats. You can also count occurrences with the `-c` option: `$ uniq -c letters`"
1. "This sort of step-by-step pipeline construction is not just an educational exercise. It’s how Linux experts actually work."
1. "Suppose you’re in a directory full of JPEG files and you want to know if any are duplicates: You’ll need another command, `md5sum`, which examines a file’s contents and computes a 32-character string called a checksum:
    ```
    $ md5sum image001.jpg
    146b163929b6533f02e91bdf21cb9563 image001.jpg
    ```
    A given file’s checksum, for mathematical reasons, is very, very likely to be unique. If two files have the same checksum, therefore, they are almost certainly duplicates."
1. "Any command that reads `stdin` or writes `stdout` can participate in pipelines. As you learn more commands, you can apply the general concepts from this chapter to forge your own powerful combinations."
1. "bash and other shells do much more than simply run commands. For example, when a command includes a wildcard (`*`) to refer to multiple files at once: 
    ```
    $ ls *.py
    data.py main.py user_interface.py
    ```
    the wildcard is handled entirely by the shell, not by the program `ls`. The shell evaluates the expression `*.py` and invisibly replaces it with a list of matching filenames before `ls` runs. In other words, `ls` never sees the wildcard."
1. "The shell also handles the pipes you saw in Chapter 1. It redirects `stdin` and `stdout` transparently so the programs involved have no idea they are communicating with each other."
1. "[In addition to the `*` wildcard, many] users have also seen the question mark (`?`) special character, which matches any single character (except a leading dot)."
1. "Any characters, not just digits, may appear within the square brackets for matching. For example, filenames that begin with a capital letter, contain an underscore, and end with an `@` symbol would be matched by the shell in this command: `$ ls [A-Z]*_*@`"
1. "If a pattern doesn’t match any files, the shell leaves it unchanged to be passed literally as a command argument. In the following command, the pattern `*.doc` matches nothing in the current directory, so `ls` looks for a filename literally named `*.doc` and fails: `$ ls *.doc`"
1. "When working with file patterns, two points are vitally important to remember. The first, as I’ve already emphasized, is that the shell, not the invoked program, performs the pattern matching. I know I keep repeating this, but I’m frequently surprised by how many Linux users don’t know it and develop superstitions about why certain commands succeed or fail. The second important point is that shell pattern matching applies only to file and directory paths. It doesn’t work for usernames, hostnames, and other types of arguments that certain commands accept. You also cannot type (say) `s?rt` at the beginning of the command line and expect the shell to run the `sort` program."
1. 'All programs that accept filenames as arguments automatically “work” with pattern matching, because the shell evaluates the patterns before the program runs. This is true even for programs and scripts you write yourself. For example, if you wrote a program `english2swedish` that translated files from English to Swedish and accepted multiple filenames on the command line, you could instantly run it with pattern matching: `$ english2swedish *.txt`.'
1. "The easiest way to watch the shell evaluate a command line is to run the `echo` command, which simply prints its arguments (after the shell is finished evaluating them)."
1. "Variables like `USER` and `HOME` are predefined by the shell. Their values are set automatically when you log in. (More on this process later.) Traditionally, such predefined variables have uppercase names."
1. "When defining a variable, no spaces are permitted around the equals sign. If you forget, the shell will assume (wrongly) that the first word on the command line is a program to run, and the equals sign and
value are its arguments, and you’ll see an error message":
    ```
    $ work = $HOME/Projects The shell assumes "work" is a command
    work: command not found
    ```
1. "This behavior is extremely important to understand, especially as we delve into more complicated commands. The shell evaluates the variables in a command—as well as patterns and other shell constructs—before executing the command."
1. "You can define an alias that has the same name as an existing command, effectively replacing that command in your shell. This practice is called shadowing the command. Suppose you like the `less` command for reading files, but you want it to clear the screen before displaying each page. This feature is enabled with the `-c` option, so define an alias called “less” that runs `less -c`"
1. "Aliases take precedence over commands of the same name"
1. "To list a shell’s aliases and their values, run alias with no arguments: `$ alias`."
1. "To delete an alias from a shell, run `unalias`: `$ unalias g`."
1. "You have just redirected `stdout` to the file `outfile` instead of the display. If the file `outfile` doesn’t exist, it’s created. If it does exist, redirection overwrites its contents. If you’d rather append to the output file rather than overwrite it, use the symbol `>>` instead."
1. "Linux commands can produce more than one stream of output. In addition to `stdout`, there is also `stderr` (pronounced “standard error” or “standard err”), a second stream of output that is traditionally reserved for error messages. The streams `stderr` and `stdout` look identical on the display, but internally they are separate. You can redirect `stderr` with the symbol `2>` followed by a filename"
1. "To redirect both `stdout` and `stderr` to the same file, use `&>` followed by a filename"
1. "Single quotes tell the shell to treat every character in a string literally, even if the character ordinarily has special meaning to the shell, such as spaces and dollar signs... Double quotes tell the shell to treat all characters literally except for certain dollar signs and a few others you’ll learn later... backslash, also called the escape character, tells the shell to treat the next character literally."
1. "A backslash at the end of a line disables the special nature of the invisible newline character, allowing shell commands to span multiple lines"
1. 'Final backslashes are great for making pipelines more readable, like this one from “Command #6: uniq”':
    ```
    $ cut -f1 grades \
    | sort \
    | uniq -c \
    | sort -nr \
    | head -n1 \
    ```
1. "A leading backslash before an alias escapes the alias, causing the shell to look for a command of the same name, ignoring any shadowing": 
    ```
    $ alias less="less -c" Define an alias
    $ less myfile Run the alias, which invokes less -c
    $ \less myfile Run the standard less command, not the alias
    ```
1. "How does the shell locate `ls` in the `/bin` directory? Behind the scenes, the shell consults a prearranged list of directories that it holds in memory, called a search path. The list is stored as the
value of the shell variable `PATH`:
    ```
    $ echo $PATH
    /home/smith/bin:/usr/local/bin:/usr/bin:/bin:/usr/games:/usr/lib/java/bin
    ```
    Directories in a search path are separated by colons (:). For a clearer view, convert the colons to newline characters by piping the output to the `tr` command, which translates one character into another (more details in Chapter 5):
    ```
    $ echo $PATH | tr : "\n"
    /home/smith/bin
    /usr/local/bin
    /usr/bin
    ```
1. "To locate a program in your search path, use the `which` command":
    ```
    $ which cp
    /bin/cp
    $ which which
    /usr/bin/which
    ```
1. "The shell consults directories in your search path from first to last when locating a program like `ls`."
1. "[instead of `which`, one can use] the more powerful (and verbose) `type` command, a shell builtin that also locates aliases, functions, and shell builtins":
    ```
    $ type cp
    cp is hashed (/bin/cp)
    $ type ll
    ll is aliased to ‘/bin/ls -l’
    ```
1. "Your search path may contain the same-named command in different directories, such as `/usr/bin/less` and `/bin/less`. The shell runs whichever command appears in the earlier directory in the path. By leveraging this behavior, you can override a Linux command by placing a same-named command in an earlier directory in your search path, such as your personal `$HOME/bin` directory."
1. "To see a shell’s history, run the `history` command, which is a shell builtin. The commands appear in chronological order with ID numbers for easy reference. The output looks something like this":
    ```
    $ history
    1000 cd $HOME/Music
    1001 ls
    ```
1. "To clear (delete) the history for the current shell, use the `-c` option: `$ history -c`"
1. "How many commands are stored in a shell’s history? The maximum is five hundred or whatever number is stored in the shell variable `HISTSIZE`, which you can change:
    ```
    $ echo $HISTSIZE
    500
    $ HISTSIZE=10000
    ```
    Computer memory is so cheap and plentiful that it makes sense to set `HISTSIZE` to a large number so you can recall and rerun commands from the distant past. (A history of 10,000 commands occupies only
about 200K of memory.) Or be daring and store unlimited commands by setting the value to `-1`."
1. "I launched a new interactive shell and it already has a history. Why? Whenever an interactive shell exits, it writes its history to the file `$HOME/.bash_history` or whatever path is stored in the shell variable `HISTFILE`... New interactive shells load this file on startup, so they immediately have a history. It’s a quirky system if you’re running many shells because they all write `$HISTFILE` on exit, so it’s a bit unpredictable which history a new shell will load."
1. "Are repeated commands appended to the history? The answer depends on the value of the variable `HISTCONTROL`. By default, if this variable is unset, then every command is appended. If the value is `ignoredups` (which I recommend), then repeated commands are not appended if they are consecutive (see `man bash` for other values): `$ HISTCONTROL=ignoredups`"
1. "variable `HISTFILESIZE` controls how many lines of history are written to the file. If you change `HISTSIZE` to control the size of the history in memory, consider updating `HISTFILESIZE` as well"
1. "History expansion is a shell feature that accesses the command history using special expressions. The expressions begin with an exclamation point, which traditionally is pronounced “bang.” For example, two
exclamation points in a row (“bang bang”) evaluates to the immediately previous command"
1. "To refer to the most recent command that began with a certain string, place an exclamation point in front of that string. So, to rerun the most recent `grep` command, run `!grep`.... To refer to the most recent command that contained a given string somewhere, not just at the beginning of the command, surround the string with question marks as well"
1. 'The shell appends exactly what you type [to your history], unevaluated. If you run `ls $HOME`, the history will contain `ls $HOME`, not `ls /home/smith` (There’s one exception: see “History Expressions Don’t Appear in the Command History”.)'
1. 'You can also retrieve a particular command from a shell’s history by its absolute position—the ID number to its left in the output of history. For example, the expression `!1203` (“bang 1023”) means “the command at position 1023 in the history”'
1. 'A negative value retrieves a command by its relative position in the history, rather than absolute position. For example, `!-3` (“bang minus three”) means “the command you executed three commands ago”'
1. "If you miscounted and typed `!-4` instead of `!-3`, you’d run `rm *` instead of the intended `head` command and delete files in your home directory by mistake! To mitigate this risk, append the modifier `:p`
to print the command from your history but not execute it:
    ```
    $ !-3:p
    head -n2 /etc/hosts Printed, not executed
    ```
    The shell appends the unexecuted command (head) to the history, so if it looks good, you can run it conveniently with a quick `!!`."
1. 'The shell appends commands to the history verbatim—unevaluated—as I mentioned in “Frequently Asked Questions About Command History”. The one exception to this rule is history expansion. Its expressions are always evaluated before they’re added to the command history' - This is because seeing a bunch of `!1023` and `!-4` in your history would be an unbelievable mess.
1. 'Some people refer to history expansion as “bang commands,” but expressions like `!!` and `!grep` are not commands. They are string expressions that you can place anywhere in a command. As a demonstration, use `echo` to print the value of `!!` on `stdout` without executing it, and count the number of words with `wc`: `echo !! | wc -w`."
1. "[Aliasing rm to always require a yes/no before acting] is cumbersome, however, because most of the time you might not want or need rm to prompt you. It also doesn’t work if you’re logged into another Linux machine without your aliases. I’ll show you a better way to avoid matching the wrong filenames with a pattern. The technique has two steps and relies on history expansion: 
    1. Verify. Before running `rm`, run `ls` with the desired pattern to see which files match.
        ```
        $ ls *.txt
        a.txt b.txt c.txt
        ```
    1. Delete. If the output of `ls` looks correct, run `rm !$` to delete the same files that were matched.
        ```
        $ rm !$
        rm *.txt
        ```
1. "The history expansion `!$` (“bang dollar”) means “the final word that you typed in the previous command.”... The shell also provides a history expansion `!*` (“bang star”), which matches all arguments you typed in the previous command, rather than just the final argument."
1. "I’ve assumed here that `cd $HOME/Finances/Bank` was the most recent matching command in the history. What if it’s not? What if you typed a whole bunch of commands that contain the same string? If so, the preceding incremental search would have displayed a different match, such as: `(reverse-i-search): cd $HOME/Music`. What now? You could type more characters to hone in on your desired command, but instead, press `Ctrl-R` a second time. This keystroke causes the shell to jump to the next matching command in the history"
1. "To recall the most recent string that you searched for and executed, begin by pressing `Ctrl-R` twice in a row."
1. "To stop an incremental search and continue working on the current command, press the Escape key, or `Ctrl-J`..."
1. "To run the [mistyped command] properly, you could recall it from the command history, cursor over to the mistake and fix it, but there’s a quicker way to accomplish your goal. Just type the old (wrong) text, the new (corrected) text, and a pair of carets (`^`), like this: `$ ^jg^jpg`.... Notice that the shell helpfully prints the new command before executing it, which is standard behavior for history expansion.... This technique changes only the first occurrence of the source string (jg) in the command."
1. "You may begin with any history expansion you like, such as `!md5sum`, which recalls the most recent command beginning with md5sum, and perform the same replacement of jg by jpg: `$ !md5sum:s/jg/jpg`"
1. "The most powerful way to edit a command line is with familiar keystrokes inspired by the text editors Emacs and Vim. If you’re already skilled with one of these editors, you can jump into this
style of command-line editing right away. The shell default is Emacs-style editing... If you prefer Vim-style editing, run the following command (or add it to your `$HOME/.bashrc` file and source it): `$ set -o vi`"
1. "Let’s begin with the basics. No matter where you go in the filesystem, you can return to your home directory by running `cd` with no arguments"
1. "[In addition to your home directory] The tilde can also refer to another user’s home directory if you place it immediately in front of their username: `$ echo ~jones`"
1. "In general, press Tab once to perform as much completion as possible, or press twice to print all possible completions. The more characters you type, the less ambiguity and the better the match."
1. "If you visit a faraway directory frequently, such as `/home/smith/Work/⁠Projects​/Web/src/include`, create an alias that performs the cd operation":
    ```
    # In a shell configuration file:
    alias work="cd $HOME/Work/Projects/Web/src/include"
    ```
1. 'If you regularly visit lots of directories with long paths, you can create aliases or variables for each of them. This approach has some disadvantages, however:
    - It’s hard to remember all those aliases/variables.
    - You might accidentally create an alias with the same name as an existing command, causing a conflict.
    - An alternative is to create a shell function like the one in Example 4-1, which I’ve named `qcd` (“quick cd”). This function accepts a string key as an argument, such as work or recipes, and runs `cd` to a selected directory path.'
1. "As a bonus, the script’s final line runs the command `complete`, a shell builtin that sets up customized tab completion for `qcd`, so it completes the four supported keys."
1. "A `cd` search path works like your command search path, `$PATH`, but instead of finding commands, it finds subdirectories. Configure it with the shell variable `CDPATH`, which has the same format as `PATH`: a list of directories separated by colons."
1. "Ordinarily, a successful `cd` prints no output. When `cd` locates a directory using your `CDPATH`, however, it prints the absolute path on stdout to inform you of your new current directory"
1. "The trick is to set up your CDPATH to include, in order:
    - `$HOME`
    - Your choice of subdirectories of `$HOME`
    - The relative path for a parent directory, indicated by two dots (`..`)
1. "By including the relative path `..` [in your `CDPATH`] however, you empower new `cd` behavior in every directory. No matter where you are in the filesystem, you can jump to any *sibling* directory (`../sibling`) by name without typing the two dots"
1. "[Using `CDPATH` to quickly navigate] works best if all subdirectories beneath the `CDPATH` directories have unique names. If you have duplicate names, such as `$HOME/Music` and `$HOME/Linux/Music`, you might not get the behavior you want. The command cd Music will always check `$HOME` before `$HOME/Linux` and consequently will not locate `$HOME/Linux/Music` by search."
1. "To jump back and forth between a pair of directories, run `cd -` repeatedly. This is a time-saver when you’re doing focused work in two directories in a single shell. There’s a catch, however: the shell remembers just one previous directory at a time."
1. "Every running shell maintains its own directory stack.... A directory stack is a list of directories that you’ve visited in the current shell and decided to keep track of. You manipulate the stack by performing two operations called pushing and popping."
1. "build a directory stack of four directories, pushing them onto
the stack one at a time:
    ```
    $ pwd
    /home/smith/Work/Projects/Web/src
    $ pushd /var/www/html
    /var/www/html ~/Work/Projects/Web/src
    $ pushd /etc/apache2
    /etc/apache2 /var/www/html ~/Work/Projects/Web/src
    $ pushd /etc/ssl/certs
    /etc/ssl/certs /etc/apache2 /var/www/html ~/Work/Projects/Web/src
    $ pwd
    /etc/ssl/certs
    ```
    The shell prints the stack after each `pushd` operation. The current directory is the leftmost (top) directory."
1. "Print a shell’s directory stack with the `dirs` command. It does not modify the stack"
1. "If you prefer to print the stack from top to bottom, use the `-p` option: `$ dirs -p`"
1. "run `dirs -v` to print the stack with numbered lines":
    ```
    $ dirs -v
    0 /etc/ssl/certs
    1 /etc/apache2
    2 /var/www/html
    3 ~/Work/Projects/Web/src
    ```
    If you prefer this top-down format, consider making an alias"
1. "The `popd` command (“pop directory”) is the reverse of `pushd`."
1. "The pushd and popd commands are such time-savers that I recommend creating two-character aliases that are as quick to type as `cd`"
1. "`pushd` with no arguments swaps the top two directories in the stack and navigates to the new top directory."
1. "Oops, the accidental `cd` command replaced `~/Work/Projects/Web/src` in the stack with `/etc/ssl/certs`. But don’t worry. You can add the missing directory back to the stack without typing its long path. Just
run `pushd` twice, once with a dash argument and once without"
1. 'This “oops, I forgot a pushd” command is useful enough that it’s worth an alias. I call it `slurp` because in my mind, it “slurps back” a directory that I lost by mistake: `alias slurp='pushd - && pushd`."
1. "What if you want to cd between directories in the stack other than the top two? `pushd` and `popd` accept a positive or negative integer argument to operate further into the stack. The command: `$ pushd +N` shifts `N` directories from the top of the stack to the bottom and then performs a `cd` to the new top directory. A negative argument (`-N`) shifts directories in the opposite direction, from the bottom to the top"
1. "To jump to the directory at the bottom of the stack, run `pushd -0`"
1. "You also can remove directories from the stack beyond the top directory, using `popd` with a numeric argument. The command: `$ popd +N` removes the directory in position N from the stack, counting down from the top. A negative argument (`-N`) counts up from the bottom of the stack instead. Counting begins at zero, so `popd +1` removes the second directory from the top"
1. "The `date` command prints the current date and/or time in various formats:
    ```
    $ date Default formatting
    Mon Jun 28 16:57:33 EDT 2021
    $ date +%Y-%m-%d Year-Month-Day format
    2021-06-28
    $ date +%H:%M:%S Hour:Minute:Seconds format
    16:57:33
    ```
    To control the output format, provide an argument that begins with a plus sign (+) followed by any text."
1. "The `seq` command prints a sequence of numbers in a range. Provide two arguments, the low and high values of the range, and `seq` prints the whole range... If you provide three arguments, the first and third define the range, and the middle number is the increment"
1. "By default, [values generated with the `seq` command] are separated by a newline character, but you can
change the separator with the `-s` option followed by any string"
1. "The option `-w` [for `seq`] makes all values the same width (in characters) by adding leading zeros as needed"
1. "The shell provides its own way to print a sequence of numbers, known as brace expansion. Start with a left curly brace, add two integers separated by two dots, and end with a right curly brace":
    ```
    $ echo {1..10} Forward from 1
    1 2 3 4 5 6 7 8 9 10
    ```
1. "Brace expansion always produces output on a single line separated by space characters. Change this by piping the output to other commands, such as `tr`"
1. "More generally, the shell expression `{x..y..z}` generates the values `x` through `y`, incrementing by `z`"
1. "The `find` command lists files in a directory recursively, descending into subdirectories and printing full paths. Results are not alphabetical (pipe the output to sort if needed): `$ find /etc -print List all of /etc recursively`"
1. "`find` has numerous options that you can combine. Here are a few highly useful ones. Limit the output only to files or directories with the option `-type`":
    ```
    $ find . -type f -print Files only
    $ find . -type d -print Directories only
    ```
1. '`find` can also execute a Linux command for each file path in the output, using `-exec`. The syntax is a bit wonky:
    - Construct a find command and omit -print.
    - Append `-exec` followed by the command to execute. Use the expression {} to indicate where the file path should appear in the command.
    - End with a quoted or escaped semicolon, such as ";"'
1. "`find -exec` works well for mass deletions of files throughout a directory hierarchy (but be careful!). Let’s delete files with names ending in a tilde (`~`) within the directory `$HOME/tmp` and its subdirectories. For safety, first run the command `echo rm` to see which files would be deleted, then remove `echo` to delete for real: `$ find $HOME/tmp -type f -name "*~" -exec echo rm {} ";"`
1. "A more practical [example of `find -exec`] performs a long listing (`ls -l`) for all `.conf` files in `/etc` and its subdirectories: `$ find /etc -type f -name "*.conf" -exec ls -l {} ";"`"
1. "`yes` can supply input to interactive programs so they can run unattended. For example, the program `fsck`, which checks a Linux filesystem for errors, may prompt the user to continue and wait for a response of `y` or `n`. The output of the `yes` command, when piped to `fsck`, responds to every prompt on your behalf, so you can walk away and let `fsck` run to completion. The main use of `yes` for our purposes is printing a string a specific number of times by piping `yes` to `head`."
1. "The `yes` command prints the same string over and over until terminated: `$ yes` Repeats "y" by default... `$ yes woof!` Repeat any other string"
1. "Fortunately, you can force `grep` to forget about regular expressions and search for every character literally in the input by using the `-F` (“fixed”) option; or, for an alternative with equivalent results, run `fgrep` instead of `grep`"
1. "Use the `-f` option (lowercase; don’t confuse it with `-F`) to match against a set of strings rather than a single string."
1. "Print the last three lines with `tail`. The option `-n` sets the number of lines to be printed, just as it does for `head`... If you precede the number with a plus sign (`+`), printing begins at that line number and proceeds to the end of the file. The following command begins at the 25th line of the file":
    ```
    $ tail -n+25 alphabet
    Y is for yak
    Z is for zebu
    ```
1. "`head` and `tail` both support a simpler syntax to specify a number of lines without `-n`. This syntax is ancient, undocumented, and deprecated but will probably remain supported forever":
    ```
    $ head -4 alphabet Same as head -n4 alphabet
    $ tail -3 alphabet Same as tail -n3 alphabet
    $ tail +25 alphabet Same as tail -n+25 alphabet
    ```
1. "In general, to print lines `M` through `N`, extract the first `N` lines with `head`, then isolate the last `N-M+1` lines with `tail`. Print lines six through eight of the alphabet file: `$ head -n8 alphabet | tail -n3`."
1. "`awk` refers to any column by a dollar sign followed by the column number: for example, `$7` for the seventh column. If the column number jhas more than one digit, surround the number with parentheses: for example, `$(25)`. To refer to the final field, use `$NF` (“number of fields”). To refer to the entire line, use `$0`."
1. `awk` does not print whitespace between values by default. If you want whitespace, separate the values with commas":
    ```
    $ echo Efficient fun Linux | awk '{print $1 $3}' No whitespace
    EfficientLinux
    $ echo Efficient fun Linux | awk '{print $1, $3}' Whitespace
    Efficient Linux
    ```
1. "If you encounter input separated by something other than space characters, `awk` can change its field separator to any regular expression with the `-F` option":
    ```
    $ echo efficient:::::linux | awk -F':*' '{print $2}' Any number of colons
    linux
    ```
1. "The `tac` command reverses a file line by line. Its name is `cat` spelled backward."
1. "`tac` is great for processing data that is already in chronological order but not reversible with the `sort -r` command. A typical case is reversing a web-server log file to process its lines from newest to oldest: `192.168.1.34 - - [30/Nov/2021:23:37:39 -0500] "GET / HTTP/1.1" ...`"
1. "The `paste` command combines files side by side in columns separated by a single tab character. It’s a partner to the `cut` command, which extracts columns from a tab-separated file"
1. "`paste` also interleaves data from two or more files if you change the separator to a newline character (`\n`)":
    ```
    $ paste -d "\n" title-words1 title-words2
    EFFICIENT
    linux
    AT
    the
    COMMAND
    line
    The
    ```
1. "Transpose the output, producing pasted rows instead of pasted columns, with the  `-s` option":
    ```
    $ paste -d, -s title-words1 title-words2
    EFFICIENT,AT,COMMAND
    linux,the,line
    ```
1. "`diff` compares two files line by line and prints a terse report about their differences"
1. "`diff` output may contain other notation and can take other forms. This short explanation is enough for our main purpose, however, which is to use diff as a text processor that interleaves lines from two files. Many users don’t think of diff this way, but it’s great for forming pipelines to solve certain kinds of problems. For example, you can isolate the differing lines with `grep` and `cut`":
    ```
    $ diff file1 file2 | grep '^[<>]'
    < Linux is all about efficiency.
    > MacOS is all about efficiency.
    > Have a nice day.
    $ diff file1 file2 | grep '^[<>]' | cut -c3-
    Linux is all about efficiency.
    MacOS is all about efficiency.
    Have a nice day.
    ```
1. "`tr` takes two sets of characters as arguments, and it translates members of the first set into the corresponding members of the second. Common uses are converting text to uppercase or lowercase: `$ echo efficient | tr a-z A-Z` and deleting whitespace with the `-d` (delete) option: `$ echo efficient linux | tr -d ' \t'`
1. "Beyond the obvious entertainment value, `rev` is handy for extracting tricky information from files. Suppose you have a file of celebrity names:
    ```
    $ cat celebrities
    Jamie Lee Curtis
    Zooey Deschanel
    Zendaya Maree Stoermer Coleman
    Rihanna
    ```
    and you want to extract the final word on each line (Curtis, Deschanel, Coleman, Rihanna). This would be easy with cut `-f` if each line had the same number of fields, but the number varies. With `rev`, you can reverse all the lines, cut the first field, and reverse again to achieve your goal"
1. 'Don’t worry about memorizing every feature of `awk` or `sed`. Success with these commands really means:
    - Understanding the kinds of transformations they make possible, so you can think, “Ah! This is a job for `awk` (or `sed`)!” and apply them in your time of need
    - Learning to read their manpages and to find complete solutions on Stack Exchange and other online resources"
1. "`awk` and `sed` are harder to learn than the other commands I’ve covered, because each of them has a miniature programming language built in. They have so many capabilities that whole books have been written on them."
1. "`awk` transforms lines of text from files (or `stdin`) into any other text, using a sequence of instructions called an `awk` program. The more skilled you become in writing `awk` programs, the more flexibly you can manipulate text. You can supply the `awk` program on the command line: `$ awk program input-files` You can also store one or more `awk` programs in files and refer to them with the `-f` option, and the programs run in sequence: `$ awk -f program-file1 -f program-file2 -f program-file3 input-files`"
1. "When supplying an awk program on the command line, enclose it in quotes to prevent the shell from evaluating `awk`’s special characters. Use single or double quotes as needed."
1. "An `awk` program includes one or more actions, such as calculating values or printing text, that run when an input line matches a pattern. Each instruction in the program has the form: `pattern {action}`"
1. "[in an `awk` program] an action with no pattern runs for every line of input."
1. "[in an `awk` program] a pattern with no action runs the default action `{print}`, which just prints any matching input lines unchanged":
    ```
    $ echo efficient linux | awk '/efficient/'
    efficient linux
    ```
1. "To learn `awk` beyond what can be covered in a few book pages, take an `awk` tutorial at tutorialspoint.com/awk or riptutorial.com/awk or search the web for “awk tutorial.” You’ll be glad you did."
1. "This feat requires rearranging three fields and adding some characters like parentheses and double quotes. The following `awk` program does the trick, employing the option `-F` to change the input separator from spaces to tabs (`\t`): `$ awk -F'\t' '{print $4, "(" $3 ").", "\"" $2 "\""}' animals.txt`."
1. "`sed`, like `awk`, transforms text from files (or `stdin`) into any other text, using a sequence of instructions called a `sed` script. `sed` scripts are pretty cryptic on first glance. An example is `s/Windows/Linux/g`, which means to replace every occurrence of the string Windows with Linux. The word script here does not mean a file (like a shell script) but a string. Invoke `sed` with a single script on the command line: `$ sed script input-files` or use the `-e` option to supply multiple scripts that process the input in sequence: `$ sed -e script1 -e script2 -e script3 input-files` You can also store `sed` scripts in files and refer to them with the `-f` option, and they run in sequence: `$ sed -f script-file1 -f script-file2 -f script-file3 input-files`"
1. Using an array with `awk` to count occurences:
    ```
    md5sum *.jpg \
    | awk '{counts[$1]++; names[$1]=names[$1] " " $2} \
    END {for (key in counts) print counts[key] " " key ":" names[key]}' \
    | grep -v '^1 ' \
    | sort -nr
    3 f6464ed766daca87ba407aede21c8fcc: image007.jpg image012.jpg image014.jpg
    2 c7978522c58425f6af3f095ef1de1cd5: image019.jpg image020.jpg
    2 146b163929b6533f02e91bdf21cb9563: image001.jpg image003.jpg
    ```
1. "The forward slashes in a [`sed`] substitution may be replaced by any other convenient character. This is helpful when a regular expression itself includes forward slashes (which would otherwise need escaping). These three `sed` scripts are equivalent: `s/one/two/` `s_one_two_` `s@one@two@`."
1. "Another common type of `sed` script is a deletion script. It removes lines by their line number: `$ seq 10 14 | sed 4d`... or lines that match a regular expression: `$ seq 101 200 | sed '/[13579]$/d'`"
1. "To learn `sed` beyond what can be covered in a few book pages, take a `sed` tutorial at tutorialspoint.com/sed or grymoire.com/Unix/Sed.html or search the web for “sed tutorial.”
1. "So, in a moment of need, how do you locate a new program—or tailor a program that you already know—to accomplish your goals? Your first (obvious) step is a web search engine. For example, if you need a command that limits the width of lines in a text file, wrapping any lines that are too long, search the web for (say) “Linux command wrap lines” and you’ll be pointed to the `fold` command"
1. "To discover commands that are already installed on your Linux system, run the command `man -k` (or equivalently, `apropos`). Given a word, `man -k` searches for that word in the brief descriptions at the top of manpages:" 
    ```
    $ man -k width
    DisplayWidth (3) - image format functions and macros
    DisplayWidthMM (3) - image format functions and macros
    fold (1) - wrap each input line to fit in specified width
    ```
1. "`man -k` accepts `awk`-style regular expressions in search strings (see Table 5-1): `$ man -k "wide|width"`"
1. "The purpose of the shell—to run commands—is so fundamental to Linux that you might think the shell is built into Linux in some special way. It is not. A shell is just an ordinary program like `ls` or `cat`. It is programmed to repeat the following steps over and over and over and over…
    - Print a prompt.
    - Read a command from stdin.
    - Evaluate and run the command.
 Linux does a great job of hiding the fact that a shell is an ordinary program. When you log in, Linux automatically runs an instance of the shell for you, known as your login shell. It launches so seamlessly that it appears to be Linux, when really it’s just a program launched on your behalf to interact with Linux."
1. "Since `bash` is just a program, you can run it manually like any other
command: `$ bash`"
1. "`bash` is also not the only shell on your system, most likely. Valid shells are usually listed, one per line, in the file /etc/shells:
    ```
    $ cat /etc/shells
    /bin/sh
    /bin/bash
    /bin/csh
    /bin/zsh
    ```
    To see which shell you’re running, `echo` the shell variable `SHELL`: `$ echo $SHELL`"
1. "In theory, a Linux system can treat any program as a valid login shell, if a user account is configured to invoke it on login and it’s listed in `/etc/shells` (if required on your system). With superuser privileges, you can even write and install your own shell, like the script in Example 6-1. It reads any command and responds, “I’m sorry, I’m afraid I can’t do that.” This custom shell is intentionally silly, but it demonstrates that other programs can be just as legitimate a shell as `/bin/bash`."
1. "Every time you run a simple command, you create a child process. This is such an important point for understanding Linux that I’ll say it again: even when you run a simple command like ls, that command secretly runs inside a new child process with its own (copied) environment. That means any changes you make to a child, like changing the prompt variable `PS1` in a child shell, affect only the child and are lost when the child exits. Likewise, any changes to the parent won’t affect its children that are already running. Changes to the parent can affect its future children, however"
1. 'Every process has its own environment. An environment, which you might recall from “Environments and Initialization Files, the Short Version”, includes a current directory, search path, shell prompt, and other important information held in shell variables. When a child is created, its environment is largely a copy of its parent’s environment.'
1. "If Linux programs cannot change your shell’s current directory, then how does the command `cd` manage to change it? Well, `cd` isn’t a program. It’s a built-in feature of the shell (a.k.a. a shell builtin). If `cd` were a program external to the shell, directory changes would be impossible—they would run in a child process and be unable to affect the parent."
1. "View a shell’s environment variables with the `printenv` command. The output is one variable per line, unsorted, and can be quite long, so pipe it though `sort` and `less` for friendlier viewing."
1. "Local variables do not appear in the output of `printenv`. Display their values by preceding the variable name with a dollar sign and printing the result with `echo`"
1. "To turn a local variable into an environment variable, use the `expor`t command:
    ```
    $ MY_VARIABLE=10 A local variable
    $ export MY_VARIABLE Export it to become an environment variable
    $ export ANOTHER_VARIABLE=20 Or, set and export in a single command
    ```
    export specifies that the variable and its value will be copied from the current shell to any future children. Local variables are not copied to future children"
1. "These sorts of system-defined environment variables are so rarely modified in the real world that they seem global, but they are just ordinary variables that play by the ordinary rules. (You may even change their values in a running shell, but you might disrupt the expected behavior of that shell and other programs."
1. "A subshell, in contrast, is a complete copy of its parent. It includes all the parent’s variables, aliases, functions, and more. To launch a command in a subshell, enclose the command in parentheses."
1. "To check if a shell instance is a subshell, print the variable `BASH_SUBSHELL`. The value is nonzero in subshells, zero otherwise"
1. "Table 6-1 lists the standard bash configuration files.
They come in several types:
    - Startup files: Configuration files that execute automatically when you log in—that is, they apply only to your login shell. An example command in this file might set and export an environment variable. Defining an alias in this file would be less helpful, however, because aliases are not copied to children.
    - Initialization (“init”) files: Configuration files that execute for every shell instance that is not a login shell—for example, when you run an interactive shell by hand or a (noninteractive) shell script. An example initialization file command might set a variable or define an alias.
    - Cleanup files:  Configuration files that execute immediately before your login shell exits. An example command in this file might be clear to blank your screen on logout.
1. "In each of these cases, it’s generally worthwhile to have your personal startup file source your personal initialization file:
    ```
    # Place in $HOME/.bash_profile or other personal startup file
    if [ -f "$HOME/.bashrc" ]
    then
    source "$HOME/.bashrc"
    fi
    ```
1. "Whatever you do, try not to place identical configuration commands in two different configuration files. That’s a recipe for confusion, and it’s hard to maintain, because any change you make to one file you must remember to duplicate in the other (and you’ll forget, trust me). Instead, source one file from the other as I’ve shown."
1. "The commands in a conditional list don’t have to be simple commands; they can also be pipelines and other combined commands."
1. "As another example, if you use the version-control system Git to maintain files, you’re probably familiar with the following sequence of commands after you change some files: run `git add` to prepare
files for a commit, then `git commit`, and finally `git push` to share your committed changes. If any of these commands failed, you wouldn’t run the rest (until you fixed the cause of the failure). Therefore, these three commands work well as a conditional list: `$ git add . && git commit -m"fixed a bug" && git push`"
1. "Commands in a list don’t have to depend on one another. If you separate the commands with semicolons, they simply run in order. Success or failure of a command does not affect later ones in the list."
1. "Unconditional lists are a convenience feature: they produce the same results (mostly) as typing the commands individually and pressing Enter after each. The only significant difference relates to exit codes. In an unconditional list, the exit codes of the individual commands are thrown away except the last one. Only the exit code of the final command in the list is assigned to the shell variable `?`.
1. "Just as the `&&` operator runs a second command only if the first succeeds, the related operator `||` (pronounced “or”) runs a second command only if the first fails. For example, the following command tries to enter `dir`, and if it fails to do so, it creates `dir`: `$ cd dir || mkdir dir`"
1. 'Tedious, right? Wouldn’t be great if you could tell the shell, “Move all files that contain the string Artist: Kansas to the directory kansas.” In Linux terms, you’d like to take the list of names from the preceding `grep -l` command and hand it to `mv`. Well, you can do this easily with the help of a shell feature called command substitution: `$ mv $(grep -l "Artist: Kansas" *.txt) kansas`. The syntax: `$(any command here)` executes the command inside the parentheses and replaces the command by its output. So on the preceding command line, the `grep -l` command is replaced by the list of filenames that it prints, as if you had typed the filenames"
1. "For example, to get the filenames containing Kansas songs and store them in a variable, use command substitution like so: `$ kansasFiles=$(grep -l "Artist: Kansas" *.txt) ` The output might have multiple lines, so to preserve any newline characters, make sure you quote the value wherever you use it: `$ echo "$kansasFiles"`"
1. "The original syntax for command substitution was backquotes (backticks). The following two commands are equivalent: `$ echo Today is $(date +%A)`...``$ echo Today is `date +%A`.``"
1. "Command substitution, which you just saw, replaces a command with its output in place, as a string. Process substitution also replaces a command with its output, but it treats the output as if it were stored in a file."
1. 'That’s a lot of steps. Wouldn’t it be grand to compare the two lists of names directly and not bother with temporary files? The challenge is that `diff` can’t compare two lists from `stdin`; it requires files as arguments. Process substitution solves the problem. It makes both lists appear to `diff` as files. (The sidebar “How Process Substitution Works” provides the technical details.) The syntax: `<(any command here)` runs the command in a subshell and presents its output as if it were contained in a file. For example, the following expression represents the output of `ls -1 | sort -n` as if it were contained in a file: `<(ls -1 | sort -n)`'
1. "The expression `<(…)` creates a file descriptor for reading. The related expression `>(…)` creates a file descriptor for writing, but in 25 years I’ve never needed it."
1. "Process substitution is a non-POSIX feature that might be disabled in your shell. To enable non-POSIX features in your current shell, run `set +o posix`."
1. "When the Linux operating system opens a disk file, it represents that file with an integer called a file descriptor. Process substitution mimicks a file by running a command and associating its output with a file descriptor, so the output appears to be in a disk file from the perspective of programs that access it. You can view the file descriptor with `echo`: 
    ```
    $ echo <(ls)
    /dev/fd/63
    ```
    In this case, the file descriptor for `<(ls)` is `63`, and it’s tracked in the system directory `/dev/fd`. Fun fact: `stdin`, `stdout`, and `stderr` are represented by the file descriptors `0`, `1`, and `2`, respectively. That’s why redirection of stderr has the syntax `2>`."
1. "Suppose you want to create a log file in the system directory `/var/log`, which is not writable by ordinary users. You run the following `sudo` command to gain superuser privileges and create the log file, but it mysteriously fails:
    ```
    $ sudo echo "New log file" > /var/log/custom.log
    bash: /var/log/custom.log: Permission denied
    ```
    Wait a minute—`sudo` should give you permission to create any file anywhere. How can this command possibly fail? Why didn’t `sudo` even prompt you for a password? The answer is: because `sudo` didn’t run. You applied `sudo` to the `echo` command but not to the output redirection, which ran first and failed. In detail:
    - You pressed Enter.
    - The shell began to evaluate the whole command, including redirection (`>`).
    - The shell tried to create the file custom.log in a protected directory, `/var/log`.
    - You didn’t have permission to write to `/var/log`, so the shell gave up and printed the “Permission denied” message.
 That’s why `sudo` never ran."
1. "The shell reads every command that you type on `stdin`. That means `bash` the program can participate in pipelines. For example, print the string `"ls -l"` and pipe it to `bash`, and `bash` will treat the string as a command and run..."
1. "To solve [`sudo` failing to work with output redirection], you need to tell the shell, “Run the entire command, including output redirection, as the superuser.” This is exactly the kind of situation that `bash -c` solves so well. Construct the command you want to run, as a string: `'echo "New log file" > /var/log/custom.log'` and pass it as an argument to `sudo bash -c`: `$ sudo bash -c 'echo "New log file" > /var/log/custom.log'`."
1. "[`bash -c`] is terrific when you need to run many similar commands in a row. If you can print the commands as strings, then you can pipe the strings to `bash` for execution."
1. "The steps you just completed are a repeatable pattern:
    - Print a sequence of commands by manipulating strings.
    - View the results with `less` to check correctness.
    - Pipe the results to `bash`."
1. "`xargs` accepts two inputs:
    - On stdin: A list of strings separated by whitespace. An example is file paths produced by `ls` or `find`, but any strings will do. I’ll call them the input strings.
    - On the command line: An incomplete command that’s missing some arguments, which I’ll call the command template.
 `xargs` merges the input strings and the command template to produce and run new, complete commands, which I’ll call the generated commands."
1. "When piping commands to `ssh`, the remote host might print diagnostic or other messages. These generally do not affect the remote command, and you can suppress them:
    - If you see messages about pseudo-terminals or pseudo-ttys, such as “Pseudo-terminal will not be allocated because stdin is not a terminal,” run `ssh` with the `-T` option to prevent the remote SSH server from allocating a terminal: `$ echo "ls > outfile" | ssh -T myhost.example.com`
    - If you see welcome messages that normally appear when you log in (“Welcome to Linux!”) or other unwanted messages, try telling `ssh` explicitly to run bash on the remote host, and the messages should disappear: `$ echo "ls > outfile" | ssh myhost.example.com bash`"
1. "By combining `find` and `xargs`, you can empower any command to run recursively through the filesystem, affecting only files (and/or directories) that match your stated criteria. (In some cases, you can produce the same effect with `find` alone, using its option `-exec`, but `xargs` is often a cleaner solution.)"
1. 'When combining `find` and `xargs`, use `xargs -0` (dash zero) rather than `xargs` alone to protect against unexpected special characters in the input strings. Pair it with the output produced by `find -print0` (instead of `find -print`): `$ find options... -print0 | xargs -0 options...`. Normally, xargs expects its input strings to be separated by whitespace, such as newline characters. This is a problem when the input strings themselves contain other whitespace, such as filenames with spaces in them. By default, `xargs` will treat those spaces as input separators and operate on incomplete strings, producing incorrect results. For example, if the input to xargs includes a line `prickly pear.py`, `xargs` will treat it as two input strings, and you’re likely to see an error like this:
    ```
    prickly: No such file or directory
    pear.py: No such file or directory
    ```
    To avoid this problem, use `xargs -0` (that’s a zero) to accept a different character as the input separator, namely, the null character (ASCII zero). Nulls rarely appear in text, so they are ideal, unambiguous separators for input strings. How can you separate your input strings with nulls instead of newlines? Fortunately, `find` has an option to do exactly that: `-print0`, rather than `-print`. The `ls` command unfortunately does not have an option to separate its output with nulls, so my earlier toy examples with `ls` are not safe. You can convert newlines to nulls with tr: `$ ls | tr '\n' '\0' | xargs -0 ...` Or use this handy alias that lists the current directory with entries separated by nulls, suitable for piping to `xargs`: `alias ls0="find . -maxdepth 1 -print0"`."
1. "`xargs` has numerous options (see `man xargs`) that control how it creates and runs the generated commands. The most important ones in my view (other than `-0`) are `-n` and `-I`. The `-n` option controls how many arguments are appended by `xargs` onto each generated command. The default behavior is to append as many arguments as will fit within the shell’s limits... The `-I` option controls where the input strings appear in the generated command. By default, they’re appended to the command template, but you can make them appear elsewhere. Follow `-I` with any string (of your choice), and that string becomes a placeholder in the command template, indicating exactly where input strings should be inserted: `$ ls | xargs -I XYZ echo XYZ is my favorite food`."
1. "`xargs` is a problem solver when command lines grow very long. Suppose your current directory contains one million files named `file1.txt` through `file1000000.txt` and you try to remove them by pattern matching:
    ```
    $ rm *.txt
    bash: /bin/rm: Argument list too long
    ```
    The pattern `*.txt` evaluates to a string of more than 14 million characters, which is longer than Linux supports. To work around this limitation, pipe a list of the files to `xargs` for deletion. xargs will split the list of files across multiple rm commands. Form the list of files by piping a full directory listing to grep, matching only filenames ending in `.txt`, then pipe to `xargs`: `$ ls | grep '\.txt$' | xargs rm`. This solution is better than file pattern matching (`ls *.txt`), which will produce the same “Argument list too long” error. Better yet, run `find -print0` as described in “Safety with find and xargs”: `$ find . -maxdepth 1 -name \*.txt -type f -print0 | xargs -0 rm`."
1. "To run a command in the background, simply append an ampersand (`&`). The shell responds with a cryptic-looking message indicating that the command is backgrounded and presents the next prompt:
    ```
    $ wc -c my_extremely_huge_file.txt & Count characters in a huge file
    [1] 74931 
    ```
    ...Output from backgrounded commands may appear at any time, even while you are typing."
1. "The ampersand is also a list operator, like `&&` and `||`:
    ```
    $ command1 & command2 & command3 & All 3 commands
    [1] 57351 in background
    [2] 57352
    [3] 57353
    $ command4 & command5 & echo hi All in background
    [1] 57431 but "echo"
    ```
1. "A job is a shell’s unit of work: a single instance of a command running in a shell. Simple commands, pipelines, and conditional lists are all examples of jobs—basically anything
you can run at the command line. A job is more than a Linux process. A job may consist of one process, two processes, or more. A pipeline of six programs, for example, is a single job that includes (at least) six processes. Jobs are a construct of the shell. The Linux operating system doesn’t keep track of jobs, just the underlying processes. At any moment, a shell may have multiple jobs running. Each job in a given shell has a positive integer ID, called the job ID or job number. When you run a command in the background, the shell prints the job number and the ID of the first process it runs within the job.
1. "A related technique is to run a foreground command, change your mind during execution, and send it to the background. Press `Ctrl-Z` to stop the command temporarily (called suspending the command) and return to the shell prompt; then type `bg` to resume running the command in the background."
1. "Table 7-1. Job control commands"
    | Command  | Meaning |
    |----------|---------|
    | `bg`     | Move the current suspended job into the background |
    | `bg %n`  | Move suspended job number `n` into the background (example: `bg %1`) |
    | `fg`     | Move the current background job into the foreground |
    | `fg %n`  | Move background job number `n` into the foreground (example: `fg %2`)|
    | `kill %n`| Terminate background job number `n` (example: `kill %3`) |
    | `jobs`   | View a shell’s jobs |
1. "Screen output from a background job can appear at any time while the job runs. To avoid this sort of messiness, redirect `stdout` to a file, then examine the file at your leisure: `$ sort /usr/share/dict/words | head -n2 > /tmp/results &`."
1. "Other odd things happen when a background job attempts to read from `stdin`. The shell suspends the job, prints a Stopped message, and waits for input in the background. Demonstrate this by backgrounding cat with no arguments so it reads stdin:
    ```
    $ cat &
    [1] 82455
    [1]+ Stopped cat
    ```
    Jobs can’t read input in the background, so bring the job into the foreground with `fg` and then supply the input"
1. "Normally when you run a command, the shell runs it in a separate process that is destroyed when the command exits, as described in “Parent and Child Processes”. You can change this behavior with the `exec` command, which is a shell builtin. It replaces the running shell (a process) with another command of your choice (another process). When the new command exits, no shell prompt will follow because the original shell is gone."
1. "When applied to a whole command, this technique isn’t super useful, except maybe to save you from running a second cd command to return to your previous directory. However, if you place parentheses around one piece of a combined command, you can perform some useful tricks. A
typical example is a pipeline that changes directory in the middle of execution."
1. "It’s tempting to view bash parentheses as if they simply group commands together, like parentheses in arithmetic. They do not. Each pair of parentheses causes a subshell to be launched."
1. "Why would you ever run `exec`? One reason is to conserve resources by not launching a second process. Shell scripts sometimes make use of this optimization by running `exec` on the final command in the script. If the script is run many times (say, millions or billions of executions), the savings might be worth it. `exec` has a second ability—it can reassign `stdin`, `stdout`, and/or `stderr` for the current shell. This is most practical in a shell script, such as this toy example that prints information to a file,
    ```
    /tmp/outfile:
    #!/bin/bash
    echo "My name is $USER" > /tmp/outfile
    echo "My current directory is $PWD" >> /tmp/outfile
    echo "Guess how many lines are in the file /etc/hosts?" >> /tmp/outfile
    wc -l /etc/hosts >> /tmp/outfile
    echo "Goodbye for now" >> /tmp/outfile
    ```
    Instead of redirecting the output of each command to `/tmp/outfile` individually, use `exec` to redirect stdout to `/tmp/outfile` for the entire script. Subsequent commands can simply print to stdout"
1. "Running `exec` May Be Fatal: If you run `exec` in a shell, the shell exits afterward. If the shell was running in a terminal window, the window closes. If the shell was a login shell, you will be logged out."
1. "Table 7-2. Common idioms for running commands"
    | Problem | Solution |
    |---------|----------|
    | Sending stdout from one program to stdin of another | Pipelines |
    | Inserting output (stdout) into a command | Command substitution |
    | Providing output (stdout) to a command that doesn’t read from stdin, but does read disk files | Process substitution |
    | Executing one string as a command | `bash -c`, or piping to `bash` |
    | Printing multiple commands on stdout and executing them |Piping to `bash` |
    | Executing many similar commands in a row | `xargs`, or constructing the commands as strings and piping them to bash |
    | Managing commands that depend on one other’s success | Conditional lists |
    | Running several commands at a time | Backgrounding |
    | Running several commands at a time that depend on one another’s success | Backgrounding a conditional list |
    | Running one command on a remote host | Run `ssh` host command |
    | Changing directory in the middle of a pipeline | Explicit subshells |
    | Running a command later | Unconditional list with `sleep` followed by the command |
    | Redirecting to/from protected files |Run `sudo bash -c "command > file`" |
1. "The command `mkdir -p dir`, which creates a directory path only if it doesn’t already exist, would be a more elegant solution here."
1. "A key to writing brash one-liners is flexibility. You’ve learned some awesome tools by this point—a core set of Linux programs (and umpteen ways to run them) along with command history, command-line editing, and more. You can combine these tools in many ways, and a given problem usually has multiple solutions."
1. "Every brash one-liner begins with the output of a simple command. That output might be the contents of a file, part of a file, a directory listing, a sequence of numbers or letters, a list of users, a date and time, or other data. Your first challenge, therefore, is to produce the initial data for your command."
1. "Be flexible and understand your tools so you can apply the best one in your time of need. That is a wizard’s skill when creating brash one-liners. All of the following commands list .jpg files in the current directory. Try to figure out how each command works"
    ```
    $ echo $(ls *.jpg)
    $ bash -c 'ls *.jpg'
    $ cat <(ls *.jpg)
    $ find . -maxdepth 1 -type f -name \*.jpg -print
    $ ls > tmp && grep '\.jpg$' tmp && rm -f tmp
    $ paste <(echo ls) <(echo \*.jpg) | bash
    $ bash -c 'exec $(paste <(echo ls) <(echo \*.jpg))'
    $ echo 'monkey *.jpg' | sed 's/monkey/ls/' | bash
    $ python -c 'import os; os.system("ls *.jpg")'
    ```
1. "Use `ls` or add `echo` to test destructive commands. If your command invokes `rm`, `mv`, `cp`, or other commands that might overwrite or remove files, place `echo` in front of them to confirm which files will be affected. (So, instead of executing `rm`, execute `echo rm`.) Another safety tactic is to replace `rm` with `ls` to list files that would be removed."
1. "Add `echo` to test your expressions. If you aren’t sure how an expression will evaluate, print it with `echo` beforehand to see the evaluated results on `stdout`."
1. "Use command history and command-line editing. Don’t retype commands while you experiment. Use techniques from Chapter 3 to recall previous commands, tweak them, and run them."
1. "Insert a `tee` to view intermediate results. If you want to view the output (stdout) in the middle of a long pipeline, insert the `tee` command to save output to a file for examination."
1. "The `-n` option prevents echo from printing newline characters, which would throw off each count by one.) Finally, pipe the commands to bash to run them, sort the numeric results from high to low, and grab the maximum value (the first line) with `head -n1`:
    ```
    $ ls | awk '{print "echo -n", $0, "| wc -c"}' | bash | sort -nr | head -n1
    23
    ```
    This last example was tricky, generating pipelines as strings and passing them to a further pipeline. Nevertheless, the general principle is the same: figure out your starting data and manipulate it to fit your needs."
1. "The pattern I just presented is reusable in all kinds of situations to run a sequence of related commands:
    - Generate the command arguments as lists on `stdout`.
    - Print the lists side by side with `paste` and process substitution.
    - Prepend a command name with `sed` by replacing the beginning-of-line character (`^`) with a program name and a space.
    - Pipe the results to `bash`."
1. "Fortunately, bash has a variable, `RANDOM`, that holds a random positive integer between 0 and 32,767. Its value changes every time you access the variable"
1. "Next, you need one thousand output files, or more specifically, one thousand different filenames. To generate filenames, run the program pwgen, which generates random strings of letters and digits:
    ```
    $ pwgen
    eng9nooG ier6YeVu AhZ7naeG Ap3quail poo2Ooj9 OYiuri9m iQuash0E voo3Eph1
    IeQu7mi6 eipaC2ti exah8iNg oeGhahm8 airooJ8N eiZ7neez Dah8Vooj dixiV1fu
    Xiejoti6 ieshei2K iX4isohk Ohm5gaol Ri9ah4eX Aiv1ahg3 Shaew3ko zohB4geu
    ⋮
    ```
    Add the option `-N1` to generate just a single string, and specify the string length (10) as an argument":
    ```
    $ pwgen -N1 10
    ieb2ESheiw
    ```
1. "To shuffle the dictionary into random order, use the aptly named command `shuf`."
1. "If you’d prefer one thousand random image files instead of text files, use the same technique (`yes`, `head`, and `bash`) and replace `shuf` with a command that generates a random image. Here’s a brash one-liner that I adapted from a solution by Mark Setchell on Stack Overflow. It runs the command `convert`, from the graphics package ImageMagick, to produce random images of size 100 x 100 pixels consisting of multicolored squares":
    ```
    $ yes 'convert -size 8x8 xc: +noise Random -scale 100x100 $(pwgen -N1 10).png' \
    | head -n 1000 \
    | bash
    $ ls
    Bahdo4Yaop.png Um8ju8gie5.png aing1QuaiX.png ohi4ziNuwo.png
    Eem5leijae.png Va7ohchiep.png eiMoog1kou.png ohnohwu4Ei.png
    Eozaing1ie.png Zaev4Quien.png hiecima2Ye.png quaepaiY9t.png
    ⋮
    $ display Bahdo4Yaop.png View the first image
    ```
1. "The preceding solutions, like many others in this book, begin with an existing text file and manipulate its contents with commands. It’s time to reverse that approach and intentionally design new text files that partner well with Linux commands. This is a winning strategy to get work done efficiently on a Linux system. All it takes is four steps:
    - Notice a business problem you want to solve that involves data.
    - Store the data in a text file in a convenient format.
    - Invent Linux commands that process the file to solve the problem.
    - (Optional.) Capture those commands in scripts, aliases, or functions to be simpler to run."
1. "Suppose your home directory contains tens of thousands of files and subdirectories, and every so often, you can’t remember where you put one of them. The find command locates a file by name, such as animals.txt: `$ find $HOME -name animals.txt -print` but `find` is slow because it searches your entire home directory, and you need to locate files regularly. This is step 1, noticing a business problem that involves data: finding files in your home directory quickly by name. Step 2 is storing the data in a text file in a convenient format. Run `find` once to build a list of all your files and directories, one file path per line, and store it in a hidden file: `$ find $HOME -print > $HOME/.ALLFILES` ... Now you have the data: a line-by-line index of your files. Step 3 is inventing Linux commands to speed up searches for files, and for that, use `grep`. It’s much quicker to `grep` through a large file than to
run `find` in a large directory tree: `$ grep animals.txt $HOME/.ALLFILES`"
1. "Start with the `whois` command, which queries a domain
registrar for information about a domain":
    ```
    $ whois example.com | less
    Domain Name: EXAMPLE.COM
    Registry Domain ID: 2336799_DOMAIN_COM-VRSN
    Registrar WHOIS Server: whois.iana.org
    Updated Date: 2021-08-14T07:01:44Z
    Creation Date: 1995-08-14T04:00:00Z
    Registry Expiry Date: 2022-08-13T04:00:00
    ```
1. "Tip: Arrange the fields with predictable lengths first, so columns appear neatly lined up to the eye. Look how messy the file appears if you put the city names in the first column:
    ```
    Hackensack, Jersey City 201 NJ
    Washington 202 DC
    ```
1. "For easier editing of the `vault.gpg` file, both `emacs` and `vim` have modes for editing GnuPG-encrypted files. Begin by adding this line to a bash configuration file and sourcing it in any associated shells: `export GPG_TTY=$(tty)`... For `vim`, try the plugin `vim-gnupg` and add these lines to the configuration file `$HOME/.vimrc`":
    ```
    let g:GPGPreferArmor=1
    let g:GPGDefaultRecipients=["GnuPG ID here"]
    ```
1. "Most Linux desktop environments, such as GNOME, KDE Plasma, Unity, and Cinnamon, provide some way to define hotkeys or custom keyboard shortcuts—special keystrokes that launch commands or perform other operations. I strongly recommend that you define keyboard shortcuts
for these common operations:
    - Opening a new shell window (a terminal program)
    - Opening a new web browser window
 ...On my desktop, I assign the keyboard shortcut `Ctrl-Windows-T` to run konsole and `Ctrl-Windows-C` to run google-chrome."
1. "To cycle through all windows on the desktop that belong to the same application, such as all Firefox windows, press ``Alt-` `` (Alt-backquote, or Alt plus the key above Tab). To cycle backward, add the Shift key (Alt-Shift-backquote)"
1. "If you do serious work on Linux and you’re using just one desktop, you’re missing out on a great way to organize your work. Multiple desktops, also called workspaces or virtual desktops, are just what they sound like. Instead of a single desktop, you might have four or six or more, each with its own windows"
1. "Table 10-1. The most important keyboard shortcuts for Firefox, Google Chrome, and Opera"
| Action | Keyboard shortcut |
|--------|-------------------|
| Open new window | `Ctrl-N` |
| Open new private/incognito window | `Ctrl-Shift-P` (Firefox), `Ctrl-Shift-N` (Chrome and Opera) |
| Open new tab | `Ctrl-T` |
| Close tab | `Ctrl-W` |
|Cycle through browser tabs | `Ctrl-Tab` (cycle forward) and `Ctrl-Shift-Tab` (cycle backward) |
| Jump to address bar | `Ctrl-L` (or `Alt-D` or `F6`) |
| Find (search) for text in current page | `Ctrl-F` |
1. "You may be accustomed to launching a web browser by clicking or tapping an icon, but you can also do it from the Linux command line. If the browser isn’t running yet, add an ampersand to run it in the background so you get your shell prompt back:
    ```
    $ firefox &
    $ google-chrome &
    $ opera &
    ```
    If a given browser is already running, omit the ampersand. The command tells an existing browser instance to open a new window or tab. The command immediately exits and gives you the shell prompt back."
1. "A backgrounded browser command might print diagnostic messages and clutter up your shell window. To prevent this, redirect all output to `/dev/null` when you first launch the browser. For example: `$ firefox &> /dev/null &`."
1. "To open a browser and visit a URL from the command line, provide the
URL as an argument: `$ firefox https://oreilly.com`...  By default, the preceding commands open a new tab and bring it into focus. To force them to open a new window instead, add an option: `$ firefox --new-window https://oreilly.com` ... To open a private or incognito browser window, add the appropriate command-line option:
    ```
    $ firefox --private-window https://oreilly.com
    $ google-chrome --incognito https://oreilly.com
    $ opera --private https://oreilly.com
    ```
    The preceding commands might seem like a lot of typing and effort, but you can be efficient by defining aliases for sites you visit often"
1. "Likewise, if you have a file that contains a URL of interest, extract the URL with `grep`, `cut`, or other Linux commands and pass it to the browser on the command line with command substitution. Here’s an example with a tab-separated file with two columns":
    ```
    $ cat urls.txt
    duckduckgo.com My search engine
    nytimes.com My newspaper
    spotify.com My music
    ...
    $ google-chrome https://$(grep music urls.txt | cut -f1)
    ```
1. 'Some sites don’t support retrieval by `wget` and `curl`. Both commands can masquerade as another browser in such cases. Just tell each program to change its user agent—the string that identifies a web client to a web server. A convenient user agent is “Mozilla”':
    ```
    $ wget -U Mozilla url
    $ curl -A Mozilla url
    ```
1. "If your sed scripts become so long they look like random noise: `s/\([0-9]*\)@\([A-Z][A-Z]\)@\([^@]*\)@/\1\t\2\t\3\n/g` try splitting them up. Store parts of the regular expression in several shell variables, and combine the variables later"
1. "Run `hxselect` to extract the area code data from each table cell, supplying the `-c` option to omit the `td` tags from the output. Print the results as one long line, with fields separated by a character of your choice (using the `-s` option). I chose the character `@` for its easy visibility on the page:
    ```
    $ curl -s https://efficientlinux.com/areacodes.html \
    | hxnormalize -x \
    | hxselect -c -s@ '#ac .ac, #ac .state, #ac .cities
    ```
1. "`lynx` and `links` are also great for checking out a suspicious-looking link when you’re unsure if it’s legitimate or malicious. These text-based browsers don’t support JavaScript or render images, so they are less vulnerable to attack. (They can’t promise complete security, of course, so use your best judgment.)"
1. 'But did you know that you can process the clipboard directly from the command line? A bit of background first: copy and paste operations on Linux are part of a more general mechanism called X selections. A selection is a destination for copied content, such as the system clipboard. “X” is just the name of the Linux windowing software. Most Linux desktop environments that are built on X, such as GNOME, Unity, Cinnamon, and KDE Plasma, support two selections. The first is the `clipboard`, and it works just like clipboards on other operating systems. When you run cut or copy operations in an application, the content goes to the clipboard, and you retrieve the content with a paste operation. A less familiar X selection is called the `primary selection`. When you select text in certain applications, it’s written to the primary selection even if you don’t run a copy operation. An example is highlighting text in a terminal window with the mouse. That text is automatically written to the primary selection.'
1. "Table 10-2. Accessing X selections in common terminal programs"
    | Operation | Clipboard | Primary selection |
    |-----------|-----------|-------------------|
    | Copy (mouse) |Open the right button menu and select Copy | Click and drag; or double-click to select the current word; or triple-click to select the current line |
    | Paste (mouse) | Open the right button menu and select Paste | Press the middle mouse button (usually the scroll wheel) |
    | Copy (keyboard) | `Ctrl-Shift-C` | n/a |
    | Paste (keyboard), gnome-terminal | `Ctrl-Shift-V` or `Ctrl-Shift-Insert` |  `Shift-Insert` |
    | Paste (keyboard), konsole | `Ctrl-Shift-V` or `Shift-Insert` | `Ctrl-Shift-Insert` |
1. "Linux provides a command, `xclip`, that connects X selections to `stdin` and `stdout`. You can therefore insert copy and paste operations into pipelines and other compound commands."
1. "To copy text to the clipboard instead of the primary selection, run `xclip` with the option `-selection clipboard`":
    ```
    $ echo https://oreilly.com | xclip -selection clipboard Copy
    $ xclip -selection clipboard -o Paste
    https://oreilly.com
    ```
1. "Conversely, you may have pasted text into a file to process it with Linux commands like this:
    - Use your mouse to copy a bunch of text in an application program.
    - Paste it into a text file.
    - Process the text file with Linux commands.
 With `xclip -o`, you can skip the intermediate text file:
    - Use your mouse to copy a bunch of text in an application program.
    - Pipe the output of `xclip -o` to other Linux commands for processing."
1. "Clear the primary selection by setting its value to the empty string with `echo -n`: `$ echo -n | xclip` The `-n` option is important; otherwise, `echo` prints a newline character on stdout that ends up in the primary selection."
1. "Linux offers another command, `xsel`, that also reads and writes X selections. It has a few extra features, like clearing a selection (`xsel -c`) and appending to a selection (`xsel -a`)."
1."By default, `xclip` reads `stdin` and writes the primary selection. It can read from a file: `$ xclip < myfile.txt` or from a pipe: `$ echo "Efficient Linux at the Command Line" | xclip` Now print the text to `stdout`, or pipe the selection contents to other
commands, such as `wc`":
    ```
    $ xclip -o Paste to stdout
    Efficient Linux at the Command Line
    $ xclip -o > anotherfile.txt Paste to a file
    $ xclip -o | wc -w Count words
    6
    ```
1. "When you’re viewing a text file with `less` and want to edit the file, don’t exit `less`. Just press `v` to launch your preferred text editor. It loads the file and places the cursor right at the spot you were viewing in `less`. Exit the editor and you’re back in `less` at the original location. For this trick to work best, set the environment variable `EDITOR` and/or `VISUAL` to an editing command. These environment variables represent your default Linux text editor that may be launched by various commands, including `less`, `lynx`, `git`, `crontab`, and numerous email programs. For example, to set `emacs` as your default editor, place either of the following lines (or both) in a shell configuration file and source it":
    ```
    VISUAL=emacs
    EDITOR=emacs
    ```
1. 'If you consistently misspell a command, define aliases for your most common mistakes so the correct command runs anyway:
    ```
    alias firfox=firefox
    alias les=less
    alias meacs=emacs
    ```
    Be careful not to shadow (override) an existing Linux command accidentally by defining an alias with the same name. Search for your proposed alias first with the command `which` or `type` (see “Locating Programs to Be Run”), and run the `man` command to be extra sure there’s no other same-named command'
1. "Pick a common command, such as `cut` or `grep`, and read its manpage thoroughly. You’ll probably discover an option or two that you’ve never used and will find valuable. Repeat this activity every so often to polish and extend your Linux toolbox."
1. "Run `man bash` to display the full, official documentation on bash, and read the whole thing—yes, all 46,318 words of it... Take a few days. Work through it slowly. You’ll definitely learn a lot to make your daily Linux use easier."
1. "... run `crontab -e` to edit your personal file of scheduled commands. `crontab` launches your default editor and opens an empty file to specify the commands. That file is called your crontab."
1. "I recommend learning the program `crontab` to set up scheduled commands for yourself. For example, you could back up files to an external drive on a schedule, or send yourself reminders by email for a monthly event.... Briefly, a scheduled command in a crontab file, often called a cron job, consists of six fields, all on a single (possibly long) line. The first five fields determine the job’s schedule by minute, hour, day of month, month, and day of week, respectively."
1. "Once you’ve created all six fields, saved the [chrontab file], and exited your editor, the command is launched automatically (by a program called `cron`) according to the schedule you defined. The syntax for schedules is short and cryptic but well-documented on the manpage (`man 5 crontab`) and numerous online tutorials"
1. "I also recommend learning the `at` command, which schedules commands to run once, rather than repeatedly, at a specified date and time. Run `man at` for details."
1. "To remove a pending job before it’s executed, run `atrm` with the job number: `$ atrm 699`"
1. "To list your pending at jobs, run `atq`"
1. "To view the commands in an `at` job, run at `-c` with the job number, and print the final few lines: `$ at -c 699 | tail`."
1. "The command `rsync` is a smarter copy program. It copies only the differences between the first and second directories. `$ rsync -a dir1/ dir2` Note: The forward slash in the preceding command means to copy the files inside `dir1`. Without the slash, `rsync` would copy `dir1` itself, creating `dir2/dir1`."
1. "`rsync` has dozens of options. These are some particularly useful ones:
    - `-v` (meaning “verbose”): To print the names of files as they’re copied
    - `-n`: To pretend to copy; combine with `-v` to see which files would be copied
    - `-x`: To tell rsync not to cross filesystem boundaries"
1. "`make` was developed for programmers, but with a little study, you can use it for nonprogramming tasks. Anytime you need to update files that depend on other files, you can likely simplify your work by writing a Makefile."
1. "The dot and double dot are not expressions evaluated by the shell. They are hard links present in every directory."