---
title: Unix Basics
author: Anugrah SR
date: 2020-09-17 02:00:00 +0800
categories: [Bugbounty, 30daysofPentesterlab]
tags: [Bugbounty, unix]
math: true
---
# Day 1
## Basic unix commands

|   Command      |     Description      |
| ------------- |:-------------:|
|   `ls`        |Lists all files and directories |
|`ls -a`   | Lists hidden files as well  |
|`cd`   |  To change to a particular directory |
| `cd ..`|Move one level up   |
|`cat filename`   | Displays the file content  |


Learn basic bash scripting from
*  https://linuxconfig.org/bash-scripting-tutorial-for-beginners
* https://www.tutorialspoint.com/unix/shell_scripting.htm
* https://devhints.io/bash
* https://overthewire.org/wargames/bandit/

**find**

Find `.bash_history` in `/home` directory
```bash
find /home -name .bash_history
```
Finding with grep
```bash
find /home -name .bashrc -exec grep [PATTERN] {} \;
```
**grep**
```bash
find . -name .bash_history -exec grep -A 1 '^xyzfindme' {} \;
```
`^` for starting

**/tmp**

tmp folder may contain secrets

```bash
ls -ld /tmp
drwxrwxrwt 1 root root 4096 Sep 15 13:10 /tmp
```
* d : directory
* rwx: read, write, executable
* t: sticky bit

```bash
#unzip
gzip -d xyz.tgz
#uncompress tar
tar -xf xyz.tar
```
```bash
#Decompress bzip tar file
#j if bzip
tar xvjf xyz.tbz
```
```bash
#z if gz
tar xvzf file.tar.gz
```
```bash
#uncompress bz2
bzip2 -d your-filename-here.bz2
```
`file xyz` can tell what file it is <br>
`strings` extract strings from a file

**Cronjob** <br>
you can find the cron jobs run daily in `/etc/cron.daily/`

```bash
#!/bin/bash
tar -zcvf /tmp/backup.tgz /home/victim
openssl enc -aes256 -k PASS -in /tmp/backup.tgz  -out /tmp/backup.tgz.enc
rm /tmp/backup.tgz
```
Reverse the process Using
```bash
openssl aes-256-cbc -d -in backup.tgz.enc -out backup.tgz
enter aes-256-cbc decryption password:
#PASS
tar -xvzf backup.tgz
```
```bash
#gunzip
cp backup test.gz
gunzip test.gz
tar -xvf test.gz
```
