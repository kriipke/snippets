# PowerShell and Environment Variables: A Deep Dive
Using PowerShell to set Windows environment variables, read environment variables and create new environment variables is easy once you know the trick. The tricks you learn in this article will work for Windows 10 environment variables as well as any Windows client/server after Windows 7 SP1/Windows Server 2008.

PowerShell provides many different ways to interact with Windows environment variables from the `$env:` PSDrive. the registry, and the `[System.Environment]` .NET class. You’ll learn about each method including understand environment variable scope in this step-by-step walkthrough.

## Assumptions

Throughout this article, I’ll be using Windows PowerShell 5.1 on Windows 10. But if you have any version of Windows PowerShell later than v3 on Windows 7 SP1 or later, the techniques I’m about to show you should work just fine.

## What are Environment Variables?

Environment variables, as the name suggests, store information about the environment that is used by Windows and applications. Environment variables can be accessed by graphical applications such as Windows Explorer and plain text editors like Notepad, as well as the cmd.exe and PowerShell.

Using environment variables helps you to avoid hard-coding file paths, user or computer names and much more in your PowerShell scripts or modules.

0 seconds of 1 minute, 4 secondsVolume 0%

This ad will end in 10

00:05

00:09

00:15

## Common Environment Variables

As you begin to learn more about how to work with environment variables in PowerShell, you’ll come across many different variables. Some are more useful than others. Below is a list of some of the common environment variables and their usage for reference.

| Variable                           | Usage                                                                                                                                                                                                                                                                                                   |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ClientName                         | The name of the remote computer connected via a Remote Desktop session.                                                                                                                                                                                                                                 |
| SessionName                        | This helps to identify if the current Windows session is regarded by the operating system as running at the console. For console sessions SessionName will be ‘Console’. Enhanced Session connections to Hyper-V Virtual Machines do not report SessionName as ‘Console’, whereas Standard Sessions do. |
| ComputerName                       | The name of the computer.                                                                                                                                                                                                                                                                               |
| SystemRoot and Windir              | The path to the current Windows installation.                                                                                                                                                                                                                                                           |
| ProgramFiles and ProgramFiles(x86) | The default locations for x64 and x86 programs.                                                                                                                                                                                                                                                         |
| ProgramW6432                       | The default location for programs, avoiding 32/64 bit redirection. This variable only applies for 32 bit processes running on a 64 bit platform. This means that you can use it to identify when a 32 bit instance of PowerShell is running on a 64 bit system.                                         |
| UserDNSDomain                      | The Fully Qualified Domain Name of the Active Directory domain that the current user logged on to. Only present for domain logons.                                                                                                                                                                      |
| UserDomain                         | The NETBIOS-style name of the domain that the current user logged on to. Can be a computer name if there’s no domain.                                                                                                                                                                                   |
| UserDomainRoamingProfile           | The location of the central copy of the roaming profile for the user, if any. Only present for domain logons.                                                                                                                                                                                           |
| UserName                           | The name of the currently logged on user.                                                                                                                                                                                                                                                               |
| UserProfile                        | The location of the profile of the current user on the local computer.                                                                                                                                                                                                                                  |

## Environment Variable Scopes

There are three _scopes_ of environment variables. Think of scopes as layers of variables that build up to give a total picture. Combined, these “layers” provide many different environment variables to any running process in Windows.

### Environment Variable Scope “Hierarchy”

Each of these “layers” either combine or overwrite one another. They are defined in a hierarchy like: _machine_ –> _user_ –> _process_ with each scoped variable overwriting the parent variable if one exists in the parent scope.

For example, a common environment variable is `TEMP`. This variable stores the folder path to Windows’ local temporary folder. This environment variable is set to:

-   `C:\WINDOWS\TEMP` in the _machine_ scope
-   `C:\Users\<username>\AppData\Local\Temp` in the _user_ scope
-   `C:\Users\<username>\AppData\Local\Temp` in the _process_ scope.

If there’s no `TEMP` environment variable set in the _user_ scope then the end result will be `C:\WINDOWS\TEMP`.

### Environment Variable Scope Types

There are three different environment variable scopes in Windows.

### Machine

Environment variables in the _machine_ scope are associated with the running instance of Windows. Any user account can read these, but setting, changing or deleting them needs to done with elevated privileges.

### User

Environment variables in the _user_ scope are associated with the user running the current process. _User_ variables overwrite \_machine-\_scoped variables having the same name.

