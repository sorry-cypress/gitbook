---
description: Sorry-cypress installation instructions for Google Cloud Run
---

# Google Cloud

The suggested setup uses the following stack:

* [Google Cloud Run](https://cloud.google.com/run) services to run sorry-cypress Director, API and Dashboard
* [Minio Gateway](https://docs.min.io/docs/minio-gateway-for-s3.html) to Google Cloud Storage
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

### Minio Gateway

{% hint style="info" %}
You can use AWS S3 Storage instead, please refer to [AWS S3 setup instructions](broken-reference/)
{% endhint %}

MinIO GCS Gateway allows to access Google Cloud Storage (GCS) with AWS S3-compatible APIs.

#### Create Service Account

1. Navigate to the [API Console Credentials page](https://console.developers.google.com/project/\_/apis/credentials)
2. Select a project or create a new project. Note the project ID.
3. Select the **Create credentials** drop-down on the **Credentials** page, and click **Service account key**.
4. Select **New service account** from the **Service account** drop-down.
5. Populate the **Service account name** and **Service account ID**.
6. Click the drop-down under **Grant this service account access to the project,** the **Role** and choose **Storage** > **Storage Admin** _(Full control of GCS resources)_.
7. Click on the service account and select **Add Key > Create New Key** key
8. Download the **JSON** file and rename it as `credentials.json`

{% hint style="warning" %}
The service account is granted admin access to all GC storage objects. Please refer to Google Cloud and Minio documentation to limit access.
{% endhint %}

#### Deploy Minio Gateway

Grab the following Dockerfile and place it in the same directory as created earlier `credentials.json`

```bash
.
├── credentials.json
└── Dockerfile
```

{% code title="Dockerfile" %}
```bash
FROM minio/minio
COPY credentials.json ./
ENV GOOGLE_APPLICATION_CREDENTIALS=/credentials.json
ENV MINIO_ACCESS_KEY=<choose_access_key>
ENV MINIO_SECRET_KEY=<choose_secret_key>
CMD ["gateway", "gcs", "<project>"]
```
{% endcode %}

Replace `project` and choose secure `MINIO_ACCESS_KEY` and `MINIO_SECRET_KEY`, build the image and push to GCR.

```bash
docker build -t gcr.io/<project>/minio .
docker push gcr.io/<project>/minio
```

Create and deploy Cloud Run `minio`service.

```bash
gcloud run deploy minio \
--image gcr.io/<project>/minio \
--platform managed \
--allow-unauthenticated \
--port 9000

## Example output:
# Service [minio0demo] revision [minio2-00001-fab] has been deployed and is serving 100 percent of traffic.
# Service URL: https://minio-dwpifb4gla-uc.a.run.app
```

Upon successful deployment, note the `Service URL` of the deployed service. You'd be able to open browsers and access Minio dashboard with the credentials you've set in Dockerfile earlier.

#### Create a New Bucket

Run the next command to create a new bucket (`<bucket_name>`) and set policy using `mc` - minio client Docker image

```bash
docker run -it minio/mc \
  mc config host add gcs <minio_service_url> <minio_access_key> <minio_secret_key> && \
  mc mb gcs/<bucket_name> && \
  mc policy set download gcs/<bucket_name>

## Example output:
# Bucket created successfully `gcs/sorry-cypress-demo`.
# Access permission for `gcs/sorry-cypress-demo` is set to `download`
```

🎉 You have setup Minio Gateway that sorry-cypress can use to store the recordings of your runs.

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

{% hint style="info" %}
`MINIO_ENDPOINT` is the hostname of Minio URL you've obtained while setting up Minio Gateway. E.g.
{% endhint %}

```bash
# director configuration
gcloud run services update <services_prefix>-director \
--platform managed \
--set-env-vars DASHBOARD_URL="<dashboard_service_url>" \
--set-env-vars EXECUTION_DRIVER="../execution/mongo/driver" \
--set-env-vars MONGODB_URI="<mongodb_uri>" \
--set-env-vars MONGODB_DATABASE="<mongodb_dbname>" \
--set-env-vars MINIO_ACCESS_KEY="<minio_access_key>" \
--set-env-vars MINIO_SECRET_KEY="<minio_secret_key>" \
--set-env-vars MINIO_ENDPOINT="example-minio-dwpifb4gla-uc.a.run.app" \
--set-env-vars MINIO_URL="https://exampleminio-dwpifb4gla-uc.a.run.app" \
--set-env-vars MINIO_BUCKET="<minio_bucket_name>"

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

Don't forget to [reconfigure cypress agents](../integrating-cypress/configuring-cypress-agent.md) to use Director service before running test.
