---
description: Sorry-cypress full setup with persistency and web dashboard
---

# Full Setup

The full sorry-cypress setup allows to use all the supported featured but comes with an overhead of maintaining the infrastructure required to run the services:

* sorry-cypress-director in "persisting" mode
  * MongoDB
  * Test Recordings storage
* sorry-cypress-api
* sorry-cypress-dashboard

### Director Service

Director service is responsible for

* parallelization and coordination of test runs
* integration with Slack, GitHub, MS Teams and emitting generic WebHooks
* saving tests results
* generating signed upload URL for saving failed tests screenshots

When you launch Cypress agents on a CI environment with multiple machines, each agent contacts the director service and gets instructions on the next spec file to run.

After running the test spec, the agent reports the results to the Director service, receives the instructions for the next run, and so on until all the tests are done.

The director service coordinates those activities for multiple agents and different runs, stores the test results in a database.

Full setup requires the director to run in "persisting" mode to use MongoDB driver and provide credentials.,

```
EXECUTION_DRIVER="../execution/mongo/driver"
MONGODB_URI="monodgb://your-DB-URI"
MONGODB_DATABASE="your-DB-name"
```

Also, see [Director Configuration](director-configuration/) options.

### API Service

API Service is a simple GraphQL wrapper that exposes a convenient way to query the data stored by Director.

The service is only required as an interface for the Web Dashboard, but can be used to query the database and describe the internal data models.

Also, see [API Configuration](api-configuration.md) options.

### Web Dashboard Service

The dashboard allows end-users to interact with sorry-cypress using a browser and to:

* track test runs progress
* browser test results, videos, and failures screenshots
* set projects configuration like WebHooks, Slack, MS Teams and GitHub integration
* create and delete entries (projects, runs)

Also, see [Dashboard Configuration](dashboard-configuration/) options.

### Recordings Storage

Cypress comes with the ability to take [screenshots and videos](https://docs.cypress.io/guides/guides/screenshots-and-videos.html#Screenshots), whether you are running via `cypress open` or `cypress run`, even in CI.

We need remote storage to store the generated screenshots and videos. Director service will send a signed upload URL that cypress agent will use to upload the generated artifacts.

> A signed URL is a URL that provides limited permission and time to make a request. Signed URLs contain authentication information in their query string, allowing users without credentials to perform specific actions on a resource.

Sorry-cypress integrates with the major remote cloud storage solutions:

* AWS S3
* Minio integration (via [Minio S3 Gateway](https://docs.min.io/docs/minio-gateway-for-s3.html)) that is compatible with
  * Google Cloud Storage
  * IBM COS
  * NAS
  * HDFS
* Azure Blob Storage

To disable remote storage, we need to set a "dummy" screenshots driver for Director service.

```
SCREENSHOTS_DRIVER="../screenshots/dummy.driver"
```

To enable remote storage, see [Director Configuration](director-configuration/) options.

{% hint style="info" %}
Refer to specific cloud platform instructions for remote cloud storage configuration guidelines.
{% endhint %}

### MongoDB

Director and API services work with MongoDB as a persistency layer. It's up to you to choose MongoDB solution that works for your needs.

[MongoDB Atlas](https://www.mongodb.com/cloud/atlas) is a simple and popular managed solution that also has a free tier.

[AWS DocumentDB](https://docs.aws.amazon.com/documentdb/latest/developerguide/what-is.html) is a managed NoSQL DB in AWS which is [partially compatible](https://docs.aws.amazon.com/documentdb/latest/developerguide/compatibility.html) to the MongoDB API. It has no free tier option, but can be still be a suitable option. Sorry-cypress added compatibility in [v1.0.0-rc.8](https://github.com/sorry-cypress/sorry-cypress/releases/tag/v1.0.0-rc.8)
