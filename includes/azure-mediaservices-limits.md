---
ms.openlocfilehash: 8f34935a84f068d3b4fc6ece5cbb2b48fe5bbc6f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
资源|默认限制|最大限制
---|---|---
在单个订阅的 azure 媒体服务 (AMS) 帐户||25
每个 AMS 帐户的资产||1000000
每个作业的链接的任务||30
每个任务的资产||50
每个作业的资产||100
每个 AMS 科目的作业 ||50000<sup>2</sup>
唯一一次与资产相关联的定位器||5<sup>4</sup>
每个帐户 AMS 实时信道 </p></td>|5</p></td>|N/A<sup>1</sup>
在停止的状态，每个通道中的程序 </p></td>|50</p></td>|N/A<sup>1</sup>
程序在运行每个通道的状态 </p></td>|3</p></td>|N/A<sup>1</sup>
在运行每个帐户 AMS 的状态中的流终结点</p></td>|2</p></td>|N/A<sup>1</sup>
流式传输流终结点为单位 </p></td>|10 </p></td>|N/A<sup>1</sup>
AMS 帐户编码为单位 </p></td>|25</p></td>|N/A<sup>1</sup>
存储帐户 | |1000<sup>5</sup>

<sup>1</sup>您可以请求打开支持票来更新此配额的限制。 不要创建更多的 AMS 帐户来提高限制，而是提交的支持票。

<sup>2</sup>这个数字包括排队、 已完成、 活动和已取消作业。 它不包含已删除的作业。 您可以删除旧的作业使用**IJob.Delete**或**删除**HTTP 请求。

<sup>3</sup>到列表作业实体发出请求时将每个请求返回最多 1000 元。 如果您需要跟踪的所有已提交的作业，您可以使用顶部/跳过[OData 系统查询选项](http://msdn.microsoft.com/library/gg309461.aspx)中所述。

<sup>4</sup>定位程序不适用于管理每个用户的访问控制。 要为单个用户的不同访问权限，请使用数字版权管理 (DRM) 解决方案。

<sup>5</sup>存储帐户必须在相同的 Azure 订阅。
