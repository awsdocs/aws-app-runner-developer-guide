# Using App Runner with VPC endpoints<a name="security-data-protection-vpce"></a>

Your AWS application might integrate AWS App Runner services with other AWS services running in an [Amazon Virtual Private Cloud \(Amazon VPC\)](https://docs.aws.amazon.com/vpc/latest/userguide/)\. Parts of your application might make requests to App Runner from within the VPC\. For example, you might use AWS CodePipeline to continuously deploy to your App Runner service\. One way to improve the security of your application is to send these App Runner requests \(and requests to other AWS services\) over a VPC endpoint\.

A *VPC endpoint* enables you to privately connect your VPC to supported AWS services and VPC endpoint services powered by AWS PrivateLink, without requiring an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection\. 

Resources in your VPC don't use public IP addresses to interact with App Runner resources\. Traffic between your VPC and App Runner doesn't leave the Amazon network\. For complete information about VPC endpoints, see [VPC endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints.html) in the *AWS PrivateLink Guide*\.

App Runner supports AWS PrivateLink, which provides private connectivity to App Runner and eliminates exposure of traffic to the public internet\. To enable your application to send requests to App Runner using AWS PrivateLink, you configure a type of VPC endpoint known as an *interface VPC endpoint* \(interface endpoint\)\. For more information, see [Interface VPC endpoints \(AWS PrivateLink\)](https://docs.aws.amazon.com/vpc/latest/privatelink/vpce-interface.html) in the *AWS PrivateLink Guide*\.

**Note**  
The web application in your App Runner service runs in a VPC that App Runner provides and configures\. This VPC is public—it's connected to the public internet\. App Runner doesn't support running your application in a private VPC or creating a VPC endpoint for it\.

## Setting up a VPC endpoint for App Runner<a name="security-data-protection-vpce.setup"></a>

To create the interface VPC endpoint for the App Runner service in your VPC, follow the [Create an interface endpoint](https://docs.aws.amazon.com/vpc/latest/privatelink/vpce-interface.html#create-interface-endpoint) procedure in the *AWS PrivateLink Guide*\. For **Service Name**, choose `com.amazonaws.region.apprunner`\.

## VPC network privacy considerations<a name="security-data-protection-vpce.private"></a>

**Important**  
Using a VPC endpoint for App Runner doesn't guarantee that all traffic from your VPC stays off of the internet\. The VPC might be public \(internet connected\), and some parts of your solution might not use VPC endpoints to make AWS API calls\. For example, AWS services might call other services using their public endpoints\. If traffic privacy is required for the solution in your VPC, read this section\.

To ensure privacy of network traffic in your VPC, consider the following:
+ *Enable DNS name* – Parts of your application might still send requests to App Runner over the internet using the `apprunner.region.amazonaws.com` public endpoint\. If your VPC is configured with public internet access, these requests succeed with no indication to you\. You can prevent this by ensuring that **Enable DNS name** is enabled during endpoint creation \(true by default\)\. This adds a DNS entry in your VPC that maps the public service endpoint to the interface VPC endpoint\.
+ *Configure VPC endpoints for additional services* – Your solution might send requests to other AWS services\. For example, AWS CodePipeline might send requests to AWS CodeBuild\. Configure VPC endpoints for these services too, and enable DNS names on these endpoints\.

## Using endpoint policies to control access with VPC endpoints<a name="security-data-protection-vpce.policy"></a>

By default, a VPC endpoint allows full access to the service with which it's associated\. When you create or modify a VPC endpoint for App Runner, you can attach an *endpoint policy* to it\. 

An endpoint policy is an AWS Identity and Access Management \(IAM\) resource policy that controls access from the endpoint to the specified service\. The endpoint policy is specific to the endpoint\. It's separate from any user or instance IAM policies that your environment might have and doesn't override or replace them\. For details about authoring and using VPC endpoint policies, see [Controlling Access to Services with VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-access.html) in the *AWS PrivateLink Guide*\.

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