# Using roles for networking<a name="using-service-linked-roles-networking"></a>

AWS App Runner uses AWS Identity and Access Management \(IAM\)[ service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to App Runner\. Service\-linked roles are predefined by App Runner and include all the permissions that the service requires to call other AWS services on your behalf\. 

A service\-linked role makes setting up App Runner easier because you don’t have to manually add the necessary permissions\. App Runner defines the permissions of its service\-linked roles, and unless defined otherwise, only App Runner can assume its roles\. The defined permissions include the trust policy and the permissions policy, and that permissions policy cannot be attached to any other IAM entity\.

You can delete a service\-linked role only after first deleting their related resources\. This protects your App Runner resources because you can't inadvertently remove permission to access the resources\.

For information about other services that support service\-linked roles, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes **in the **Service\-Linked Role** column\. Choose a **Yes** with a link to view the service\-linked role documentation for that service\.

## Service\-linked role permissions for App Runner<a name="service-linked-role-permissions-networking"></a>

App Runner uses the service\-linked role named **AWSServiceRoleForAppRunnerNetworking**\.

The role allows App Runner to perform the following tasks:
+ Attach a VPC to your App Runner service and manage network interfaces\.

The AWSServiceRoleForAppRunnerNetworking service\-linked role trusts the following services to assume the role:
+ `networking.apprunner.amazonaws.com`

The role permissions policy named AppRunnerNetworkingServiceRolePolicy contains all of the permissions that App Runner needs to complete actions on your behalf\.

### AppRunnerNetworkingServiceRolePolicy<a name="service-linked-role-permissions-networking.policy"></a>

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeNetworkInterfaces",
        "ec2:DescribeVpcs",
        "ec2:DescribeDhcpOptions",
        "ec2:DescribeSubnets",
        "ec2:DescribeSecurityGroups"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "ec2:CreateNetworkInterface",
      "Resource": "*",
      "Condition": {
        "ForAllValues:StringEquals": {
          "aws:TagKeys": [
            "AWSAppRunnerManaged"
          ]
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": "ec2:CreateTags",
      "Resource": "arn:aws:ec2:*:*:network-interface/*",
      "Condition": {
        "StringEquals": {
          "ec2:CreateAction": "CreateNetworkInterface"
        },
        "StringLike": {
          "aws:RequestTag/AWSAppRunnerManaged": "*"
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": "ec2:DeleteNetworkInterface",
      "Resource": "*",
      "Condition": {
        "Null": {
          "ec2:ResourceTag/AWSAppRunnerManaged": "false"
        }
      }
    }
  ]
}
```

You must configure permissions to allow an IAM entity \(such as a user, group, or role\) to create, edit, or delete a service\-linked role\. For more information, see [Service\-Linked Role Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

## Creating a service\-linked role for App Runner<a name="create-service-linked-role-networking"></a>

You don't need to manually create a service\-linked role\. When you create a VPC connector in the AWS Management Console, the AWS CLI, or the AWS API, App Runner creates the service\-linked role for you\. 

If you delete this service\-linked role, and then need to create it again, you can use the same process to recreate the role in your account\. When you create a VPC connector, App Runner creates the service\-linked role for you again\. 

## Editing a service\-linked role for App Runner<a name="edit-service-linked-role-networking"></a>

App Runner does not allow you to edit the AWSServiceRoleForAppRunnerNetworking service\-linked role\. After you create a service\-linked role, you cannot change the name of the role because various entities might reference the role\. However, you can edit the description of the role using IAM\. For more information, see [Editing a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting a service\-linked role for App Runner<a name="delete-service-linked-role-networking"></a>

If you no longer need to use a feature or service that requires a service\-linked role, we recommend that you delete that role\. That way you don’t have an unused entity that is not actively monitored or maintained\. However, you must clean up your service\-linked role before you can manually delete it\.

### Cleaning Up a Service\-Linked Role<a name="service-linked-role-review-before-delete-networking"></a>

Before you can use IAM to delete a service\-linked role, you must first delete any resources used by the role\.

In App Runner, this means disassociating VPC connectors from all App Runner services in your account, and deleting the VPC connectors\. For more information, see [Enabling VPC access for your service](network-vpc.md)\.

**Note**  
If the App Runner service is using the role when you try to delete the resources, then the deletion might fail\. If that happens, wait for a few minutes and try the operation again\.

### Manually Delete the Service\-Linked Role<a name="slr-manual-delete-networking"></a>

Use the IAM console, the AWS CLI, or the AWS API to delete the AWSServiceRoleForAppRunnerNetworking service\-linked role\. For more information, see [Deleting a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.

## Supported regions for App Runner service\-linked roles<a name="slr-regions-networking"></a>

App Runner supports using service\-linked roles in all of the regions where the service is available\. For more information, see [AWS App Runner endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/apprunner.html) in the *AWS General Reference*\.