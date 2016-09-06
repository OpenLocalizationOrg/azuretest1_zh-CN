---
ms.openlocfilehash: 520c4420c5940ee09bc18598e3bb7e9be1a757ca
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
传播您的域名的记录后，必须将其与您的 Web 应用程序中关联。 使用以下步骤启用 web 浏览器使用的域名。

> [AZURE.NOTE] 它可能需要一些时间在传播通过 DNS 系统前面的步骤中创建的 CNAME 记录。 直到 CNAME 传播，不能添加到您的 web 应用程序的域名称。 如果您使用的一条 A 记录，不能 A 记录的域的名称添加到您的 web 应用程序之前在上一步中创建的**awverify** CNAME 记录传播。
>
> 您可以使用服务，如<a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a>来验证 CNAME 是可用的。

1. 在浏览器中，打开[Azure 管理门户网站](https://portal.azure.com)。

2. 在**Web 应用程序**选项卡，单击您的 web 应用程序的名称，选择**设置**，然后选择**自定义的域和 SSL**

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

3. 在**自定义的域和 SSL**刀片式服务器，请单击**置于外部域**。

    ![](./media/custom-dns-web-site/dncmntask-cname-7.png)

4. 使用**域名**文本框中输入此 web 应用程序相关联的域名。

    ![](./media/custom-dns-web-site/dncmntask-cname-8.png)

5. 单击**保存**以保存的域的名称配置。

    完成配置后，将在您的 web 应用程序的**域名**部分列出自定义域名。

此时，您应该能够在浏览器中输入自定义域名并查看，它成功地将带您到您的 web 应用程序。
