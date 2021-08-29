---
description: Director AWS S3 configuration
---

# AWS S3 Configuration

`AWS_ACCESS_KEY_ID=null`

AWS Access Key



`AWS_SECRET_ACCESS_KEY=null`

AWS Secret  


`S3_BUCKET="sorry-cypress"`

AWS S3 Bucket name



`S3_REGION="us-east-1"`

AWS S3 Region



`S3_ACL="public-read"`

[AWS S3 ACL](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutObjectAcl.html) for `putObject` operation



`S3_READ_URL_PREFIX=null`

Custom prefix for generating "read" URL for generated artifacts. By default, the read `${S3_BUCKET}.s3.amazonaws.com/${objectKey}`, if `S3_READ_URL_PREFIX`is set, then it becomes `${S3_READ_URL_PREFIX}/${objectKey}`



`S3_IMAGE_KEY_PREFIX=null`

Custom prefix for stored images, if set the prefix will be applied e.g.: `${S3_BUCKET}.s3.amazonaws.com/${S3_IMAGE_KEY_PREFIX}${objectKey}`



`S3_VIDEO_KEY_PREFIX=null`

Custom prefix for stored videos, if set the prefix will be applied e.g.: `${S3_BUCKET}.s3.amazonaws.com/${S3_VIDEO_KEY_PREFIX}${objectKey}`

