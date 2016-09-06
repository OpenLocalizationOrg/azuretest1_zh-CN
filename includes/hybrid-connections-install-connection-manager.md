---
ms.openlocfilehash: 86150467d8e12680cb609fc9f583edcb93ad28df
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

混合连接管理器使您内部的计算机连接到 Azure 和 TCP 通信中继。 必须管理器安装到一台内部计算机可以连接到 SQL Server 实例。

1. 您刚创建的连接应具有**状态**的**premesis 上安装不完整**。 单击此连接，单击**本地设置**。

    ![内部部署安装程序](./media/hybrid-connections-install-connection-manager/5-1.png)

2. 单击**安装和配置**。

    这将安装自定义连接管理器的实例，它已经预先配置为使用您刚才创建的混合连接。

3. 完成其余的安装步骤用于连接管理器。

    安装完毕后，混合连接状态将变为**1 实例连接**。 您可能需要刷新浏览器，然后等待几分钟。 

现在，混合连接设置已完成。