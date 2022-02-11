# Configuring health checks for your service<a name="manage-configure-healthcheck"></a>

AWS App Runner monitors the health of your service by performing health checks\. The default health check protocol is TCP\. App Runner pings the domain assigned to your service\. By using the App Runner API or the AWS CLI you can alternatively set the health check protocol to HTTP\. App Runner sends health check HTTP requests to your web application\.

You can configure a few settings related to health checks\. The following table describes the health check settings and their default values\.


|  **Setting**  |  **Description**  |  **Default**  | 
| --- | --- | --- | 
|  Protocol  |  The IP protocol that App Runner uses to perform health checks for your service\. If you set the protocol to `TCP`, App Runner pings the default domain assigned to your service at the port that your application is listening to\. If you set the protocol to `HTTP`, App Runner sends health check requests to the configured path\.  |  `TCP`  | 
|  Path  |  The URL that App Runner sends HTTP health check requests to\. Applicable only to HTTP checks\.  |  `/`  | 
|  Interval  |  The time interval, in seconds, between health checks\.  |  `5`  | 
|  Timeout  |  The time, in seconds, to wait for a health check response before deciding it failed\.  |  `2`  | 
|  Healthy threshold  |  The number of consecutive checks that must succeed before App Runner decides that the service is healthy\.  |  `1`  | 
|  Unhealthy threshold  |  The number of consecutive checks that must fail before App Runner decides that the service is unhealthy\.  |  `5`  | 

## Configure health checks<a name="manage-configure-healthcheck.configure"></a>

Configure health checks for your App Runner service using one of the following methods:

------
#### [ App Runner console ]

When you create your App Runner service using the App Runner console, or when you update its configuration later, you can configure health check settings\. For full console procedures, see [Creating an App Runner service](manage-create.md) and [Configuring an App Runner service](manage-configure.md)\. In both cases, look for the **Health check** configuration section on the console page\.

![\[App Runner console configuration page showing health check options\]](http://docs.aws.amazon.com/apprunner/latest/dg/images/console-health-check.png)

**Note**  
When you use the App Runner console, you can configure only TCP health checks\. Use App Runner API or AWS CLI for the complete health check options\.

------
#### [ App Runner API or AWS CLI ]

When you call the [CreateService](https://docs.aws.amazon.com/apprunner/latest/api/API_CreateService.html) or [UpdateService](https://docs.aws.amazon.com/apprunner/latest/api/API_UpdateService.html) API actions, you can use the `HealthCheckConfiguration` parameter to specify health check settings\.

For information about the parameter's structure, see [HealthCheckConfiguration](https://docs.aws.amazon.com/apprunner/latest/api/API_HealthCheckConfiguration.html) in the *AWS App Runner API Reference*\.

------