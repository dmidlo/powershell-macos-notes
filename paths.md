# Powershell MacOS Paths

- $PSHOME is `/usr/local/microsoft/powershell/7`
    - `/usr/local/bin/pwsh` is a symlink to `$PSHOME/pwsh`
- Profiles
    - User: `~/.config/powershell/profile.ps1`
    - Default: `$PSHOME/profile.ps1`
- Modules
    - User: `~/.local/share/powershell/Modules`
    - Shared: `/usr/local/share/powershell/Modules`
    - Default: `$PSHOME/Modules`
- PSReadLine history
    - `~/.local/share/powershell/PSReadline/ConsoleHost_history.txt`