# Configuring service settings using sharable resources<a name="manage-configure-resources"></a>

For some features, it makes sense to share configuration across AWS App Runner services\. For example, you might want a set of services to have the same auto scaling behavior\. App Runner lets you share settings by using separate sharable resources\. You create a resource that defines a set of configuration settings for a feature, and then you provide the Amazon Resource Name \(ARN\) of this configuration resource to one or more App Runner services\.

App Runner implements sharable configuration resources for the following features:
+ [Auto scaling](manage-autoscaling.md)
+ [VPC access](network-vpc.md)

The document page for each of these features provides information about the available settings and the management procedures\.

Features using separate configuration resources share some design traits and considerations\.
+ **Revisions** – Some configuration resources can have revisions\. In these cases, each configuration has a *name* and a numeric *revision*\. Multiple revisions of a configuration have the same name and different revision numbers\. You can use different configuration names for different scenarios\. For each name, you can add multiple revisions to fine\-tune the settings for a specific scenario\.

  The first configuration that you create with a name gets the revision number 1\. Subsequent configurations with the same name get consecutive revision numbers \(starting with 2\)\. You can associate your App Runner service with a specific configuration revision or with the latest revision of configuration\.
+ **Shared** – You can share a single configuration resource across multiple App Runner services\. This is useful if you want to maintain identical configurations across these services\. In particular, for resources that support revisions, you can configure multiple services to all use the latest version of a configuration by specifying the configuration name but not specifying a revision\. By doing this, any of the services that you configured this way receives configuration updates when you update the service\. For more information about configuration changes, see [Configuring an App Runner service](manage-configure.md)\.
+ **Resource management** – You can use App Runner to create and delete configurations\. You can't directly update a configuration\. Instead, for resources that support revisions, you can create a new revision to an existing configuration name to effectively update the configuration\.
**Note**  
At this time, you can only create a configuration with a single revision in the App Runner console\. To create more revisions, and to delete configurations, use the App Runner API\.
+ **Resource quota** – There are set quotas for the number of unique configuration names and revisions that you can have for your configuration resources in each AWS Region\. If you reach these quotas, you must either delete a configuration name or at least some of its revisions before you can create more\. Use the App Runner API to delete them\. For more information, see [App Runner resource quotas](architecture.md#architecture.quotas)\.
+ **No resource cost** – You don't incur additional cost for creating a configuration resource\. You might incur cost for the feature itself \(for example, you are charged for normal AWS X\-Ray cost when you turn on X\-Ray tracing\), but not for the App Runner configuration resource that configures the feature for your App Runner service\.