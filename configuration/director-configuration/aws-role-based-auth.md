---
description: Director AWS Role Based Authentication
---

# AWS Role Assumption via Service Account

Service Account Name

`serviceAccountName=""`

In case of role based authentication, a service account that is annotated with a trust relationship should be used. A service account K8s object should be deployed, and annotated as per [the AWS docs](https://docs.aws.amazon.com/eks/latest/userguide/iam-roles-for-service-accounts-technical-overview.html).

The service account should be linked to an IAM role, with the end goal of the director assuming the role, therefore having access to the appropriate AWS resources (eg. an S3 bucket with test artifacts)
