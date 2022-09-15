# Git <!-- omit in toc -->

- [General Configuration](#general-configuration)
- [Aliases](#aliases)
- [Signing Commits using an SSH Key](#signing-commits-using-an-ssh-key)
- [Windows](#windows)
  - [Editor](#editor)
- [Linux](#linux)
  - [Prompt](#prompt)

## General Configuration
Location: `~/.gitconfig`
```
[rebase]
    autosquash = true
[push]
    default = tracking
[branch]
    autsetuprebase = always
[diff]
    renames = copies
[rerere]
    enabled = true

[github]
    user = Montellese
[user]
    name = Montellese
    email = sascha.montellese@gmail.com

[commit]
        gpgsign = true
[gpg]
        format = ssh
[gpg "ssh"]
        allowedSignersFile = C:/Users/Sascha/.ssh/allowed_signers
```

## Aliases
```
[alias]
    s = status
    l = "log --oneline"
    c = checkout
    cm = "checkout master"
    cd = "checkout development"
    f = fetch
    p = pull
    pr = "pull --rebase"
    fix = "!fix() { git commit --fixup $1 && GIT_SEQUENCE_EDITOR=: git rebase --interactive --autosquash --autostash $1~1; }; fix"
    rc = "rebase --continue"
    rs = "rebase --skip"
    ra = "rebase --abort"
    force = "push --force-with-lease"
    unstage = "reset HEAD --"
    cp = cherry-pick
    fixup = "commit --fixup"
```

## Signing Commits using an SSH Key
Source: [Signing Git Commits with Your SSH Key](https://calebhearth.com/sign-git-with-ssh)
```bash
git config --global commit.gpgsign true
git config --global gpg.format ssh
git config --global user.signingkey "<SSH private key identifier>"
```
Verifying signed commits using SSH keys:
```bash
git config --global gpg.ssh.allowedsignersfile=~/.ssh/allowed_signers
touch ~/.ssh/allowed_signers
echo "* <SSH public key identifier>" > ~/.ssh/allowed_signers
```

## Windows

### Editor
```
git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -nosession -noPlugin ''"
```

## Linux

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
