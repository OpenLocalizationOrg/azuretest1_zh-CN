---
ms.openlocfilehash: f98970d5d5ef69b1b5cf164d9ded608a8ec9b130
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="板载运营的见解到以分钟为单位 |Microsoft Azure"
    description="了解如何您可以设置与 Azure 运营见解以分钟为单位"
    services="operational-insights"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="operational-insights"
    ms.workload="operational-insights"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/27/2015"
    ms.author="banders"/>

# 板载到 Azure 运营见解以分钟为单位


[AZURE.INCLUDE [operational-insights-note-moms](../../includes/operational-insights-note-moms.md)]

您可以启动并运行 Azure 运营洞察力与以分钟为单位。 选择如何创建操作见解区，类似于一个帐户时，您有两个选项︰

- Microsoft 操作管理套件
- Microsoft Azure 订阅

您还可以使用操作管理套件网站运营管理套件工作区。 或者，您可以使用 Microsoft Azure 订阅创建运营洞察力区。 目前，这两个区是等价的。 两者之间的唯一不同是的名称。 如果您使用 Azure 的订阅，您可以使用该订阅要访问其他 Azure 服务。 无论您使用来创建工作区的方法，您将与 Microsoft 客户或组织帐户创建工作区。

下面是看这个过程︰

![服务过程](./media/operational-insights-onboard-in-minutes/onboard-oms.png)

## 登录使用操作管理套件的三个步骤

