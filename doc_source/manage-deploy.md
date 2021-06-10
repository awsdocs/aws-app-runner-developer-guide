# Deploying a new application version to App Runner<a name="manage-deploy"></a>

When you [create a service](manage-create.md) in AWS App Runner, you configure an application source—a container image or a source repository\. App Runner provisions resources to run your service and deploys your application to them\.

This topic describes ways to redeploy your application source to your App Runner service when a new version becomes available\. This can be a new image version in the image repository or a new commit in the code repository\. App Runner provides two methods to deploy to a service: *automatic* and *manual*\.

## Deployment methods<a name="manage-deploy.methods"></a>

App Runner provides the following methods for you to control how application deployments are initiated\.

**Automatic deployment**  
Use automatic deployment when you want continuous integration and deployment \(CICD\) behavior for your service\. App Runner monitors your image or code repository\. Whenever you push a new image version to your image repository, or a new commit to your code repository, App Runner automatically deploys it to your service without further action on your side\.

**Manual deployment**  
Use manual deployment when you want to explicitly initiate each deployment to your service\. You initiate a deployment if the repository that you configured for your service has a new version that you want to deploy\. For more information, see [Manual deployment](#manage-deploy.manual)\.

You can configure the deployment method for your service in the following ways:
+ *Console* – For a new service you're creating or for an existing service, in the **Deployment settings** section of the **Source and deployment** configuration page, choose **Manual** or **Automatic**\.  
![\[App Runner deployment method configuration\]](http://docs.aws.amazon.com/apprunner/latest/dg/images/manage-deploy.methods.config.png)
+ *API or AWS CLI* – In a call to either the [CreateService](https://docs.aws.amazon.com/apprunner/latest/api/API_CreateService.html) or [UpdateService](https://docs.aws.amazon.com/apprunner/latest/api/API_UpdateService.html) action, set the `AutoDeploymentsEnabled` member of the [SourceConfiguration](https://docs.aws.amazon.com/apprunner/latest/api/API_SourceConfiguration.html) parameter to `False` for manual deployment or `True` for automatic deployment\.

## Manual deployment<a name="manage-deploy.manual"></a>

With manual deployment, you need to explicitly initiate each deployment to your service\. When you have a new version of your application image or code ready to deploy, you can refer to the following sections to learn how to perform a deployment using the console and the API\.

### Deploy an application version using the App Runner console<a name="manage-deploy.manual.console"></a>

**To deploy using the App Runner console**

1. Open the [App Runner console](https://console.aws.amazon.com/apprunner), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Services**, and then choose your App Runner service\.

   The console displays the service dashboard with a **Service overview**\.

1. Choose **Deploy**\.

   Result: Deployment of the new version starts\. On the service dashboard page, the service **Status** changes to **Operation in progress**\.

1. Wait for the deployment to end\. On the service dashboard page, the service **Status** should change back to **Running**\.

1. To verify that the deployment is successful, on the service dashboard page, choose the **Default domain** value—it's the URL to your service's website\. Inspect or interact with your web application and verify your version change\.

### Deploy an application version using the App Runner API or AWS CLI<a name="manage-deploy.manual.api"></a>

To deploy using the App Runner API or AWS CLI, call the [StartDeployment](https://docs.aws.amazon.com/apprunner/latest/api/API_StartDeployment.html) API action\. The only parameter to pass is your service ARN\. You already configured your application source location when you created the service, and App Runner can find the new version\. Your deployment starts if the call returns a successful response\.