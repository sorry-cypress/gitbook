---
description: Director service configuration options
---

# Director Service

### Common Configuration

`PORT=1234`

Director will listen on that port



`DASHBOARD_URL="http://localhost:8080"`

"Run URL" shown by Cypress agent when running tests



`ALLOWED_KEYS=null`

List of comma delimited record keys \(provided to the Cypress Runner using --key option\) which are accepted by the director service.

This can be useful when Cypress is running on external CI servers and we need to expose director to the internet.

Empty or not provided variable means that all record keys are allowed.

### Persistence Configuration

`EXECUTION_DRIVER="../execution/in-memory"`

Set the execution driver for Director service. Possible values are:

* `../execution/in-memory` - Director will keep all the data in-memory. See [Basic Setup](in-memory.md).
* `../execution/mongo/driver` - use MongoDB as a persistence. See [Full Setup](persistent.md#director-service).



`MONGODB_URI="mongodb://mongo:27017"`

MongoDB connection URL, only used by "mongo" execution driver.



`MONGODB_DATABASE="sorry-cypress"`

MongoDB database name, only used by "mongo" execution driver.

### Remote Storage Configuration

`SCREENSHOTS_DRIVER="../screenshots/dummy.driver"`

Set the execution driver for Director service. Possible values are:

* `../screenshots/dummy.driver` - don't store anything, dummy driver
* `../screenshots/s3.driver` - use AWS S3. See [Full Setup](persistent.md#director-service) for details.
* `../screenshots/minio.driver`- use Minio. See [Full Setup](persistent.md#director-service) for details.

#### AWS S3 Remote Storage Configuration

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



#### Minio Configuration

{% hint style="danger" %}
Treat your Minio keys and secrets AWS credentials and hide them. 
{% endhint %}

{% hint style="info" %}
Refer to[`docker-compose.minio.yml`](https://github.com/sorry-cypress/sorry-cypress/blob/master/docker-compose.minio.yml)for Minio setup example.
{% endhint %}

`MINIO_ACCESS_KEY="defaultAccessKey"`

Minio Access Key



`MINIO_SECRET_KEY="defaultSecret"`

Minio Secret



`MINIO_BUCKET="sorry-cypress"`

Bucket name for storing generated artifacts. Please make sure that the bucket is created and configured properly before using it.



`MINIO_URL="https://storage.yourdomain.com"`

The public URL used for public read access to the stored screenshots and videos. This URL should be available from your browser and it will be used to fetch generated screenshots and videos.



`MINIO_PORT=9000`

Port that `director` and cypress agents will use to communicate with Minio.



`MINIO_ENDPOINT="storage.yourdomain.com"`

Hostname or IP address that **both `director` and cypress agents** will use to communicate with `minio` service.

* Please make sure that your network configuration allows access to Minio resource for cypress agents and for Director service
* To run on the local machine, edit your `/etc/hosts` file to allow cypress agents discover the local instance of Minio `127.0.0.1 localhost`



`MINIO_READ_URL_PREFIX=null`

You can override the whole read URL, including the bucket name using this variable. Most chances you won't need it, if you do, see the [source code](https://github.com/sorry-cypress/sorry-cypress/blob/master/packages/director/src/screenshots/minio/minio.ts#L42).



`MINIO_USESSL="false"`

Whether `director` should use SSL for communicating with `minio`.

