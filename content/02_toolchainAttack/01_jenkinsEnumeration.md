---
title: Jenkins Enumeration
weight: 21
---

Many projects and companies utilize Jenkins for years.
There are very well maintained instances - but for a guess you'd expect most Jenkins' as default configuration setups.
That's what we'll try to cover in this chapter.

# Ideation

"I want access to that Jenkins".
Sounds like it's that easy.
But actually, we need to plan this.

Let's start with imagining the typical way of interacting with Jenkins:
Users (most often developers) open Jenkins WebUI in their favourite Webbrowser.
Once open, they login with a username and password combination.
That would be best, wouldn't it? Having a valid combination.
Unfortunately we don't have it.

# Scan Jenkins Instance

Let's start by getting some details about jenkins. In order to do that, let's use [metasploit](https://www.metasploit.com/):

{{% expand "Solution" %}}

```bash
msfconsole
```

{{% /expand %}}

Now we can use an enumeration module for jenkins provided by metasploit as a scanner for http-based applications:

{{% expand "Solution" %}}

```bash
use auxiliary/scanner/http/jenkins_enum
```

{{% /expand %}}

Now we need to configure that module, setting the RHOST, RPORT and TARGETURI and then try to exploit:

{{% expand "Solution" %}}

```bash
set RHOSTS jenkins.test.nevervictimconsult.xyz
set RPORT 80
set TARGETURI /
```

```bash
exploit
```

{{% /expand %}}

What do you see?

{{% expand "Solution" %}}
We detect that theres a pretty modern version of Jenkins running.

```bash
[+] 34.159.77.191:80    - Jenkins Version 2.387.3
[*] /script restricted (403)
[*] /view/All/newJob restricted (403)
[*] /asynchPeople/ restricted (403)
[*] /systemInfo restricted (403)
```

{{% /expand %}}

# Look for exploits

Well that makes it more difficult for us to get access. However, we can make use of metasploit to search for exploits for that version:
{{% expand "Solution" %}}

```bash
searchsploit Jenkins 2.3
```

{{% /expand %}}

Unfortunately there are no easy-to-use exploits available for this version, but we can still use other modules! Let's use the login module.
