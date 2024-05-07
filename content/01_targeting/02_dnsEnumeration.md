---
title: DNS Enumeration
weight: 12
---

We'll start enumeration (collect information) with Nevervictimconsult's domain.
To do so, we'll do a DNS Enumeration

## Basic Enumeration

Kali Linux comes with a palette of enumeration tooling.
A very basic one is `dnsenum` which expects the domain to scan as parameter.
Use it :)

{{% expand "Solution" %}}

Using `dnsenum`:

```bash
dnsenum nevervictimconsult.xyz
```
shows
```
Name Servers:
______________

ns-cloud-a3.googledomains.com.           4283     IN    A        216.239.36.106

Trying Zone Transfer for nevervictimconsult.xyz on ns-cloud-a3.googledomains.com ... 
AXFR record query failed: REFUSED
```

{{% /expand %}}

There's already a plenty of information in these results:
Nevervictimconsult is using Google Cloud Platform - at least as a managed DNS for their domain

Thats nice to know.
But there's more in this.
We now know, that we need to put more effort in our enumeration attempts.
Because GCP's DNS is a good for their users: it prevents them for Zone-Transfer-Enumeration.
If we'd have a successful Zone-Transfer, we now would know mostly all subdomain configurations.
Read more about Zone Transfer here: [Beagle Security Blog](https://beaglesecurity.com/blog/vulnerability/dns-zone-transfer.html) 

## Advanced Enumeration

As we really wanna know more about their domain, we need to use an alternative enumeration tool.
Simple but useful: brute forcing DNS.

We need two things here:
* a tool that can ask DNS "is there a subdomain named xyz.nevervictimconsult?" in a automated way
* a good list of interesting subdomains.

For the first one, `dnsrecon` comes with many configuration parameters and will surely do the job.

For the second one, we'll use a wordlist that is public available on Github: [subdomains-top1million-5000](https://github.com/danielmiessler/SecLists/blob/master/Discovery/DNS/subdomains-top1million-5000.txt).
This wordlist is already in your Kali-VM present at `/usr/share/wordlists/subdomains-top1million-5000.txt`

Use `dnsrecon` to scan the domain
* in bruteforce mode
* with 25 threads in parallel to speed a bit up
* subdomain wordlist as input

{{% expand "Solution" %}}
```bash
dnsrecon -t brt -d nevervictimconsult.xyz --threads 25 -D /usr/share/wordlists/subdomains-top1million-5000.txt
```
{{% /expand %}}

You'll recognize that there's many valueable information in the output:
* They're using Office 365
* They're using domain verification tokens
* and there seems to be a staging

Let's dive deeper and do the same enumeration on the test-subdomain.

{{% expand "Solution" %}}
```bash
dnsrecon -t brt -d test.nevervictimconsult.xyz --threads 25 -D /usr/share/wordlists/subdomains-top1million-5000.txt
```
{{% /expand %}}

You now can recognize, that there's much more tooling thats familiar for developers.
One catchs our attention: Jenkins:

```bash
[+]      A jenkins.test.nevervictimconsult.xyz 34.159.77.191
[+] 15 Records Found
```

We definetly should learn more about that Jenkins in a next steps.
