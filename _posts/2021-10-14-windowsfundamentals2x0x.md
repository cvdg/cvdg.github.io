---
layout: post
title: "TryHackMe - Windows Fundamentals 2"
date: 2021-10-14T13:00:00+02:00
categories: ["TryHackMe"]
tags: ["thm"]
---

| Key   | Value
| ----: | :--------
| Room  | [windowsfundamentals2x0x](https://tryhackme.com/room/windowsfundamentals2x0x)
| Date  | 2021-10-14
| User  | [wastebasket](https://tryhackme.com/p/wastebasket)

## Task 1: Introduction

Read above and start the virtual machine. 

## Task 2: System Configuration 

What is the name of the service that lists Systems Internals as the manufacturer?

`psshutdown`

Whom is the Windows license registered to?

`Windows user`

What is the command for Windows Troubleshooting?

`C:\Windows\System32\control.exe /name Microsoft.Troubleshooting`

What command will open the Control Panel? (The answer is  the name of .exe, not the full path)

`control.exe`

## Task 3: Change UAC Settings 

What is the command to open User Account Control Settings? (The answer is the name of the .exe file, not the full path)

`UserAccountControlSettings.exe`

## Task 4: Computer Management 

What is the command to open Computer Management? (The answer is the name of the .msc file, not the full path)

`compmgmt.msc`

At what time every day is the GoogleUpdateTaskMachineUA task configured to run?

`6:15 AM`

What is the name of the hidden share?

`sh4r3dF0Ld3r`

## Task 5: System Information 

What is the command to open System Information? (The answer is the name of the .exe file, not the full path)

`msinfo32.exe`

What is listed under System Name?

`THM-WINFUN2`

Under Environment Variables, what is the value for ComSpec?

`%SystemRoot%\system32\cmd.exe`

## Task 6: Resource Monitor 

What is the command to open Resource Monitor? (The answer is the name of the .exe file, not the full path) 

`resmon.exe`

## Task 7: Command Prompt 

In System Configuration, what is the full command for Internet Protocol Configuration?

`C:\Windows\System32\cmd.exe /k %windir%\system32\ipconfig.exe`

For the ipconfig command, how do you show detailed information?

`ipconfig /all`

## Task 8: Registry Editor  

What is the command to open the Registry Editor? (The answer is the name of  the .exe file, not the full path)

`regedt32.exe`

## Task 9: Conclusion 
