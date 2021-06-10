# Troubleshooting App Runner identity and access<a name="security_iam_troubleshoot"></a>

Use the following information to help you diagnose and fix common issues that you might encounter when working with AWS App Runner and IAM\.

For other App Runner security topics, see [Security in App Runner](security.md)\.

**Topics**
+ [I'm not authorized to perform an action in App Runner](#security_iam_troubleshoot-no-permissions)
+ [I'm an administrator and want to allow others to access App Runner](#security_iam_troubleshoot-admin-delegate)
+ [I want to allow people outside of my AWS account to access my App Runner resources](#security_iam_troubleshoot-cross-account-access)

## I'm not authorized to perform an action in App Runner<a name="security_iam_troubleshoot-no-permissions"></a>

If the AWS Management Console tells you that you're not authorized to perform an action, contact your administrator for assistance\. Your administrator is the person that provided you with your AWS user name and password\.

The following example error occurs when an IAM user named `marymajor` tries to use the console to view details about an App Runner service but doesn't have `apprunner:DescribeService` permissions\.

```
User: arn:aws:iam::123456789012:user/marymajor is not authorized to perform: apprunner:DescribeService on resource: my-example-service
```

In this case, Mary asks her administrator to update her policies to allow her to access the `my-example-service` resource using the `apprunner:DescribeService` action\.

## I'm an administrator and want to allow others to access App Runner<a name="security_iam_troubleshoot-admin-delegate"></a>

To allow others to access App Runner, you must create an IAM entity \(user or role\) for the person or application that needs access\. They will use the credentials for that entity to access AWS\. You must then attach a policy to the entity that grants them the correct permissions in App Runner\.

To get started right away, see [Creating your first IAM delegated user and group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-delegated-user.html) in the *IAM User Guide*\.

## I want to allow people outside of my AWS account to access my App Runner resources<a name="security_iam_troubleshoot-cross-account-access"></a>

You can create a role that users in other accounts or people outside of your organization can use to access your resources\. You can specify who is trusted to assume the role\. For services that support resource\-based policies or access control lists \(ACLs\), you can use those policies to grant people access to your resources\.

To learn more, consult the following:
+ To learn whether App Runner supports these features, see [How App Runner works with IAM](security_iam_service-with-iam.md)\.
+ To learn how to provide access to your resources across AWS accounts that you own, see [Providing access to an IAM user in another AWS account that you own](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_common-scenarios_aws-accounts.html) in the *IAM User Guide*\.
+ To learn how to provide access to your resources to third\-party AWS accounts, see [Providing access to AWS accounts owned by third parties](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_common-scenarios_third-party.html) in the *IAM User Guide*\.
+ To learn how to provide access through identity federation, see [Providing access to externally authenticated users \(identity federation\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_common-scenarios_federated-users.html) in the *IAM User Guide*\.
+ To learn the difference between using roles and resource\-based policies for cross\-account access, see [How IAM roles differ from resource\-based policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_compare-resource-policies.html) in the *IAM User Guide*\.