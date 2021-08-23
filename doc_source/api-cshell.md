# Using AWS CloudShell to work with AWS App Runner<a name="api-cshell"></a>

AWS CloudShell is a browser\-based, pre\-authenticated shell that you can launch directly from the AWS Management Console\. You can run AWS CLI commands against AWS services \(including AWS App Runner\) using your preferred shell \(Bash, PowerShell or Z shell\)\. And you can do this without needing to download or install command line tools\.

You [launch AWS CloudShell from the AWS Management Console](https://docs.aws.amazon.com/cloudshell/latest/userguide/working-with-cloudshell.html#launch-options), and the AWS credentials you used to sign in to the console are automatically available in a new shell session\. This pre\-authentication of AWS CloudShell users allows you to skip configuring credentials when interacting with AWS services such as App Runner using AWS CLI version 2 \(pre\-installed on the shell's compute environment\)\.

**Topics**
+ [Obtaining IAM permissions for AWS CloudShell](#api-cshell.permissions)
+ [Interacting with App Runner using AWS CloudShell](#api-cshell.call-apprunner)
+ [Verifying your App Runner service using AWS CloudShell](#api-cshell.call-your-service)

## Obtaining IAM permissions for AWS CloudShell<a name="api-cshell.permissions"></a>

Using the access management resources provided by AWS Identity and Access Management, administrators can grant permissions to IAM users so they can access AWS CloudShell and use the environment's features\.

The quickest way for an administrator to grant access to users is through an AWS managed policy\. An [AWS managed policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) is a standalone policy that's created and administered by AWS\. The following AWS managed policy for CloudShell can be attached to IAM identities:
+ `AWSCloudShellFullAccess`: Grants permission to use AWS CloudShell with full access to all features\.

 If you want to limit the scope of actions that an IAM user can perform with AWS CloudShell, you can create a custom policy that uses the `AWSCloudShellFullAccess` managed policy as a template\. For more information about limiting the actions that are available to users in CloudShell, see [Managing AWS CloudShell access and usage with IAM policies](https://docs.aws.amazon.com/cloudshell/latest/userguide/sec-auth-with-identities.html) in the *AWS CloudShell User Guide*\.

**Note**  
Your IAM identity also requires a policy that grants permission to make calls to App Runner\. For more information, see [How App Runner works with IAM](security_iam_service-with-iam.md)\.

## Interacting with App Runner using AWS CloudShell<a name="api-cshell.call-apprunner"></a>

After you launch AWS CloudShell from the AWS Management Console, you can immediately start to interact with App Runner using the command line interface\. 

In the following example, you retrieve information about one of your App Runner services using the AWS CLI in CloudShell\.

**Note**  
When using AWS CLI in AWS CloudShell, you don't need to download or install any additional resources\. Moreover, because you're already authenticated within the shell, you don't need to configure credentials before making calls\.

**Example Retrieving App Runner service information using AWS CloudShell**  

1. From the AWS Management Console, you can launch CloudShell by choosing the following options available on the navigation bar:
   +  Choose the CloudShell icon\. 
   + Start typing **cloudshell** in the search box, and then choose the **CloudShell** option when you see it in the search results\.

1. To list all current App Runner services in your AWS account in the console session's AWS Region, enter the following command in the CloudShell command line:

   ```
   $ aws apprunner list-services
   ```

   The output lists summary information for your services\.

   ```
   {
     "ServiceSummaryList": [
       {
         "ServiceName": "my-app-1",
         "ServiceId": "8fe1e10304f84fd2b0df550fe98a71fa",
         "ServiceArn": "arn:aws:apprunner:us-east-2:123456789012:service/my-app-1/8fe1e10304f84fd2b0df550fe98a71fa",
         "ServiceUrl": "psbqam834h.us-east-1.awsapprunner.com",
         "CreatedAt": "2020-11-20T19:05:25Z",
         "UpdatedAt": "2020-11-23T12:41:37Z",
         "Status": "RUNNING"
       },
       {
         "ServiceName": "my-app-2",
         "ServiceId": "ab8f94cfe29a460fb8760afd2ee87555",
         "ServiceArn": "arn:aws:apprunner:us-east-2:123456789012:service/my-app-2/ab8f94cfe29a460fb8760afd2ee87555",
         "ServiceUrl": "e2m8rrrx33.us-east-1.awsapprunner.com",
         "CreatedAt": "2020-11-06T23:15:30Z",
         "UpdatedAt": "2020-11-23T13:21:22Z",
         "Status": "RUNNING"
       }
     ]
   }
   ```

1. To get a detailed description of a particular App Runner service, enter the following command in the CloudShell command line, using one of the ARNs retrieved in the previous step:

   ```
   $ aws apprunner describe-service --service-arn arn:aws:apprunner:us-east-2:123456789012:service/my-app-1/8fe1e10304f84fd2b0df550fe98a71fa
   ```

   The output lists a detailed description of the service you specified\.

   ```
   {
     "Service": {
       "ServiceName": "my-app-1",
       "ServiceId": "8fe1e10304f84fd2b0df550fe98a71fa",
       "ServiceArn": "arn:aws:apprunner:us-east-2:123456789012:service/my-app-1/8fe1e10304f84fd2b0df550fe98a71fa",
       "ServiceUrl": "psbqam834h.us-east-1.awsapprunner.com",
       "CreatedAt": "2020-11-20T19:05:25Z",
       "UpdatedAt": "2020-11-23T12:41:37Z",
       "Status": "RUNNING",
       "SourceConfiguration": {
         "CodeRepository": {
           "RepositoryUrl": "https://github.com/my-account/python-hello",
           "SourceCodeVersion": {
             "Type": "BRANCH",
             "Value": "main"
           },
           "CodeConfiguration": {
             "CodeConfigurationValues": {
               "BuildCommand": "[pip install -r requirements.txt]",
               "Port": "8080",
               "Runtime": "PYTHON_3",
               "RuntimeEnvironmentVariables": [
                 {
                   "NAME": "Jane"
                 }
               ],
               "StartCommand": "python server.py"
             },
             "ConfigurationSource": "API"
           }
         },
         "AutoDeploymentsEnabled": true,
         "AuthenticationConfiguration": {
           "ConnectionArn": "arn:aws:apprunner:us-east-2:123456789012:connection/my-github-connection/e7656250f67242d7819feade6800f59e"
         }
       },
       "InstanceConfiguration": {
         "CPU": "1 vCPU",
         "Memory": "3 GB"
       },
       "HealthCheckConfiguration": {
         "Protocol": "TCP",
         "Path": "/",
         "Interval": 10,
         "Timeout": 5,
         "HealthyThreshold": 1,
         "UnhealthyThreshold": 5
       },
       "AutoScalingConfigurationSummary": {
         "AutoScalingConfigurationArn": "arn:aws:apprunner:us-east-2:123456789012:autoscalingconfiguration/DefaultConfiguration/1/00000000000000000000000000000001",
         "AutoScalingConfigurationName": "DefaultConfiguration",
         "AutoScalingConfigurationRevision": 1
       }
     }
   }
   ```

## Verifying your App Runner service using AWS CloudShell<a name="api-cshell.call-your-service"></a>

When you [create an App Runner service](manage-create.md), App Runner creates a default domain for your service's website, and shows it in the console \(or returns it in the API call result\)\. You can use CloudShell to make calls to your website and verify that it's working correctly\.

For example, after you create an App Runner service as described in [Getting started with App Runner](getting-started.md), run the following command in CloudShell:

```
$ curl https://qxuadi4qwp.us-east-2.awsapprunner.com/; echo
```

The output should show the expected page content\.

![\[Browser window showing AWS CloudShell with a command to display the content of an App Runner service page\]](http://docs.aws.amazon.com/apprunner/latest/dg/images/api-cshell-curl.png)