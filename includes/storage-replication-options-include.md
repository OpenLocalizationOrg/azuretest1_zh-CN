---
ms.openlocfilehash: 564593f9607333be659cce5457714eec7d75251f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
始终复制您的 Microsoft Azure 存储帐户中的数据以确保持续性和高可用性，会议[Azure 存储 SLA](http://azure.microsoft.com/support/legal/sla/)即使面对暂时的硬件故障。 创建存储帐户时，必须选择下面的复制选项之一︰  

- **本地冗余存储 (LRS)。** 本地冗余存储维护三个副本，您的数据。 LRS 中单个区域中的单个设备复制三次。 LRS 保护您的数据，从正常的硬件故障，而不是从单个设备的故障。  
  
    LRS 享受价格折扣。 最大的耐用性，建议使用地理冗余存储，如下所述。


- **区域冗余存储 (ZRS)。** 区域冗余存储维护三个副本，您的数据。 ZRS 复制三次跨两到三个设施，在一个区域内或跨两个地区，提供较高的耐久性比 LRS。 ZRS 可确保您的数据在单个区域内持久。  

    ZRS 提供耐用性的级别高于 LRS;但是，对于最大的耐用性，我们建议使用地理冗余存储，如下所述。  

    > [AZURE.NOTE] ZRS 目前仅为块 blob。
    > 
    > 创建您的存储帐户并选择 ZRS 后，您不能将它用于任何其他类型的复制，反之亦然。 

- **Geo 冗余存储 (GRS)**。 当您创建它时，地理冗余存储默认启用对您的存储帐户中。 GRS 维护数据的六份。 GRS，与您的数据的主区域内复制三次，还可在辅助区域数百英里为主的地区，提供耐用性的最高级别复制三次。 在发生故障时的主要地区，Azure 存储将故障切换到辅助区域。 GRS 可确保您的数据在两个单独的区域是耐用。


- **读取访问地理冗余存储 (RA GRS)**。 读取访问地理冗余存储将数据复制到一个辅助的地理位置，并且还提供对在辅助位置有数据的读访问。 读取访问地理冗余存储允许您从主或辅助位置，访问您的数据的一个位置不可用。

    > [AZURE.IMPORTANT] 您可以更改您的数据的复制方式创建存储帐户后, 除非在创建帐户时指定 ZRS。 但是，请注意您可能遭受其他零星数据传输费用如果从 LRS 切换到 GRS 或 RA GRS。
 
有关存储复制选项的其他详细信息，请参阅[Azure 存储复制](../articles/storage/storage-redundancy.md)。

有关定价的存储帐户复制的信息，请参阅[存储定价详细信息](http://azure.microsoft.com/pricing/details/storage/)。

有关使用 Azure 存储持久性的体系结构的详细信息，请参阅在[Azure 存储 SOSP 纸张](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)。

