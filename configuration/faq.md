---
description: Common questions and setup issues
---

# FAQ

This is a collection of most common questions associated with Sorry Cypress setup

### Can I use a private AWS  S3 bucket with sorry-cypress?

This is not trivial. If you find a proper configuration, please contribute to this documentation for others to follow. Please refer to the following scheme for reference.

1. **Upload flow:** Cypress runner reports its results to director service
2. Director service get [signed S3 upload URL](https://docs.aws.amazon.com/AmazonS3/latest/userguide/PresignedUrlUploadObject.html) from the configured AWS S3 bucket (or any other object storage compatible service - e.g. [minio](director-configuration/minio-configuration.md))
3. Director service sends back the signed S3 upload URL, stores the read URL in a DB
4. Cypress runner uses the signed upload URL to upload the screenshots / videos
5. **Read flow:** a browser reads the test results and uses the read URL from a DB
6. AWS S3 returns the content to the browser

![Cypress AWS S3 upload / read flow](../.gitbook/assets/sorry-cypress-s3.png)

