---
description: Sorry-cypress installation instructions for Google Cloud Run
---

# Google Cloud

The suggested setup uses the following stack:

* [Google Cloud Run](https://cloud.google.com/run) services to run sorry-cypress Director, API and Dashboard
* [Google Cloud Storage](https://cloud.google.com/storage) with signed Read and Write URLs
* MongoDB setup of your choice

{% hint style="info" %}
Please make sure that you have

* recent version of [`gcloud`](https://cloud.google.com/sdk/docs/quickstart) installed
* Google Cloud project is configured and you have [sufficient permissions](https://cloud.google.com/sdk/docs/authorizing)
* recent version of Docker installed
{% endhint %}

### MongoDB Setup

Please use MongoDB provider of your choice. [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) is a simple and popular managed solution that also has a free tier.

Once you've created MongoDB cluster and a database, please obtain credentials and database name, you will need it in subsequent steps.

### Storage Setup

{% hint style="info" %}
You can use AWS S3 Storage instead, please refer to [AWS S3 setup instructions](../broken-reference/)
{% endhint %}

#### Create Bucket

1. Navigate to the [API Console Storage page](https://console.cloud.google.com/storage/browser)
2. Select a project or create a new project. Note the project ID.
3. Select **Create** and follow the steps to create the bucket. Note the bucket name.

{% hint style="info" %}
Consider setting up a [Lifecycle Rule](https://cloud.google.com/storage/docs/lifecycle) to keep only the most recent objects and keep costs down. Eg: _Delete object, 7+ days since object was created_
{% endhint %}

#### Authentication

Sorry-cypress Director authenticates on Google Cloud Storage using the Service Account assigned on the cloud run revision (usually Compute Engine default service account)&#x20;

Make sure that service account has the `Storage Object Creator` Role, or assign the role / create a new service account with the role instead.

#### Access Control

By default the **google-cloud-storage** driver uses Signed URLs for both Read & Write URLs. This means the bucket access can be set to `Uniform` - `Not Public`

However, Google Cloud Storage Signed URLs have a [max expiration time of 7 days](https://cloud.google.com/storage/docs/access-control/signed-urls). If you need to view the recording of your runs from sorry-cypress Dashboard for more than 7 days consider one of the following:

* Allow public read access to the bucket
* Use MinIO Gateway to access Google Cloud Storage instead. [Instructions](google-cloud.md)

### Deploying sorry-cypress Kit

Let's create 3 Cloud Run Services and deploy sorry-cypress components. We are going to run the following sequence of commands for each service:

1. Pull latest docker image from Dockerhub
2. Tag and push image to GCR associated with your project
3. Deploy Google Cloud Run service using the newly generated image

Running a simple script hosted on GitHub would deploy the services.

* `-p` is the current Google Cloud project
* `-n` is the name prefix for generated Google Cloud Run services

```bash
curl -sL https://git.io/Jt4cB  \
|  source /dev/stdin -p <project> -n <services-prefix>

## Example output:
# 🏁  Finished deployment to Google Cloud Run
#
# test001-director: https://test001-dashboard-dwpifb4gla-uc.a.run.app
# test001-api: https://test001-dashboard-dwpifb4gla-uc.a.run.app
# test001-dashboard: https://test001-dashboard-dwpifb4gla-uc.a.run.app
```

Note the URLs of the generated services, we'll use those in the next step to configure the services so they'll be able to communicate one with another.

### Configuring sorry-cypress Services

Run the commands below, please be careful while substituting template strings with values obtained at previous steps

```bash
# director configuration
gcloud run services update <services_prefix>-director \
--platform managed \
--set-env-vars DASHBOARD_URL="<dashboard_service_url>" \
--set-env-vars EXECUTION_DRIVER="../execution/mongo/driver" \
--set-env-vars MONGODB_URI="<mongodb_uri>" \
--set-env-vars MONGODB_DATABASE="<mongodb_dbname>" \
--set-env-vars SCREENSHOTS_DRIVER="../screenshots/google-cloud-storage.driver" \
--set-env-vars GCS_BUCKET="<bucket_name>" \
--set-env-vars GCS_PROJECT_ID="<project_id>" \
--set-env-vars GCS_IMAGE_KEY_PREFIX="screenshot/" \ # optional, default blank
--set-env-vars GCS_VIDEO_KEY_PREFIX="video/" \      # optional, default blank
--set-env-vars GCS_IS_BUCKET_PUBLIC_READ="false"    # optional, use 'true' only if the bucket has public read access

# api configuration
gcloud run services update <services_prefix>-api \
  --platform managed \
--set-env-vars MONGODB_URI="<mongodb_uri>" \
--set-env-vars MONGODB_DATABASE="<mongodb_dbname>" \
--set-env-vars APOLLO_PLAYGROUND="<apollo_playground>"

# dashboard configuration
gcloud run services update <services_prefix>-dashboard \
  --platform managed \
  --set-env-vars GRAPHQL_SCHEMA_URL="<api_service_url>"
```

🎉 Congratulations!

You've finished setting up sorry-cypress on Google Cloud - now you can open the Dashboard URL to see the dashboard.

Don't forget to [reconfigure cypress agents](../../integrating-cypress/configuring-cypress-agent.md) to use Director service before running test.
