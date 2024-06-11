+++
title = "K8s Access"
weight = 50
chapter = true
+++

# Kubernetes Access

GCP container clusters are managed through Google Kubernetes Engine (GKE) and consist of groups of virtual machines known as nodes, which work together to run containerized applications. As we know, k8s clusters provide a robust orchestration tool for deploying, managing, and scaling applications with high availability and resilience.

As a result, they can be an attractive target to attack, since they often host critical applications and sensitive data and if misconfigured, they can expose administrative interfaces or grant excessive permissions, providing an entry point for attackers.

Moreover, the dynamic and scalable nature of container environments can be exploited to launch attacks that spread rapidly across nodes, compromising multiple containers in one go. Hackers may also leverage vulnerabilities in container images or the underlying infrastructure to gain unauthorized access, escalate privileges, or execute malicious code.

Let's get access to the k8s cluster.

Follow the steps in this chapter
