---
title: Tetragon
weight: 61
---

## Basic Info

[Tetragon](https://tetragon.io/) has been deployed as a defending tool. It's a kubernetes-aware security observabilty and runtime enforcement tool which is eBPF-based. It enables defining and applying policies and filtering on the OS kernel level at runtime.

![Tetragon](https://tetragon.io/images/diagram-illustration.png)

## Tracing Policy

By default tetragon observes the process lifecycle (execution and termination), however this could be extended for more custom and advanced use cases using Tracing Policies.

Tetragon’s TracingPolicy is a user-configurable Kubernetes custom resource that allows users to trace arbitrary events in the kernel and optionally define actions to take on a match.

In order to defend on the prod environment, the security department at nevervictimconsult defined and enforced the following Tracing Policy, which sends a SIGKILL signal whenever the process "tcpdump" tries to create a file descriptor:

```bash
apiVersion: cilium.io/v1alpha1
kind: TracingPolicy
metadata:
  name: "tcpdump-blocking"
spec:
  kprobes:
  - call: "fd_install"
    syscall: false
    args:
    - index: 0
      type: "int"
    - index: 1
      type: "file"
    selectors:
    - matchBinaries:
      - operator: "In"
        values:
        - "/usr/bin/tcpdump"
      matchActions:
      - action: Sigkill
```
