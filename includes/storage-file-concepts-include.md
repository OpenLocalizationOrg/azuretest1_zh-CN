---
ms.openlocfilehash: 74f9dbb4c1fad78816c90497a41c4cbaa15c529f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
## 什么是 Azure 文件存储？

文件存储提供了使用标准的 SMB 2.1 协议的应用程序的共享的存储。 Microsoft Azure 的虚拟机和云服务可以跨应用程序组件通过装载的共享，共享文件数据和内部部署的应用程序可以访问文件存储 API 通过共享中的文件数据。

云服务 Azure 的虚拟机中运行的应用程序可以装载文件存储的共享访问文件数据，就像桌面应用程序将装入一个典型的 SMB 共享。 任意数量的 Azure 的虚拟机或角色可以装载并同时访问文件存储共享。

由于文件存储共享标准的 SMB 2.1 文件共享，在 Azure 上运行的应用程序可以访问文件 I/O Api 通过共享中的数据。 因此，开发人员可以利用他们现有的代码和将现有应用程序迁移的技能。 IT 专业人员可以使用 PowerShell cmdlet 创建、 装载和 Azure 应用程序的管理的一部分管理文件存储共享。 本指南将向这两个示例。

文件存储的常见用途包括︰

- 迁移部署的应用程序依赖于文件共享在 Azure 的虚拟机上运行或云服务，而无需昂贵的重写
- 存储共享应用程序设置，例如在配置文件中
- 如存储诊断数据日志、 指标和崩溃转储在共享位置 
- 存储工具和实用程序所需的开发或管理 Azure 的虚拟机或云服务

## 文件存储概念

文件存储包含以下组件︰

![文件概念][files-concepts]

-   **存储帐户︰**对 Azure 存储的所有访问都是通过存储帐户。 有关帐户的存储容量的详细信息，请参阅[Azure 存储可扩展性和性能目标](http://msdn.microsoft.com/library/azure/dn249410.aspx)。

-   **共享︰**文件存储共享是 Azure 中的 SMB 2.1 文件共享。 
    必须在父共享中创建所有目录和文件。 帐户可以包含任意的数量的共享，并且共享可以存储任意的数量的文件的存储帐户的容量限制。

-   **目录︰**目录的可选层次结构。 

-   **文件︰**此共享中的文件。 文件可能已达 1TB 的大小。

-   **的 URL 格式︰**文件是可寻址使用以下 URL 格式︰   
    https://`<storage
    account>`.file.core.windows.net/`<share>`/`<directory/directories>`/`<file>`  
    
    下面的示例 URL 无法用于地址在上图中的文件之一︰  
    `http://samples.file.core.windows.net/logs/CustomLogs/Log1.txt`

有关如何命名的共享、 目录和文件的详细信息，请参阅[命名和引用共享、 目录、 文件和元数据](http://msdn.microsoft.com/library/azure/dn167011.aspx)。

[文件概念]: ./media/storage-file-concepts-include/files-concepts.png