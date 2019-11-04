# Git <!-- omit in toc -->

- [General Configuration](#general-configuration)
- [rerere](#rerere)
- [Aliases](#aliases)
- [Windows](#windows)
  - [Editor](#editor)
- [Linux](#linux)
  - [fixup](#fixup)
  - [Prompt](#prompt)

## General Configuration
Location: `~/.gitconfig`
```
rebase.autosquash=true
push.default=tracking
branch.autsetuprebase=always
diff.renames=copies
rerere.enabled=true
github.user=Montellese
```

## rerere
```
git config --global rerere.enabled true
```

## Aliases
```
alias.s=status
alias.l="log --oneline"
alias.c=checkout
alias.cm="checkout master"
alias.cd="checkout development"
alias.f=fetch
alias.p=pull
alias.pr="pull --rebase"
alias.fix="commit --fixup"
alias.fixup="rebase --autosquash --autostash -i"
alias.rc="rebase --continue"
alias.rs="rebase --skip"
alias.ra="rebase --abort"
alias.force="push --force-with-lease"
alias.unstage="reset HEAD --"
alias.cp=cherry-pick
```

## Windows

### Editor
```
git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -nosession -noPlugin ''"
```

## Linux
### fixup
Location: `~/.bash_aliases`
```bash
function git-fixup () {
    if [ $# -ne 1 ]; then
        echo "Invalid number of arguments. Expecting a single commit hash!"
    else
        local FIXUP_COMMIT="$1"
        local FIXUP_COMMIT_MESSAGE=$(git log --format=%B -1 $FIXUP_COMMIT)
        echo "Fixing up commit \"$FIXUP_COMMIT_MESSAGE\" ($FIXUP_COMMIT) with current staged changes"

        git commit --fixup "$FIXUP_COMMIT" && \
            GIT_SEQUENCE_EDITOR=: git rebase --interactive --autosquash --autostash "$FIXUP_COMMIT"~1
    fi
}

alias gitfix="git-fixup"
```

### Prompt
Location: `~/.bash_aliases`
```bash

function git-find-branch {
  if [[ $head == ref:\ refs/heads/* ]]; then
    git_branch=" ${head#*/*/}"
  elif [[ $head != '' ]]; then
    git_branch=" ${head:0:6}"
  else
    git_branch=' (unknown)'
  fi
}

function git-find-head-file {
  local dir=$(pwd)
  local head=''
  local submod=''
  until [ "$dir" -ef / ]; do
  if [ -f "$dir/.git" ]; then
    submod=`basename $dir`
    git_at="@"
    git_repo="${submod}"
    until [ "$dir" -ef / ]; do
      if [ -f "$dir/.git/modules/$submod/HEAD" ]; then
        head=$(< "$dir/.git/modules/$submod/HEAD")
        git-find-branch
        return
      fi
      dir=`dirname $dir`
    done
  elif [ -f "$dir/.git/HEAD" ]; then
    git_at="@"
    git_repo="`basename $dir`"
    head=$(< "$dir/.git/HEAD")
    git-find-branch
    return
  fi
    dir=`dirname $dir`
  done
  git_branch=''
  git_at=''
  git_repo=''
}

# definition of bash prompt
export GIT_PROMPT="\[\e[0m\][\[\e[0;32m\]\u\[\e[0m\]@\[\e[0;33m\]\h\[\e[0;35m\]\$git_branch \[\e[0m\]\w]$\[\e[0m\]"
```

Location: `~/.bashrc`
```bash
# set regular executed command befor prompt printing
PROMPT_COMMAND="git-find-head-file; $PROMPT_COMMAND"
# definition of bash prompt
export PS1="$GIT_PROMPT "
```
