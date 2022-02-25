# Windows <!-- omit in toc -->

- [Autostart](#autostart)
- [Software Packages](#software-packages)
  - [chocolatey](#chocolatey)
    - [Global Confirmation](#global-confirmation)
    - [List of Packages](#list-of-packages)
  - [X Server](#x-server)
  - [SSH Keys / pageant](#ssh-keys--pageant)
- [PuTTY](#putty)
  - [nord-putty](#nord-putty)

## Autostart
1. Windows + R
2. `shell:Startup`
3. Create / Paste shortcut

## Software Packages

### chocolatey
* Source: https://chocolatey.org/
* Installation: https://chocolatey.org/install
* Packages: https://chocolatey.org/packages

#### Global Confirmation
```bash
choco feature enable -n allowGlobalConfirmation
```

#### List of Packages
```
7zip.install
adobereader
chocolatey
chocolatey-core.extension
cmake
git.install
GoogleChrome
javaruntime
jre8
notepadplusplus.install
paint.net
passwordsafe
putty.install
vcxsrv
virtualbox
vlc
vmware-workstation-player
vscode
winmerge
```

### X Server
Create an autostart shortcut with the following **Target**:
```bash
"C:\Program Files\VcXsrv\vcxsrv.exe" :0 -ac -terminate -lesspointer -multiwindow -clipboard -wgl
```

### SSH Keys / pageant
Create an autostart shortcut with the following **Target**:
```bash
"C:\Program Files\PuTTY\pageant.exe" [multiple-paths-to-.ppk's]
```

## PuTTY

### nord-putty

Source: https://github.com/arcticicestudio/nord-putty

1. Download [nord.reg](https://github.com/arcticicestudio/nord-putty/blob/develop/src/nord.reg)
2. Execute `nord.reg`
3. Open PuTTY
4. Switch to the `Session` node in the tree
5. Select `Nord` from the `Saved Sessions` list
