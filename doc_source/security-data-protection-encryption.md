# Protecting data using encryption<a name="security-data-protection-encryption"></a>

AWS App Runner reads your application source \(source image or source code\) from a repository that you specify and stores it for deployment to your service\. For more information, see [App Runner architecture and concepts](architecture.md)\. 

Data protection refers to protecting data while *in transit* \(as it travels to and from App Runner\) and *at rest* \(while it is stored in AWS data centers\)\.

For more information about data protection, see [Data protection in App Runner](security-data-protection.md)\.

For other App Runner security topics, see [Security in App Runner](security.md)\.

## Encryption in transit<a name="security-data-protection-encryption.in-transit"></a>

You can achieve data protection in transit in two ways: encrypt the connection using Transport Layer Security \(TLS\), or use client\-side encryption \(where the object is encrypted before it is sent\)\. Both methods are valid for protecting your application data\. To secure the connection, encrypt it using TLS whenever your application, its developers and administrators, and its end users send or receive any objects\. App Runner sets up your application to receive traffic over TLS\.

Client\-side encryption isn't a valid method for protecting the source image or code that you provide to App Runner for deployment\. App Runner needs access to your application source, so it can't be encrypted\. Therefore, be sure to secure the connection between your development or deployment environment and App Runner\.

## Encryption at rest and key management<a name="security-data-protection-encryption.at-rest"></a>

To protect your application's data at rest, App Runner encrypts all stored copies of your application source image or source bundle\. When you create an App Runner service, you can provide an AWS KMS key\. If you provide one, App Runner uses your provided key to encrypt your source\. If you don't provide one, App Runner uses an AWS managed key instead\.

For details about App Runner service creation parameters, see [CreateService](https://docs.aws.amazon.com/apprunner/latest/api/API_CreateService.html)\. For information about AWS Key Management Service \(AWS KMS\), see the [AWS Key Management Service Developer Guide](https://docs.aws.amazon.com/kms/latest/developerguide/)\.