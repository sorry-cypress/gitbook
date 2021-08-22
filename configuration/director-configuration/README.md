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

This can be useful when cypress is running on external CI servers and we need to expose director to the internet.

Empty or not provided variable means that all record keys are allowed.

### Persistence Configuration

`EXECUTION_DRIVER="../execution/in-memory"`

Set the execution driver for Director service. Possible values are:

* `../execution/in-memory` - Director will keep all the data in-memory. See [Basic Setup](../in-memory.md).
* `../execution/mongo/driver` - use MongoDB as a persistence. See [Full Setup](../persistent.md#director-service).



`MONGODB_URI="mongodb://mongo:27017"`

MongoDB connection URL, required if using "mongo" execution driver.



`MONGODB_DATABASE="sorry-cypress"`

MongoDB database name, required if using "mongo" execution driver.



`MONGODB_AUTH_MECHANISM`

MongoDB authentication mechanism. See [MongoDB Authentication](https://mongodb.github.io/node-mongodb-native/3.0/tutorials/connect/authenticating/).



`MONGODB_AUTH_USER`

MongoDB authentication user, when authentication mechanism is `DEFAULT`.



`MONGODB_AUTH_PASSWORD`

MongoDB authentication `password`, when authentication mechanism is `DEFAULT`.

### Remote Storage Configuration

`SCREENSHOTS_DRIVER="../screenshots/dummy.driver"`

Set the execution driver for Director service. Possible values are:

* `../screenshots/dummy.driver` - don't store anything, dummy driver
* `../screenshots/s3.driver` - use AWS S3. See [Full Setup](../persistent.md#director-service) for details.
* `../screenshots/minio.driver`- use Minio. See [Full Setup](../persistent.md#director-service) for details.

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

Read [Minio Configuration](minio-configuration.md) for Director



