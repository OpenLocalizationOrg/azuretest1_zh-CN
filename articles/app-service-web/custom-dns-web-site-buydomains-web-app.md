---
ms.openlocfilehash: d25ce5c6b43355dcd3c0ab8570a9127219460997
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties
    pageTitle="如何购买 Azure 应用程序服务 Web 应用程序中的自定义域名"
    description="了解如何购买在 Azure 应用程序服务 web 应用程序自定义的域名。"
    services="app-service\web"
    documentationCenter=""
    authors="MikeWasson"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2015"
    ms.author="mwasson"/>

# 购买并在 Azure 应用程序服务中配置自定义域名

> [AZURE.SELECTOR]
- [购买的 Web 应用程序的域](custom-dns-web-site-buydomains-web-app.md)
- [Web 应用程序与外部域](web-sites-custom-domain-name.md)
- [Web 应用程序使用通信管理器](web-sites-traffic-manager-custom-domain-name.md)
- [GoDaddy](web-sites-godaddy-custom-domain-name.md)




[AZURE.INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]

创建 web 应用程序时，Azure 将其赋给一个 azurewebsites.net 的子域中。 例如，如果您的 web 应用程序名为**contoso**，URL 是**contoso.azurewebsites.net**。 Azure 还会指定一个虚拟 IP 地址。

用于生产的 web 应用程序，您可能希望用户看到自定义域名。 本文解释如何购买和使用[应用程序服务 Web 应用程序](http://go.microsoft.com/fwlink/?LinkId=529714)配置自定义的域。 

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]


## 概述

> [AZURE.NOTE] 请不要尝试购买使用不具有与之关联的活动信用卡订阅域。 这可能导致您的订阅被禁用。 

如果您没有为您的 web 应用程序的域名，您可以方便地购买[Azure 管理门户](https://portal.azure.com)上的第一个。 在采购过程中您可以选择有 WWW 和根域的 DNS 记录将自动映射到您的 web 应用程序。 您还可以管理 Azure 门户内您域的权利。


使用以下步骤来购买域名，并将分配给您的 web 应用程序。

1. 在浏览器中，打开[Azure 管理门户网站](https://portal.azure.com)。

2. 在**Web 应用程序**选项卡，单击您的 web 应用程序的名称，选择**设置**，然后选择**自定义的域和 SSL**

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

3. 在**自定义的域和 SSL**刀片式服务器，请单击**购买域**。

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

4. 在**购买域**刀片式服务器，使用文本框中键入域名称要购买，并按 enter 键。 将显示建议可用的域被丢在文本框。 选择您想要买哪些的域。 您可以选择同时购买多个域。 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

5. 单击**联系人信息**，并填充域的联系人信息窗体。

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-3.png)

> [AZURE.NOTE] 请务必填写尽可能精确越好，尤其是电子邮件地址的所有必填字段。 在购买无"隐私保护"域，您可能会要求域生效之前验证您的电子邮件。 在某些情况下，不正确的数据有关的联系信息将导致购买域失败。 

6. 现在，您可以选择，

    a）"自动更新"域每年
    
    b） 自愿加入"隐私保护"免费购买价格中包含
    
    c）"分配默认主机名"WWW 和根域添加到当前 Web 应用程序。 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.5.png)
  
> [AZURE.NOTE] 选项 C 配置 DNS 绑定和自动为您的主机名称绑定。  这样一来，可一旦采购完毕 (baring DNS 传播延迟，在一些情况下) 使用自定义的域访问您的 Web 应用程序。 在的情况下，您的 Web 应用程序是 Azure 流量管理器后，您看不到选项，将根域中，A 记录不工作与流量管理器。 
>
>您可以始终指定域/子-domains 购买通过一个 Web 应用程序向另一个 Web 应用程序，反之亦然。 第 8 步的更多详细信息，请参阅。 

    
7. 单击**选择****购买域**刀片式服务器，则会出现购买信息**购买确认**刀片式服务器上。 如果您接受的法律条款，并单击**购买**，将提交您的订单，您可以监视**通知**采购流程。 域采购可能需要几分钟才能完成。 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-4.png)

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-5.png)

8. 是否成功订购一个域，您可以管理域并将分配给您的 web 应用程序。 单击**"..."**在您的域的右侧。 然后，您可以**取消采购**或**管理域**。 单击**管理域**，然后我们可以将**子域**绑定到我们**管理域**刀片式服务器上的 web 应用程序。 如果您想要将**子域**绑定到不同的 Web 应用程序再执行此从各自的 Web 应用程序的上下文中的步骤。 在这里您只需选择流量管理器分配到通信管理器终结点的域 （如果 Web 应用程序后 TM） 选择命名从下拉菜单。 这样，域/子域将自动分配给该流量管理器终结点后面的所有 Web 应用程序。 

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-6.png)

> [AZURE.NOTE] 您可以"取消采购"在 5 日内以全额退款。 后 5 天内，您将不能对"取消采购"，相反，您将看到一个选项"删除"域。 删除域会导致释放它从您的订阅而不退款，并将成为可用的域。 

    Once configuration has completed, the custom domain name will be listed in the **Hostname bindings** section of your web app.

此时，您应该能够在浏览器中输入自定义域名并查看，它成功地将带您到您的 web 应用程序。
 

测试
