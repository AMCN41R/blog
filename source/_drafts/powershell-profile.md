---
title: Making things easier with a PowerShell profile
tags: 
    - PowerShell
    - Workflow
    - Developer
    - Productivity
    - Shell
    - Scripting
---

PowerShell is my go-to terminal. I use it a lot, mainly for git commands, but for things like mongo, js tooling (gulp etc), the dotnet cli, among others. 

Like similar tools, PowerShell allows you to create a custom profile that gets loaded each time you start a new session. This profile can be utilised for creating shortcuts, aliases and functions that make everyday tasks and commands quicker and easier. 

By default, each time PowerShell is launched, it will try and load a `.ps1` file called `Microsoft.PowerShell_profile.ps1` from `%USERDIR%\Documents\WindowsPowerShell\`. 
You can see the expected profile path by typing `$PROFILE`...

![PowerShell profile location](/blog/images/powershell-profile/profile-path.png)

Unfortunately, this file does not exist by default, so you will have to create it. The easiest way to do that is...

...create the directory if it does not exist:
```powershell
mkdir $env:USERPROFILE\Documents\WindowsPowerShell
```

...and then create your profile with:
```powershell
New-Item $PROFILE
```

Once the file has been created, you can open it for editing...
```powershell
ii $PROFILE
```
*`ii` is an alias for the PowerShell command [Invoke-Item](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/invoke-item?view=powershell-5.1).*


### Aliases
Create aliases to commonly used commands or programs using the [Set-Alias](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/set-alias?view=powershell-5.1) command.  For example, create an alias to launch notepad from the terminal by typing `np`...
```powershell
Set-Alias np notepad.exe
```


### Functions
Alternatively, you can create a function to encapsulate more behaviour. 

This example creates a shortcut for listing globally installed npm packages...
```powershell
function npmGlobals(){ npm ls -g --depth=0 }
```
![function example: npmGlobals](/blog/images/powershell-profile/function-1.png) 


#### Some simple git examples
As I said before, I primarily use PowerShell for running git commands, so I've created some shortcuts for the git commands I use often:

| Shortcut | Command           | Function                                   |
| -------- | ----------------- | ------------------------------------------ |
| gs       | git status        | function GS() { git status }               |
| gf       | git fetch --prune | function GF() { git fetch --prune }        |
| gc       | git commit -m     | function GC() { git commit -m $args[0] }   |


### Modifying the prompt
The default PowerShell prompt looks like this... 

![the default prompt](/blog/images/powershell-profile/prompt-1.png) 

You can override this by including a `Prompt` function in your profile...
```powershell
function Prompt {
    Write-Host ""
    Write-Host ("$ ") -NoNewline -ForegroundColor Gray
    Write-Host ($(Get-Location)) -NoNewline -ForegroundColor Yellow
    Write-Host (">") -NoNewline -ForegroundColor Gray
    return " "
}
```
![prompt example](/blog/images/powershell-profile/prompt-2.png) 


### Sharing your profile across multiple machines
I'm sure there are many ways to do this, but the way that works for me is using [OneDrive](https://onedrive.live.com). I have my personal profile stored in my OneDrive so that it is synced to each computer that I use. Then I just create the file explained in the steps above and simply 'dot-source' my actual profile...

My Microsoft.PowerShell_profile.ps1
```powershell
. 'C:\Users\Alex McNair\OneDrive\PowershellProfile.ps1'
```

### My Profile
You can check out my current profile on [GitHub](https://gist.github.com/AMCN41R/8e7ba6d1c59f930fff957a45b73caff4).


### Further reading
These are just a few examples that I use to make daily tasks a bit easier. There are plenty of helpful blogs and resources online, including the following to articles:

[Understanding the Six PowerShell Profiles](https://blogs.technet.microsoft.com/heyscriptingguy/2012/05/21/understanding-the-six-powershell-profiles/)
[Persistent PowerShell: The PowerShell Profile](https://www.red-gate.com/simple-talk/sysadmin/powershell/persistent-powershell-the-powershell-profile/)
