# Git cheat sheet

- [Git cheat sheet](#git-cheat-sheet)
- [Git settings to make your life better](#git-settings-to-make-your-life-better)
  - [Create remote branch on push](#create-remote-branch-on-push)
  - [Set default editor](#set-default-editor)
    - [VSCode](#vscode)
    - [Neovim](#neovim)
  - [Better merge experience in rebase](#better-merge-experience-in-rebase)
  - [Git aliases](#git-aliases)
- [checkout previous branch](#checkout-previous-branch)
- [add to changes to last commit](#add-to-changes-to-last-commit)
- [Stash all files including untracked](#stash-all-files-including-untracked)


# Git settings to make your life better

## Create remote branch on push
create a remote branch on push without having to do `git branch --set-upstream-to <remote-branch>`. This is my favorite "new" get feature. 

```bash
git config --global push.autoSetupRemote true
```

## Set default editor
set the default editor for dealing with git messages
### VSCode
```bash
git config --global core.editor "code --wait"
```
### Neovim
```bash
git config --global core.editor "nvim -f"
```

## Better merge experience in rebase

git rerere is a tool that helps you avoid the merge conflicts by remembering the state of the files after the last merge.

```bash
git config rerere.enabled true
```

## Git aliases

Git aliases are shortcuts for git commands. You can create your own aliases to save time and make your workflow more efficient.

```bash
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

# checkout previous branch 

`-` is a special character in git that refers to the previous branch

```bash
git co -
```

```bash
git checkout -
```

# add to changes to last commit

```bash 
git add .

git commit --amend --no-edit

git push --force-with-lease
```


# Stash all files including untracked

```bash
git stash -u
```

