# Managing App Runner automatic scaling<a name="manage-autoscaling"></a>

AWS App Runner automatically scales compute resources \(instances\) up or down for your App Runner application\. Automatic scaling provides adequate request handling when incoming traffic is high, and reduces your cost when traffic slows down\. You can configure a few parameters to adjust auto scaling behavior for your service\.

App Runner maintains auto scaling settings in a resource called *AutoScalingConfiguration*\. You can provide an auto scaling configuration resource when you create or update a service\. The App Runner console creates one for you when you create a new App Runner service\. Providing an auto scaling configuration is optional\. If you don't provide one, App Runner provides a default auto scaling configuration with recommended values\.

An auto scaling configuration has a *name* and a numeric *revision*\. Multiple revisions of a configuration have the same name and different revision numbers\. You can use different configuration names for different auto scaling scenarios, such as high availability or low cost\. For each name, you can add multiple revisions to fine\-tune the settings for a specific scenario\.

The following is some important information about auto scaling configurations:
+ **Settings** – Here's what you can configure:
  + *Max concurrency* – The maximum number of concurrent requests that an instance processes\. When the number of concurrent requests exceeds this quota, App Runner scales up the service\.
  + *Max size* – The maximum number of instances that your service scales up to\. At most this number of instances are actively serving traffic for your service\.
  + *Min size* – The minimum number of instances that App Runner provisions for your service\. The service always has at least this number of provisioned instances\. Some of them actively serve traffic\. The rest of them \(provisioned and inactive instances\) stand by as a cost\-effective compute capacity reserve, which is ready to be quickly activated\. You pay for the memory usage of all provisioned instances\. You pay for the CPU usage of only the active subset\.

    App Runner temporarily doubles the number of provisioned instances during deployments, to maintain the same capacity for both old and new code\.
+ **Revisions** – The first configuration that you create with a name gets the revision number 1\. Subsequent configurations with the same name get consecutive revision numbers \(starting with 2\)\. You can associate your App Runner service with a specific auto scaling configuration revision or with the latest revision of configuration\.
+ **Shared** – You can share a single auto scaling configuration resource across multiple App Runner services\. This is useful if they have similar scaling requirements\. In particular, you can configure multiple services to all use the latest version of a configuration by specifying the configuration name but not specifying a revision\. By doing this, any of the services that you configured this way receives auto scaling configuration updates when you update the service\. For more information about configuration changes, see [Configuring an App Runner service](manage-configure.md)\.
+ **Resource management** – You can use App Runner to create and delete auto scaling configurations\. You can't directly update a configuration\. Instead, you can create a new revision to an existing configuration name to effectively update the configuration\.
**Note**  
At this time, you can only create a configuration with a single revision in the App Runner console\. To create more revisions, and to delete configurations, use the App Runner [API](#manage-autoscaling.api)\.
+ **Resource quota** – There are set quotas for the number of unique configuration names and revisions that you can have for your auto scaling configuration resources in each AWS Region\. If you reach these quotas, you must either delete a configuration name or at least some of its revisions before you can create more\. Use the App Runner [API](#manage-autoscaling.api) to delete them\. For more information, see [App Runner resource quotas](architecture.md#architecture.quotas)\.

## Manage auto scaling using the App Runner console<a name="manage-autoscaling.console"></a>

When you [create a service](manage-create.md) in the App Runner console, you can use the default auto scaling configuration or a custom configuration\. To use a custom configuration, either choose an existing configuration or provide a new name and settings\. If it's a new configuration, App Runner creates a new auto scaling configuration resource for you, and then associates it with your new service\.

## Manage auto scaling using the App Runner API or AWS CLI<a name="manage-autoscaling.api"></a>

You can use the following App Runner API actions to manage your auto scaling configurations\.
+ [CreateAutoScalingConfiguration](https://docs.aws.amazon.com/apprunner/latest/api/API_CreateAutoScalingConfiguration.html) – Creates a new auto scaling configuration or a revision to an existing one\.
+ [ListAutoScalingConfigurations](https://docs.aws.amazon.com/apprunner/latest/api/API_ListAutoScalingConfigurations.html) – Returns a list of the auto scaling configurations that are associated with your AWS account, with summary information\.
+ [DescribeAutoScalingConfiguration](https://docs.aws.amazon.com/apprunner/latest/api/API_DescribeAutoScalingConfiguration.html) – Returns a full description of an auto scaling configuration\.
+ [DeleteAutoScalingConfiguration](https://docs.aws.amazon.com/apprunner/latest/api/API_DeleteAutoScalingConfiguration.html) – Deletes an auto scaling configuration\. You can delete a specific revision or the latest active revision\. You might need to delete unnecessary auto scaling configurations if you reach the auto scaling configuration quota for your AWS account\.