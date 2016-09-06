---
ms.openlocfilehash: 2361aefae6a02ce7a21a1b8c331e788e6c61be31
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="与在 Microsoft Azure 应用程序服务上部署 SAP 服务器集成"
    description="学习如何与内部部署 SAP 服务器集成"
    authors="rajeshramabathiran"
    manager="dwrede"
    editor=""
    services="app-service\logic"
    documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/02/2015"
    ms.author="sameerch"/>


# 与内部部署 SAP 服务器集成
使用 SAP 连接器，可以连接到现有的 SAP 服务器的 Azure 应用程序服务 web、 移动设备、 和逻辑的应用程序。 您可以调用 Rfc、 BAPIs、 tRFCs，以及将 IDOCs 发送到 SAP 服务器。

SAP 服务器甚至可以是您防火墙内部的后面。 对于内部部署服务器连接是通过建立混合侦听器，如下所示︰

![混合连接流][1]

云中的 SAP 连接器无法直接连接到防火墙的 SAP 服务器。 该混合侦听器通过允许连接器牢固地建立到 SAP 服务器的连接的中继终结点承载桥梁。


## 与 SAP 集成的不同方法
支持以下操作︰

- RFC 调用
- 调用 TRFC
- BAPI 调用
- 发送 IDoc

## 先决条件
SAP 特定的客户端库都需要客户机安装混合侦听器的位置上并且正在运行。 捕获的精确细节[此处][9]下章节**为 SAP 适配器**。


## 创建一个新的 SAP 适配器
1. 登录到 Microsoft Azure 管理门户。
2. 选择**新建**。
3. 在创建刀片式服务器，选择**计算** > **Azure 市场**。
4. 在市场刀片式服务器，选择**API 的应用程序**，和 SAP 在搜索栏中搜索︰

    ![SAP 接口 API 的应用程序][2]
5. 选择由 Microsoft 发布的**SAP 连接器**。
6. 在 SAP 连接器刀片式服务器，选择**创建**。
7. 在打开新刀片，输入以下命令︰
    1. **位置**-选择您想要部署的连接器的地理位置
    2. **预订**-选择您想要在中创建此连接器的订阅
    3. **资源组**的选择或创建资源组连接器所在的位置
    4. **Web 托管计划**-选择或创建 web 宿主计划
    5. **定价层**-选择连接器定价层
    6. **名称**-输入 SAP 连接器的名称
    7. **程序包设置**
        - **服务器名称**-输入 SAP 服务器名称。 示例:"SAPserver"或者"SAPserver.mydomain.com"。
        - **用户名称**-输入要连接到 SAP 服务器的有效用户名。
        - **密码**-请输入有效的密码以连接到 SAP 服务器。
        - **系统号**-输入 SAP 应用程序服务器的系统数。
        - **语言**的输入登录使用的语言，如"EN"。 如果没有输入值，则认为"EN"。
        - **内部**的输入 SAP 服务器是否内部防火墙后面。 如果设置为 TRUE，则需要安装侦听器代理可以访问您的 SAP 服务器的服务器上。 您可以转到您的应用程序 API 摘要页并选择混合连接安装代理。
        - **服务总线连接字符串**-如果您的 SAP 服务器是内部输入此参数。 这应该是一个有效的服务总线 Namespace 连接字符串。
        - **Rfc**的进入权由连接器的 SAP 的 Rfc。
        - **TRFCs** -在允许由该连接器的 SAP 中输入 TRFCs。
        - **BAPI** -在允许由该连接器的 SAP 中输入 BAPIs。
        - **IDOCs**的 IDOCs 进入可通过该连接器发送的 SAP。
    8. 选择选择。 在几分钟内创建 SAP 连接器。


## 安装混合侦听器
浏览至**浏览**通过创建 SAP 连接器 > **API 应用程序** > *您的连接器的名称*

在连接器刀片式服务器，请注意混合连接状态处于挂起状态。 选择混合连接。 打开混合连接刀片式服务器︰

![混合连接刀片式服务器][3]

复制主网关配置字符串。 您以后使用它作为混合监听器安装安装程序的一部分。

选择**下载和配置**的链接，并单击运行一次安装程序︰

![单击一次安装程序混合连接。][4]

选择**安装**，然后输入先前复制的网关配置设置︰

![中继侦听连接字符串][5]

选择**安装**以完成混合连接管理器设置︰

![正在安装混合连接管理器][6]

![混合连接管理器的安装已完成][7]

## 验证混合连接
浏览至**浏览**通过创建 SAP 连接器 > **API 应用程序** > *您的连接器的名称*

在连接器刀片式服务器，请注意混合连接状态是*连接*︰

![混合的连接状态的连接][8]


## 在应用程序逻辑中使用 SAP 连接器
创建 SAP 接口后，可以在您的应用程序逻辑的工作流使用它。

创建新的逻辑应用程序通过**新建** > **逻辑应用程序** > **创建**。 逻辑应用程序包括资源组输入元数据。

选择 T**riggers 和操作**。 逻辑应用程序工作流设计器打开。

从右窗格中，选择 SAP 接口，并从操作选项卡中选择操作。

> [AZURE.NOTE] 操作列表基于输入时创建 SAP 连接器的配置。

对于选定的操作，您可以看到输入和输出参数。 您可以在操作中输入输入并使用当前操作的输出在其他 API 的应用程序，可能是进一步在工作流中的决策制定。

<!--Image references-->
[1]: ./media/app-service-logic-integrate-with-an-on-premise-SAP-server/HybridConnectivityFlow.PNG
[2]: ./media/app-service-logic-integrate-with-an-on-premise-SAP-server/SAPConnector.APIApp.PNG
[3]: ./media/app-service-logic-integrate-with-an-on-premise-SAP-server/HybridConnection.PNG
[4]: ./media/app-service-logic-integrate-with-an-on-premise-SAP-server/HybridConnection.ClickOnceInstaller.PNG
[5]: ./media/app-service-logic-integrate-with-an-on-premise-SAP-server/HybridConnection.ClickOnceInstaller.RelayInformation.PNG
[6]: ./media/app-service-logic-integrate-with-an-on-premise-SAP-server/HybridConnectionManager.Install.InProgress.PNG
[7]: ./media/app-service-logic-integrate-with-an-on-premise-SAP-server/HybridConnectionManager.Install.Completed.PNG
[8]: ./media/app-service-logic-integrate-with-an-on-premise-SAP-server/SAPConnector.HybridConnection.Connected.PNG
[9]: http://download.microsoft.com/download/2/D/7/2D7CE8DF-A6C5-45F0-8319-14C3F1F9A0C7/InstallationGuide.htm

测试
