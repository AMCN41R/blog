---
title: powershell-profile
tags:
---

I use PowerShell a lot. For whatever reason, I am a fan, and it's my go to terminal. I mainly use it for git, but also for things like mongo, js tooling (gulp etc), as well as other things.

Like similar tools (such as bash etc), PowerShell allows you to create a custom profile that will get loaded each time you start a new session.

This profile can be utilised for creating shortcuts, aliases and functions to make everyday tasks and commands quicker and easier. 

By default, each time PoewrShell is launched, it will try and load a ps1 file called `Microsoft.PowerShell_profile.ps1` in `%USERDIR%\Documents\WindowsPowerShell\`. You can see this by simply typing `$PROFILE`...

![PowerShell profile location](/blog/images/powershell-profile/profile-path.png)

Unfortunately, this file does not exist by default, so you will have to create it. I'm sure you're already a PowerShell pro, but if you didn't know, the easiest way to create this file is simply...
```powershell
New-Item $PROFILE
```

Once the file has been created, you can open it for editing...
```powershell
ii $PROFILE
```
*`ii` is an alias for the PowerShell command `Invoke-Item`.*

### Aliases

### Functions

### Modifying the prompt

### Git examples

### Windows explorer shortcuts

### Don't forget to clear the terminal!

### Profile across multiple machines