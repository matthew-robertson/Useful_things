1. You can remove untracked files using `git clean`. `-n` will show what will be removed, `-f` does the clean, `-d` will target directories.
1. If you encounter a merge conflict and want to get their changes only, `git checkout --theirs path/to/file` will work. Though it only works on unmerged paths, so you can't do this to sub-folders.
