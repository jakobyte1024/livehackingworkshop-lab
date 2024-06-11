---
title: Attacking Again & Defending
weight: 62
---

Now let's try to play both roles of attacker and defender on prod environment!

## Getting Jenkins Login Attempts

As we mentioned previously, ingress-nginx has been deployed in front of jenkins to log the traffic on the prod environment.

Now play the role of the defender and after accessing the K8s cluster on your terminal, switch to the prod cluster:
{{% expand "Solution" %}}

```bash
gcloud container clusters get-credentials --region=europe-west3 conduit-k8s-prod
```

{{% /expand %}}

Now watch the logs of the ingress pod on your terminal, which is deployed on the prod K8s cluster:

{{% expand "Solution" %}}

```bash
kubectl get pod -n ingress-nginx
```

```bash
kubectl logs <Ingress-Controller-Pod-Name> -f -n ingress-nginx
```

{{% /expand %}}

At the same time as you are getting the logs of ingress as a defender, on another terminal play the role of the attacker and brute force jenkins following the same steps you followed in the chapter [02_jenkinsAccess]({{< ref "/02_toolchainAttack/02_jenkinsAccess" >}}) but on the prod environment this time.

Now as a defender you will get all the login attempts this time in your ingress logs.

Well in real-world scenarios this should ideally be connected to a SIEM system, where you gather the logs and events from your resources and get notified about such security threats.

## Preventing Receiving K8s Pod TCP Traffic Passively

Now as an attacker, switch to the prod environment after accessing the K8s Cluster either using octant or the command line:

{{% expand "Solution" %}}

```bash
octant
```

or

```bash
gcloud container clusters get-credentials --region=europe-west3 conduit-k8s-prod
```

{{% /expand %}}

You will try to passively receive all TCP traffic sent to the conduit-backend container in order to analyse it and make use of it later on.
In order to do that, you need to inject a sidecar container in the app pod, which captures all TCP traffic passing through the pod's network interface and saves the received network traffic into a file.
The dockerfile looks like this:

```bash
#!/bin/sh
# Dockerfile for the sidecar container
FROM alpine
# Install necessary dependencies or tools
RUN apk --no-cache add tcpdump
# Set the command to run in the container
CMD ["tcpdump", "-i", "eth0", "-w", "/captured_traffic.pcap"]
```

In order to save sometime, a docker image using that file has already been built and pushed to the following path in dockerhub:

```bash
mirna/sidecar:0.9
```

All you need now as an attacker is to edit the app deployment either on browser directly opened after using octant or using the command line:

{{% expand "Solution" %}}

```bash
kubectl edit deployment/conduit-backend -n conduit-app
```

{{% /expand %}}

Then inject the sidecar container, directly below containers on column 0:
{{% expand "Solution" %}}

```bash
      - image: mirna/sidecar:0.9
        name: sidecar
```

{{% /expand %}}

The new container will not be deployed properly, since a SigKill will be sent to end that process from the [tetragon policy]({{< ref "/05_defend/01_tetragon/#tracing-policy" >}}) that the security departement defined before.

Now as a defender check the status of the app pods. What do you notice?
{{% expand "Solution" %}}

```bash
kubectl get pod -n conduit-app
```

An error status will be noticed for the pods as in the following example:

```bash
conduit-backend-fdc76f894-596d6            1/2     Error              4 (51s ago)   98s
```

{{% /expand %}}

For more info about those trials to inject the sidecar and the SigKill action, you can check the tetragon logs. What do you see?:
{{% expand "Solution" %}}

```bash
kubectl logs -n kube-system -l app.kubernetes.io/name=tetragon -c export-stdout -f | tetra getevents -o compact
```

You will see something like this in the logs:
![tetragon logs](/images/tetragon_logs.png)

{{% /expand %}}
