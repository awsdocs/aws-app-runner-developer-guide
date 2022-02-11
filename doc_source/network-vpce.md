# Using App Runner with VPC endpoints<a name="network-vpce"></a>

Your AWS application might integrate AWS App Runner services with other AWS services running in a VPC from [Amazon Virtual Private Cloud](https://docs.aws.amazon.com/vpc/latest/userguide/) \(Amazon VPC\)\. Parts of your application might make requests to App Runner from within the VPC\. For example, you might use AWS CodePipeline to continuously deploy to your App Runner service\. One way to improve the security of your application is to send these App Runner requests \(and requests to other AWS services\) over a VPC endpoint\.

Using a *VPC endpoint*, you can privately connect your VPC to supported AWS services and VPC endpoint services that are powered by AWS PrivateLink\. You don't need an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection\. 

Resources in your VPC don't use public IP addresses to interact with App Runner resources\. Traffic between your VPC and App Runner doesn't leave the Amazon network\. For more information about VPC endpoints, see [VPC endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints.html) in the *AWS PrivateLink Guide*\.

App Runner supports AWS PrivateLink, which provides private connectivity to App Runner and eliminates exposure of traffic to the internet\. To enable your application to send requests to App Runner using AWS PrivateLink, configure a type of VPC endpoint known as an *interface VPC endpoint* \(interface endpoint\)\. For more information, see [Interface VPC endpoints \(AWS PrivateLink\)](https://docs.aws.amazon.com/vpc/latest/privatelink/vpce-interface.html) in the *AWS PrivateLink Guide*\.

**Note**  
By default, the web application in your App Runner service runs in a VPC that App Runner provides and configures\. This VPC is public—it's connected to the internet\. You can optionally associate your application with a custom VPC\. For more information, see [Enabling VPC access for your service](network-vpc.md)\.  
App Runner doesn't support creating a VPC endpoint for your application\.

## Setting up a VPC endpoint for App Runner<a name="network-vpce.setup"></a>

To create the interface VPC endpoint for the App Runner service in your VPC, follow the [Create an interface endpoint](https://docs.aws.amazon.com/vpc/latest/privatelink/vpce-interface.html#create-interface-endpoint) procedure in the *AWS PrivateLink Guide*\. For **Service Name**, choose `com.amazonaws.region.apprunner`\.

## VPC network privacy considerations<a name="network-vpce.private"></a>

**Important**  
Using a VPC endpoint for App Runner doesn't guarantee that all traffic from your VPC stays off of the internet\. The VPC might be public\. Moreover, some parts of your solution might not use VPC endpoints to make AWS API calls\. For example, AWS services might call other services using their public endpoints\. If traffic privacy is required for the solution in your VPC, read this section\.

To ensure privacy of network traffic in your VPC, consider the following:
+ *Enable DNS name* – Parts of your application might still send requests to App Runner over the internet using the `apprunner.region.amazonaws.com` public endpoint\. If your VPC is configured with internet access, these requests succeed with no indication to you\. You can prevent this by ensuring that **Enable DNS name** is enabled when you create the endpoint\. By default, it's set to true\. This adds a DNS entry in your VPC that maps the public service endpoint to the interface VPC endpoint\.
+ *Configure VPC endpoints for additional services* – Your solution might send requests to other AWS services\. For example, AWS CodePipeline might send requests to AWS CodeBuild\. Configure VPC endpoints for these services, and enable DNS names on these endpoints\.
+ *Configure a private VPC* – If possible \(if your solution doesn't need internet access at all\), set up your VPC as private, which means that it has no internet connection\. This ensures that a missing VPC endpoint would cause a visible error, allowing you to add the missing endpoint\.

## Using endpoint policies to control access with VPC endpoints<a name="network-vpce.policy"></a>

By default, a VPC endpoint allows full access to the service with which it's associated\. When you create or modify a VPC endpoint for App Runner, you can attach an *endpoint policy* to it\. 

An endpoint policy is an AWS Identity and Access Management \(IAM\) resource policy that controls access from the endpoint to the specified service\. The endpoint policy is specific to the endpoint\. It's separate from any user or instance IAM policies that your environment might have and doesn't override or replace them\. For more information about authoring and using VPC endpoint policies, see [Controlling Access to Services with VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-access.html) in the *AWS PrivateLink Guide*\.

The following example allows read\-only access from the VPC endpoint to App Runner\. These are minimal access rights when using the VPC endpoint\. Principals might have additional permissions through other policies\.

```
{
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "apprunner:List*",
        "apprunner:Describe*"
      ],
      "Resource": "*"
    }
  ]
}
```