# App Runner service based on a source image<a name="service-source-image"></a>

You can use AWS App Runner to create and manage services based on two fundamentally different types of service source: *source code* and *source image*\. Regardless of the source type, App Runner takes care of starting, running, scaling, and load balancing your service\. You can use the CI/CD capability of App Runner to track changes to your source image or code\. When App Runner discovers a change, it automatically builds \(for source code\) and deploys the new version to your App Runner service\.

This chapter discusses services based on a source image\. For information about services based on source code, see [App Runner service based on source code](service-source-code.md)\.

A *source image* is a public or private container image stored in an image repository\. You point App Runner to an image, and it starts a service running a container based on this image\. No build stage is necessary\. Rather, you provide a ready\-to\-deploy image\.

## Image repository providers<a name="service-source-image.providers"></a>

App Runner supports the following image repository providers:
+ **Amazon Elastic Container Registry \(Amazon ECR\)** – Stores private images in your AWS account\.
+ **Amazon Elastic Container Registry Public \(Amazon ECR Public\)** – Stores publicly readable images\.

### Deploying from Amazon ECR<a name="service-source-image.providers.ecr"></a>

[Amazon ECR](https://docs.aws.amazon.com/AmazonECR/latest/userguide/) stores images in repositories\. There are private and public repositories\. To deploy your image to an App Runner service from a private repository, App Runner needs permission to read your image from Amazon ECR\. To give that permission to App Runner, you need to provide App Runner with an *access role*\. This is an AWS Identity and Access Management \(IAM\) role that has the necessary Amazon ECR action permissions\. When you use the App Runner console to create the service, you can choose an existing role in your account\. Alternatively, you can use the IAM console to create a new custom role, or choose for the App Runner console to create a role for you based on managed policies\.

When you use the App Runner API or the AWS CLI, you complete a two\-step process\. First, you use the IAM console to create an access role\. You can use a managed policy that App Runner provides or enter your own custom permissions\. Then, you provide the access role during service creation using the [CreateService](https://docs.aws.amazon.com/apprunner/latest/api/API_CreateService.html) API action\.

For information about App Runner service creation, see [Creating an App Runner service](manage-create.md)\.

### Deploying from Amazon ECR Public<a name="service-source-image.providers.ecrpublic"></a>

[Amazon ECR Public](https://docs.aws.amazon.com/AmazonECR/latest/public/) stores publicly readable images\. These are the main differences between Amazon ECR and Amazon ECR Public that you should be aware of in the context of App Runner services:
+ Amazon ECR Public images are publicly readable\. You don't need to provide an access role when you create a service based on an Amazon ECR Public image\.
+ App Runner doesn't support automatic deployment for Amazon ECR Public images\.