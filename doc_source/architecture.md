# App Runner architecture and concepts<a name="architecture"></a>

AWS App Runner takes your source code or source image from a repository, and creates and maintains a running web service for you in the AWS Cloud\. Typically, you need to call just one App Runner action, [CreateService](https://docs.aws.amazon.com/apprunner/latest/api/API_CreateService.html), to create your service\.

With a source image repository, you provide a ready\-to\-use container image that App Runner can deploy to run your web service\. With a source code repository, you can provide code and instructions for building and running a web service designed for one of several runtime environments managed by App Runner\. 

At this time, App Runner can retrieve your source code from a [GitHub](https://github.com/) repository, or retrieve your source image from [Amazon Elastic Container Registry \(Amazon ECR\)](https://docs.aws.amazon.com/AmazonECR/latest/userguide/) in your AWS account\.

The following diagram shows an overview of the App Runner service architecture\. In the diagram, there are two example services: one deploys source code from GitHub, and the other deploys a source image from Amazon ECR\.

![\[The App Runner architecture\]](http://docs.aws.amazon.com/apprunner/latest/dg/images/architecture.png)

## App Runner concepts<a name="architecture.concepts"></a>

The following are key concepts related to your web service that's running in App Runner:
+ *App Runner service* – An AWS resource that App Runner uses to deploy and manage your application based on its source code repository or container image\. An App Runner service is a running version of your application\. For more information about creating a service, see [Creating an App Runner service](manage-create.md)\.
+ *Source type* – The type of source repository that you provide for deploying your App Runner service: [source code](service-source-code.md) or [source image](service-source-image.md)\.
+ *Repository provider* – The repository service that contains your application source \(for example, [GitHub](service-source-code.md#service-source-code.providers.github) or [Amazon ECR](service-source-image.md#service-source-image.providers.ecr)\)\.
+ *App Runner connection* – An AWS resource that lets App Runner access a repository provider account \(for example, a GitHub account or organization\)\. For more information about connections, see [Managing App Runner connections](manage-connections.md)\.
+ *Runtime* – A base image for deploying a source code repository\. App Runner provides a variety of *managed runtimes* for different programming environments\. For more information, see [App Runner service based on source code](service-source-code.md)\.
+ *Deployment* – An action that applies a version of your source repository \(code or image\) to an App Runner service\. The first deployment to the service occurs as part of service creation\. Later deployments can occur in one of two ways:
  + *Automatic deployment* – A CI/CD capability\. You can configure an App Runner service to automatically build \(for source code\) and deploy each version of your application as it appears in the repository\. This can be a new commit in a source code repository or a new image version in a source image repository\.
  + *Manual deployment* – A deployment to your App Runner service that you explicitly start\.
+ *Custom domain* – A domain that you associate with your App Runner service\. Users of your web application can use this domain to access your web service instead of the default App Runner subdomain\. For more information, see [Managing custom domain names for an App Runner service](manage-custom-domains.md)\.
+ *Maintenance* – An activity that App Runner occasionally performs on the infrastructure that runs your App Runner service\. When maintenance is in progress, service status temporarily changes to `OPERATION_IN_PROGRESS` \(**Operation in progress** in the console\) for a few minutes\. Actions on your service \(for example, deployment, configuration update, pause/resume, or deletion\) are blocked during this time\. Try the action again a few minutes later, when the service status returns to `RUNNING`\.
**Note**  
If your action fails, it doesn't mean that your App Runner service is down\. Your application is active and keeps handling requests\. It's unlikely for your service to experience any downtime\.

  In particular, App Runner migrates your service if it detects issues in the underlying hardware hosting the service\. To prevent any service downtime, App Runner deploys your service to a new set of instances and shifts traffic to them \(a blue\-green deployment\)\. You might occasionally see a slight temporary increase in charges\.

## App Runner resources<a name="architecture.resources"></a>

When you use App Runner, you create and manage a few types of resources in your AWS account\. These resources are used to access your code and manage your services\.

The following table provides an overview of these resources:


|  **Resource name**  |  **Description**  | 
| --- | --- | 
|  Service  |  Represents a running version of your application\. Much of the rest of this guide describes service types, management, configuration, and monitoring\. ARN: `arn:aws:apprunner:region:account-id:service/service-name[/service-id]`  | 
|  Connection  |  Provides your App Runner services with access to private repositories stored with third\-party providers\. Exists as a separate resource for sharing across multiple services\. For more information about connections, see [Managing App Runner connections](manage-connections.md)\. ARN: `arn:aws:apprunner:region:account-id:connection/connection-name[/connection-id]`  | 
|  AutoScalingConfiguration  |  Provides your App Runner services with settings that control the automatic scaling of your application\. Exists as a separate resource for sharing across multiple services\. For more information about automatic scaling, see [Managing App Runner automatic scaling](manage-autoscaling.md)\. ARN: `arn:aws:apprunner:region:account-id:autoscalingconfiguration/config-name[/config-revision[/config-id]]`  | 

## App Runner resource quotas<a name="architecture.quotas"></a>

AWS imposes some quotas \(also known as limits\) on your account for AWS resource usage in each AWS Region\. The following table lists quotas related to App Runner resources\. Quotas are also listed in [AWS App Runner endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/apprunner.html) in the *AWS General Reference*\.


|  **Resource quota**  |  **Description**  |  **Default value**  |  **Adjustable?**  | 
| --- | --- | --- | --- | 
|  Services  |  The maximum number of services that you can create in your account for each AWS Region\.  |  10  |   ✓ Yes  | 
|  Connections  |  The maximum number of connections that you can create in your account for each AWS Region\. You can share a single connection across multiple services\.  |  10  |   ✓ Yes  | 
|  Auto scaling configurations—names  |  The maximum number of unique names that you can have in auto scaling configurations that you create in your account for each AWS Region\. You can use an auto scaling configuration in multiple services\.  |  10  |   ✓ Yes  | 
|  Auto scaling configurations—revisions for each name  |  The maximum number of auto scaling configuration revisions that you can create in your account for each AWS Region for each unique name\. You can use an auto scaling configuration revision in multiple services\.  |  10  |   ☓ No  | 

Most quotas are adjustable, and you can request a quota increase for them\. For more information, see [Requesting a quota increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) in the Service Quotas User Guide\.