# Using the App Runner console<a name="console"></a>

Use the AWS App Runner console to create, manage, and monitor your App Runner services and related resources, such as connections\. You can view existing services, create new ones, and configure a service\. You can view the status of an App Runner service as well as view logs, monitor activity, and track metrics\. You can also navigate to the website of your service or to your source repository\.

The following sections describe the layout and functionality of the console, and point you to related information\.

## Overall console layout<a name="console.layout"></a>

The App Runner console has three areas\. From left to right:
+ **Navigation pane** – A side pane that can be collapsed or expanded\. Use it to choose the top\-level console page you want to use\.
+ **Content pane** – The main part of the console page\. Use it to view information and perform your tasks\.
+ **Help pane** – A side pane for more information\. Expand it to get help about the page you're on\. Or choose any **Info** link on a console page to get contextual help\.

![\[The App Runner console layout, showing the navigation, content, and help panes\]](http://docs.aws.amazon.com/apprunner/latest/dg/images/getting-started-services.png)

## The Services page<a name="console.services"></a>

The **Services** page lists App Runner services in your account\. You can scope the list down by using the filter text box\.

**To get to the **Services** page**

1. Open the [App Runner console](https://console.aws.amazon.com/apprunner), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Services**\.

Things you can do here:
+ Create an App Runner service\. For more information, see [Creating an App Runner service](manage-create.md)\.
+ Choose a service name to go to the service dashboard console page\.
+ Choose a service domain to open the service web app page\.

## The service dashboard page<a name="console.dashboard"></a>

You can view information about an App Runner service and manage it from the service dashbaord page\. At the top of the page, you can see the service name\.

To get to the service dashboard, navigate to the **Services** page \(see previous section\), and then choose your App Runner service\.

The **Service overview** section provides basic details about the App Runner service and your application\. Things you can do here:
+ View service details such as status, health, and ARN\.
+ Navigate to the **Default domain**—the domain that App Runner provides for the web application running in your service\. This is a subdomain in the `awsapprunner.com` domain owned by App Runner\.
+ Navigate to the source repository deployed to the service\.
+ Start a source repository deployment to your service\.
+ Pause, resume, and delete your service\.

The tabs below the service overview are for service [management](manage.md) and [monitoring](monitor.md)\.

## The GitHub connections page<a name="console.connections-github"></a>

The **GitHub connections** page lists App Runner connections to GitHub in your account\. You can scope the list down by using the filter text box\. For more information about connections, see [Managing App Runner connections](manage-connections.md)\.

**To get to the **GitHub connections** page**

1. Open the [App Runner console](https://console.aws.amazon.com/apprunner), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **GitHub connections**\.

![\[App Runner connections page\]](http://docs.aws.amazon.com/apprunner/latest/dg/images/console-connections-github.png)

Things you can do here:
+ View a list of GitHub connections in your account\. To scope the list down, enter any text in the filter text box\.
+ Choose a connection name to go to the related GitHub account or organization\.
+ Select a connection to complete the handshake for a connection that you just established \(as part of creating a service\), or to delete the connection\.