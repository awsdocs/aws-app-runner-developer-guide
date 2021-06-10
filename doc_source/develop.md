# Developing application code for App Runner<a name="develop"></a>

This chapter discusses runtime information and development guidelines that you should consider when developing or migrating application code for deployment to AWS App Runner\.

## Runtime information<a name="develop.considerations"></a>

Whether you provide a container image or App Runner builds one for you, App Runner runs your application code in a container instance\. Here are a few key aspects of the container instance runtime environment\.
+ **Framework support** – App Runner supports any image that implements a web application\. It's agnostic to the programming language that you choose and to the web application server or framework that you use, if you use any\. For your convenience, we provide language\-specific managed runtimes to streamline the application build process and abstract image creation\.
+ **Web requests** – Your container instance must listen to HTTP requests, on port 8080 by default\. For more information about configuring your service, see [Configuring an App Runner service](manage-configure.md)\. You don't need to implement handling of HTTPS secure traffic\. App Runner requires incoming HTTPS traffic and terminates HTTPS before passing requests to your container instance\.
+ **Stateless apps** – App Runner doesn't guarantee state persistence beyond the duration of processing a single incoming web request\.
+ **Storage** – App Runner implements the file system in your container instance as *ephemeral storage*\. Files are transient\. For example, they don't persist when you pause and resume your App Runner service\. More generally, files aren't guaranteed to persist beyond the processing of a single request, as part of the stateless nature of your application\. Stored files do, however, take up part of the storage allocation of your App Runner service for the duration of their lifespan\.
**Note**  
Although ephemeral storage files might not persist across requests, they *sometimes* do persist\. This can be useful in certain situations\. For example, when handling a request, you can cache files that your application downloads if future requests might need them\. This might speed up future request handling, but can't guarantee the speed gains\. Your code shouldn't assume that a file that has been downloaded in a previous request still exists\.  
For guaranteed caching using a high throughput, low latency in\-memory data store, use a service such as [Amazon ElastiCache](https://aws.amazon.com/elasticache/)\.
+ **Environment variables** – By default, App Runner makes the `PORT` environment variable available in your container instance\. You can configure the variable value with port information, and add custom environment variables and values\. For more information about configuring your service, see [Configuring an App Runner service](manage-configure.md)\.
+ **Instance role** – If your application code makes calls to any AWS services, using the service APIs or one of the AWS SDKs, create an instance role using AWS Identity and Access Management \(IAM\)\. Then, attach it to your App Runner service when you create it\. Include all AWS service action permissions that your code requires in your instance role\. For more information, see [Instance role](security_iam_service-with-iam.md#security_iam_service-with-iam-roles-service.instance)\.

## Code development guidelines<a name="develop.tips"></a>

Consider these guidelines when developing code for an App Runner web application\.
+ **Design stateless code** – Design the web application you deploy to your App Runner service to be stateless\. Your code should assume that no state persists beyond the duration of processing a single incoming web request\.
+ **Delete temporary files** – When you create files, they're stored on a file system, and take up part of the storage allocation of your service\. To avoid out\-of\-storage errors, don't keep temporary files for extended periods\. Balance storage size with request handling speed when making file caching decisions\.