1. 转到[操作管理套件](http://microsoft.com/oms)网站，然后单击**免费试用的**。 登录您的 Microsoft 帐户如 Outlook.com，或与组织的帐户，您的公司或教育机构提供的用于 Office 365 或其他 Microsoft 服务。
2. 提供了一个独特的工作区的名称。 工作区是管理数据的存储位置的逻辑容器。 它提供了您对您的组织中的不同团队之间的数据进行分区的方法因为数据是排斥到其工作区。 指定电子邮件地址和您想要您存储的数据的区域。  
    ![创建工作区并将其链接订阅](./media/operational-insights-onboard-in-minutes/create-workspace-link-sub.png)
3. 接下来，您可以创建新的 Azure 订阅或链接到现有的 Azure 订阅。 如果您想继续使用免费的试用版，请单击**不是现在**。

您就可以着手操作管理套件门户。

您可以了解有关如何设置您的工作区和链接现有 Azure 帐户加入工作区创建操作管理套件的[设置您的工作区，并且管理设置](operational-insights-setup-workspace.md)。

## 使用 Microsoft Azure 快速注册

1. 转到[Azure 门户](https://manage.windowsazure.com)并登录，然后在服务列表中，选择**操作的见解**。
    ![Azure 门户](./media/operational-insights-onboard-in-minutes/azure-portal-op-insights.png)
2. 单击**创建工作区**，然后单击**快速创建**，然后**帐户**，输入工作区名称、 选择一个层，然后选择一个位置来存储工作区数据。 如果您有多个订阅，您可以选择哪一个来使用，然后单击**创建工作区**。
    ![Azure 门户](./media/operational-insights-onboard-in-minutes/quick-create.png)
3. 选择的工作区，您创建，然后单击**访问您的运营洞察力帐户**打开操作管理套件网站。
    ![访问帐户](./media/operational-insights-onboard-in-minutes/visit-account.png)
4. 在操作管理套件网站中，输入您的电子邮件地址，然后单击**确认并继续**。 确认电子邮件将发送给您。 打开电子邮件并在其中，单击**确认现在**。
5. 操作管理套件网站显示概述页。 单击**开始**继续。

您就可以着手操作管理套件门户。

您可以了解有关如何设置您的工作区和链接到 Azure 订阅操作管理套件使用设计器创建的现有工作区[设置您的工作区，并且管理设置](operational-insights-setup-workspace.md)。

## 与操作管理套件门户快速入门
选择解决方案并连接到要管理的服务器，单击**开始**拼贴，请按照本节中的步骤操作。  

![创建工作区并将其链接订阅](./media/operational-insights-onboard-in-minutes/get-started.png)  

- 选择您想要使用，然后单击**添加所选的解决方案**的解决方案。  
    ![解决方案](./media/operational-insights-onboard-in-minutes/solutions.png)
- 选择您想如何连接到您的服务器环境，以收集数据︰
    - 连接任何 Windows 服务器或客户端直接安装代理。
    - 系统中心操作管理器可用于附加您的管理组或整个运营经理部署。
    - 使用 Azure 存储帐户配置了 Windows 或 Linux Azure 诊断 VM 扩展。
- 配置至少一个日志来填充您的数据。 可以选择 IIS 日志和/或添加事件日志，然后选择页面底部的**保存**。 对于事件日志可以包括要监视的错误、 警告和信息消息的类型。
    ![解决方案](./media/operational-insights-onboard-in-minutes/logs.png)

## 或者，服务器直接连接到操作管理套件的安装代理
1. 在开始视图中，单击**连接数据源**节点，然后单击**下载 Windows 代理**。 只能安装在 Windows 服务器 2008 SP 1 或更高版本或 Windows 7 SP1 或更高版本的代理。 服务器需要具有 x64 体系结构。
2. 在一个或多个服务器上安装代理。 您可以安装一个通过一个代理或自动化程度更高的方法中使用[自定义脚本](operational-insights-direct-agent.md#configure-the-microsoft-monitoring-agent-optional)，或者您可以使用现有的软件分发解决方案，可能有。
3. 您同意该许可协议并选择安装文件夹后，选择**连接到 Microsoft Azure 运营洞察力的代理**。  
    ![代理安装程序](./media/operational-insights-onboard-in-minutes/agent.png)
4. 在下一个页面上，将要求您为您的工作区 ID 和区键。 下载代理文件的位置在屏幕上显示您的工作区 ID 和密钥。  
    ![连接服务器](./media/operational-insights-onboard-in-minutes/key.png)
5. 在安装期间，您可以单击**高级**（可选） 设置您的代理服务器，并提供身份验证信息。 单击**下一步**按钮以返回工作区信息页面。
6. 单击**下一步**要验证您的工作区 ID 和密钥。 如果发现任何错误，您可以单击**后**可进行校正。 验证您的工作区 ID 和密钥后，请单击**安装**以完成代理安装。
7. 重新登录到操作管理套件门户，然后单击概述页上的**设置**平铺。 代理与操作管理套件服务通信时，将显示一个绿色复选标记图标。 最初，这大约需要 5-10 分钟。

> [AZURE.NOTE] 产能管理和配置评估解决方案当前不支持由服务器直接连接到操作管理套件。

您还可以连接到系统中心操作管理器 2012 SP1 和更高版本的代理。 为此，请选择**连接到系统中心操作管理器代理**。 当您选择选项，您将数据发送到该服务，而无需其他硬件或追加到您的管理组。

您可以阅读更多有关代理直接连接到在[连接的计算机直接到运营洞察力](operational-insights-direct-agent.md)操作管理套件。

## （可选） 将使用系统中心操作管理器的服务器的连接

1. 在操作管理器控制台中，选择**管理**。
2. 展开**操作的见解**节点并选择**操作的见解连接**。
3. 单击右上角向**注册运营的见解到**链接并按照说明进行操作。
4. 完成登记向导后，请单击**添加计算机/组**链接。
5. 在**计算机搜索**对话框中您可以搜索计算机或组通过操作管理器监视。 选择计算机或组添加到板载它们到运营的见解，单击**添加**，然后单击**确定**。 您可以验证操作见解服务转到**使用**图块操作管理套件门户中接收数据。 数据应出现在大约 5-10 分钟。

您可以阅读更多有关连接到在[连接到运营洞察力从系统中心操作管理器](operational-insights-connect-scom.md)操作管理套件的运营经理。

## （可选） 分析在 Microsoft Azure 的云服务中的数据

与操作管理套件，您可以快速搜索事件和 IIS 日志，云服务和虚拟机的 Azure 云服务启用诊断。 通过安装 Microsoft 监控代理，也可以接收其他见解 Azure 的虚拟机。 您可以阅读更多有关如何配置 Azure 环境[分析数据从 Microsoft Azure 中的服务器](operational-insights-analyze-data-azure.md)在使用操作管理套件。


## 下一步行动
- 开始使用[的解决方案](operational-insights-solutions.md)。
- 了解[搜索](operational-insights-search.md)。
- 使用[仪表板](operational-insights-use-dashboards.md)可以保存并显示您的自定义搜索。
