---
layout: post

title: PS GUI - Hiding PowerShell Console Window
---
A typical problem when running PowerShell GUI tools is what to do with the console window that is active for the duration of the script.
You can always move it out of the way so it's not an eyesore but its not an elegant solution and deep down you know its there judging
your inability to make it go away.

Luckily there are some C# tricks that we can employ to make our GUI tools look more professional.

```javascript
#--------------------------------------------------------------------------------
$showWindowAsync = Add-Type –memberDefinition @” 
[DllImport("user32.dll")] 
public static extern bool ShowWindowAsync(IntPtr hWnd, int nCmdShow); 
“@ -name “Win32ShowWindowAsync” -namespace Win32Functions –passThru

function Show-PowerShell() { 
     [void]$showWindowAsync::ShowWindowAsync((Get-Process –id $pid).MainWindowHandle, 10) 
}

function Hide-PowerShell() { 
    [void]$showWindowAsync::ShowWindowAsync((Get-Process –id $pid).MainWindowHandle, 2) 
}

#-------------------------------------------------------------------------------
Add-Type -Name Window -Namespace Console -MemberDefinition '
[DllImport("Kernel32.dll")]
public static extern IntPtr GetConsoleWindow();

[DllImport("user32.dll")]
public static extern bool ShowWindow(IntPtr hWnd, Int32 nCmdShow);
'

function ShowConsole
{
    $consolePtr = [Console.Window]::GetConsoleWindow()
    [Console.Window]::ShowWindow($consolePtr, 5) #5 show
}

function HideConsole
{
    $consolePtr = [Console.Window]::GetConsoleWindow()
    [Console.Window]::ShowWindow($consolePtr, 0) #0 hide
}

#-------------------------------------------------------------------------------
Add-Type -Name Window -Namespace Console -MemberDefinition '
    [DllImport("Kernel32.dll")]
    public static extern IntPtr GetConsoleWindow();
    [DllImport("user32.dll")]
    public static extern bool ShowWindow(IntPtr hWnd, Int32 nCmdShow);
'
function Hide-Console {
    $consolePtr = [Console.Window]::GetConsoleWindow()
    [void][Console.Window]::ShowWindow($consolePtr, 0)
}
hide-console
```
#-------------------------------------------------------------------------------
Reference:
https://stackoverflow.com/questions/40617800/opening-powershell-script-and-hide-command-prompt-but-not-the-gui
https://www.sapien.com/forums/viewtopic.php?t=6833
https://www.reddit.com/r/PowerShell/comments/4gt73m/run_powershell_script_without_a_console_window/
