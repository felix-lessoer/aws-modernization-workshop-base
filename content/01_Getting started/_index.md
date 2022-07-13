---
title: "Getting started"
chapter: false
weight: 20
pre: "<b>1. </b>"
---

## Start the Elastic Environment via AWS Marketplace

To get started, the first thing you need is your Elastic environment within AWS. While you have the option to deploy Elastic manually on top of EC2 instances, the easiest way to start is by using Elastic Cloud. You can start a seven-day free trial of Elastic Cloud via the subscription button on [AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-voru33wi6xs7k?ref_=unifiedsearch).

If you prefer to navigate to it manually, please follow these steps:
Log in to the [AWS Console](https://aws.amazon.com/console/)
Search for Elastic within the AWS Marketplace
Choose Elastic Cloud (Elasticsearch Service)

Now you need to click the “Click to subscribe” button and follow the sign up process within the Elastic Portal. Within the Elastic Portal you need to register a new account.

Note: You need to use an email address that *has not been used before*. It's not possible to convert existing accounts into AWS Marketplace accounts.

Using this newly created account you are able to create your first Elastic Cluster based on your needs.

The free trial includes provisioning of a single deployment and you will not be charged for the first 7 days (billing starts automatically after the 7-day trial period ends if you do not delete your deployment).

When you subscribe to the Elasticsearch Service directly from the AWS Marketplace you then have the convenience of viewing your Elasticsearch Service subscription as part of your AWS bill, and you do not have to supply any additional credit card information to Elastic.

More details can be found in [Elastic documentation](https://www.elastic.co/guide/en/cloud/current/ec-billing-aws.html).

## Get familiar with the environment
When accessing Elastic Cloud for the first time, you will enter via the Elastic Cloud Portal. The Portal provides all necessary information and tools in order to manage your Elastic deployment(s) and everything associated (like receiving first class vendor support).

### Elastic Cloud Portal
Within the Elastic Cloud Portal you can create new Elastic environments. As Elastic is able to fulfill plenty of different use cases it might be useful to separate the different tasks by having one environment per use case. However for the purpose of this workshop, you can simply work with the one environment you get by using the trial.

In the top left panel you can see the overview of your current deployments within the Elasticsearch Service. Click on the name to get into the environment. When you click on the gear icon, you can get into the configuration options for the deployment. Here you can configure autoscaling, AWS Private Connect, IP filtering and other important security and size-related options.

### Elastic Deployment
When accessing the Elastic Deployment for the first time, you won’t have any data in it. Elastic offers you the option to load sample data. If you just want to look around, this is a quick and easy way to familiarize yourself with Elastic.

However, in order to complete this workshop the best choice is to click on “Explore on my own” and navigate to “Add integrations”. But before we do this, let's have a quick look at the rest of the navigation.

As described in the Introduction, Elastic offers 3 major Solutions: Enterprise Search, Observability and Security. The navigation reflects these solutions. In addition, you will also see the Analytics and Management section. Both sections are useful across all solutions.

Within the Analytics section, you have access to all the necessary tools to search and analyze your data. Machine Learning and custom dashboarding are also part of this section.

The Management section provides a UI to manage everything that belongs to the Elastic Stack. Here you will find data onboarding as well as alerts creation and security detail configurations.  

## Activate the first AWS integration
To add your first integration, you need to have an Elastic Agent up and running. The Elastic Agent can run on any EC2 instance or other VM. There are other options, like Lambda functions to load data as well.

Before we start adding the first AWS integration, we need to make sure that we have the right AWS Credentials and Permissions first.

### AWS Credentials

AWS credentials are required to run AWS integrations. There are a few ways to provide AWS credentials:
* Use access keys directly
* Use temporary security credentials
* Use a shared credentials file
* Use an IAM role Amazon Resource Name (ARN)
For this workshop, we just use the access keys directly. To do that, navigate to the IAM settings → Users → Choose the user you would like to use → Security credentials and create a new access key pair.

### AWS Permissions

Specific AWS permissions are required for the IAM user to make specific AWS API calls. To enable the AWS integration to collect metrics and logs from all supported services, make sure these permissions are given:
* ec2:DescribeInstances
* ec2:DescribeRegions
* cloudwatch:GetMetricData
* cloudwatch:ListMetrics
* iam:ListAccountAliases
* rds:DescribeDBInstances
* rds:ListTagsForResource
* s3:GetObject
* sns:ListTopics
* sqs:ChangeMessageVisibility
* sqs:DeleteMessage
* sqs:ListQueues
* sqs:ReceiveMessage
* sts:AssumeRole
* sts:GetCallerIdentity
* tag:GetResources

By clicking on “Add integrations”, you get to the list of all available integrations. Click on “AWS'' in the left bar in order to see the list of all available services. Please choose Amazon EC2 to start. Just click on “Add Amazon EC2” in the overview page of the integration and configure the details as shown below.

The Elastic frontend will guide you through the process of installing an agent, adding the integration, and waiting for the first data to arrive..
! Note that the integration will collect the data via AWS CloudWatch and is only able to get the e.g. the EC2 logs if the [EC2 instance is configured to collect them in CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/QuickStartEC2Instance.html).

If you want to collect the System metrics and Logs directly this is possible by activating the system integration as well. The system integration is collecting all relevant metrics and logs from the system that is running the agent. If you like to collect those from all EC2 instances you need to run one agent per instance.

## Check the out of the box assets that you got
After the successful setup of the AWS EC2 integration you will be able to see the data of your environment in your dashboards. To test is navigate to dashboards and type “EC2” in the search bar.
You will find a Dashboard called [Metrics AWS] EC2 Overview that will contain the data you are looking for.
Working with AWS integrations is a great way to collect the monitoring data that is provided by AWS via CloudWatch or the logs that are stored in S3. If you want to have a deeper view into your EC2 instances you can extend the data that is collected by the agent by adding more integrations. Learn more about that in Observe the AWS platform environment

## [Optional] Security / Authentication
tbd
