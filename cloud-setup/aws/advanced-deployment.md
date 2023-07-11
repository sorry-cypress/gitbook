---
description: Sorry-cypress installation instructions for AWS Advanced Deployment CloudFormation Template
---

# AWS Advanced Deployment

## Stack Overview

The advanced deployment uses ECS to run sorry-cypress services, and has many parameters for customization.

Here are the main differences between this template and the simple deployment template.

1. Does not create a VPC

   This template expects a VPC, subnets and routing to exist already. If you don't have any of this deployed in the account yet, deploy the standard-networking CloudFormation template and it will deploy:

   - 2 public subnets
   - 2 private subnets
   - an Internet Gateway
   - a NAT Gateway
   - route tables
   - all required attachments and associations

2. Deploy a remote DB

   You have the option of deploying a remote AWS DocumentDB that the ECS cluster uses for storing cypress test results data. When using a local database, the data is stored in a mongodb container that is running inside the ECS task. That means the data is lost when the task is deleted or restarted, and then only one task can run at a time. A remote DB allows for running multiple tasks to handle higher load, and restarting tasks while maintaining data.

3. Automated Task Scaling

   You can configure automatic scaling of the ECS tasks to scale up and down during peak and off hours. This helps save costs when sorry-cypress isn't being used, and helps provide service availability during peak work hours.

4. Stronger Security

   This template deploys a customer-managed KMS key (CMK) that is used to encrypt the database, and optionally, the S3 bucket. You can also provide an ACM certificate for HTTPS access to the sorry-cypress dashboard.

5. Provide Dockerhub Credentials

   If you have a dockerhub account, you can provide the credentials to the template. They are stored in AWS SecretsManager, and provided to the ECS tasks to allow them to authenticate with dockerhub when pulling the sorry-cypress images. This prevents being throttled by dockerhub.

6. S3 Storage Lifecycle

   You can set a lifecycle on the S3 bucket that stores videos and screenshots of the cypress tests. The value set is how long they will be retained, in days.

---

The artifacts created by the stack are:

* Director URL - this is what you provide when [configure cypress agent to use the alternative dashboard.](../../integrating-cypress/configuring-cypress-agent.md)
* Dashboard URL - web dashboard access URL
* API URL - GraphQL API access URL
* S3 Bucket - for storing tests video recordings and screenshots
* Cloudwatch log groups for debugging and troubleshooting

---

## Template Configuration

Below are descriptions of the various parameters in this template and what they do.

### Network Configuration

`VPC`

The VPC in which to deploy sorry-cypress. This can be a pre-existing VPC or the one deployed by the standard-networking template.

`PrivateSubnets`

At least one private subnet for the deployment.

`LoadBalancerScheme`

The scheme for the load balancer: internet-facing or internal. If you're deploying an internet-facing load balancer, select public subnets for the `LoadBalancerSubnets` parameter. If internal, select private subnets.

`LoadBalancerSubnets`

The subnets in which to deploy the user-facing load balancer. These can be public or private, depending on your needs.

### Security Configuration

`AccessCIDR`

The CIDR range that is allowed to reach the sorry-cypress front-end load balancer.

This CIDR range is permitted to the load balancer via it's attached security group.

More ranges can be added by hand later on, if need be.

`AllowedCIDRRanges`

A comma-separated list of CIDR ranges that can download from the S3 bucket.

Keep in mind that downloads come from your browser when access the sorry-cypress dashboard, so these CIDR ranges should be / include the IP address from which you and other users will access sorry-cypress.

In most cases, this will be the same as the `AccessCIDR` parameter.

Example: `1.1.1.1/32,2.2.2.2/32`

`ACMCertificateArn`

The ARN of a verified ACM certificate. This is attached to the sorry-cypress front-end load balancer to provide HTTPS access to the dashboard.

`S3ObjectACL (default: public-read)`

The ACL to apply to uploaded objects in S3 (videos and screenshots). This value
will be set in the pre-signed URL that sorry-cypress generates when a job is about
to upload something to the S3 bucket.

The recommended value is `public-read`. The bucket, however, will not be fully public. It will have a bucket policy that restricts downloads to CIDR ranges that you provide.

### ECS Task Configuration

`TaskCpu (default: 1024)`

The amount of CPU units dedicated to running the services. Sorry-cypress uses AWS Fargate as compute platform, and runs all the services as a single task, i.e. those CPU units are shared among all the services. Read more about at [AWS Documentation](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task\_definition\_parameters.html#task\_size)

`TaskMemory (default: 2048)`

The amount of memory units dedicated to running the services. This resource is also shared between the services and defined at task-level. Read more at [AWS Documentation](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task\_definition\_parameters.html#task\_size)``

`DirectorPort (default: 8080)`

The port number for accessing the director service. You'll need to use it as a destination when [configuring cypress agents](../../integrating-cypress/configuring-cypress-agent.md).

`SorryCypressVersion (default: latest)`

The version tag of the sorry-cypress image to pull when starting the tasks.

`DockerUsername`

(Optional) The username to dockerhub, for authenticating when pulling the sorry-cypress images.

`DockerPassword`

(Optional) The password to dockerhub, for authenticating when pulling the sorry-cypress images.

### Screenshots Storage Configuration

`S3LifecycleExpirationDays (default: 7)`

The number of days after which to expire objects in the screenshots storage S3 bucket

### Database Configuration

`DBType (default: remote)`

The type of database. If `remote`, a DocumentDB database will be created which runs externally from the ECS cluster. If `local`, a mongodb container will run in the ECS tasks.

`DBInstanceClass (default: db.t3.medium)`

(Conditional) The class for the DB instance, if using a remote database. If using a local database, this can be ignored.

`DBPassword`

(Conditional) The password for the database, if using a remote database. If using a local database, this can be ignored.

### Scheduled Scaling Configuration

`MinCapacityOff (default: 0)`

The minimum number of sorry-cypress tasks to run during "off" hours.

`MinCapacityOn (default: 1)`

The minimum number of sorry-cypress tasks to run during "on" hours.

`MaxCapacityOff (default: 0)`

The maximum number of sorry-cypress tasks to run during "off" hours.

`MaxCapacityOn (default: 2)`

The maximum number of sorry-cypress tasks to run during "on" hours.

`ScaleUpHour (default: 5)`

The hour in EST (24-hour format) at which to scale up the sorry-cypress service tasks each day.

`ScaleDownHour (default: 23)`

The hour in EST (24-hour format) at which to scale down the sorry-cypress service tasks each day.

## AWS Pricing

You're only paying for AWS resources. Here's a rough estimator of price / month for using the resources used. The actual usage might be higher (or lower) based on actual usage

* Fargate pricing based on [calculator](http://fargate-pricing-calculator.site.s3-website-us-east-1.amazonaws.com/) **35.546 USD** (1 vCPU, 2GB RAM) or **17.773 USD** (0.5 vCPU, 1GB RAM)

* EC2 Application Load Balancer based on [calculator](https://aws.amazon.com/elasticloadbalancing/pricing/) **19.35 USD** (0.5 GB / hour, 0.5 connections / second)

* DocumentDB database based on [calculator](https://aws.amazon.com/documentdb/pricing/) **61.90 USD** (db.t3.medium instance @ $0.078 per hour)

* S3 + Cloudwatch = varies based on usage

