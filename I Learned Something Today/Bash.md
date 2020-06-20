## Unix Philosophy/Best Practices
1. If you want to mass-rename stuff, don't copy the files to somewhere else, just make hardlinks to the backup folder.
1. Running services that use a port below 1024 requires `sudo` (or something like nginx to redirect from one port to another).
1. "Those that don't remember history are doomed to press Up repeatedly."

## Tool specific 
1. `grep [options] [pattern] [file]`. Use it. `-r` does recursive, `-i` does case-insensitive, `-n` gives line numbers too.
1. To use `scp` to copy a file to a remote server, use something like `scp -i Identity.pem localFile.ext username@domain:/path/to/copy/to`. To copy from, swap the order.
1. If you need to locate a file you know the name of, use `find .` (replacing with whatever directory you want), with the argument `-name 'fileName'`. `-iname` will make it case-insensitive, `-maxdepth n` lets you set how deep to recurse.
1. `alt+b` and `alt+f` move backwards and forwards by a word.
1. `ctrl+a` and `ctrl+e` move to the start/end of the line.
1. `ctrl+r` and `ctrl+s` will search through the history, which is wild.
1. You cna repeat previous data using a couple different things. `!!` will repeat the previous command, useful for building pipelines. `!$` is shorthand for the last argument of the previous command, and `!*` is shorthand for all of the arguments of the previous command.
