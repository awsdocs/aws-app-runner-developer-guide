# Managing custom domain names for an App Runner service<a name="manage-custom-domains"></a>

When you create an AWS App Runner service, App Runner allocates a domain name for it\. This is a subdomain in the `awsapprunner.com` domain that's owned by App Runner\. It can be used to access the web application that's running in your service\.

If you own a domain name, you can associate it to your App Runner service\. After App Runner validates your new domain, it can be used to access your application in addition to the App Runner domain\. You can associate up to five custom domains\.

**Note**  
You can optionally include the `www` subdomain of your domain\. However, this is currently only supported in the API\. The App Runner console doesn't support it\.

When you associate a custom domain with your service, App Runner provides you with a set of CNAME records to add to your Domain Name System \(DNS\)\. Add certificate validation records to your DNS so that App Runner can validate that you own or control the domain\. In addition, add DNS target records to your DNS to target the App Runner domain\. You need to add one record for the custom domain, and another for the `www` subdomain, if you chose this option\. Then wait for the custom domain status to become **Active** in the App Runner console\. This typically takes several minutes \(but might take 24\-48 hours\)\. At this point, your custom domain is validated, and App Runner starts routing traffic from this domain to your web application\.

You can specify a domain to associate with your App Runner service in the following ways:
+ *A root domain* – For example, `example.com`\. You can optionally associate `www.example.com` too as part of the same operation\.
+ *A subdomain* – For example, `login.example.com` or `admin.login.example.com`\. You can optionally associate the `www` subdomain too as part of the same operation\.
+ *A wildcard* – For example, `*.example.com`\. You can't use the `www` option in this case\. You can specify a wildcard only as the immediate subdomain of a root domain, and only on its own \(these aren't valid specifications: `login*.example.com`, `*.login.example.com`\)\. This wildcard specification associates all immediate subdomains, and doesn't associate the root domain itself \(the root domain would have to be associated in a separate operation\)\.

A more specific domain association overrides a less specific one\. For example, `login.example.com` overrides `*.example.com`\. The certificate and CNAME of the more specific association are used\.

The following example shows how you can use multiple custom domain associations:

1. Associate `example.com` with the home page of your service\. Enable the `www` to also associate `www.example.com`\.

1. Associate `login.example.com` with the login page of your service\.

1. Associate `*.example.com` with a custom "not found" page\.

You can disassociate \(unlink\) a custom domain from your App Runner service\. When you unlink a domain, App Runner stops routing traffic from this domain to your web application\. You must delete the records for this domain from your DNS\.

App Runner internally creates certificates that track domain validity\. They're stored in AWS Certificate Manager \(ACM\)\. App Runner doesn't delete these certificates for seven days after a domain is disassociated from your service or after the service is deleted\.

## Manage custom domains<a name="manage-custom-domains.manage"></a>

Manage custom domains for your App Runner service using one of the following methods:

------
#### [ App Runner console ]

**To associate \(link\) a custom domain using the App Runner console**

1. Open the [App Runner console](https://console.aws.amazon.com/apprunner), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Services**, and then choose your App Runner service\.

   The console displays the service dashboard with a **Service overview**\.  
![\[App Runner service dashboard page showing Activity list\]](http://docs.aws.amazon.com/apprunner/latest/dg/images/console-dashboard.png)

1. On the service dashboard page, choose the **Custom domains** tab\.

   The console shows the custom domains that are associated with your service, or **No custom domains**\.  
![\[The Custom domains tab on the App Runner service dashboard page, showing no associated custom domains\]](http://docs.aws.amazon.com/apprunner/latest/dg/images/service-dashboad-domains-empty.png)

1. On the **Custom domains** tab, choose **Link domain**\.

1. In the **Link custom domain** dialog, enter a domain name, and then choose **Link custom domain**\.

1. Follow the instructions on the **Configure DNS** page to start the domain validation process\.  
![\[The Configure DNS page, showing certificate validation and DNS target records to add to customer's DNS\]](http://docs.aws.amazon.com/apprunner/latest/dg/images/custom-domain-configure.png)

1. Choose **Close**

   The console shows the dashboard again\. The **Custom domains** tab has a new tile showing the domain you just linked in the **Pending certificate DNS validation** status\.  
![\[The Custom domains tab on the App Runner service dashboard page, showing a custom domain tile\]](http://docs.aws.amazon.com/apprunner/latest/dg/images/service-dashboad-domains-tile.png)

1. When the domain status changes to **Active**, verify that the domain works for routing traffic by browsing to it\.  
![\[The Custom domains tab on the App Runner service dashboard page, showing a custom domain tile\]](http://docs.aws.amazon.com/apprunner/latest/dg/images/service-dashboad-domains-tile-active.png)

**To disassociate \(unlink\) a custom domain using the App Runner console**

1. On the **Custom domains** tab, select the tile for the domain you want to disassociate, and then choose **Unlink domain**\.

1. In the **Unlink domain** dialog, verify the action by choosing **Unlink domain**\.

------
#### [ App Runner API or AWS CLI ]

To associate a custom domain with your service using the App Runner API or AWS CLI, call the [AssociateCustomDomain](https://docs.aws.amazon.com/apprunner/latest/api/API_AssociateCustomDomain.html) API action\. When the call succeeds, it returns a [CustomDomain](https://docs.aws.amazon.com/apprunner/latest/api/API_CustomDomain.html) object that describes the custom domain that's being associated with your service\. The object should show a status of `CREATING`, and contains a list of [CertificateValidationRecord](https://docs.aws.amazon.com/apprunner/latest/api/API_CertificateValidationRecord.html) objects\. These are records you can add to your DNS\.

To disassociate a custom domain from your service using the App Runner API or AWS CLI, call the [DisassociateCustomDomain](https://docs.aws.amazon.com/apprunner/latest/api/API_DisassociateCustomDomain.html) API action\. When the call succeeds, it returns a [CustomDomain](https://docs.aws.amazon.com/apprunner/latest/api/API_CustomDomain.html) object that describes the custom domain that's being disassociated from your service\. The object should show a status of `DELETING`\.

------