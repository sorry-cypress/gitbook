---
description: Azure Blob Storage and Sorry Cypress
---

# Azure Blob Storage Configuration

[Azure Blob Storage](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction) is the object storage solution provided by Microsoft, like S3 is provided by AWS.

Sorry Cypress can use an Azure Blob Storage container in order to stores the screenshots and videos taken during tests.

In order to use minio as storage driver provider, you need to configure director service

```
SCREENSHOTS_DRIVER="../screenshots/azure-blob-storage.driver"
```

You also need to patch the cypress runner code using [cy2-azure](https://github.com/sorry-cypress/cy2-azure). The package will modify the runner code called when uploading a file in order to add specific headers needed by Azure Blob Storage.

Please note that the implementation of Azure Blob Storage uses signed URLs for both writing and reading operations. This means that your container does not need to be public. It also means that both type of URLs will expire. Signed URLs used for writing will expire after the time set using `AZURE_UPLOAD_URL_EXPIRY_IN_HOURS` (defaults to a day). Signed URLs used for reading will expire after a year, which is the maximum duration.

#### Configuration Options

{% hint style="danger" %}
Treat your connexion string as a secret and hide it.
{% endhint %}

{% hint style="info" %}
Refer to[`docker-compose.azure-blob-storage.yml`](https://github.com/sorry-cypress/sorry-cypress/blob/master/docker-compose.azure-blob-storage.yml)for setup example.
{% endhint %}

`AZURE_CONNEXION_STRING="*********"`

Connexion string to your Azure Blob Storage Account (documentation [here](https://docs.microsoft.com/en-us/azure/storage/common/storage-configure-connection-string))

`AZURE_UPLOAD_URL_EXPIRY_IN_HOURS="24"`

Duration during which the signed upload urls stay valid.

`AZURE_CONTAINER_NAME="sorry-cypress"`

Container name for storing generated artifacts. Please make sure that the container is created and configured properly before using it.

Azure Blob Storage has the same caveats as [MinIO](minio-configuration.md) : the hostname is part of the signed URL. Please see the Minio documentation to understand the implications in terms of network configuration.
