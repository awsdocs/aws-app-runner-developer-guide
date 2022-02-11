# Enabling VPC access for your service<a name="network-vpc"></a>

By default, your AWS App Runner application can send messages to public endpoints\. This includes your own solutions, AWS services, and any other public website or web service\. Your application can even send messages to public endpoints of applications that run in a public VPC from [Amazon Virtual Private Cloud](https://docs.aws.amazon.com/vpc/latest/userguide/) \(Amazon VPC\)\.

If your application needs to send messages to applications that run in a private VPC, you can associate your service with this VPC\. When you associate your service with a VPC, all outbound message traffic from your application is directed through this VPC\. This includes traffic to private endpoints in the VPC and to public endpoints\. In this situation, the private endpoints might be your applications or VPC endpoints to AWS services\. All networking rules of the VPC apply to your application's outbound traffic\.

**Note**  
The following traffic isn't affected when you associate your service with a VPC:  
**Inbound traffic** – Incoming messages that your application receives are unaffected by an associated VPC\. The messages are routed through the public domain name that's associated with your service and don't interact with the VPC\.
**App Runner traffic** – App Runner manages several actions on your behalf, such as pulling source code and images, pushing logs, and retrieving secrets\. The traffic that these actions generate isn't routed through your VPC\.

To associate your service with a VPC from Amazon VPC, choose one or more subnets of one VPC\. Each subnet is in a specific Availability Zone\. You can optionally specify the security groups that App Runner uses to access AWS resources under the specified subnets\. If you don't specify security groups, App Runner uses the default security group of the VPC\. The default security group allows all outbound traffic\. 

**Note**  
A few Availability Zones in some AWS Regions don't support subnets that can be used with App Runner services\. If you choose subnets in these Availability Zones, your service fails to be created or updated\. For these situations, App Runner provides a detailed error message pointing to the unsupported subnets and Availability Zones\. Remove the unsupported subnets from your request and try again\.

## Manage VPC access<a name="network-vpc.manage"></a>

Manage VPC access for your App Runner services using one of the following methods:

------
#### [ App Runner console ]

When you [create a service](manage-create.md) using the App Runner console, or when you [update its configuration later](manage-configure.md), you can choose to associate your service with a VPC from Amazon VPC\. Look for the **Networking** configuration section on the console page\. For **Outgoing network traffic**, choose **Custom VPC**\. You can choose an existing VPC connector\. Alternatively, you can add a new VPC connector and provide a name and settings\. App Runner creates a VPC connector resource for you, and then associates it with your service\.

------
#### [ App Runner API or AWS CLI ]

When you call the [CreateService](https://docs.aws.amazon.com/apprunner/latest/api/API_CreateService.html) or [UpdateService](https://docs.aws.amazon.com/apprunner/latest/api/API_UpdateService.html) App Runner API actions, use the `EgressConfiguration` member of the `NetworkConfiguration` parameter to specify a VPC connector resource for your service\.

Use the following App Runner API actions to manage your VPC Connector resources\.
+ [CreateVpcConnector](https://docs.aws.amazon.com/apprunner/latest/api/API_CreateVpcConnector.html) – Creates a new VPC connector\.
+ [ListVpcConnectors](https://docs.aws.amazon.com/apprunner/latest/api/API_ListVpcConnectors.html) – Returns a list of the VPC connectors that are associated with your AWS account, with full descriptions\.
+ [DescribeVpcConnector](https://docs.aws.amazon.com/apprunner/latest/api/API_DescribeVpcConnector.html) – Returns a full description of a VPC connector\.
+ [DeleteVpcConnector](https://docs.aws.amazon.com/apprunner/latest/api/API_DeleteVpcConnector.html) – Deletes a VPC connector\. You might need to delete unnecessary VPC connectors if you reach the VPC connector quota for your AWS account\.

------