---
title: Kubernetes Look Around
weight: 51
---

Now that we have access to GCP and seen that there are k8s clusters, let's analyse what is running on test and prod clusters.

In order to do that, we need to connect to K8s and then start some shells:

{{% expand "Solution" %}}

```bash
gcloud container clusters get-credentials --region=europe-west3 conduit-k8s-prod
gcloud container clusters get-credentials --region=europe-west3 conduit-k8s-test
```

```bash
octant
```

{{% /expand %}}

Now you are in! You can have a look around and discover what they have on there clusters.
