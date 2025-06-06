# Git cheat sheet

- [Git cheat sheet](#git-cheat-sheet)
  - [git settings to make your life better](#git-settings-to-make-your-life-better)
    - [Create remote branch on push](#create-remote-branch-on-push)
    - [Set default editor](#set-default-editor)
      - [VSCode](#vscode)
      - [Neovim](#neovim)
  - [Better merge experience](#better-merge-experience)
  - [git aliases](#git-aliases)


## git settings to make your life better

### Create remote branch on push
create a remote branch on push with out having to do `git branch --set-upstream-to <remote-branch>`

```bash
git config --global push.autoSetupRemote true
```

### Set default editor
set the default editor for git commits
#### VSCode
```bash
git config --global core.editor "code --wait"
```
#### Neovim
```bash
git config --global core.editor "nvim -f"
```

## Better merge experience

git rerere is a tool that helps you avoid the merge conflicts by remembering the state of the files after the last merge.

```bash
git config rerere.enabled true
```

## git aliases

```bash
git config --global alias.co checkout
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.br branch
git config --global alias.unstage 'reset HEAD --'  # unstage all changes
git config --global alias.last 'log -1 HEAD'  # show last commit
git config --global alias.undo 'reset HEAD~1'  # undo last commit
git config --global alias.main 'checkout main'  # checkout main branch
```

