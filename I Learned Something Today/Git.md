1. You can remove untracked files using `git clean`. `-n` will show what will be removed, `-f` does the clean, `-d` will target directories.
1. If you encounter a merge conflict and want to get their changes only, `git checkout --theirs path/to/file` will work. Though it only works on unmerged paths, so you can't do this to sub-folders.
1. `git stash pop` will remove the change from the stack, while `git stash apply` will keep it.
1. Both `git pull` and `git clone` call `git fetch` under the hood, which is why the clone stats on your repos look weird as heck.
