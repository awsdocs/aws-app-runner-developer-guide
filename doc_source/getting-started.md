# Getting started with App Runner<a name="getting-started"></a>

AWS App Runner is an AWS service that provides a fast, simple, and cost\-effective way to turn an existing container image or source code directly into a running web service in the AWS Cloud\.

This tutorial covers how you can use AWS App Runner to deploy your application to an App Runner service\. It walks through configuring the source code and deployment, the service build, and the service runtime\. It also shows how to deploy a code version, make a configuration change, and view logs\. Last, the tutorial shows how to clean up the resources that you created while following the tutorial's procedures\.

**Topics**
+ [Prerequisites](#getting-started.prereq)
+ [Step 1: Create an App Runner service](#getting-started.create)
+ [Step 2: Change your service code](#getting-started.deploy)
+ [Step 3: Make a configuration change](#getting-started.config)
+ [Step 4: View logs for your service](#getting-started.logs)
+ [Step 5: Clean up](#getting-started.cleanup)
+ [What's next](#getting-started.next)

## Prerequisites<a name="getting-started.prereq"></a>

Before you start the tutorial, be sure to take the following actions:

1. Complete the setup steps in [Setting up for App Runner](setting-up.md)\.

1. Create a [GitHub](https://github.com/) account, if you don't already have one\. If you're new to GitHub, see [Getting started with GitHub](https://docs.github.com/en/github/getting-started-with-github) in the *GitHub Docs*\.

1. Create a repository in your GitHub account\. This tutorial uses the repository name `python-hello`\. Create files in the root directory of the repository, with the names and content specified in the following examples\.

### Files for the `python-hello` example repository<a name="getting-started.prereq.files"></a>

**Example requirements\.txt**  

```
pyramid==2.0
```

**Example server\.py**  

```
from wsgiref.simple_server import make_server
from pyramid.config import Configurator
from pyramid.response import Response
import os

def hello_world(request):
    name = os.environ.get('NAME')
    if name == None or len(name) == 0:
        name = "world"
    message = "Hello, " + name + "!\n"
    return Response(message)

if __name__ == '__main__':
    port = int(os.environ.get("PORT"))
    with Configurator() as config:
        config.add_route('hello', '/')
        config.add_view(hello_world, route_name='hello')
        app = config.make_wsgi_app()
    server = make_server('0.0.0.0', port, app)
    server.serve_forever()
```

## Step 1: Create an App Runner service<a name="getting-started.create"></a>

In this step, you create an App Runner service based on the example source code repository that you created on GitHub as part of [Prerequisites](#getting-started.prereq)\. The example contains a simple Python website\. These are the main steps you take to create a service:

1. Configure your source code\.

1. Configure source deployment\.

1. Configure application build\.

1. Configure your service\.

1. Review and confirm\.

The following diagram outlines the steps for creating an App Runner service:

![\[App Runner service creation workflow diagram\]](http://docs.aws.amazon.com/apprunner/latest/dg/images/getting-started-create-service-workflow.png)

**To create an App Runner service based on a source code repository**

1. Configure your source code\.

   1. Open the [App Runner console](https://console.aws.amazon.com/apprunner), and in the **Regions** list, select your AWS Region\.

   1. If the AWS account doesn't have any App Runner services yet, the console home page is displayed\. Choose **Create an App Runner service**\.  
![\[App Runner console home page showing the create service button\]](http://docs.aws.amazon.com/apprunner/latest/dg/images/getting-started-home.png)

      If the AWS account has existing services, the **Services** page with a list of your services is displayed\. Choose **Create service**\.  
![\[App Runner console services page\]](http://docs.aws.amazon.com/apprunner/latest/dg/images/getting-started-services.png)

   1. On the **Source and deployment** page, in the **Source** section, for **Repository type**, choose **Source code repository**\.

   1. Under **Connect to GitHub** choose **Add new**, and then, if prompted, provide your GitHub credentials\.
**Note**  
The following steps to install the AWS Connector for GitHub to your GitHub account are one\-time steps\. You can reuse the connection for creating multiple App Runner services based on repositories in this account\. When you have an existing connection, choose it and skip to repository selection\.

   1. In the **Install AWS Connector for GitHub** dialog box, if prompted, choose your GitHub account name\.

   1. If prompted to authorize the AWS Connector for GitHub, choose **Authorize AWS Connections**\.

   1. Choose **Install**\.

      Your account name appears as the selected **GitHub account/organization**\. You can now choose a repository in your account\.

   1. For **Repository**, choose the example repository you created, `python-hello`\. For **Branch**, choose the default branch name of your repository \(for example, **main**\)\.

1. Configure your deployments: In the **Deployment settings** section, choose **Automatic**, and then choose **Next**\.
**Note**  
With automatic deployment, each new commit to your repository automatically deploys a new version of your service\.  
![\[Source and deployment settings while creating an App Runner service\]](http://docs.aws.amazon.com/apprunner/latest/dg/images/getting-started-create-source-depl.png)

1. Configure application build\.

   1. On the **Configure build** page, for **Configuration file**, choose **Configure all settings here**\.

   1. Provide the following build settings:
      + **Runtime** – Choose **Python 3**\.
      + **Build command** – Enter **pip install \-r requirements\.txt**\.
      + **Start command** – Enter **python server\.py**\.
      + **Port** – Enter **8080**\.

   1. Choose **Next**\.
**Note**  
The Python 3 runtime builds a Docker image using a base Python 3 image and your example Python code\. It then launches a service that runs a container instance of this image\.  
![\[Build settings while creating an App Runner service\]](http://docs.aws.amazon.com/apprunner/latest/dg/images/getting-started-create-build.png)

1. Configure your service\.

   1. On the **Configure service** page, in the **Service settings** section, enter a service name\.

   1. Under **Environment variables**, add a single environment variable\. For **Key**, enter **NAME**, and for **Value**, enter any name \(for example, your first name\)\.
**Note**  
The example application reads the name you set in this environment variable and displays the name on its webpage\.

   1. Choose **Next**\.  
![\[Service settings while creating an App Runner service\]](http://docs.aws.amazon.com/apprunner/latest/dg/images/getting-started-create-service.png)

1. On the **Review and create** page, verify all the details you've entered, and then choose **Create and deploy**\.

   If the service is successfully created, the console shows the service dashboard, with a **Service overview** of the new service\.  
![\[App Runner service dashboard page\]](http://docs.aws.amazon.com/apprunner/latest/dg/images/getting-started-create-dashboard.png)

1. Verify that your service is running\.

   1. On the service dashboard page, wait until the service **Status** is **Running**\.

   1. Choose the **Default domain** value—it's the URL to the website of your service\.

      A webpage displays: **Hello, *your name*\!**  
![\[The application web page of an App Runner service\]](http://docs.aws.amazon.com/apprunner/latest/dg/images/getting-started-create-webpage.png)

## Step 2: Change your service code<a name="getting-started.deploy"></a>

In this step, you make a change to your source code repository\. The App Runner CI/CD capability automatically builds and deploys the change to your service\.

**To make a change to your service code**

1. Navigate to your example GitHub repository\.

1. Choose the file name `server.py` to navigate to that file\.

1. Choose **Edit this file** \(the pencil icon\)\.

1. In the expression assigned to the variable `message`, change the text `Hello` to `Good morning`\.  
![\[GitHub file page with edit icon and message highlighted\]](http://docs.aws.amazon.com/apprunner/latest/dg/images/getting-started-deploy-edit.png)

1. Choose **Commit changes**\.

   The new commit starts to deploy\. On the service dashboard page, the service **Status** changes to **Operation in progress**\.

1. Wait for the deployment to end\. On the service dashboard page, the service **Status** should change back to **Running**\.

1. Verify that the deployment is successful: refresh the browser tab where the webpage of your service is displayed\.

   The page now displays the modified message: **Good morning, *your name*\!**

## Step 3: Make a configuration change<a name="getting-started.config"></a>

In this step, you make a change to the **NAME** environment variable value, to demonstrate a service configuration change\.

**To view logs for your service**

1. Open the [App Runner console](https://console.aws.amazon.com/apprunner), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Services**, and then choose your App Runner service\.

   The console displays the service dashboard with a **Service overview**\.  
![\[App Runner service dashboard page showing Activity list\]](http://docs.aws.amazon.com/apprunner/latest/dg/images/console-dashboard.png)

1. On the service dashboard page, choose the **Configuration** tab\.

   The console displays your service configuration settings in several sections\.

1. In the **Configure service** section, choose **Edit**\.  
![\[The Service configuration section of the Configuration tab on the App Runner service dashboard page\]](http://docs.aws.amazon.com/apprunner/latest/dg/images/service-dashboad-config-service.png)

1. For the environment variable with the key **NAME**, change the value to a different name\.

1. Choose **Apply changes**\.

   App Runner starts the update process\. On the service dashboard page, the service **Status** changes to **Operation in progress**\.

1. Wait for the update to end\. On the service dashboard page, the service **Status** should change back to **Running**\.

1. Verify that the update is successful: refresh the browser tab where the webpage of your service is displayed\.

   The page now displays the modified name: **Good morning, *new name*\!**

## Step 4: View logs for your service<a name="getting-started.logs"></a>

In this step, you use the App Runner console to view logs for your App Runner service\. App Runner streams logs to Amazon CloudWatch Logs \(CloudWatch Logs\) and displays them on your service's dashboard\. For information about App Runner logs, see [Viewing App Runner logs streamed to CloudWatch Logs](monitor-cwl.md)\.

**To view logs for your service**

1. Open the [App Runner console](https://console.aws.amazon.com/apprunner), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Services**, and then choose your App Runner service\.

   The console displays the service dashboard with a **Service overview**\.  
![\[App Runner service dashboard page showing Activity list\]](http://docs.aws.amazon.com/apprunner/latest/dg/images/console-dashboard.png)

1. On the service dashboard page, choose the **Logs** tab\.

   The console displays a few types of logs in several sections:
   + **Event log** – Activity in the lifecycle of your App Runner service\. The console displays the latest events\.
   + **Deployment logs** – Source repository deployments to your App Runner service\. The console displays a separate log stream for each deployment\.
   + **Application logs** – The output of the web application that's deployed to your App Runner service\. The console combines the output from all running instances into a single log stream\.  
![\[The Logs tab on the App Runner service dashboard page\]](http://docs.aws.amazon.com/apprunner/latest/dg/images/service-dashboad-logs.png)

1. To find specific deployments, scope down the deployment log list by entering a search term\. You can search for any value that appears in the table\.

1. To view a log's content, choose **View full log** \(event log\) or the log stream name \(deployment and application logs\)\.

1. Choose **Download** to download a log\. For a deployment log stream, select a log stream first\.

1. Choose **View in CloudWatch** to open the CloudWatch console and use its full capabilities to explore your App Runner service logs\. For a deployment log stream, select a log stream first\.
**Note**  
The CloudWatch console is particularly useful if you want to view application logs of specific instances instead of the combined application log\.

## Step 5: Clean up<a name="getting-started.cleanup"></a>

You've now learned how to create an App Runner service, view logs, and make some changes\. In this step, you delete the service to remove resources that you don't need anymore\.

**To delete your service**

1. On the service dashboard page, choose **Actions**, and then choose **Delete service**\.

1. In the confirmation dialog, enter the requested text, and then choose **Delete**\.

   Result: The console navigates to the **Services** page\. The service that you just deleted shows a status of **DELETING**\. A short time later it disappears from the list\.

Consider also deleting the GitHub connection that you created as part of this tutorial\. For more information, see [Managing App Runner connections](manage-connections.md)\.

## What's next<a name="getting-started.next"></a>

Now that you've deployed your first App Runner service, learn more in the following topics:
+ [App Runner architecture and concepts](architecture.md) – The architecture, main concepts, and AWS resources related to App Runner\.
+ [Image\-based service](service-source-image.md) and [Code\-based service](service-source-code.md) – The two types of application source that App Runner can deploy\.
+ [Developing application code for App Runner](develop.md) – Things you should know when developing or migrating application code for deployment to App Runner\.
+ [Using the App Runner console](console.md) – Manage and monitor your service using the App Runner console\.
+ [Managing your App Runner service](manage.md) – Manage the lifecycle of your App Runner service\.
+ [Logging and monitoring for your App Runner service](monitor.md) – Monitor your App Runner service by viewing metrics, reading logs, and tracking service action calls\.
+ [App Runner configuration file](config-file.md) – A configuration\-based way to specify options for the build and runtime behavior of your App Runner service\.
+ [The App Runner API](api.md) – Use the App Runner application programming interface \(API\) to create, read, update, and delete App Runner resources\.
+ [Security in App Runner](security.md) – The different ways that AWS and you ensure cloud security while you use App Runner and other services\.