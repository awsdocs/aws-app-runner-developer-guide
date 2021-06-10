# App Runner service based on source code<a name="service-source-code"></a>

You can use AWS App Runner to create and manage services based on two fundamentally different types of service source: *source code* and *source image*\. Regardless of the source type, App Runner takes care of starting, running, scaling, and load balancing your service\. You can use the CI/CD capability of App Runner to track changes to your source image or code\. When App Runner discovers a change, it automatically builds \(for source code\) and deploys the new version to your App Runner service\.

This chapter discusses services based on source code\. For information about services based on a source image, see [App Runner service based on a source image](service-source-image.md)\.

Source code is application code that App Runner builds and deploys for you\. You point App Runner to a source code repository and choose a suitable *runtime*\. App Runner builds an image that's based on the base image of the runtime and your application code\. It then starts a service that runs a container based on this image\.

App Runner provides convenient language\-specific *managed runtimes*\. Each one of these runtimes builds a container image from your source code, and adds language runtime dependencies into your image\. You don't need to provide container configuration and build instructions such as a Dockerfile\.

Subtopics of this chapter discuss the various *runtimes* that App Runner supports—the generic Dockerfile runtime, and the *managed runtimes* for different programming environments\.

**Topics**
+ [Source code repository providers](#service-source-code.providers)
+ [App Runner managed runtimes](#service-source-code.managed-runtimes)
+ [Using the Python managed runtime](service-source-code-python.md)
+ [Using the Node\.js managed runtime](service-source-code-nodejs.md)

## Source code repository providers<a name="service-source-code.providers"></a>

App Runner deploys your source code by reading it from a source code repository\. App Runner supports one source code repository provider: [GitHub](https://github.com/)\.

### Deploying from GitHub<a name="service-source-code.providers.github"></a>

To deploy your source code to an App Runner service from a [GitHub](https://github.com/) repository, App Runner establishes a connection to GitHub\. If your repository is private \(that is, it isn't publicly accessible on GitHub\), you must provide App Runner with connection details\. When you use the App Runner console to [create a service](manage-create.md), you provide connection details as part of the service creation procedure\.

When you use the App Runner API or the AWS CLI, a connection is a separate resource\. First, you create the connection using the [CreateConnection](https://docs.aws.amazon.com/apprunner/latest/api/API_CreateConnection.html) API action\. Then, you provide the connection's ARN during service creation using the [CreateService](https://docs.aws.amazon.com/apprunner/latest/api/API_CreateService.html) API action\.

For more information about App Runner service creation, see [Creating an App Runner service](manage-create.md)\. For more information about App Runner connections, see [Managing App Runner connections](manage-connections.md)\.

## App Runner managed runtimes<a name="service-source-code.managed-runtimes"></a>

App Runner provides managed runtimes for various programming environments\. Each managed runtime makes it easy to build and run containers based on a particular programming language or runtime environment\. When you use a managed runtime, App Runner starts with a managed runtime image\. This image is based on the [Amazon Linux Docker image](https://hub.docker.com/_/amazonlinux) and contains a language runtime package as well as some tools and popular dependency packages\. App Runner uses this managed runtime image as a base image, and adds your application code to build a Docker image\. It then deploys this image to run your web service in a container\.

 You specify a runtime for your App Runner service when you [create a service](manage-create.md) using the App Runner console or the [CreateService](https://docs.aws.amazon.com/apprunner/latest/api/API_CreateService.html) API\. You can also specify a runtime as part of your source code\. Use the `runtime` keyword in a [App Runner configuration file](config-file.md) that you include in your code repository\. The naming convention of a managed runtime is *<language\-name><major\-version>*\. 

App Runner updates the runtime for your service to the latest version on every deployment or service update\. If your application requires a specific version of a managed runtime, you can specify it using the `runtime-version` keyword in the [App Runner configuration file](config-file.md)\. Specify a minor version as *<major>*\.*<minor>* to lock the major and minor versions \(App Runner updates only patch versions\)\. Specify a particular patch level as *<major>*\.*<minor>*\.*<patch>* to lock your service on a specific runtime version \(App Runner never updates the runtime\)\.