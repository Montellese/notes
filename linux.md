# Linux <!-- omit in toc -->

- [Bash](#bash)
  - [synth-shell](#synth-shell)
- [SSH](#ssh)
  - [Autostart ssh-agent](#autostart-ssh-agent)


## Bash

### synth-shell

Source: https://github.com/andresgongora/synth-shell

```bash
git clone --recursive https://github.com/andresgongora/synth-shell.git
chmod +x synth-shell/setup.sh
cd synth-shell
./setup.sh
```

## SSH

### Autostart ssh-agent
Location: `~/.profile`
```bash
...

# see http://mah.everybody.org/docs/ssh#run-ssh-agent

SSH_ENV="$HOME/.ssh/environment"

function start_agent {
     echo "Initialising new SSH agent..."
     /usr/bin/ssh-agent | sed 's/^echo/#echo/' > "${SSH_ENV}"
     echo succeeded
     chmod 600 "${SSH_ENV}"
     . "${SSH_ENV}" > /dev/null
     /usr/bin/ssh-add;
}

# only start ssh-agent if it isn't already running (e.g. agent forwarding)
if [ -z "${SSH_AUTH_SOCK}" ]; then
    # source SSH settings, if applicable
    if [ -f "${SSH_ENV}" ]; then
        . "${SSH_ENV}" > /dev/null
        # ps ${SSH_AGENT_PID} doesn't work under cywgin
        ps -ef | grep ${SSH_AGENT_PID} | grep ssh-agent$ > /dev/null || {
            start_agent;
        }
    else
        start_agent;
    fi
fi
```
