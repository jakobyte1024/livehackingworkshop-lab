---
title: The Setup
weight: 6
---

This workshop comes with a fully automated provisioned infra- and app-structure on Google Cloud Platform.
You'll find a mix of

* VM based workloads
* PaaS usage
* IaaS usage
* Container based workloads
* GCP's IAM system integratoins

## Participant VM

Access to all of that will not happen directly from your notebook.
So you do not need to install any software.

## VNC Usage

We configured a participant VM for you, which is accessible via Webviewer.
You'll get an IP for your VM.
All you need to do then is opening the Webviewer.
It does not come with a valid signed certificate, so you need to accept the risk.

Example `https://<IP>/vnc.html`

After hitting connect, you'll be asked for a password.
`ichdarf0815` it is.

Pro tip:
make sure that you enable *local scaling* in the Webviewer's preferences.

Your successfully connected VM may look like this:
![vmConnect](/images/vncView.png)

## SSH Usage

As of now, there is no usage via SSH configured.

## Kali Linux

Your VM is based on Kali, which comes with many penetration testing tools.
The base image is public available here:

* [Kali Linux](https://www.kali.org/)
* [Tech latest Kali VM](https://console.cloud.google.com/marketplace/product/techlatest-public/kali-linux-browser)

## Solutions

Many steps will come with a *Solution* expand field.
You should decide on your own if you want to find out the solution yourself or see our attempts.
