---
description: Sorry-cypress installation instructions for Google Cloud Run with MinIO
---

# Google Cloud & MinIO - Deprecated

### Minio Gateway

MinIO GCS Gateway allows to access Google Cloud Storage (GCS) with AWS S3-compatible APIs.

{% hint style="warning" %}
MinIO Gateway is Deprecated [since February 2022](https://blog.min.io/deprecation-of-the-minio-gateway/?ref=docs-redirect)
{% endhint %}

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

### Continue the setup steps for deploying sorry-cypress

Continue with the [setup here ](./#deploying-sorry-cypress-kit)and use these environment variables instead:

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
