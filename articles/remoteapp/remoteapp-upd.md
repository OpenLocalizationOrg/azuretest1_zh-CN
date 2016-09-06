---
ms.openlocfilehash: 1b46b09e445a25b3289aaff1868244c48b46204f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties 
    pageTitle="在 Azure 远程应用程序的用户配置文件数据"
    description="了解如何存储和访问在 Azure RemoteApp 用户数据"
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/12/2015" 
    ms.author="elizapo" />



# Azure RemoteApp 如何保存用户数据和设置？

Azure RemoteApp 设备和会话之间保存用户标识和自定义项。 此用户数据存储在称为用户配置文件磁盘 (UPD) 的每个用户每个集合磁盘中。 磁盘取决于该用户，并确保用户有一致的体验，而不考虑他们登录。 保存 

用户配置文件的磁盘是否对用户完全透明 — — 用户 （在什么似乎是一个本地驱动器） 将文档保存到他们的文档的文件夹并更改其应用程序设置为普通。 同时，所有的个人设置保持不变时从任何设备连接到 Azure RemoteApp。 用户看到的只是他们在同一位置的数据。

每个 UPD 50 GB 的持久性存储且包含两个用户数据和应用程序设置。 

阅读有关用户配置文件数据。

## 管理员可以如何获取数据？

如果您需要访问您的用户 （用于灾难恢复或如果用户离开了公司） 之一的数据，联系[Azure 远程应用程序](mailto:remoteappforum@microsoft.com)，并收集和用户标识提供的订阅信息。 Azure RemoteApp 团队将为您提供可用于访问数据的 URL 从那里浏览位置和检索所有文档或所需的文件。


## 已备份的数据？

是的我们将保存每个地理位置的用户数据的备份。 数据是只读的和常规的数据会 （联系人 Azure RemoteApp 获取），可以以相同的方式访问主数据中心故障时。

## 用户如何在服务器端上看到 UPD？

每个用户将映射到它们 UPD 服务器上具有其自己的目录︰ c:\Users\username。

## 使用 Outlook 和插入更新的最佳方法是什么？

Azure 的远程应用程序会话之间保存 Outlook 状态 （邮箱，Pst）。 若要启用此功能，我们需要 PST 存储在用户配置文件数据 (c:\users\<用户名 >)。 这是数据，因此，只要不更改位置的默认位置，数据将会话之间保留。

我们还建议您在 Outlook 中使用"缓存"模式下，使用"服务器/在线"的模式来进行搜索。

## 我们可以使用共享的数据的解决方案？
是的使用共享的数据的解决方案-尤其是商业和收存箱 OneDrive Azure 远程应用程序支持。 但是请注意，OneDrive 使用者 （个人版本） 和框中不受支持。

## 什么是重定向？
您可以配置 Azure 远程应用程序，以使用户可以通过设置[重定向](remoteapp-redirection.md)来访问本地设备。 然后，本地设备将能够访问上插入更新的数据。

## 是否可以使用我 UPD 为网络共享？
不能，因为 UPD 不能持久保持。 当用户有效地连接到 Azure RemoteApp UPD 才可用。

## 从集合中删除用户，则删除它们 UPD 吗？

否，删除用户时，我们不会自动删除 UPD-相反，我们将存储数据之前删除该集合。 90 天之后删除该集合，我们将删除所有 UPDs。 

如果您需要从集合中删除 UPD，联系 Azure RemoteApp-我们可以从我们端删除 UPD。

## 是否可以访问我的用户 UPDs （当前或已删除的用户）？

是的如果联系[Azure 远程应用程序](mailto:remoteappforum@microsoft.com)时，我们可以将您设置使用 URL 以访问数据。 将有大约 10 个小时的时间来访问过期之前下载插入更新的任何数据或文件。

## UPDs 提供了脱机？

现在，我们不提供脱机访问为 UPDs，超过 10 个小时 access 窗口上面所述。 这意味着，我们目前没有办法向您提供访问长不足以完成更复杂的任务，如在 UPDs 上运行防病毒软件或访问数据的审核。

## 禁用 UPDs 集合？

是的可以要求 Azure RemoteApp 禁用 UPDs 订阅，但不是能执行该自己。 这意味着 UPDs 将被禁用的订阅中的所有集合。

## 可以限制用户只能将数据保存到系统驱动器

是的但是需要先建立的模板映像中创建集合。 使用以下步骤来阻止到系统驱动器的访问权限︰

1. 在模板映像上运行**gpedit.msc** 。
2. 定位到**用户配置 > 管理模板 > Windows 组件 > 浏览器**。
3. 选择以下选项︰
    - **隐藏在我的电脑中的这些指定的驱动器**
    - **防止访问从本机驱动器**

## 可以设定种子 UPDs？ 我想要一些数据置于插入更新的可用用户签署的第一次。

是的当您创建的模板图像，您可以添加信息，默认配置文件。 然后，该信息将添加到插入更新。

## 是否可以更改的大小取决于要存储多少数据 UPD？

不是，所有的 UPDs 有 50GB 的存储容量。 如果您想要存储的数据量，请尝试下列解决方法︰

1. 禁用 UPDs，找不到。
2. 设置用户可以访问的文件共享。
3. 使用启动脚本加载该文件共享。 有关在 Azure RemoteApp 的启动脚本的详细信息，请参阅下面。
4. 直接用户，将所有数据保存到文件共享。

您还可以为企业使用类似 OneDrive 的数据同步应用程序。

## 如何在 Azure RemoteApp 运行启动脚本？

如果您想要运行启动脚本，首先您要使用的集合模板映像中创建计划的任务。 (此*之前*运行 sysprep。) 

![创建系统任务](./media/remoteapp-upd/upd1.png)

![创建一个在用户登录时运行的系统任务](./media/remoteapp-upd/upd2.png)

已计划的任务将启动您的启动脚本，使用用户的凭据。 计划任务运行每次用户登录时。

![如"在登录"设置任务的触发器](./media/remoteapp-upd/upd3.png)

您还可以使用[基于组策略启动脚本](https://technet.microsoft.com/library/cc779329%28v=ws.10%29.aspx)。 

## 怎样将启动脚本放在 「 开始 」 菜单中？ 这样会起作用吗？

换句话说，可以创建一个.bat 文件，运行一个脚本，配置窗口并将其保存到 c:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp 文件夹中，然后让用户启动远程应用程序会话时运行该脚本？

不对，不是支持这种使用 Azure 远程应用程序，它使用 RDSH，也不支持启动脚本中的开始菜单。

## 可以使用 mstsc.exe （远程桌面程序） 来配置登录脚本？

不对，不支持通过 Azure 远程应用程序。