> _Note: Microsoft recommends that Machine and User scoped environment variable values contain no more than 2048 characters._

### Process

Environment variables in the _process_ scope are a combination of the _machine_ and _user_ scopes, along with some variables that Windows creates dynamically.

Below is a list of environment variables available to a running process. All of these variables are dynamically created.

-   `ALLUSERSPROFILE`
-   `APPDATA`
-   `COMPUTERNAME`
-   `HOMEDRIVE`
-   `HOMEPATH`
-   `LOCALAPPDATA`
-   `LOGONSERVER`
-   `PROMPT`
-   `PUBLIC`
-   `SESSION`
-   `SystemDrive`
-   `SystemRoot`
-   `USERDNSDOMAIN`
-   `USERDOMAIN`
-   `USERDOMAIN_ROAMINGPROFILE`
-   `USERNAME`
-   `USERPROFILE`

## Environment Variables in the Registry

Environment variables are stored in two registry locations, one for the user scope and one for the machine scope.

### Don’t Use the Registry to Manage Environment Variables

There’s a catch when making changes to variables inside of the registry. Any running processes will not see variable changes in the registry. Processes only see the registry variables and values that were present when the process was started, unless Windows notifies them that there has been a change.

Instead of modifying the registry directly, you can use a .NET class instead. The .NET `[System.Environment]` class can modify machine and user\_–\_scoped environment variables and handle the registry housekeeping for you.

Modifying environment variables in the registry directly, whilst possible, doesn’t make sense. The .NET class offers a simpler, Microsoft-supported approach. You’ll learn about using the `[System.Environment]` .NET class later in this article.

### Environment Variable Registry Locations and Querying

I hope you’ve been convinced to not modify the registry directly but if you’d like to take a peek at what’s in there, you can find all user environment variables in the `HKEY_CURRENT_USER\Environment` key. Machine-scoped environment variables are stored at `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment`.

Inside of either of these keys lies registry values of type `REG_SZ` or `REG_EXPAND_SZ`. `REG_EXPAND_SZ` values hold environment variables embedded as part of their value. These environment variables are expanded when the value is retrieved.

To demonstrate this, use the `REG` utility. This is a small command-line utility included with Windows.

Query the `TEMP` environment variable as seen below. Run `REG` with the `QUERY` parameter to retrieve the value of the `TEMP` variable.

    > REG QUERY HKCU\Environment /V TEMP

    HKEY_CURRENT_USER\Environment     
        TEMP    REG_EXPAND_SZ    %USERPROFILE%\AppData\Local\Temp

> _You’ll sometimes notice environment variables displayed surrounded by percentage symbols (`%COMPUTERNAME%`) like above. This is the old-school way of showing environment variables via cmd.exe and batch files. Know that PowerShell does not recognize this format._

The `REG` utility allows us to see the native value of the registry value. The value type is `REG_EXPAND_SZ` and the value contains the `%USERPROFILE%` environment variable.

If you’d rather use PowerShell to retrieve the registry value, you can so using the `Get-Item` cmdlet as shown below.

    PS51> Get-ItemProperty -Path HKCU:\Environment -Name TEMP

    TEMP         : C:\Users\<your username>\AppData\Local\Temp
    PSPath       : Microsoft.PowerShell.Core\Registry::HKEY_CURRENT_USER\Environment
    PSParentPath : Microsoft.PowerShell.Core\Registry::HKEY_CURRENT_USER
    PSChildName  : Environment
    PSDrive      : HKCU
    PSProvider   : Microsoft.PowerShell.Core\Registry

## View and Set Windows Environment Variables via the GUI

To see a GUI view of the user and system environment variables, run _SystemPropertiesAdvanced.exe_ from PowerShell, a command prompt or from Windows Key+R to display the _System Properties Advanced_ tab. Click on the _EnvironmentVariables_ button, which is highlighted in the image below.

