# Viewing App Runner service metrics reported to CloudWatch<a name="monitor-cw"></a>

Amazon CloudWatch monitors your Amazon Web Services \(AWS\) resources and the applications you run on AWS in real time\. You can use CloudWatch to collect and track metrics, which are variables you can measure for your resources and applications\. You can also use it to create alarms that watch metrics\. When a certain threshold is reached, CloudWatch sends notifications, or automatically makes changes to the monitored resources\. For more information, see [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.

AWS App Runner collects a variety of metrics that provide you with greater visibility into the usage, performance, and availability of your App Runner services\. Some metrics track individual instances that run your web service, whereas others are at the overall service level\. The following sections list App Runner metrics and show you how to view them in the App Runner console\.

## App Runner metrics<a name="monitor-cw.metrics"></a>

App Runner collects the following metrics relating to your service and publishes them to CloudWatch in the `AWS/AppRunner` namespace\.

**Instance level metrics** are collected for each instance \(scaling unit\) individually\.


|  **What's measured?**  |  **Metric**  |  **Description**  | 
| --- | --- | --- | 
|  CPU utilization  |  `CPUUtilization`  |  The average CPU usage during one\-minute periods\.  | 
|  Memory utilization  |  `MemoryUtilization`  |  The average memory usage during one\-minute periods\.  | 

**Service level metrics** are collected for the entire service\.


|  **What's measured?**  |  **Metrics**  |  **Description**  | 
| --- | --- | --- | 
|  HTTP request count  |  `Requests`  |  The number of HTTP requests that the service received\.  | 
|  HTTP status counts  |  `2xxStatusResponses` `4xxStatusResponses` `5xxStatusResponses`  |  The number of HTTP requests that returned each response status, grouped by category \(2XX, 4XX, 5XX\)\.  | 
|  HTTP request latency  |  `RequestLatency`  |  The time it took your web service to process HTTP requests\.  | 
|  Instance counts  |  `ActiveInstances`   |  The number of instances that are processing HTTP requests for your service\.   | 

## Viewing App Runner metrics in the console<a name="monitor-cw.console"></a>

The App Runner console graphically displays the metrics that App Runner collects for your service and provides more ways to explore them\.

**Note**  
At this time, the console displays only service metrics\. To view instance metrics, use the CloudWatch console\.

**To view logs for your service**

1. Open the [App Runner console](https://console.aws.amazon.com/apprunner), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Services**, and then choose your App Runner service\.

   The console displays the service dashboard with a **Service overview**\.

1. On the service dashboard page, choose the **Metrics** tab\.

   The console displays a set of metrics graphs\.   

1. Choose a duration \(for example, **12h**\) to scope metrics graphs to the recent period of that duration\.

1. Choose **Add to dashboard** at the top of one of the graph sections, or use the menu on any graph, to add the relevant metrics to a dashboard in the CloudWatch console for further investigation\.