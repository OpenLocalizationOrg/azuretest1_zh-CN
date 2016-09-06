---
ms.openlocfilehash: 10458ba53c9f323f4e5bf56393e59232c5ae1a45
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="设置您的工作区和管理设置"
    description="了解如何设置您的工作区和管理运营 Microsoft Azure 的见解中的设置"
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
    ms.topic="get-started-article"
    ms.date="08/05/2015"
    ms.author="banders"/>

# 设置您的工作区和管理设置

[AZURE.INCLUDE [operational-insights-note-moms](../../includes/operational-insights-note-moms.md)]

若要创建新的 Microsoft Azure 运营见解工作区，您可以选择一个工作区名称，将其与您的帐户，并且您选择地理位置。 操作的见解区是本质上是一个容器，包括简单的配置信息的帐户和帐户信息。 您或您的组织中的其他成员可以使用多个运营见解工作区来管理不同的收集所有的数据集或 IT 基础结构的一部分。

工作区创建后，可以执行使用工作区的其他任务，如管理运营的见解、 添加解决方案、 连接数据源、 添加日志、 选择存储帐户，在仪表板中查看使用率数据，您可以管理每个工作区设置。

[以分钟为单位 Onboard](./operational-insights-onboard-in-minutes.md)指南向您介绍如何快速获取和运行和本文的其余部分更详细地介绍一些您将需要执行完成入门和管理您的工作区的操作。

我们将讨论您在以下各节中使用的所有常用的任务︰

1. 添加解决方案
2. 将数据源连接
3. 添加和管理日志
4. 管理帐户和用户

![步骤](./media/operational-insights-setup-workspace/steps.png)
## 1 添加解决方案

Microsoft Azure 运营洞察力包括基本配置评估功能，因此不需要安装解决方案以启用它。 但是，可以通过设置或解决方案库中向它添加解决方案实现其他功能的页面。

添加了一种解决方案后，被从您的基础架构中的服务器收集数据并将其发送到运营经验服务。 运营的见解进行处理服务可以花几分钟到几个小时。 该服务处理数据后，可以查看它在运营的见解。

当不再需要时，您可以轻松地删除解决方案。 当您删除一个解决方案时，其数据不发给运营的见解，从而减少您的每日配额使用的数据量。

### 支持通过 Microsoft 监视代理的解决方案

这次直接连接到 Microsoft Azure 运营见解使用 Microsoft 监视代理的服务器可以使用的大部分解决方案可用，包括︰

- [系统更新](operational-insights-updates.md)
- [反恶意软件](operational-insights-antimalware.md)
- [更改跟踪](operational-insights-change-tracking.md)
- [SQL 和 Active Directory 评估](operational-insights-assessment.md)

但是，下面的解决方案都*不*支持向 Microsoft 监控代理，需要系统中心操作管理器 (SCOM)。

