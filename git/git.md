# Git cheat sheet

- [Git cheat sheet](#git-cheat-sheet)
- [Git settings to make your life better](#git-settings-to-make-your-life-better)
- [Daily usage Tips](#daily-usage-tips)
  - [Checkout previous branch](#checkout-previous-branch)
  - [Add to changes to last commit](#add-to-changes-to-last-commit)
  - [Stash all files including untracked](#stash-all-files-including-untracked)
- [Rebasing branches](#rebasing-branches)


# Git settings to make your life better

```bash
# create a remote branch on push without having to do `git branch --set-upstream-to <remote-branch>`. This is my favorite "new" get feature. 

git config --global push.autoSetupRemote true

# set VSCode as default editor for git commits messages
git config --global core.editor "code --wait"


# git rerere is a tool that helps you avoid the merge conflicts by remembering the state of the files after the last merge.
git config rerere.enabled true

# some of my favorite git aliases 
git config --global alias.co checkout
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.br branch
git config --global alias.unstage 'reset HEAD --'  # unstage all changes
git config --global alias.last 'log -1 HEAD'  # show last commit
git config --global alias.undo 'reset HEAD~1'  # undo last commit
git config --global alias.main 'checkout main'  # checkout main branch
git config --global alias.logs log # I never remember the command to show logs is log or logs so it is now both 

```

# Daily usage Tips 

## Checkout previous branch 

`-` is a special character in git that refers to the previous branch

```bash
git co -
```

```bash
git checkout -
```

## Add to changes to last commit

```bash 
git add .

git commit --amend --no-edit

git push --force-with-lease
```


## Stash all files including untracked
```bash
git stash -u
```
Note: oh my zsh git addon has `gstu` alias for this too

# Rebasing branches

Rebasing checkouts out the branch you are rebasing and then applies the changes from the branch you are rebasing onto. 

`git rebase -it <branch-to-rebase-onto>`

The current changes are changes from the branch you are rebasing onto and the incoming changes are changes from the branch you are rebasing.

-current changes -- "production code"

-incoming changes -- **my** changes.

when you rebase you can use the following commands to help you:

- `git rebase --continue` - continue the rebase process
- `git rebase --abort` - abort the rebase process
- `git rebase --skip` - skip the current commit
- `git rebase --edit-todo` - edit the todo list
- `git rebase --onto <new-branch> <branch-to-rebase-onto>` - rebase onto a new branch



