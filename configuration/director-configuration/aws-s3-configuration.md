---
description: Director AWS S3 configuration
---

# AWS S3 Configuration

`AWS_ACCESS_KEY_ID=null`

AWS Access Key



`AWS_SECRET_ACCESS_KEY=null`

AWS Secret



`S3_URL="https://s3.amazonaws.com/"`

AWS S3 URL



`S3_BUCKET="sorry-cypress"`

AWS S3 Bucket name



`S3_REGION="us-east-1"`

AWS S3 Region



`S3_IMAGE_KEY_PREFIX=null`

Custom prefix for stored images, if set the prefix will be applied e.g.: `${S3_URL}/${S3_BUCKET}/${S3_IMAGE_KEY_PREFIX}${objectKey}`



`S3_VIDEO_KEY_PREFIX=null`

Custom prefix for stored videos, if set the prefix will be applied e.g.: `${S3_URL}/${S3_BUCKET}/${S3_VIDEO_KEY_PREFIX}${objectKey}`



```typescript
UPLOAD_EXPIRY_SECONDS=90
```

The expiration time for signed upload URLs to be valid. The director service generates the signed URLs that clients use for uploading the artifacts.
