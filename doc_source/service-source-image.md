# App Runner service based on a source image<a name="service-source-image"></a>

You can use AWS App Runner to create and manage services based on two fundamentally different types of service source: *source code* and *source image*\. Regardless of the source type, App Runner takes care of starting, running, scaling, and load balancing your service\. You can use the CI/CD capability of App Runner to track changes to your source image or code\. When App Runner discovers a change, it automatically builds \(for source code\) and deploys the new version to your App Runner service\.

This chapter discusses services based on a source image\. For information about services based on source code, see [App Runner service based on source code](service-source-code.md)\.

A *source image* is a public or private container image stored in an image repository\. You point App Runner to an image, and it starts a service running a container based on this image\. No build stage is necessary\. Rather, you provide a ready\-to\-deploy image\.

## Image repository providers<a name="service-source-image.providers"></a>

App Runner supports the following image repository providers:
+ **Amazon Elastic Container Registry \(Amazon ECR\)** – Stores images that are private to an AWS account\.
+ **Amazon Elastic Container Registry Public \(Amazon ECR Public\)** – Stores images that are publicly readable\.

**Topics**
+ [Using an image stored in Amazon ECR *in your AWS account*](#service-source-image.providers.ecr)
+ [Using an image stored in Amazon ECR *in a different AWS account*](#service-source-image.providers.ecr-cross)
+ [Using an image stored in Amazon ECR Public](#service-source-image.providers.ecrpublic)

### Using an image stored in Amazon ECR *in your AWS account*<a name="service-source-image.providers.ecr"></a>

[Amazon ECR](https://docs.aws.amazon.com/AmazonECR/latest/userguide/) stores images in repositories\. There are private and public repositories\. To deploy your image to an App Runner service from a private repository, App Runner needs permission to read your image from Amazon ECR\. To give that permission to App Runner, you need to provide App Runner with an *access role*\. This is an AWS Identity and Access Management \(IAM\) role that has the necessary Amazon ECR action permissions\. When you use the App Runner console to create the service, you can choose an existing role in your account\. Alternatively, you can use the IAM console to create a new custom role\. Or, you can choose for the App Runner console to create a role for you based on managed policies\.

When you use the App Runner API or the AWS CLI, you complete a two\-step process\. First, you use the IAM console to create an access role\. You can use a managed policy that App Runner provides or enter your own custom permissions\. Then, you provide the access role during service creation using the [CreateService](https://docs.aws.amazon.com/apprunner/latest/api/API_CreateService.html) API action\.

For information about App Runner service creation, see [Creating an App Runner service](manage-create.md)\.

### Using an image stored in Amazon ECR *in a different AWS account*<a name="service-source-image.providers.ecr-cross"></a>

When you create an App Runner service, you can use an image stored in an Amazon ECR repository that belongs to an AWS account other than the one that your service is in\. There are a few additional considerations to keep in mind when using a cross\-account image, in addition to those listed in the previous section about a same\-account image\.
+ The cross\-account repository should have a policy attached to it\. The repository policy provides your access role with permissions to read images in the repository\. Use the following policy for this purpose\. Replace `access-role-arn` with the Amazon Resource Name \(ARN\) of your access role\.

  ```
  {
    "Version": "2008-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Principal": {
          "AWS": "access-role-arn"
        },
        "Action": [
          "ecr:BatchGetImage",
          "ecr:DescribeImages",
          "ecr:GetDownloadUrlForLayer"
        ]
      }
    ]
  }
  ```

  For information about attaching a repository policy to an Amazon ECR repository, see [Setting a repository policy statement](https://docs.aws.amazon.com/AmazonECR/latest/userguide/set-repository-policy.html) in the *Amazon Elastic Container Registry User Guide*\.
+ App Runner doesn't support automatic deployment for Amazon ECR images in a different account than the one that your service is in\.

### Using an image stored in Amazon ECR Public<a name="service-source-image.providers.ecrpublic"></a>

[Amazon ECR Public](https://docs.aws.amazon.com/AmazonECR/latest/public/) stores publicly readable images\. These are the main differences between Amazon ECR and Amazon ECR Public that you should be aware of in the context of App Runner services:
+ Amazon ECR Public images are publicly readable\. You don't need to provide an access role when you create a service based on an Amazon ECR Public image\. The repository doesn't need any policy attached to it\.
+ App Runner doesn't support automatic \(continuous\) deployment for Amazon ECR Public images\.

#### Launch a service directly from Amazon ECR Public<a name="service-source-image.providers.ecrpublic.direct"></a>

You can directly launch container images of compatible web applications that are hosted on the [Amazon ECR Public Gallery](https://gallery.ecr.aws) as web services running on App Runner\. When browsing the gallery, look for **Launch with App Runner** on the gallery page for an image\. An image with this option is compatible with App Runner\. For more information about the gallery, see [Using the Amazon ECR Public Gallery](https://docs.aws.amazon.com/AmazonECR/latest/public/public-gallery.html) in the *Amazon ECR Public user guide*\.

![\[Amazon ECR Public Gallery showing a container image page with a Launch with App Runner button\]](http://docs.aws.amazon.com/apprunner/latest/dg/images/ecr-gallery-image-launch.png)

**To launch a gallery image as an App Runner service**

1. On the gallery page of an image, choose **Launch with App Runner**\.

   Result: The App Runner console opens in a new browser tab\. The console displays the **Create service** wizard, with most of the required new service details pre\-filled\.

1. If you want to create your service in an AWS Region other than the one that the console is showing, choose the Region displayed on the console header\. Then, select another Region\.

1. For **Port**, enter the port number that the image application listens on\. You can typically find it on the gallery page for the image\.

1. Optionally, change any other configuration details\.

1. Choose **Next**, review the settings, and then choose **Create & deploy**\.

## Image example<a name="service-source-image.example"></a>

The App Runner team maintains the **hello\-app\-runner** example image in an Amazon ECR Public Gallery\. You can use this example to get started with creating an image\-based App Runner service\. For more information, see [hello\-app\-runner](https://gallery.ecr.aws/aws-containers/hello-app-runner)\.