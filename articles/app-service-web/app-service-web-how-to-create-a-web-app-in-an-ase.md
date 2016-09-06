---
ms.openlocfilehash: 501a519ffe91e414a255ee4e76be22facfe96a95
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何在应用程序服务的环境中创建 Web 应用程序"
    description="对于 web 应用程序和应用程序服务计划检查其应用程序服务环境创建流"
    services="app-service\web"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article" 
    ms.date="04/27/2015"
    ms.author="ccompy"/>

# 如何在应用程序服务的环境中创建 Web 应用程序 #

创建 web 应用程序是几乎相同的应用程序服务环境 (ASE)，因为它通常是。  如果您不熟悉的应用程序服务环境功能，然后读取该文档这里[什么是应用程序的服务环境](app-service-app-service-environment-intro.md)。

在 ASE 中创建 web 应用程序需要通过让 ASE 首次启动。   有关的详细信息在创建 ASE 阅读此处的文档︰[如何创建一个应用程序的服务环境](app-service-web-how-to-create-an-app-service-environment.md)。

创建 web 应用程序的第一步是创建或选择的应用程序服务计划 (ASP)。  在通常与它相同 does ASE 开始即会创建 ASP web 应用程序创建通过流从新开始-> Web + 手机-> Web 应用程序。

![][1]


如果您使用的您已经在您的应用程序服务的环境中创建的应用程序服务计划，请选中该计划、 输入您的 web 应用程序的名称，然后选择创建。  当您创建一个 web 应用程序通常是同一个流中。  这里主要的区别是，将在到达您的 web 应用程序︰

[*站点名*]。[*您的应用程序服务环境的名称*]。 p.azurewebsites.net

而不是

[*sitename*]。 azurewebsites.net

现在，您的 web 应用程序名称必须唯一跨整个 Azure 应用程序服务。  这意味着您如果您想要创建名为"thisismywebapp"，目前不能有任何其他 Azure 应用程序服务中的名为"thisismywebapp"的 web 应用，然后一个 web 应用程序。  

### 应用程序服务计划 ###

应用程序服务计划是一组托管的 web 应用程序。  选择定价时，收取的价格应用于应用程序的服务计划，而不是单个应用程序。  若要扩展的 web 应用程序的实例数按比例放大您的 ASP 的实例，它影响所有的 web 应用程序在该计划中。  还有一些功能，例如站点插槽或 VNET 集成在计划内的数量限制。  您可以从此处的文档了解更多关于应用程序服务计划︰[深入 Azure 应用程序服务计划](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)

您要做一个新的应用程序服务计划，在应用程序服务环境创建 ASP 的一些差异。  除了其他事情外，工作人员所做的选择是不同的这是因为应用程序服务环境中有没有共享的工作人员。  您必须使用的工作人员是由管理员分配到应用程序服务环境  这意味着，若要创建新的 ASP，需要有更多的工作人员在您的 Asp 应用程序服务环境中的所有分配给您的应用程序服务环境的实例总数比。  如果您没有足够的工作人员以创建您的 ASP 应用程序服务环境中，您需要使用您的应用程序服务环境管理，以让他们添加。

应用程序服务环境的另一个差异承载 Asp 是缺乏定价选择。  当您有一个应用程序的服务环境您支付系统使用的计算资源，在该环境中 asp 没有增加的费用。  通常情况下当您创建 ASP 选择定价计划，以确定您的付费。  应用程序服务环境是本质上是私人位置，您可以在其中创建内容。  您支付环境并不是来承载您的内容。

### 选择您的应用程序服务环境 ###

因为应用程序服务环境本质上是一个专用的部署位置，首先选择您想要使用从您的位置选择器 ASE。

![][2]

所选内容后用户界面将更新和替换拾定价计划辅助池机械臂。  位置显示的 ASE 系统和区域的名称。  在 URL 下 ASE 的域名替换通常存在。 azurewebsites.net 与应用程序服务环境的名称。

![][3]

### 选择辅助池 ###

通常情况下在 Azure 应用程序服务和应用程序服务环境外，有 3 种尺寸可用的专用的价格计划的所选内容。  以类似的方式，拥有 ASE 的客户可以定义工作人员最多 3 池并指定该工作池使用 VM 的大小。  而不是选择您的 asp 的定价计划，您可以选择所谓的辅助池。  

工作人员池选择 UI 显示虚拟机用于名称下面的工作人员池的大小。  可用数量是指多少虚拟机可供使用该池中。  总池实际上可能会有比这个数字更多 Vm，但此值指的只是多少都未在使用。  如果您需要调整您的应用程序服务环境更高的计算资源，请参阅此文档中添加此处[配置您的应用程序的服务环境](app-service-web-configure-an-app-service-environment.md)。

![][4]

在此示例中，您可以看到只有两个可用的辅助池。 这是因为 ASE 管理员仅将虚拟机分配到那些两个辅助池。  虚拟机分配到它时将显示第三个。  

### Web 应用程序创建之后 ###

有几个注意事项与运行 web 应用程序和管理中需要考虑到 ASE 的 Asp。  

如前所述，ASE 的所有者负责系统的大小，因此它们也负责确保有足够的容量来承载所需的 Asp。 没有可用的工作人员，如果您将无法创建您的 ASP。  这也是设置为 true 将扩大您的 web 应用程序。  如果您需要多个实例将需要获得您的应用程序服务环境管理，以添加更多的工作人员。

创建了 web 应用程序和 ASP 后是一个好主意，它扩大。  在 ASE 始终需要有至少 2 个 ASP 您，为您的应用程序提供容错能力。  在 ASE 扩展 ASP 是正常通过 ASP 用户界面相同。  对于周围扩展的更多详细信息读取文档这里[如何扩展 web 应用程序的应用程序服务环境中](app-service-web-scale-a-web-app-in-an-app-service-environment.md)

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspnewwebapp.png
[2]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createasplocation.png
[3]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspselected.png
[4]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspworkerpool.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment
 
测试