![](https://adamtheautomator.com/wp-content/uploads/2019/09/SystemPropertiesAdvanced.png)

_The System Properties dialog, Advanced tab_

The Environment Variables dialog, shown below, allows you to view, create and modify user and machine-scoped environment variables. Note that the dialog refers to machine scoped variables as _System variables_.

![](https://adamtheautomator.com/wp-content/uploads/2019/09/EnvironmentVariables.png)

_The Environment Variables dialog_

Now that you have an understanding of environment variables, let’s get to what you’re here for, [managing them with PowerShell](https://adamtheautomator.com/powershell-scheduled-task/)!

There are a few different ways you can interact with environment variables using PowerShell.

-   **The `Env:` PSDrive and Provider** – session-based. Only sets the values of environment variables for the current PowerShell session
-   **`$env:` variables** – session-based. Only sets the values of environment variables for the current PowerShell session
-   **The `[System.Environment]` .NET Class** – allows you to persist user and system-scoped environment variables across sessions and reboots

### The `Env:` PSDrive and Provider

One of the best way to read environment variables is a PowerShell concept known as PowerShell drives (PS drives). A PS drive allows you to treat environment variables as if they are a file system through the `Env:` drive.

#### Switching to the `Env:` Drive

Like all PS drives, you reference it via paths like `Env:\TEMP`, `Env:\COMPUTERNAME`, etc. But to save some keystrokes, switch to the `Env:` drive just as you would any file system drive, as shown below.

    PS51> cd Env:
    PS51 Env:\>



    PS51> Set-Location Env:
    PS Env:\>

#### Tab-Completion with the `Env:` Drive

You can use the same commands you would use to access a file system, such as `Get-Item` and `[Get-ChildItem](https://adamtheautomator.com/get-childitem/ "Get-ChildItem")` to access environment variables. But instead of the file system, you’re reading the `Env:` drive.

Because environment variables are stored in a PS drive, you can use the tab completion feature of PowerShell to cycle through the available variables, as shown below.

![](https://adamtheautomator.com/wp-content/uploads/2020/06/TabCompletion-1-1.gif)

_Env: tab completion – finding environment variables starting with the letter C_

Let’s now jump into a couple examples of how you can use the `Env:` PS drive to work with environment variables.

### Listing Environment Variables with `Env:`

    PS51> Get-Item -Path Env:
    PS51> Get-Item -Path Env:USERNAME

### Listing Environment Variables Using a Wildcard with `Env:`

    PS51> Get-Item -Path Env:user*

### Finding Environment Variable Values with `Env:`

The results of these commands are key/value `[System.Collections.DictionaryEntry]` .NET objects. These objects hold the environment variable’s name in the `Name` property and the value in the `Value` property.

You can access a specific value of an environment variable by wrapping the `Get-Item` command reference in parentheses and referencing the `Value` property as shown below:

    PS51> (Get-Item -Path Env:computername).Value
    MYCOMPUTERHOSTNAME

In situations where you need to only return certain environment variables, use standard PowerShell cmdlets such as `Select-Object` and `[Where-Object](https://adamtheautomator.com/powershell-where-object/ "Where-Object")` to select and filter the objects returned by the `Env:` provider.

In the example below, only the environment variable `COMPUTERNAME` is returned.

    PS51> Get-ChildItem -Path Env: | Where-Object -Property Name -eq 'COMPUTERNAME'

As an alternative method, use the `[Get-Content](https://adamtheautomator.com/powershell-get-content/ "Get-Content")` cmdlet. This cmdlet returns a `[String]` object containing the value of the environment variable. This object is simpler to deal with as it returns only the value, rather than an object with `Name` and `Value` properties.

    PS51> Get-Content -Path Env:\COMPUTERNAME

#### Demo: Inserting Environment Values in a String

Using `Get-Content`, you can find the value of an environment variable and insert the `COMPUTERNAME` environment variable, for example, into a text string.

    PS51> 'Computer Name is: {0}' -f (Get-Content -Path Env:COMPUTERNAME)
    Computer Name is: MYCOMPUTER

### Setting an Environment Variable (And Creating) with `Env:`

Create new environment variables with PowerShell using the `New-Item` cmdlet. Provide the name of the environment variable in the form `Env:\<EnvVarName>` for the `Name` value and the value of the environment variable for the `Value` parameter as shown below.

    PS51> New-Item -Path Env:\MYCOMPUTER -Value MY-WIN10-PC
    Name                           Value
    ----                           -----
    MYCOMPUTER                     MY-WIN10-PC

Use the `Set-item` cmdlet to set an environment variable, or create a new one if it doesn’t already exist. You can see below using the `Set-Item` cmdlet, you can both create or modify and environment variable.

    PS51> Set-Item -Path Env:testvariable -Value "Alpha"

### Copying an Environment Variable with `Env:`

Sometimes the situation arises that you need to replicate the value of an environment variable. You can do this using the `[Copy-Item](https://adamtheautomator.com/copy-item/ "Copy-Item")` cmdlet.

Below you can see that the value of the `COMPUTERNAME` variable is copied to `MYCOMPUTER`, overwriting its existing value.

    PS51> Get-Item -Path Env:\MYCOMPUTER

    Name                           Value
    ----                           -----
    MYCOMPUTER                     MY-WIN10-PC

    PS51> Copy-Item -Path Env:\COMPUTERNAME -Destination Env:\MYCOMPUTER
    PS51> Get-Item -Path Env:\MYCOMPUTER

    Name                           Value
    ----                           -----
    MYCOMPUTER                     WIN10-1903

### Removing an Environment Variable with `Env:`

Situations will arise where an environment variable is no longer needed. You can remove environment variables using one of three methods:

-   Use the `Set-Item` cmdlet to set an environment variable to an empty value


    PS51> Set-Item -Path Env:\MYCOMPUTER -Value ''

-   Use the `[Remove-Item](https://adamtheautomator.com/powershell-delete-file/ "Remove-Item")` cmdlet.


    PS51> Remove-Item -Path Env:\MYCOMPUTER

-   Use the `Clear-Item` cmdlet.


    PS51> Clear-Item -Path Env:\MYCOMPUTER

### Renaming an Environment Variable with `Env:`

In situations where the name of an environment variable needs to be changed, you have the option to rename, rather than delete and recreate with the `Env:` provider.

Use the `Rename-Item` cmdlet to change the name of an environment variable whilst keeping its value. Below you can see that you can see that the `MYCOMPUTER` variable is renamed to `OLDCOMPUTER` whilst retaining its value.

    PS51> Rename-Item -Path Env:\MYCOMPUTER -NewName OLDCOMPUTER
    PS51> Get-Item -Path OLDCOMPUTER
    Name                           Value
    ----                           -----
    OLDCOMPUTER                    WIN10-1903

## `$Env:` Variables

Having mastered the `Env:` drive to treat environment variables as files, this section shows you how to treat them as variables. Another way you can manage in-session environment variables is using the the [PowerShell Expression Parser](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_parsing?view=powershell-5.1). This feature allows you to use the `$Env:` scope to access environment variables.

### Getting an Environment Variable with `$Env:`

Using the `$Env` scope, you can reference environment variables directly without using a command like `Get-Item` as shown below.

This method makes it easy to insert environment variables into strings like below:

    PS51> "The Computer Name is {0}" -f $env:computername
    The Computer Name is WIN10-1809
    PS51> "The Computer Name is $env:computername"
    The Computer Name is WIN10-1809

### Setting or Creating an Environment Variable with `$Env:`

Setting an environment variable using this method is straightforward. This will also create a new environment variable if one does not already exist like below.

    PS51> $env:testvariable = "Alpha"

Use the `+=` syntax to add to an existing value, rather than overwriting it.

    PS51> $env:testvariable = "Alpha"
    PS51> $env:testvariable += ",Beta"

    PS51> $env:testvariable
    Alpha,Beta

### Removing an Environment Variable with `$Env:`

To remove an environment variable using this method, simple set its value to an empty string.

    PS51> $env:testvariable = ''

## Using the `[System.Environment]` .NET Class

The .NET class `[System.Environment]` offers methods for getting and setting environment variables also. This is only method to access various environment scopes directly and set environment variables that survive across PowerShell sessions.

> _In all of the following examples, if no scope is provided, the process scope is assumed._

When using the `[System.Environment]`, you’ll use a few different [.NET static class](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/static-classes-and-static-class-members) methods. You don’t need to understand what a static method is. You only need to understand to use any of the techniques you’re about to learn, you’ll need to first reference the class (`[System.Environment]`) followed by two colons (`::`) then followed by the method.

### Listing Environment Variables with `[System.Environment]`

If you’d like to see all environment variables in a particular scope, you’d use the `GetEnvironmentVariables` method. This method returns all environment variables by the scope specified as the method argument (in parentheses).

```


Listing environment variables with GetEnvironmentVariable in each scope

`PS51> [System.Environment]::GetEnvironmentVariables('User')
PS51> [System.Environment]::GetEnvironmentVariables('Machine')
PS51> [System.Environment]::GetEnvironmentVariables('Process')

PS51> [System.Environment]::GetEnvironmentVariables()`
```

### Getting Single Environment Variables with `[System.Environment]`

If you need to find a specific environment variable you can do so two different ways.

-   `GetEnvironmentVariables().<var name>` – not recommended
-   `GetEnvironmentVariable('<var name>','<scope>')`

#### `GetEnvironmentVariables()`

Using the `GetEnvironmentVariables()` method, you use dot notation to reference the value. Enclose the `[System.Environment]` class and static method reference in parentheses followed by a dot then the name of the environment variable like below:

    PS51> ([System.Environment]::GetEnvironmentVariables()).APPDATA

> _Note that when referencing environment variables this way, you must ensure you match capitalization! In the example above, try to reference the `APPDATA` variable using `appdata`. Use the `GetEnvironmentVariable()` instead._

#### `GetEnvironmentVariable()`

Rather than using the `GetEnvironmentVariables()` method, instead use `GetEnvironmentVariable()` to find single environment variables. It gets around the issue with capitalization and also allows you to specify the scope.

To use this method, specify the environment variable name and the scope you’d like to look for that variable in separated by comma.

    PS51> [System.Environment]::GetEnvironmentVariable('ComputerName','User')

    PS51> [System.Environment]::GetEnvironmentVariable('ComputerName','Machine')

    PS51> [System.Environment]::GetEnvironmentVariable('ComputerName','Process')  WIN10-1903


    PS51> [System.Environment]::GetEnvironmentVariable('ComputerName')
    WIN10-1903

### Setting an Environment Variable with `[System.Environment]`

Use the `SetEnvironmentVariable()` method to set the value of an environment variable for the given scope, or create a new one if it does not already exist.

When setting variables in the process scope, you’ll find that the process scope is volatile while changes to the user and machine scopes are permanent.

    PS51> [System.Environment]::SetEnvironmentVariable('TestVariable','Alpha','User')

    PS51> [System.Environment]::SetEnvironmentVariable('TestVariable','Alpha','Process')

    PS51> [System.Environment]::SetEnvironmentVariable('TestVariable','Alpha','Machine')

     
    PS51> [System.Environment]::SetEnvironmentVariable('TestVariable','Alpha')

> Note: Calling the SetEnvironmentVariable method with a variable name or value of 32767 characters or more will cause an exception to be thrown.

### Removing an Environment Variable with `[System.Environment]`

Use the `SetEnvironmentVariable()` method to remove an environment variable for the given scope by setting its value to an empty string.

    PS51> [System.Environment]::SetEnvironmentVariable('TestVariable', '', 'User')

    PS51> [System.Environment]::SetEnvironmentVariable('TestVariable', '', 'Process')

    PS51> [System.Environment]::SetEnvironmentVariable('TestVariable', '', 'Machine')

    PS51> [System.Environment]::SetEnvironmentVariable('TestVariable', '')

## Useful PowerShell Environment Variables

Like many other Windows applications, PowerShell has some environment variables of it’s own. Two useful environment variables to know about are `PSExecutionPolicyPreference` and `PSModulePath`.

### `PSExecutionPolicyPreference`

The `PSExecutionPolicyPreference` environment variable stores the current PowerShell execution policy. It is created if a session-specific PowerShell execution policy is set by:

-   Running the `Set-ExecutionPolicy` cmdlet with a `Scope` parameter of `Process`
-   Running the `powershell.exe` executable to start a new session, using the `ExecutionPolicy` command line parameter to set a policy for the session.

### `PSModulePath`

The `PSModulePath` environment variable contains the path that PowerShell searches for modules if you do not specify a full path. It is formed much like the standard `PATH` environment variable, with individual directory paths separated by a semicolon.

    PS51> $env:PSModulePath
    C:\Program Files\WindowsPowerShell\Modules;C:\WINDOWS\system32\WindowsPowerShell\v1.0\Modules

> _Quick tip: Split each folder to get a string array to process each path individually using $env:PSModulePath.split(‘;’)_

## Summary

Environment variables are a useful method for getting information about a running system, or storing information across sessions and reboots. Whether you’re just reading the default Windows operating system environment variables and creating your own, you now can manage them using a variety of ways with PowerShell!

## Further Reading

-   **_[about_Environment_Variables](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_environment_variables?view=powershell-7.2)_**
-   **_The .NET [\[System.Environment\] Class](https://docs.microsoft.com/en-us/dotnet/api/system.environment?view=netframework-4.0) on Microsoft docs_**
-   **_[Learn the PowerShell string format and expanding strings](https://adamtheautomator.com/powershell-string-format/)_** 
    [https://adamtheautomator.com/powershell-environment-variables/](https://adamtheautomator.com/powershell-environment-variables/) 
    [https://adamtheautomator.com/powershell-environment-variables/](https://adamtheautomator.com/powershell-environment-variables/)
