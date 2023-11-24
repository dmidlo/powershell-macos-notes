# Pester Installation

- `sudo pwsh`
- Single-user (current) Installation: `Save-Module -Name Pester -Path ~/.local/share/powershell/Modules`
- Shared (System) Installation: `Save-Module -Name Pester -Path $PSHOME/Modules`

- set up backwards compatibility for < PSv6.0 using

  - https://github.com/pester/Pester/issues/797#issuecomment-314495326
  - https://pester.dev/docs/usage/mocking#mocking-a-function-that-is-called-by-a-method-in-a-powershell-class

  - create user powershell config directory if it doesn't already exit
    - `mkdir ~/.config/powershell`
  - create `profile.ps1` in user config directory and add `Invoke-PesterJob` to start pester sessions in a new process each time they are executed to the first module that loads a class doesn't hijack it.
    - `touch .config/powershell/profile.ps1`

```Powershell
function Invoke-PesterJob
{
[CmdletBinding(DefaultParameterSetName='LegacyOutputXml')]
    param(
        [Parameter(Position=0)]
        [Alias('Path','relative_path')]
        [System.Object[]]
        ${Script},

        [Parameter(Position=1)]
        [Alias('Name')]
        [string[]]
        ${TestName},

        [Parameter(Position=2)]
        [switch]
        ${EnableExit},

        [Parameter(ParameterSetName='LegacyOutputXml', Position=3)]
        [string]
        ${OutputXml},

        [Parameter(Position=4)]
        [Alias('Tags')]
        [string[]]
        ${Tag},

        [string[]]
        ${ExcludeTag},

        [switch]
        ${PassThru},

        [System.Object[]]
        ${CodeCoverage},

        [switch]
        ${Strict},

        [Parameter(ParameterSetName='NewOutputSet', Mandatory=$true)]
        [string]
        ${OutputFile},

        [Parameter(ParameterSetName='NewOutputSet', Mandatory=$true)]
        [ValidateSet('LegacyNUnitXml','NUnitXml')]
        [string]
        ${OutputFormat},

        [switch]
        ${Quiet}
    )

    $params = $PSBoundParameters

    Start-Job -ScriptBlock { Set-Location $using:pwd; Invoke-Pester @using:params } |
    Receive-Job -Wait -AutoRemoveJob
}

Set-Alias ipj Invoke-PesterJob
```
