---
title: Cloud Service Provider Access
weight: 41
---

Now what would you do when you have just found a secret?
Of course that looks promising to get access to a service provider!

Let's use the revealed secret in order to login to GCP!

Use gcloud to authenticate via json File:

{{% expand "Solution" %}}

```bash
cd /root/demotalk/cloudAccess
gcloud auth activate-service-account --key-file=gcpSa.json
gcloud init --console-only
```

{{% /expand %}}

It will ask for some info. For the configuration you can reuse the first session, for the account you may use the jenkins account and for the project name you can use "thorsten-jakoby-tj-projekt".

Now that you are in, you have the opportunity to have a look around!

Let's see whether they have some container clusters:
{{% expand "Solution" %}}

```bash
gcloud container clusters list
```

{{% /expand %}}

Maybe also some storage buckets

{{% expand "Solution" %}}

```bash
gcloud storage buckets list
```

{{% /expand %}}

So what did you find that might be interesting for a hacker?!

{{% expand "Solution" %}}

Some interesting things are: conduit-database-backup-test and conduit-k8s-prod!

{{% /expand %}}
