# AWS managed policies for AWS App Runner<a name="security-iam-awsmanpol"></a>







To add permissions to users, groups, and roles, it is easier to use AWS managed policies than to write policies yourself\. It takes time and expertise to [create IAM customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) that provide your team with only the permissions they need\. To get started quickly, you can use our AWS managed policies\. These policies cover common use cases and are available in your AWS account\. For more information about AWS managed policies, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

AWS services maintain and update AWS managed policies\. You can't change the permissions in AWS managed policies\. Services occasionally add additional permissions to an AWS managed policy to support new features\. This type of update affects all identities \(users, groups, and roles\) where the policy is attached\. Services are most likely to update an AWS managed policy when a new feature is launched or when new operations become available\. Services do not remove permissions from an AWS managed policy, so policy updates won't break your existing permissions\.

Additionally, AWS supports managed policies for job functions that span multiple services\. For example, the **ViewOnlyAccess** AWS managed policy provides read\-only access to many AWS services and resources\. When a service launches a new feature, AWS adds read\-only permissions for new operations and resources\. For a list and descriptions of job function policies, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.













## App Runner updates to AWS managed policies<a name="security-iam-awsmanpol-updates"></a>



View details about updates to AWS managed policies for App Runner since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the App Runner Document history page\.




| Change | Description | Date | 
| --- | --- | --- | 
|  [AWSAppRunnerReadOnlyAccess](security_iam_service-with-iam.md#security_iam_service-with-iam-users) – New policy  |  App Runner added a new policy to allow users to list and view details about App Runner resources\.  | Feb 24, 2022 | 
|  [AWSAppRunnerFullAccess](security_iam_service-with-iam.md#security_iam_service-with-iam-users) – Update to an existing policy  |  App Runner updated the resource list for the `iam:CreateServiceLinkedRole` action to allow creation of `AWSServiceRoleForAppRunnerNetworking` service\-linked role\.  | Feb 8, 2022 | 
|  [AppRunnerNetworkingServiceRolePolicy](using-service-linked-roles-networking.md) – New policy  |  App Runner added a new policy to allow App Runner to make calls to Amazon Virtual Private Cloud to attach a VPC to your App Runner service and manage network interfaces on behalf of App Runner services\. The policy is used in the `AWSServiceRoleForAppRunnerNetworking` service\-linked role\.  | Feb 8, 2022 | 
|  [AWSAppRunnerFullAccess](security_iam_service-with-iam.md#security_iam_service-with-iam-users) – New policy  |  App Runner added a new policy to allow users to perform all App Runner actions\.  | Jan 10, 2022 | 
|  [AppRunnerServiceRolePolicy](using-service-linked-roles-management.md) – New policy  |  App Runner added a new policy to allow App Runner to make calls to Amazon CloudWatch Logs and Amazon CloudWatch Events on behalf of App Runner services\. The policy is used in the `AWSServiceRoleForAppRunner` service\-linked role\.  | Mar 1, 2021 | 
|  [AWSAppRunnerServicePolicyForECRAccess](security_iam_service-with-iam.md#security_iam_service-with-iam-roles-service.access) – New policy  |  App Runner added a new policy to allow App Runner to access Amazon Elastic Container Registry \(Amazon ECR\) images in your account\.  | Mar 1, 2021 | 
|  App Runner started tracking changes  |  App Runner started tracking changes for its AWS managed policies\.  | Mar 1, 2021 | 