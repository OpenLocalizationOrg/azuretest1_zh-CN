---
ms.openlocfilehash: 4c78eb12747a325a0cb484e704c36b195b1e9ecd
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
您必须为每个虚拟机承载 Azure 的副本创建负载平衡的端点。 如果您在多个地区有副本，该地区每个副本必须在相同的 VNet 中的同一个云服务。 跨多个 Azure 区域创建可用性组副本需要配置多个 VNets。 配置跨 VNet 连接的详细信息，请参阅[配置的 VNet 与 VNet 连接](../articles/vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)。

1. 在 Azure 的门户中，导航到每个虚拟机承载副本并查看详细信息。

1. 单击虚拟机的每个**终结点**选项卡。

1. 验证**名称**和您要使用的侦听程序终结点的**公用端口**不已经在使用。 在下面的示例中，名称是"MyEndpoint"，该端口是"1433"。

1. 在本地客户端下载并安装[最新的 PowerShell 模块](http://azure.microsoft.com/downloads/)。

1. 启动**Azure PowerShell**。 新 PowerShell 会话时打开加载的 Azure 管理模块。

1. 运行**Get AzurePublishSettingsFile**。 此 cmdlet 将您定向到浏览器的发布设置文件下载到本地目录。 可能会提示您输入您的登录身份凭证 Azure 订阅。

1. 发布设置您下载的文件的路径运行**导入 AzurePublishSettingsFile**的命令︰

        Import-AzurePublishSettingsFile -PublishSettingsFile <PublishSettingsFilePath>

    一旦导入的发布设置文件时，您可以管理 Azure 订阅 PowerShell 会话中。