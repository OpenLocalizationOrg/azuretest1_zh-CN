---
ms.openlocfilehash: b35dc334a96b22035a3cfe22e1d61d43abf12bfe
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
创建可用性组侦听器后，可能需要调整该侦听器资源的**RegisterAllProvidersIP**和**HostRecordTTL**群集参数。  这些参数可能会减少重新连接的时间，这可能会阻止连接的超时故障切换后。 这些参数，以及示例代码的详细信息，请参阅[创建或配置可用性组侦听器](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover)。