- [容量管理](operational-insights-capacity.md)
- [警报管理](operational-insights-alerts.md)
- [配置评估](operational-insights-solutions.md#configuration-assessment)

在这些解决方案中使用运营经理的指导，请参阅[运营的见解与运营经理注意事项](operational-insights-operations-manager.md)。

IIS 日志集合支持使用的计算机上︰

- Windows Server 2012
- Windows Server 2012 R2

### 若要添加解决方案使用设置页

- 选择您想要添加，然后单击**添加所选的解决方案**的解决方案。 不是所有可用的解决方案均那里出现。 如果您想要添加未列出的解决方案，则使用下一个过程。
    ![添加解决方案](./media/operational-insights-setup-workspace/settings-add-sol.png)

### 若要添加使用解决方案库的解决方案

1. 在运营的见解中概述页上，单击**解决方案库**平铺。    
    ![解决方案图标的图像](./media/operational-insights-setup-workspace/sol-gallery.png)
2. 在运营的见解解决方案库页面上，您可以了解每个可用的解决方案。 单击要添加操作的见解到解决方案的名称。
3. 在页，您选择的解决方案，将显示有关该解决方案的详细的信息。 单击**添加**。
4. 在确认页上，单击**接受**同意使用条款和隐私声明。
5. 解决方案将出现在概述页操作的见解，并在可以开始使用它的运营洞察力服务处理数据之后添加一个新图块。

### 若要删除使用解决方案库的解决方案

1. 在运营的见解中概述页上，单击**解决方案库**平铺。
2. 在运营的见解解决方案库页上，您想要删除，请在解决方案下单击**删除**。
3. 在确认页上，单击**是**以删除该解决方案。

## 2 连接数据源

有三种方法可以将数据源连接︰

- 计算机直接连接到操作的见解。 有关更多信息，请参见[连接的计算机直接到运营的见解](./operational-insights-direct-agent.md)。  
    ![直接连接](./media/operational-insights-setup-workspace/attach-directly.png)
- 附加的操作管理器管理组。 有关详细信息，请参阅[连接到系统中心操作管理器从运营经验](./operational-insights-connect-scom.md)。  
    ![附加的操作管理器](./media/operational-insights-setup-workspace/attach-om.png)
- 附加的 Azure 存储帐户。 有关更多信息，请参见[Microsoft Azure 中的服务器中的分析数据](./operational-insights-analyze-data-azure.md)。  
    ![将附加 Azure](./media/operational-insights-setup-workspace/attach-azure.png)

## 3 添加和管理日志

添加日志之前，您需要一个解决方案，它将使用安装的日志数据。 然后，您可以添加新的日志收集事件，并选择哪些事件级别或严重级别所需的日志来收集。 您可以收集︰

- Windows 事件日志
- IIS 日志
- 您已经添加了其他日志

![添加日志](./media/operational-insights-setup-workspace/collect-logs.png)

### IIS 日志文件格式

目前支持的唯一 IIS 日志格式为 W3C。 不要担心-最常见的格式，然后在 IIS 7 和 IIS 8 中的默认格式。 因此，如果您登录以 NCSA 或 IIS 的本机格式，运营的见解将不会领取这些日志根本。 即使在 W3C 格式，您会看到默认情况下记录不是所有的字段。 您可以阅读更多关于在[选择 W3C 日志 (IIS 7) 字段](https://technet.microsoft.com/library/cc754702(v=WS.10).aspx)的格式。


> [AZURE.TIP] 最佳的日志搜索体验，建议选择在 IIS 中使用**日志记录**每个网站的所有日志记录字段。 此外，建议改为新的日志的**日志文件滚动更新**计划**每小时**-使较小的文件将上载到云上，节约带宽。


### Windows 事件日志收集运营经理或直接连接代理

1. 在**概述**页中，单击**设置**界面，然后单击**日志**选项卡。
2. 键入您想要收集的信息的事件日志的名称。 如果您不确定要使用的名称，在**事件查看器**中选择的 Windows 事件日志属性，将该名称复制在**全名**字段中，并将其粘贴在**收集以下事件日志中的事件**中。
3. 单击**+**添加日志。
4. 选择要从中收集日志的严重级别的事件级别。 在此版本中不支持**成功审核**和**失败审核**事件。
5. 对想要收集信息，每个日志重复上述步骤，然后单击**保存**。
6. 事件应在几分钟内，出现运营的见解，然后您可以搜索数据。

### IIS 日志收集运营经理或直接连接代理

1. 在**概述**页中，单击**设置**界面，然后单击**日志**选项卡。
2. 在**日志**选项卡上的**事件日志**，选择**收集从操作管理器日志**。


### 从 Azure 诊断收集 IIS 日志和/或 Windows 事件
这是可从 Azure 管理门户，不运营洞察力门户，在工作区下配置，请转到**存储**选项卡，则可以从该存储帐户的日志收集。

### 配置日志集合之后
配置日志集合后，您的日志收集策略发送到代理，或通过代理和服务管理组开始收集事件。

您可以访问某些初始查看**使用**页收集从被监视的服务器的日志事件的损害。

![使用页上平铺的图像](./media/operational-insights-setup-workspace/usage.png)


## 4 管理帐户和用户
可以管理帐户和用户在设置页面中使用**帐户**选项卡。 那里，您可以执行以下任务。  

![帐户选项卡](./media/operational-insights-setup-workspace/manage-users.png)

## 将用户添加到现有工作区


使用下列步骤将用户或组添加到操作的见解区。 用户或组将能够查看并处理与此工作区相关联的所有警报。

>[AZURE.NOTE] 如果您想要添加的用户或组从 Azure Active Directory 组织帐户，必须首先确保您具有与 Active Directory 域关联操作见解帐户。 请参阅[添加到现有工作区 Azure 活动目录组织](#)。

### 若要将用户添加到现有工作区
1. 在运营的见解，请单击**设置**平铺。
2. 单击**帐户**选项卡。
3. 在**管理用户**部分中，选择要添加的帐户类型︰**组织的帐户**或**Microsoft 帐户**。
    - 如果您选择 Microsoft 帐户，键入与 Microsoft 帐户相关联的用户的电子邮件地址。
    - 如果您选择组织的帐户，您可以输入用户或组的名称或电子邮件别名的一部分，将显示用户和组的列表。 选择用户或组。
        >[AZURE.NOTE] 为获得最佳性能结果，限制于两个单个操作的见解帐户相关联的 Active Directory 组数 — — 一个为管理员和用户。 使用多个组可能会影响性能的运营经验。
7. 选择要添加的用户或组的类型︰**管理员**或**用户**。  
8. 单击**添加**。

  如果要添加 Microsoft 帐户，加入工作区的邀请发送到您提供的电子邮件。 用户追随邀请加入运营的见解中的说明进行操作后，用户可以查看的警报和此操作见解帐户，帐户信息，您将能够查看**设置**页的**帐户**选项卡上的用户信息。
  如果要添加组织帐户，用户将能够立即访问操作的见解。  
  ![邀请](./media/operational-insights-setup-workspace/manage-users04.png)


### 是否需要多少工作区？
工作区被看作 Azure 管理门户中 Azure 资源。

您可以创建新工作区或链接到现有工作区可能打开前面使用系统中心操作管理器，但还没有与 Azure 的订阅 （需付费） 相关联。
工作区代表处收集、 聚合、 分析和数据运营洞察力门户中显示的级别。
您可以选择多个工作区来分离数据从不同的环境和系统;每个操作管理器管理组 （和所有代理） 或单个的虚拟机代理可以每个连接只能有一个工作区。

每个工作区中可以有多个与其关联的用户帐户和每个用户帐户 （Microsoft 客户或组织帐户） 可以进行多个操作的见解工作区访问。
默认情况下，Microsoft 客户或组织创建工作区所使用的帐户将成为工作区的管理员。 管理员然后可以邀请其他 Microsoft 帐户或挑选他 Azure Active Directory 中的用户。

## 将现有工作区链接到 Azure 的订阅

也可以从[microsoft.com/oms](https://microsoft.com/oms)中创建一个工作区。  但是，某些限制存在这些工作区，最显而易见如果您使用的免费的帐户被限制为 500 MB/天的数据上载。  到此工作区中进行更改您需要**链接到 Azure 订阅您现有的工作区**。

>[AZURE.IMPORTANT] 工作区的链接，以便您 Azure 帐户必须已经访问到您想要链接工作区。  换句话说，用来访问 Azure 的门户网站帐户应该是为您访问您运营洞察力的工作区所使用的帐户**相同**。 如果不是这种情况，请参阅[添加到现有工作区的用户](#add-an-azure-active-directory-organization-to-an-existing-workspace)。

1. 登录到 Azure 的管理门户。
2. 在门户的左下角，单击**+ 新建**。
3. 单击**应用程序服务**、 滚动到**运营的见解**并选择它。
4. 单击**快速创建**。
5. 在**帐户**列表中，您应看到您现有的工作区的列表，*还没有*已链接到 Azure 订购。 选择一个帐户。

  >[AZURE.NOTE] 如果看不到的工作区，您想要链接到此处，这意味着 Azure 订购到运营见解工作区不能访问。  您需要为内部运营洞察力区从该帐户授予访问权限。  若要执行此操作，请参阅[添加到现有工作区的用户](#add-a-user-to-an-existing-workspace)。

  ![链接帐户](./media/operational-insights-setup-workspace/link-account.png)
<p>
6. 填写其余字段，然后选择**创建工作区**。

## 将工作区升级到付费的数据计划

有三个工作区数据规划类型操作的见解︰**自由**、**标准**和**特优**。  如果您是*免费*的计划，您可能会击中您数据上限为 500 MB。  您将需要升级才能收集数据超过此限制的工作区与**随增长付费的计划**。 在任何时间您可以转换您的计划类型。  操作的见解定价的详细信息，请参阅[定价详细信息](http://azure.microsoft.com/pricing/operational-insights/)

>[AZURE.IMPORTANT] 当它们会*链接*到 Azure 的订阅，才可更改工作区计划。  如果在 Azure 创建工作区，或者你们*已经*链接您的工作区，您可以忽略此消息。  如果从[opinsights.azure.com](http://opinsights.azure.com)中创建您的工作区，您需要按照链接[到 Azure 订阅现有工作区](#link-an-existing-workspace-to-an-Azure-subscription)的步骤。

### 更改计划类型

在 Azure 管理门户中，导航到您想要升级到运营见解工作区︰

![小数位数](./media/operational-insights-setup-workspace/find-workspace.png)

选择此工作区，然后从屏幕顶部的选项卡中选择**缩放**

![选择缩放](./media/operational-insights-setup-workspace/select-scale.png)

最后，选择您想要升级到，然后单击**保存**的计划。  将会看到所做的更改会反映在门户中，现在能够收集数据超出"免费"的数据线帽。

![选择计划](./media/operational-insights-setup-workspace/plan-select.png)

## 将 Azure 活动目录组织添加到现有工作区

您可以使用 Azure Active Directory 域关联操作的见解区。 这使您可以添加用户从 Active Directory 直接到运营见解工作区而不需要单独的 Microsoft 帐户。

### 若要将 Azure 活动目录组织添加到现有工作区

1. 在运营的见解中设置页上，单击**帐户**，然后单击**添加组织**。
  ![邀请](./media/operational-insights-setup-workspace/add-org.png)
2. 查看有关组织帐户的信息，然后单击**下一步**。
3. 输入您的 Azure Active Directory 域的管理员身份信息，然后单击**登录**。
4. 单击**授予访问权限，**启用操作的理解，以便在 Active Directory 域中使用的标识信息。
  ![链接](./media/operational-insights-setup-workspace/ad-existing01.png)


## 编辑现有用户类型

您可以更改与运营洞察力帐户相关联的用户帐户角色。 您具有以下角色选项︰

 - *管理员*︰ 可以管理用户、 查看并作用于所有的警报，以及添加和删除服务器

 - *用户*︰ 可以查看和操作所有的警报，以及添加和删除服务器

### 若要编辑某个帐户
1. 在运营的见解中的**帐户**选项卡中**设置**页上，选择您想要更改的用户的角色。
2. 单击**确定**。

## 从运营见解工作区删除用户

使用以下步骤从运营见解工作区删除用户。 请注意这不会关闭用户的工作区。 相反，它将删除该用户和工作区之间的关联。 如果用户与多个工作区，该用户将仍能够登录到运营的见解。

### 若要从工作区删除用户

1. 在运营的见解的**帐户**选项卡中**设置**页面上，您想要删除的用户名旁边单击删除。
2. 单击**确定**以确认您要删除该用户。

## 关闭您运营洞察力的工作区

关闭操作的见解工作区时，与工作区相关的所有数据是从服务中删除运营见解不超过 30 天之后关闭工作区。

如果您是管理员，并且有与工作区关联的多个用户，这些用户和工作区之间的关联被破坏。 如果与其他工作区相关联的用户，他们可以继续使用这些其他工作区运行的见解。 但是，如果它们不能与其他工作区关联它们将需要创建新工作区使用运营的见解。

### 若要关闭操作的见解工作区

1. 在运营的见解的**帐户**选项卡中**设置**页上，单击**关闭工作区**。

2. 选择您的工作区，关闭的原因之一，或在文本框中输入一个不同的原因。

3. 单击**关闭工作区**。

## 其他资源
- [在 Azure 的运营洞察力的 IIS 日志格式要求](http://blogs.technet.com/b/momteam/archive/2014/09/19/iis-log-format-requirements-in-system-center-advisor.aspx)
- 请参阅其他哪些数据源和类型的日志社区让我们在我们的[反馈论坛](http://feedback.azure.com/forums/267889-azure-operational-insights/category/88086-log-management-and-log-collection-policy)中实现。
