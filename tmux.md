# tmux <!-- omit in toc -->

- [Usage](#usage)
- [General configuration](#general-configuration)
- [Specific configurations](#specific-configurations)
  - [Enable mouse control](#enable-mouse-control)
- [Plugins](#plugins)
  - [Plugin Manager (tpm)](#plugin-manager-tpm)
    - [Installation](#installation)
    - [Configuration](#configuration)
    - [Installing plugins](#installing-plugins)
    - [Updating plugins](#updating-plugins)
  - [nord-tmux](#nord-tmux)
  - [tmux-resurrect](#tmux-resurrect)
    - [Configuration](#configuration-1)
    - [Usage](#usage-1)
  - [tmux-continuum](#tmux-continuum)
- [Linux specific configuration](#linux-specific-configuration)
  - [Update SSH and DISPLAY environment variables](#update-ssh-and-display-environment-variables)

## Usage

Cheat Sheet: https://tmuxcheatsheet.com/

* Start tmux session: `tmux`
* Attach to an existing tmux session: `tmux a`
* List active tmux sessions: `tmux ls`

## General configuration
Location: `~/.tmux.conf`
```
# remap prefix from "Ctrl+B" to "Ctrl+A"
unbind C-b
set-option -g prefix C-a
bind-key C-a send-prefix

# don't automatically rename the window
set-option -g allow-rename off

# start window index at 1
set -g base-index 1

# split panes using | and -
bind | split-window -h
bind - split-window -v
unbind '"'
unbind %

# reload config file
bind r source-file ~/.tmux.conf

# switch panes using Alt-arrow without prefix
bind -n M-Left select-pane -L
bind -n M-Right select-pane -R
bind -n M-Up select-pane -U
bind -n M-Down select-pane -D

# reload the environment
set -g update-environment -r
set-environment -g 'SSH_AUTH_SOCK' ~/.ssh/ssh_auth_sock
```

## Specific configurations

### Enable mouse control

**This breaks some other tmux features and is therefore disabled!**
```
# Enable mouse control (clickable windows, panes, resizable panes)
# set -g mouse on
```

## Plugins

### Plugin Manager (tpm)

Source: https://github.com/tmux-plugins/tpm

#### Installation

```bash
mkdir -p ~/.tmux/plugins
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
tmux source ~/.tmux.conf
```

#### Configuration

```
###########################
### Tmux Plugin Manager ###
###########################

# List of plugins
set -g @plugin 'tmux-plugins/tpm'

...

# Initialize TMUX plugin manager (keep this line at the very bottom of tmux.conf)
run -b '~/.tmux/plugins/tpm/tpm'
```

#### Installing plugins

1. Add new plugin to `~/.tmux.conf` with `set -g @plugin '...'`.
2. Press `<prefix> + I` to fetch the plugin(s).

#### Updating plugins

Press `<prefix> + U` to update the installed plugins.

### nord-tmux

Source: https://github.com/arcticicestudio/nord-tmux

```
set -g @plugin 'arcticicestudio/nord-tmux'
```

### tmux-resurrect

Source: https://github.com/tmux-plugins/tmux-resurrect

#### Configuration

```
set -g @plugin 'tmux-plugins/tmux-resurrect'
```

#### Usage

* Save: `<prefix> + Ctrl-s`
* Restore: `<prefix> + Ctrl-r`

### tmux-continuum

Source: https://github.com/tmux-plugins/tmux-continuum

**Make sure tmux-resurrect is installed!**

```
# automatically start tmux server
set -g @continuum-boot 'on'
# automatically restore last saved tmux environment
set -g @continuum-restore 'on'

...

set -g @plugin 'tmux-plugins/tmux-resurrect'
set -g @plugin 'tmux-plugins/tmux-continuum'
```

## Linux specific configuration

### Update SSH and DISPLAY environment variables

Location: `~/.ssh/rc`
```bash
#!/bin/bash

# http://techblog.appnexus.com/2011/managing-ssh-sockets-in-gnu-screen/
# https://gist.github.com/martijnvermaat/8070533
# http://stackoverflow.com/questions/21378569/how-to-auto-update-ssh-agent-environment-variables-when-attaching-to-existing-tm

# Fix SSH auth socket location so agent forwarding works with tmux / screen.
if [ ! -S ~/.ssh/ssh_auth_sock ] && [ -S "$SSH_AUTH_SOCK" ]; then
    ln -sf $SSH_AUTH_SOCK ~/.ssh/ssh_auth_sock
fi

# Don't break x11 Forwarding:
# Taken from the sshd(8) manpage.
if read proto cookie && [ -n "$DISPLAY" ]; then
        if [ `echo $DISPLAY | cut -c1-10` = 'localhost:' ]; then
                # X11UseLocalhost=yes
                echo add unix:`echo $DISPLAY |
                    cut -c11-` $proto $cookie
        else
                # X11UseLocalhost=no
                echo add $DISPLAY $proto $cookie
        fi | xauth -q -
fi
```
