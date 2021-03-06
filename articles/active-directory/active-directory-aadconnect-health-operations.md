---
ms.openlocfilehash: a0df27c580c1f8df4de585c3f45f8c47b27d6201
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure AD 连接的健康运营。" 
    description="这是 Azure AD 连接健康页描述了 Azure AD 连接健康部署后可以执行的其他操作。." 
    services="active-directory" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtand"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/14/2015"
    ms.author="billmath"/> 

# Azure AD 连接健康操作

以下主题描述可以使用 Azure AD 连接状况执行的各种操作。

## 启用电子邮件通知
您可以配置 Azure AD 连接健康服务发送电子邮件通知时生成警报，指示您联合身份验证服务的基础结构并不健康。 这会生成警报时，以及当它被标记为已解析。 请按照下面的说明来配置电子邮件通知。 请注意，通过默认电子邮件通知被禁用。 


### 若要启用 Azure 的 AD 连接健康电子邮件通知

1. 打开您希望接收电子邮件通知的服务器场警报刀片式服务器。
2. 单击操作栏上的"通知设置"按钮。
3. 将电子邮件通知开关设置为 ON。
4. 选中该复选框来配置所有全局租户管理员可收到电子邮件通知。
5. 如果您想要接收电子邮件通知，在任何其他电子邮件地址，可以在电子邮件的其他收件人框中进行指定。 若要从该列表中删除电子邮件地址，请右键单击该项并选择删除。
6. 若要完成所做的更改，请单击"保存"。 仅在您选择"保存"后，所有更改将都会影响。






## 从 Azure AD 连接的运行状况服务中删除服务器

在某些情况下，您可能希望删除从被监视的服务器。 请按照下面的说明从 Azure AD 连接的运行状况服务中删除服务器。

在删除服务器时，请注意以下︰

- 此操作将停止从该服务器中收集任何进一步的数据。 此服务器将从监视服务。 后此操作，您将不能查看新警报，此服务器的监视或使用率分析数据。
- 此操作不会卸载或从服务器上删除健康代理。 如果您不具有执行此步骤之前卸载健康代理，您可能会看到错误事件与健康代理服务器上。
- 此操作不会删除已从该服务器中收集的数据。 根据 Microsoft Azure 数据保留策略，将删除该数据。 
- 执行此操作后，如果您想要开始监视同一台服务器，将需要卸载并重新安装在此服务器上的运行状况代理。 


### 从 Azure AD 连接的健康服务中删除服务器

1. 打开刀片式服务器刀片式服务器列表中选择要删除的服务器名称。 
2. 在刀片式服务器，单击操作栏上的"删除"按钮。
3. 确认在确认框中键入服务器的名称中删除服务器的操作。
4. 请单击"删除"按钮。







## 从 Azure AD 连接的运行状况服务中删除某个服务实例

在某些情况下，您可能希望删除某个服务实例。 请按照下面的说明从 Azure AD 连接的运行状况服务中删除某个服务实例。

在删除某个服务实例时，请注意以下︰

- 此操作将删除当前服务实例监视服务。 
- 此操作不会卸载或删除此服务实例的一部分作为被监视的任何的服务器健康代理。 如果您不具有执行此步骤之前卸载健康代理，您可能看到错误事件相关的健康代理的服务器上。 
- 根据 Microsoft Azure 数据保留策略，将删除此服务实例中的所有数据。 
- 之后执行此操作，如果您想要启动监视服务，请卸载并重新安装在将监视的所有服务器上运行状况代理。 执行此操作后，如果您想要开始监视同一台服务器，将需要卸载并重新安装在此服务器上的运行状况代理。


### 若要删除某个服务实例从 Azure AD 连接的健康服务

1. 选择您想要删除的服务标识符 （场名称） 从服务列表刀片式服务器打开服务刀片式服务器。 
2. 在刀片式服务器，单击操作栏上的"删除"按钮。
3. 在确认框中键入以确认该服务名称。 (例如︰ sts.contoso.com) 
4. 请单击"删除"按钮。

## 启用 AD FS 的审核

为了使使用率分析功能来收集数据和分析 Azure AD 连接健康代理需要 AD FS 审核日志中的信息。 默认情况下未启用这些日志。 这仅适用于 AD FS 联合身份验证服务器。 您不需要启用 AD FS 代理服务器或 Web 应用程序代理服务器上的审核。 使用以下过程来启用 AD FS 的审核并找到 AD FS 审核日志。

#### 若要启用审核的 AD FS 2.0

1. 单击**开始**，指向**程序**，指向**管理工具**，然后单击**本地安全策略**。
2. 导航到**安全设置 \ 本地策略 \ 用户权限管理**文件夹，然后双击生成安全审核。
3. 在**本地安全设置**选项卡上，验证列出了 AD FS 2.0 服务帐户。 如果不存在，单击**添加用户或组**并将其添加到列表中，然后单击**确定**。
4. 使用提升的权限打开命令提示符并运行以下命令来启用审核。<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable</code>
5. 关闭本地安全策略，然后再打开管理管理单元中。  若要打开管理管理单元中，单击**开始**，指向**程序**，指向**管理工具**，然后单击 AD FS 2.0 管理。
6. 在操作窗格中，单击编辑联合身份验证服务属性。
7. 在**联合身份验证服务属性**对话框中，单击**事件**选项卡。
8. 选中**成功审核**和**失败审核**复选框。
9. 单击**确定**。

#### 若要启用 Windows Server 2012 R2 上的 AD FS 的审核

1. 通过在任务栏上桌面，打开**服务器管理器**在启动屏幕上或服务器管理器中打开**本地安全策略**，然后单击**工具 / 本地安全策略**。
2. 导航到**安全设置 \ 本地策略 \ 用户权限分配**文件夹，然后双击**生成安全审核**。
3. 在**本地安全设置**选项卡上，验证列出了 AD FS 服务帐户。 如果不存在，单击**添加用户或组**并将其添加到列表中，然后单击**确定**。
4. 使用提升的权限打开命令提示符并运行以下命令来启用审核︰ <code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable.</code>
5. 关闭**本地安全策略**，然后再打开**AD FS 管理**管理单元中 （在服务器管理器中，单击工具，然后选择 AD FS 管理）。
6. 在操作窗格中，单击**编辑联合身份验证服务属性**。
7. 在联合身份验证服务属性对话框中，单击**事件**选项卡。
8. 选中的**成功审核和失败审核**复选框，然后单击**确定**。






#### 若要找到 AD FS 审核日志


1. 打开**事件查看器**。
2. 转到 Windows 日志，然后选择**安全**模式。
3. 在右侧，单击**筛选当前日志**。
4. 在事件源下选择**AD FS 审核**。

![AD FS 审核日志](./media/active-directory-aadconnect-health-requirements/adfsaudit.png)

> [AZURE.WARNING] 如果您有一个组策略，则禁用审核然后 Azure AD 连接健康代理的 AD FS 不能收集的信息。 确保您没有一个组策略，可能会禁用审计。


## 相关的链接

* [Azure AD 连接的运行状况](active-directory-aadconnect-health.md)
* [Azure AD 连接 AD fs 健康代理安装](active-directory-aadconnect-health-agent-install-adfs.md)
* [与 AD FS 使用 Azure AD 连接运行状况](active-directory-aadconnect-health-adfs.md)
* [Azure AD 连接的运行状况常见问题解答](active-directory-aadconnect-health-faq.md)测试
