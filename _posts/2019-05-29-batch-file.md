---
layout: post
title: PowerShell - Launch script with batch file
categories: [PowerShell]
---
An easy and elegant way to launch your PowerShell GUI tool is to use batch files. This will let you bypass PowerShell's execution policy
and save a few clicks.

Users will only need to double click on the batch file to run your tool.

Follow the steps below to create your batch file script launcher

1. Fire up your favorite text editor (Notepad for me) and type in the following line 
(substituting the path to your script as needed)
```
PowerShell.exe -sta -noprofile -ExecutionPolicy Bypass -File c:\temp\scripts\tools\your_script.ps1
```
![batchfile]({{ site.baseurl }}/images/launch_ps_batch_file.PNG)

2. Goto **File** --> **Save As** and enclose the batch file name in quotations with a **.bat** extension

3. Change Save as type to All Files (*) and click **Save**.
![batchfile save as]({{ site.baseurl }}/images/batchfile_save_as.PNG)

4. Double click on the saved batch file and bask in the the glory of your tool as it instantly launch like a native
windows application (almost, YMMV :))
