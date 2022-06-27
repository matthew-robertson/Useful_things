1. You can [remove untracked files](https://tekin.co.uk/2019/03/delete-untracked-files-from-your-git-repository) using `git clean`. `-n` will do a dry-run showing what will be removed, `-f` does the clean, `-d` will target directories. `-i` lets you choose for each file. 
1. Push to a different remote branch using `git push origin local-branch:remote-branch`.
1. If you encounter a merge conflict and want to get their changes only, `git checkout --theirs path/to/file` will work. Though it only works on unmerged paths, so you can't do this to sub-folders.
1. `git stash pop` will remove the change from the stack, while `git stash apply` will keep it.
1. Both `git pull` and `git clone` call `git fetch` under the hood, which is why the clone stats on your repos look weird as heck.
1. `git blame file/path` is pretty handy, and gives you what commit set any given line in the provided file
1. `git log SHA --patch`, or `-p` gives you the full commit log in addition to the commit message.
1. The `-S "SnippetToSearch"` [option](https://git-scm.com/docs/git-log#Documentation/git-log.txt--Sltstringgt) for `git log`, apparently called "the pickax", lets you search for commits that add or remove a given snippet.
1. `git rebase -i SHAToRebaseOnto` is super handy for squashing and fixing up commits. I.E: `git rebase -i HEAD^3` will rebase the last 3 commits on the branch.
1. `git push --force-with-lease` is a [safer version of `--force`](https://stackoverflow.com/a/52823955), which will warn you if someone else has made a change to the branch before you.
1. If you want to write your commits and things in something other than the system default, use `git config --global core.editor "subl -m"` or equivalent (this one will set up sublime).
1. If you find yourself wanting to write a comment, and can't refactor to avoid it, it might make more sense in a commit
1. the `--patch` or `-p` flag on [`git add`](https://git-scm.com/docs/git-add#Documentation/git-add.txt---patch) lets you stage individual changes instead of entire files, which is handy. It lets you split work into multiple commits cleanly in case a commit would be bloated.
1. Generally speaking, you should use the pickax instead of `git blame`.
1. `git annotate file/path` is like git blame, but sounds less mean and outputs in a slightly different format.
1. If you want to do something and don't know how, [Git Flight Rules](https://github.com/k88hudson/git-flight-rules) probably has something for you.
1. If you don't need to update the message when amending, use `git commit --amend -C HEAD` to reuse the commits message of the current head.
1. `git checkout -` will checkout the last branch you were on. (use `git co -` if you're cool and have a rad alias set up).
1. If you want to revert a `git rm`, you can use `git checkout HEAD path/to/file` to checkout the file again. It will take multiple files if desired.
1. `git push origin HEAD` is a shortcut for pushing the current branch. How did I not know this before now.
1. `git switch` and `git restore` are new commands introduced to alleviate confusion around checkout's overloading. `switch` lets you swap between branches, and `restore` lets you reset files to a revision. [See here](https://stackoverflow.com/a/57266005/13053386)
1. `git rerere` is a [little-known](https://www.git-scm.com/book/en/v2/Git-Tools-Rerere) feature to have git remember how you resolved a conflict and not ask you when it comes up again. Handy for big rebases and things. Set it with `git config --global rerere.enabled true`.
1. `git diff --staged` lets you know what you've changed without unstaging your commit-in-progress. Incredible.
1. `git log --grep regexp` lets you search your history using a regex.
1. `git diff @~ @` will let you know what was changed in the last commit. `@` is a shortcut for `HEAD`.
1. `git show` will show the contents of the `HEAD` commit. You can supply an argument if you want.
1. `git reflog --grep-reflog="*"` will let you grep through your reflog, which is occasionally useful.
1. `git rev-parse SHA` will let you convert a short-sha into the full sha, useful for diffing.

## Tips for maintaining a good commit history
1. These are all taken from the back half of [Branch In Time, RubyConf2018](https://youtu.be/8OOTVxKDwe0?t=1107).
1. Don't let yourself get in the habit of using `git commit -m "blablabla"`. The command line isn't conducive to writing good messages. This is why you know vim.
1. Turn on git verbose mode `git config --global commit.verbose true`, it'll give the full diff in the commit prompt.
1. Capture the why, not just the what. Capture context, since the code will tell you the what. Write what a future developer might want to know.
1. Your commits should be atomic. If you find yourself using the word "and", you might want to split it into multiple commits.
1. Write your commit messages as if you were explaining them to a co-worker beside you who didn't know what was happening. Answer: 1) Why is the change necessary, 2) how it addresses the issue, 3) what side effects it has, 4) which,if any, other solutions were considered, and 5) a reference to a discussion/ticket/document.
1. Get used to treating local commits as mutable so you can shape what will end up in master.
  1. the `git commit --amend` option is the most obvious
  1. `git rebase -i` is something to get better at though. You can always `git rebase --abort` though if things go sideways
1. Build your instincts and get used to searching through history.
1. "The ability to document code effectively using Git is just as important as being able to ship a feature, write clean code, or readable tests."
1. "Commit messages don't clutter up the code, and don't grow stale"


## Trivia
1. Git's man page is so notoriously bad a [Markov generator](https://git-man-page-generator.lokaltog.net/) exists to make fake ones.
1. Apparently, the reason `<<<<` and `>>>>` are used to mark merge conflicts is because at the time they were invalid syntax in all languages in use, at least from what [Getting More From Git - Alice Bartlett](https://youtu.be/FQ4IdcrOUz0) says.
