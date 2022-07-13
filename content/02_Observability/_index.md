---
title: "Observability"
chapter: false
weight: 30
pre: "<b>2. </b>"
---

[Monitoring the AWS environment](https://www.elastic.co/observability/aws-monitoring) is a very common use case for Elastic. You can download the complete [ebook](https://www.elastic.co/aws/the-elastic-observability-guide-for-aws) about that topic.

![AWS overview](/images/aws-overview.png)

Elastic observes cross VPC, cross region, cross AZ, cross Account. No matter how you organize your environment, Elastic will fit into it. Collect everything in a single Elastic environment and authorize only the relevant team to it. Even if you are responsible to observe multiple different cloud providers or hybrid environments Elastic can help you getting better insights into what is happening in one single tool.

Elastic supports collecting data via different ways. Using the Elastic Agent, but also [agentless](https://serverlessrepo.aws.amazon.com/applications/eu-central-1/267093732750/elastic-serverless-forwarder) and native integrations are available. In the introduction we already learned about the Elastic Agent and how this can get used. We will continue using the agent to observe the AWS platform and the apps you are running in it.

However if you prefer not using the Elastic Agent you can collect the same data using the [Elastic serverless forwarder](https://serverlessrepo.aws.amazon.com/applications/eu-central-1/267093732750/elastic-serverless-forwarder) which is a Lambda function that is able to collect the same data without the need to deploy an agent.

![AWS serverless overview](/images/aws-serverless-overview.png)

## Observe the AWS platform

### Platform Logs

In AWS you have three options to collect logs. This also holds for the platform / service specific logs. You can collect the data via the Serverless Log forwarder, via CloudWatch or collect the data from an S3 bucket.

If you decide for the S3 bucket you need to configure SQS notifications that get triggered whenever new data is written to the bucket. This approach is best practice to avoid significant lagging with polling. Elastic Agent combines notification and polling together by using Amazon SQS for Amazon S3 notification when a new Amazon S3 object is created.

To configure SQS Event Notifications click into the bucket that is holding your log data and navigate to “Properties”. Within the Properties section look for Event notifications and create a new entry.

#### Cloudtrail
AWS CloudTrail enables governance, compliance, operational auditing, and risk auditing of your AWS account. This is helpful to observe the activities happening within your AWS environment(s).
![AWS cloudtrail](/images/cloudtrail.png)

#### VPC Flow logs
Elastic Observability allows you to quickly search, view, and filter Amazon Virtual Private Cloud (Amazon VPC) Flow Logs to monitor network traffic within your Amazon VPC with Kibana. With this integration, you can analyze the flow log data and compare it with your security group configurations to maintain and improve your cloud security.
![AWS vpc flow](/images/vpc-flow.png)

### CloudWatch metrics
With Elastic’s integrations and pre-built dashboards for AWS, you can collect AWS metrics such as usage, performance and more to see how every signal correlates — enabling you to make more informed business decisions.
![AWS CloudWatch metrics](/images/metrics.png)

In order to collect AWS CloudWatch metrics you need to navigate to the installed integrations and change the AWS integration. Click on AWS → Integration policies → aws-1 (Default name)
![Elastic integration page](/images/integration-page.png)
Just enable every integration that is delivering metrics. Screenshot is not showing all possible options.
![Elastic aws metric integrations](/images/metric-integrations.png)
After enabling all the metrics you should be able to see the data in the out of the box dashboards.

You can also extend the out of the box dashboards by downloading pre-built dashboards from the Elastic Community.

### Billing data
The AWS Billing data is a special part of the metric data from the previous chapter. The purpose of this data is to get mutual understanding of the current spending within the observed AWS accounts / organizations.
Per default AWS only sends service level billing data which is nice to get a high level overview. However if your target is to reduce costs by correlating the usage data (logs and metrics) with the billing data this might be not enough details. AWS also offers to export more detailed billing data within the API or into a separate S3 bucket. Collecting this resource level billing data is very useful to compare the usage and the actual cost and therefore identify possible savings.

![Elastic aws billing integration](/images/billing-integration.png)

The AWS billing data is just another AWS integration that is available for the Elastic Agent. Just enable this integration and make sure that the access you are using has appropriate rights in order to get the data.

## Observe apps running in AWS
While apps running in AWS use many of the available AWS services they all have their custom code as well. Consequently observing the AWS services and platform is only one part of the story. The other necessary part to get full visibility is to have the ability to look deep into what's happening in your application as well by observing your individual logs, metrics and application traces.  

### Application logs
In order to get your individual logs to Elastic you need to consider two things. First how to get the logs of all parts of the application in. If your application is using container or kubernetes technology this is quite simple.

If your application is running on EC2 instances you might need to collect custom log sources.

Independently of the log collection your also need to consider the log parsing. Per default custom logs will be collected as is. But most often the interesting information are within the log line itself. Therefore it can be helpful to parse the relevant information within the log lines and store them in separate fields. This makes it really handy to create visualizations and filters based on these extracted values.

If you are not sure whether you need something to parse out or not the best option is use a runtime field. A runtime field can be generated after the data is loaded and will extract the necessary information from any document that exists. However this comes with some performance drawbacks.
If you know which kind of information you would like to extract the better option is to use pre index parsing using ingest node pipelines. Within an ingest node pipeline you can configure so-called processors to extract and fine tune the parsing before the data gets indexed to Elastic. The big advantage is that this will happen only once per document. In comparison to the runtime fields where the extraction happens on every query for every affected document.

Having a look into the integrations page within Elastic there are multiple possibilities to get your custom logs in.
![Elastic integration page](/images/integration-page-2.png)
Elastic offers Serverless log forwarding using Lambda functions, as well as Log collection from S3, CloudWatch and directly from the EC2 machine. In addition to that you also could use the diverse language clients to directly send the logs to Elastic from your application.

After you added the integration based on your need you can change the parsing like described above using the Ingest Pipelines (Under Stack Management).  

### APM (Traces)
The collection and analysis of Application transactions and traces help you to get a deep understanding of what is happening within your application and how the different parts communicate to each other.

Using distributed tracing you also can have a look into the service map that will be created automatically based on the data and the service map also incorporates Elastic Machine Learning results.
![Elastic service map](/images/service-map.png)

### Lambda functions
Many of the modern application within the AWS ecosystem also using Lambda function. With Elastic APM its also possible to collect the traces from the Lamdba functions in additions to the CloudWatch metrics that you can collect.
The following pictures illustrates how Elastic APM integrates into your Lambda processes.
![Lambda to Elastic](/images/lambda-to-elastic.png)

### Observing vulnerabilities and compliance
Operating your services within AWS enables you to use the wide range of AWS specific offerings in your application. Because of the complexity and the many different configuration options as well as the shared responsibilities between AWS your company its also possible to operate with less secure setups.

In order to prevent that Elastic also offers to support you with observing the configurations of your used services.
![Elastic Endpoint integration](/images/endpoint-integration.png)
To observe and prevent vulnerabilities you can use the Elastic Endpoint Integration that is part of the Elastic agent and use this to prevent any kind of malware or ransomware attacks to your used EC2 instances. Like with all other integrations the activation only needs a few clicks and is done after a few minutes.

To also observe compliance and configuration issues Elastic also provides Cloud Posture management in order to execute automatic audits and benchmarks against your environment.
