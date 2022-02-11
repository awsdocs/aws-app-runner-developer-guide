# Managing App Runner connections<a name="manage-connections"></a>

When you [create a service](manage-create.md) in AWS App Runner, you configure an application source—a container image or a source repository that's stored with a provider\. If a repository that's stored with a third\-party provider is private \(not publicly readable\), App Runner has to establish an authenticated and authorized connection with the provider\. Then, App Runner can read your repository and deploy it to your service\. App Runner doesn't require connection establishment when you create a service that accesses code stored in your AWS account or in a public code location\.

App Runner maintains connection information in a resource called a *connection*\. App Runner requires a connection resource when you create a service that needs third\-party connection information\. The following is some important information about connections:
+ **Providers** – App Runner currently requires connection resources with [GitHub](https://github.com/)\.
+ **Shared** – You can use a connection resource to create multiple App Runner services that use the same repository provider account\.
+ **Resource management** – In App Runner, you can create and delete connections\. However, you can't modify an existing connection\.
+ **Resource quota** – Connection resources have a set quota that's associated with your AWS account in each AWS Region\. If you reach this quota, you might need to delete a connection before you can connect to a new provider account\. You can delete a connection using the App Runner console or API as described in the following section, [Manage connections](#manage-connections.manage)\. For more information, see [App Runner resource quotas](architecture.md#architecture.quotas)\.

## Manage connections<a name="manage-connections.manage"></a>

Manage your App Runner connections using one of the following methods:

------
#### [ App Runner console ]

When you use the App Runner console to [create a service](manage-create.md), you provide connection details\. You don't have to explicitly create a connection resource\. In the console, you can choose to connect to a GitHub account that you've connected to before, or connect to a new account\. When necessary, App Runner creates a connection resource for you\. For a new connection, some providers \(for example, GitHub\) require you to complete an authentication handshake before you can use the connection\. The console takes you through this process\.

The console also has a page for managing your existing connections\. You can complete the authentication handshake for a connection if you didn't do it when you created your service\. You can also delete connections that you're no longer using\. The following procedure shows how you can manage GitHub connections\.

**To manage GitHub connections in your account**

1. Open the [App Runner console](https://console.aws.amazon.com/apprunner), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **GitHub connections**\.

   The console then displays a list of GitHub connections in your account\.  
![\[App Runner connections page\]](http://docs.aws.amazon.com/apprunner/latest/dg/images/console-connections-github.png)

1. You can now do one of the following actions with any connection on the list:
   + *Open GitHub account or organization* – Choose the name of the connection\.
   + *Complete authentication handshake* – Select the connection, and then choose **Complete handshake**\. The console takes you through the authentication handshake process\.
   + *Delete connection* – Select the connection, and then choose **Delete**\. Follow the instructions on the deletion prompt\.

------
#### [ App Runner API or AWS CLI ]

You can use the following App Runner API actions to manage your connections\.
+ [CreateConnection](https://docs.aws.amazon.com/apprunner/latest/api/API_CreateConnection.html) – Creates a connection to a repository provider account\. After the connection is created, you must manually complete the authentication handshake using the App Runner console\. This process is explained in the previous section\.
+ [ListConnections](https://docs.aws.amazon.com/apprunner/latest/api/API_ListConnections.html) – Returns a list of App Runner connections associated with your AWS account\.
+ [DeleteConnection](https://docs.aws.amazon.com/apprunner/latest/api/API_DeleteConnection.html) – Deletes a connection\. You might need to delete unnecessary connections if you reach the connection quota for your AWS account\.

------