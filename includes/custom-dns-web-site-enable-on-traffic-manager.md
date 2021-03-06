---
ms.openlocfilehash: dc65f6768b31b24fb254328ccd1378acb7750262
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
传播您的域名的记录后，您应能够使用您的浏览器以验证可以使用您的自定义域名来访问您的 web 应用程序在 Azure 应用程序服务。

> [AZURE.NOTE] 它可能需要一些时间为您 CNAME 通过 DNS 系统传播。 您可以使用服务，如<a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a>来验证 CNAME 是可用的。

如果您尚未为通讯管理器终结点添加 web 应用程序，必须执行此操作，则进行名称解析，为自定义域名称路由到通信管理器之前。 通信管理器，然后路由到您的 web 应用程序。 使用[添加或删除终结点](../traffic-manager/traffic-manager-endpoints.md)中的信息作为终结点通信管理器配置文件中添加您的 web 应用程序。

> [AZURE.NOTE] 如果未列出您的 web 应用程序，添加终结点时，请验证它被配置为**标准**的应用程序服务计划模式。 工作与流量管理器，您必须使用您的 web 应用程序的**标准**模式。

1. 在浏览器中，打开[Azure 门户](https://portal.azure.com)。

2. 在**Web 应用程序**选项卡，单击您的 web 应用程序的名称，选择**设置**，然后选择**自定义的域和 SSL**

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

3. 在**自定义的域和 SSL**刀片式服务器，请单击**置于外部域**。

    ![](./media/custom-dns-web-site/dncmntask-cname-7.png)

4. 使用**域名**文本框中输入的流量管理器域名 (contoso.trafficmanager.net) 将与此 web 应用程序相关联。

    ![](./media/custom-dns-web-site/dncmntask-cname-8.png)

5. 单击**保存**以保存的域的名称配置。

    完成配置后，将在您的 web 应用程序的**域名**部分列出自定义域名。

此时，您应该能够在您的浏览器中输入流量管理器名称 (contoso.trafficmanager.net) 域名并查看，它成功地将带您到您的 web 应用程序。
