---
description: Sorry-cypress installation instructions for Heroku
---

# Heroku

### Basic sorry-cypress Setup <a id="running-a-stateless-director-service"></a>

Click the button below to deploy the basic, in-memory, standalone `director` service to Heroku.

[![](../.gitbook/assets/button.svg)](https://heroku.com/deploy?template=https://github.com/agoldis/sorry-cypress/tree/master)

### Full sorry-cypress kit on Heroku

{% hint style="info" %}
* Download and install the [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli)
* You must have [Docker](https://docs.docker.com/get-docker/) set up locally to continue
{% endhint %}

We'll create 3 separate Heroku applications - one for each service. Publicly available docker images of 3 services are available at:

* [https://hub.docker.com/repository/docker/agoldis/sorry-cypress-director](https://hub.docker.com/repository/docker/agoldis/sorry-cypress-director)
* [https://hub.docker.com/repository/docker/agoldis/sorry-cypress-api](https://hub.docker.com/repository/docker/agoldis/sorry-cypress-api)
* [https://hub.docker.com/repository/docker/agoldis/sorry-cypress-dashboard](https://hub.docker.com/repository/docker/agoldis/sorry-cypress-dashboard)

The images are automatically updated on each release and tagged in accordance with GitHub releases.

sorry-cypress uses MongoDB as a persistence layer for storing and retrieving test results. We'll use a free  hosted solution to run a managed instance of [Atlas](https://www.mongodb.com/cloud/atlas) MongoDB.

#### Creating Heroku Application

Create 3 new Heroku application and give them appropriate names

```bash
heroku create <prefix>-director
heroku create <prefix>-api
heroku create <prefix>-dashboard
```

Run the commands to deploy  `director` , `API` and `Dashboard` services

```bash
# Sign into Heroku Container Registry.
heroku container:login

# Pull services image
docker pull agoldis/sorry-cypress-director:latest
docker pull agoldis/sorry-cypress-api:latest
docker pull agoldis/sorry-cypress-dashboard:latest

# Tag service images as Heroku app image
docker tag agoldis/sorry-cypress-director:latest registry.heroku.com/<name_of_director_app>/web
docker tag agoldis/sorry-cypress-api:latest registry.heroku.com/<name_of_api_app>/web
docker tag agoldis/sorry-cypress-dashboard:latest registry.heroku.com/<name_of_dashboard_app>/web

# Push the images to Heroku Container Registry
docker push registry.heroku.com/<name_of_director_app>/web
docker push registry.heroku.com/<name_of_api_app>/web
docker push registry.heroku.com/<name_of_dashboard_app>/web

# Deploy the image
heroku container:release --app <name_of_director_app> web
heroku container:release --app <name_of_api_app> web
heroku container:release --app <name_of_dashboard_app> web
```

#### Setup MongoDB

Choose the MongoDB provider of your choice and obtain connection details. You will need to set the credentials for newly deployed services.

Heroku has a plenty of add-ons that allows attaching a MongoDB cluster. The recommended way is to attach a MongoDB add-on to `director` application and use the same credentials for `API` service. 

All you'll need is the database name and the access credentials so you can fill the Heroku config variables as we'll see right after. So go ahead to the [MongoDB Atlas docs](https://docs.atlas.mongodb.com/getting-started/), get your database running and grab that data!

Because the creation of this cluster is very straightforward and well-written in the docs, we'll not cover that here.

#### Setup Recordings Storage

Please refer to [Storage Configuration](../configuration/director-configuration.md#remote-storage-configuration) instructions to configure Recordings Storage \(failed tests screenshots and videos\) and obtains credentials.

#### Setup `director` Service

```bash
# Use stateful mode and keep test results in MongoDB
EXECUTION_DRIVER="../execution/mongo/driver"

# Dashboard app url
DASHBOARD_URL=<dashboard_app_url>

# MongoDB database name
MONGODB_DATABASE=<atlas_database_name>

# MongoDB connection string
MONGODB_URI=<atlas_database_access_credentials>

# If you've set up S3 bucket for keeping screenshots
# Screenshots driver path
SCREENSHOTS_DRIVER="../screenshots/s3.driver"
# If you've set up minio for keeping screenshots
#SCREENSHOTS_DRIVER="../screenshots/minio"

# S3 Bucket name
S3_BUCKET="bucket_name"

# AWS region, default value is "us-east-1"
S3_REGION="us-east-1"

# AWS / MIIO credentials with write access to AWS S3 bucket
AWS_ACCESS_KEY_ID="key_id"
AWS_SECRET_ACCESS_KEY="secret_access"
```

#### Setup `API` Service

```bash
# MongoDB database name
MONGODB_DATABASE=<atlas_database_name>

# MongoDB connection string
MONGODB_URI=<atlas_database_access_credentials>
```

#### Setup `Dashboard` Service

```bash
# For communicating with API
GRAPHQL_SCHEMA_URL=<api_app_url>
```

[Reconfigure cypress agents](../cypress-agent/configuring-cypress-agent.md) and try running some tests. You will see test results appear in the newly installed dashboard.

