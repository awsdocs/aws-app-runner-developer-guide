# Logging and monitoring in App Runner<a name="security-monitoring"></a>

Monitoring is an important part of maintaining the reliability, availability, and performance of your AWS App Runner service\. Collecting monitoring data from all parts of your AWS solution allows you to more easily debug a failure if one occurs\. App Runner integrates with several AWS tools for monitoring your App Runner services and responding to potential incidents\.

**Amazon CloudWatch alarms**  
With Amazon CloudWatch alarms, you can watch a service metric over a time period that you specify\. If the metric exceeds a given threshold for a given number of periods, you receive a notification\.  
App Runner collects a variety of metrics about the service as a whole and the instances \(scaling units\) that run your web service\. For more information, see [Metrics \(CloudWatch\)](monitor-cw.md)\.

**Application logs**  
App Runner collects the output of your application code and streams it to Amazon CloudWatch Logs\. What's in this output is up to you\. For example, you could include detailed records of requests made to your web service\. These log records might prove useful in security and access audits\. For more information, see [Logs \(CloudWatch Logs\)](monitor-cwl.md)\.

**AWS CloudTrail action logs**  
App Runner is integrated with AWS CloudTrail, a service that provides a record of actions taken by a user, role, or an AWS service in App Runner\. CloudTrail captures all API calls for App Runner as events\. You can view the most recent events in the CloudTrail console, and you can create a trail to enable continuous delivery of CloudTrail events to an Amazon Simple Storage Service \(Amazon S3\) bucket\. For more information, see [API actions \(CloudTrail\)](monitor-ct.md)\.