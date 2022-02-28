# AWS App Runner Developer Guide

-----
*****Copyright &copy; Amazon Web Services, Inc. and/or its affiliates. All rights reserved.*****

-----
Amazon's trademarks and trade dress may not be used in 
     connection with any product or service that is not Amazon's, 
     in any manner that is likely to cause confusion among customers, 
     or in any manner that disparages or discredits Amazon. All other 
     trademarks not owned by Amazon are the property of their respective
     owners, who may or may not be affiliated with, connected to, or 
     sponsored by Amazon.

-----
## Contents
+ [What is AWS App Runner?](what-is-apprunner.md)
+ [Setting up for App Runner](setting-up.md)
+ [Getting started with App Runner](getting-started.md)
+ [App Runner architecture and concepts](architecture.md)
+ [App Runner service based on a source image](service-source-image.md)
+ [App Runner service based on source code](service-source-code.md)
   + [Using the Python platform](service-source-code-python.md)
      + [Python runtime release information](service-source-code-python-releases.md)
   + [Using the Node.js platform](service-source-code-nodejs.md)
      + [Node.js runtime release information](service-source-code-nodejs-releases.md)
   + [Using the Java platform](service-source-code-java.md)
      + [Java runtime release information](service-source-code-java-releases.md)
+ [Developing application code for App Runner](develop.md)
+ [Using the App Runner console](console.md)
+ [Managing your App Runner service](manage.md)
   + [Creating an App Runner service](manage-create.md)
   + [Deploying a new application version to App Runner](manage-deploy.md)
   + [Configuring an App Runner service](manage-configure.md)
      + [Configuring service settings using sharable resources](manage-configure-resources.md)
      + [Configuring health checks for your service](manage-configure-healthcheck.md)
   + [Managing App Runner connections](manage-connections.md)
   + [Managing App Runner automatic scaling](manage-autoscaling.md)
   + [Managing custom domain names for an App Runner service](manage-custom-domains.md)
   + [Pausing and resuming an App Runner service](manage-pause.md)
   + [Deleting an App Runner service](manage-delete.md)
+ [Networking with App Runner](network.md)
   + [Enabling VPC access for your service](network-vpc.md)
   + [Using App Runner with VPC endpoints](network-vpce.md)
+ [Logging and monitoring for your App Runner service](monitor.md)
   + [Tracking App Runner service activity](monitor-activity.md)
   + [Viewing App Runner logs streamed to CloudWatch Logs](monitor-cwl.md)
   + [Viewing App Runner service metrics reported to CloudWatch](monitor-cw.md)
   + [Handling App Runner events in EventBridge](monitor-ev.md)
   + [Logging App Runner API calls with AWS CloudTrail](monitor-ct.md)
+ [Setting App Runner service options using a configuration file](config-file.md)
   + [App Runner configuration file examples](config-file-examples.md)
   + [App Runner configuration file reference](config-file-ref.md)
+ [The App Runner API](api.md)
   + [Using AWS CloudShell to work with AWS App Runner](api-cshell.md)
+ [Security in App Runner](security.md)
   + [Data protection in App Runner](security-data-protection.md)
      + [Protecting data using encryption](security-data-protection-encryption.md)
      + [Internetwork traffic privacy](security-data-protection-internetwork.md)
   + [Identity and access management for App Runner](security-iam.md)
      + [How App Runner works with IAM](security_iam_service-with-iam.md)
      + [App Runner identity-based policy examples](security_iam_id-based-policy-examples.md)
      + [Using service-linked roles for App Runner](security-iam-slr.md)
         + [Using roles for management](using-service-linked-roles-management.md)
         + [Using roles for networking](using-service-linked-roles-networking.md)
      + [AWS managed policies for AWS App Runner](security-iam-awsmanpol.md)
      + [Troubleshooting App Runner identity and access](security_iam_troubleshoot.md)
   + [Logging and monitoring in App Runner](security-monitoring.md)
   + [Compliance validation for App Runner](security-compliance.md)
   + [Resilience in App Runner](security-resilience.md)
   + [Infrastructure security in AWS App Runner](security-infrastructure.md)
   + [Configuration and vulnerability analysis in App Runner](security-shared-responsibility.md)
   + [Security best practices for App Runner](security-best-practices.md)
+ [AWS glossary](glossary.md)