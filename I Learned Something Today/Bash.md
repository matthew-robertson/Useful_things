## Unix Philosophy/Best Practices
1. If you want to mass-rename stuff, don't copy the files to somewhere else, just make hardlinks to the backup folder.
1. Running services that use a port below 1024 requires `sudo` (or something like nginx to redirect from one port to another).
1. "Those that don't remember history are doomed to press Up repeatedly."
1. `date` is an exceptional tool

## Tool specific 
1. `grep [options] [pattern] [file]`. Use it. `-r` does recursive, `-i` does case-insensitive, `-n` gives line numbers too.
1. To use `scp` to copy a file to a remote server, use something like `scp -i Identity.pem localFile.ext username@domain:/path/to/copy/to`. To copy from, swap the order.
1. If you need to locate a file you know the name of, use `find .` (replacing with whatever directory you want), with the argument `-name 'fileName'`. `-iname` will make it case-insensitive, `-maxdepth n` lets you set how deep to recurse.
1. `alt+b` and `alt+f` move backwards and forwards by a word.
1. `ctrl+a` and `ctrl+e` move to the start/end of the line.
1. `ctrl+r` and `ctrl+s` will search through the history, which is wild.
1. You can repeat previous data using a couple different things. `!!` will repeat the previous command, useful for building pipelines. `!$` is shorthand for the last argument of the previous command, and `!*` is shorthand for all of the arguments of the previous command.
1. `df -h` gives you the disk sizes in a human-readable format, while `du -h` does the same for file sizes.
1. start services (like docker, for when an AWS instance gets reset!) using `sudo service docker start`.
1. `strace` is a cool little (and performance-heavy) tool, that tracks all syscalls a call makes. `strace ls .` records every system call `ls` would make.
1. `grep -rl "foo" dict/to/search | xargs sed -i 's/foo/bar/g'`  will replace all instances of foo with bar in files in the target directory. If you're on mac, `-i` requires an argument. `-i ""` should cover that.
1. `cp -r /path/to/copy path/to/add` will copy the contents of the first folder into the second, creating it if needed. This can get funky if you're trying to copy it into a sub-folder. Don't do that.
1. `awk 'BEGIN {srand(); print srand()}'` will get the number of seconds [since epoch](https://stackoverflow.com/a/41324810). Don't ask me how, it has to do with `srand` generally being seeded with the current time.
1. `ctrl+z` will suspend a task, `fg` will bring it back to the foreground. Notably, you can give `fg` an argument to foreground something other than your most recently suspended/backgrounded task.
1. `jobs -l` will let you know the jobid for all the suspended/backgrounded tasks, for use with `fg`.

## Scripting
1. `if [ -d /folder ]` will return true if a folder exists. `-f` will check files, and `[ ! -d /folder ]` will return true if it *doesn't* exist. A bunch of these tests exist, check them [here](https://www.geeksforgeeks.org/bash-scripting-how-to-check-if-file-exists/).
1. When working with bash functions, you can't return anything other than a status code. One trick if you need to capture a result is to `echo` it, and capture it with `result=$(funt foo)`. This obviously only works if you only return once.
1. Arguments are fetched from a bash function like `$1`, etc... `$@` will get all arguments. If you for some godforsaken reason need a 10th argument, you need to use `${10}.
1. `${var:?error message}` will require the argument to exist else it'll throw an error.
1. `${var:-default_val}` will give an argument a default value. Handy.
1. `foo | bar` no longer involves running `foo`, and *then* piping it into `bar`, there's a lot of parallel execution here, which means it's hard if not impossible to prevent [the second half of a pipe from running](http://rachelbythebay.com/w/2022/04/05/pipe/) in the case of an error. `set -o pipefail` is usually what people are looking for, but doesn't actually help.
