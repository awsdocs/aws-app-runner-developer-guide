# Configuring an App Runner service<a name="manage-configure"></a>

When you [create an AWS App Runner service](manage-create.md), you set various configuration values\. You can change some of these configuration settings after you create the service\. Other settings can be applied only while creating the service and cannot be changed thereafter\. This topic discusses the configuration of your service using the App Runner API, the App Runner console, and an App Runner configuration file\.

## Configure your service using the App Runner API or AWS CLI<a name="manage-configure.api"></a>

The API defines which settings can be changed after service creation\. The following list discusses the relevant actions, types, and limitations\.
+ [UpdateService](https://docs.aws.amazon.com/apprunner/latest/api/API_UpdateService.html) action – Can be called after creation to update some configuration settings\.
  + *Can be updated* – You can update settings in the `SourceConfiguration`, `InstanceConfiguration`, and `HealthCheckConfiguration` parameters\. However, in `SourceConfiguration`, you can't switch your source type from code to image or the other way around\. You must provide the same repositoryparameter as you provided when you created the service\. It's either `CodeRepository` or `ImageRepository`\.

    You can also update `AutoScalingConfigurationArn`, the ARN of the auto scaling configuration resource associated with the service\.
  + *Cannot be updated* – You can't change the `ServiceName` and `EncryptionConfiguration` parameters that are available in the [CreateService](https://docs.aws.amazon.com/apprunner/latest/api/API_CreateService.html) action\. They can't be changed after they're created\. The [UpdateService](https://docs.aws.amazon.com/apprunner/latest/api/API_UpdateService.html) action doesn't include these parameters\.
  + *API vs\. file* – You can set the `ConfigurationSource` parameter of the [CodeConfiguration](https://docs.aws.amazon.com/apprunner/latest/api/API_CodeConfiguration.html) type \(used for source code repositories as part of `SourceConfiguration`\) to `Repository`\. In this case, App Runner ignores the configuration settings in `CodeConfigurationValues`, and reads these settings from a [configuration file](config-file.md) in your repository\. If you set `ConfigurationSource` to `API`, App Runner gets all configuration settings from the API call and ignores the configuration file, even if one exists\.
+ [TagResource](https://docs.aws.amazon.com/apprunner/latest/api/API_TagResource.html) action – Can be called after your service is created to add tags to the service or update values of existing tags\.
+ [UntagResource](https://docs.aws.amazon.com/apprunner/latest/api/API_UntagResource.html) action – Can be called after your service is created to remove tags from the service\.

## Configure your service using the App Runner console<a name="manage-configure.console"></a>

The console uses the App Runner API to apply configuration updates\. The update rules that the API imposes, as defined in the previous section, determine what you can configure using the console\. Some settings that were available during service creation aren't available for modification later on\. In addition, if you decide to use a [configuration file](config-file.md), additional settings are hidden in the console, and App Runner reads them from the file\.

**To configure your service**

1. Open the [App Runner console](https://console.aws.amazon.com/apprunner), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Services**, and then choose your App Runner service\.

   The console displays the service dashboard with a **Service overview**\.

1. On the service dashboard page, choose the **Configuration** tab\.

   Result: The console displays the current configuration settings of your service in several sections: **Source and deployment**, **Configure build**, and **Configure service**\.

1. To update settings in any category, choose **Edit**\.

1. On the configuration edit page, make any desired changes, and then choose **Save changes**\.

## Configure your service using an App Runner configuration file<a name="manage-configure.file"></a>

When you create or update an App Runner service, you can instruct App Runner to read some configuration settings from a configuration file that you provide as part of your source repository\. By doing this, you can manage the settings that are related to your source code under source control, together with the code itself\. The configuration file also provides certain advanced settings that you can't set using the console or the API\. For more information, see [Setting App Runner service options using a configuration file](config-file.md)\.