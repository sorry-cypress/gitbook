---
description: Director service configuration options
---

# Director Service

## Common Configuration

`PORT=1234`

Director will listen on that port

`DASHBOARD_URL="http://localhost:8080"`

"Run URL" shown by Cypress agent when running tests

`INACTIVITY_TIMEOUT_SECONDS=180`

Director uses the timeout value to define how long we should wait before checking for a run’s inactivity.

`ALLOWED_KEYS=null`

List of comma delimited record keys \(provided to the Cypress Runner using --key option\) which are accepted by the director service.

This can be useful when cypress is running on external CI servers and we need to expose director to the internet.

Empty or not provided variable means that all record keys are allowed.

## Persistence Configuration

`EXECUTION_DRIVER="../execution/in-memory"`

Set the execution driver for Director service. Possible values are:

* `../execution/in-memory` - Director will keep all the data in-memory. See [Basic Setup](../in-memory.md).
* `../execution/mongo/driver` - use MongoDB as a persistence. See [Full Setup](../persistent.md#director-service).

## MongoDB Configuration

Used when mogo persistence configuration is selected. Refer to [MongoDB Configuration](../mongodb-configuration.md).

## Remote Storage Configuration

`SCREENSHOTS_DRIVER="../screenshots/dummy.driver"`

Set the execution driver for Director service. Possible values are:

* `../screenshots/dummy.driver` - don't store anything, dummy driver
* `../screenshots/s3.driver` - use AWS S3. See [Full Setup](../persistent.md#director-service) for details.
* `../screenshots/minio.driver`- use Minio. See [Full Setup](../persistent.md#director-service) for details.

### AWS S3 Remote Storage Configuration

Refer to [AWS S3 Configuration.](aws-s3-configuration.md)

### Minio Configuration

Read [Minio Configuration](minio-configuration.md) for Director

## Proxy Settings for Webhooks

If your Sorry-Cypress instance is only allowed to communicate outpound using a Proxy, the following values needs to be set (all values need to be provided!):

`PROXY_URL`
Sets the proxy DNS name to be used, for example: `my-proxy.com`

`PROXY_PORT`
Sets the port to be used, for example: `8080`

`PROXY_USERNAME`
Sets the proxy username to be used, for example: `my-proxy-username`

`PROXY_PASSWORD`
Sets the corresponding proxy password to be used, for example: `my-secret-proxy-password`
