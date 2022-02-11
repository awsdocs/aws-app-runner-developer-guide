# Pausing and resuming an App Runner service<a name="manage-pause"></a>

If you need to disable your web application temporarily and stop the code from running, you can pause your AWS App Runner service\. App Runner reduces the compute capacity for the service to zero\.

When you're ready to run your application again, you can resume your App Runner service\. App Runner provisions new compute capacity, deploys your application to it, and runs the application\. Your application source isn't redeployed, and no build is necessary\. Rather, App Runner resumes with your currently deployed version\. Your application retains its App Runner domain\.

**Important**  
When you pause your service, your application loses its state\. For example, any ephemeral storage that your code used is lost\. For your code, pausing and resuming your service is the equivalent of deploying to a new service\.
If you pause a service due to a flaw in your code \(for example, a discovered bug or security issue\), you can't deploy a new version before resuming the service\.  
Therefore, we recommend that you keep the service running and roll back to your last stable application version instead\.
When you resume your service, App Runner deploys the last application version that was used before you paused the service\. If you added any new source versions since pausing your service, App Runner doesn't automatically deploy them even if automatic deployment is selected\. For example, assume you have new image versions in the image repository or new commits in the code repository\. These versions aren't automatically deployed \.  
To deploy a newer version, perform a manual deployment or add another version to your source repository after resuming your App Runner service\.

## Pausing and deleting compared<a name="manage-pause.pause-vs-delete"></a>

*Pause* your App Runner service to *temporarily* disable it\. Only compute resources are terminated, and your stored data \(for example, the container image with your application version\) remains intact\. Resuming your service is quick—your application is ready to be deployed to new compute resources\. Your App Runner domain remains the same\.

*Delete* your App Runner service to *permanently* remove it\. Your stored data is deleted\. If you need to recreate the service, App Runner needs to fetch your source again, and also to build it if it's a code repository\. Your web application gets a new App Runner domain\.

## When your service is paused<a name="manage-pause.paused"></a>

When you pause your service and it's in the **Paused** status, it responds differently to action requests, including API calls or console operations\. When a service is paused, you can still perform App Runner actions that don't modify the definition or configuration of the service in a way that affects its runtime\. In other words, if an action changes the behavior, scale, or other characteristics of a running service, you cannot perform that action on a paused service\.

The following lists provide information about API actions that you can and cannot perform on a paused service\. The equivalent console operations are similarly allowed or denied\.

**Actions you *can* perform on a paused service**
+ *`List*` and `Describe*` actions* – Actions that only read information\.
+ *`DeleteService`* – You can always delete a service\.
+ *`TagResource`, `UntagResource`* – Tags are associated with a service, but aren't part of its definition and don't affect its runtime behavior\.

**Actions you *cannot* perform on a paused service**
+ *`StartDeployment` actions* \(or a [manual deployment](manage-deploy.md#manage-deploy.manual) using the console\)
+ *`UpdateService`* \(or a configuration change using the console, except for tagging changes\)
+ *`CreateCustomDomainAssociations`, `DeleteCustomDomainAssociations`*
+ *`CreateConnection`, `DeleteConnection`*

## Pause and resume your service<a name="manage-pause.manage"></a>

Pause and resume your App Runner service using one of the following methods:

------
#### [ App Runner console ]

**To pause your service using the App Runner console**

1. Open the [App Runner console](https://console.aws.amazon.com/apprunner), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Services**, and then choose your App Runner service\.

   The console displays the service dashboard with a **Service overview**\.  
![\[App Runner service dashboard page showing Activity list\]](http://docs.aws.amazon.com/apprunner/latest/dg/images/console-dashboard.png)

1. Choose **Actions**, and then choose **Pause**\.

   On the service dashboard page, the service **Status** changes to **Operation in progress**, and then changes to **Paused**\. Your service is now paused\.

**To resume your service using the App Runner console**

1. Choose **Actions**, and then choose **Resume**\.

   On the service dashboard page, the service **Status** changes to **Operation in progress**\.

1. Wait for the service to resume\. On the service dashboard page, the service **Status** changes back to **Running**\.

1. To verify that resuming the service is successful, on the service dashboard page, choose the **App Runner domain** value\. It's the URL for your service's website\. Verify that your web application is running correctly\.

------
#### [ App Runner API or AWS CLI ]

To pause your service using the App Runner API or AWS CLI, call the [PauseService](https://docs.aws.amazon.com/apprunner/latest/api/API_PauseService.html) API action\. If the call returns a successful response with a [Service](https://docs.aws.amazon.com/apprunner/latest/api/API_Service.html) object showing `"Status": "OPERATION_IN_PROGRESS"`, App Runner starts pausing your service\.

To resume your service using the App Runner API or AWS CLI, call the [ResumeService](https://docs.aws.amazon.com/apprunner/latest/api/API_ResumeService.html) API action\. If the call returns a successful response with a [Service](https://docs.aws.amazon.com/apprunner/latest/api/API_Service.html) object showing `"Status": "OPERATION_IN_PROGRESS"`, App Runner starts resuming your service\.

------