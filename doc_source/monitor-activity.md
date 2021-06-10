# Tracking App Runner service activity<a name="monitor-activity"></a>

AWS App Runner uses a list of operations to keep track of activity in your App Runner service\. An operation represents an asynchronous call to an API action, such as creating a service, updating a configuration, and deploying a service\. The following sections show you how to track activity in the App Runner console and using the API\.

## Tracking App Runner service activity in the console<a name="monitor-activity.console"></a>

The App Runner console displays your App Runner service activity and provides more ways to explore operations\.

**To view activity of your service**

1. Open the [App Runner console](https://console.aws.amazon.com/apprunner), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Services**, and then choose your App Runner service\.

   The console displays the service dashboard with a **Service overview**\.

1. On the service dashboard page, choose the **Activity** tab, if it isn't already chosen\.

   The console displays a list of operations\.

1. To find specific operations, scope down the list by entering a search term\. You can search for any value that appears in the table\.

1. Choose any listed operation to see or download the related log\.

## Retrieving App Runner service operations using the App Runner API or AWS CLI<a name="monitor-activity.api"></a>

The [ListOperations](https://docs.aws.amazon.com/apprunner/latest/api/API_ListOperations.html) action, given the Amazon Resource Name \(ARN\) of an App Runner service, returns a list of operations that occurred on this service\. Each list item contains an operation ID and some tracking details\.