---
description: Setting up AWS S3 Bucket for storing cypress recordings
---

# AWS S3 Manual Setup

{% hint style="info" %}
The following configuration is already included in [CloudFormation setup](./#cloud-formation)
{% endhint %}

* Create a new S3 bucket, enable public access \(uncheck `Block all public access`\)
* Set bucket's CORS configuration:

```text
<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
<CORSRule>
    <AllowedOrigin>*</AllowedOrigin>
    <AllowedMethod>POST</AllowedMethod>
    <AllowedMethod>GET</AllowedMethod>
    <AllowedMethod>PUT</AllowedMethod>
    <AllowedMethod>DELETE</AllowedMethod>
    <AllowedMethod>HEAD</AllowedMethod>
    <AllowedHeader>*</AllowedHeader>
</CORSRule>
</CORSConfiguration>
```

or for new AWS dashboard:

```text
[
  {
    "AllowedHeaders": ["*"],
    "AllowedMethods": ["POST", "GET", "PUT", "DELETE", "HEAD"],
    "AllowedOrigins": ["*"],
    "ExposeHeaders": []
  }
]
```

* Open IAM dashboard
* Create new user, enable programmatic access. Keep the access key and the secret.
* Create and attach the policy to the user:

  ```text
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Sid": "VisualEditor0",
              "Effect": "Allow",
              "Action": [
                  "s3:PutObject",
                  "s3:PutObjectAcl"
              ],
              "Resource": "arn:aws:s3:::<your-bucket-name>/*"
          }
      ]
  }
  ```

