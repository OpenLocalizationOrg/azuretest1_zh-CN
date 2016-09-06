---
ms.openlocfilehash: 36b2abf3a0ecd7cd8e3c3fafd1a9c1b4ca52b142
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
## 什么是队列存储？

Azure 队列存储是一种服务来存储大量的邮件，可以从任何位置访问世界上通过身份验证的调用使用 HTTP 或 HTTPS。 单个队列消息可达 64 KB 的大小，并且队列可以包含数以百万计的存储帐户的总容量限制的邮件。 存储帐户可以包含多达 500 TB 的 blob、 队列和表数据。 有关帐户的存储容量的详细信息，请参阅[Azure 存储可扩展性和性能目标](http://msdn.microsoft.com/library/azure/dn249410.aspx)。

队列存储的常见用途包括︰

-   创建异步处理的工作的积压
-   将邮件从 Azure Web 角色传递到 Azure 辅助角色

## 队列服务概念

该队列服务包含以下组件︰

![Queue1](./media/storage-queue-concepts-include/queue1.png)


- **的 URL 格式︰**队列是可寻址使用以下 URL 格式︰   
    http://`<storage account>`.queue.core.windows.net/`<queue>` 
      
    下面的 URL 地址在图中的队列︰  
        
        http://myaccount.queue.core.windows.net/imagesToDownload

- **存储帐户︰**对 Azure 存储的所有访问都是通过存储帐户。 有关帐户的存储容量的详细信息，请参阅[Azure 存储可扩展性和性能目标](../articles/storage/storage-scalability-targets.md)。

- **队列︰**队列中包含一的组消息。 所有的邮件必须在队列中。

- **消息︰**任何格式，达 64 KB 的邮件。
