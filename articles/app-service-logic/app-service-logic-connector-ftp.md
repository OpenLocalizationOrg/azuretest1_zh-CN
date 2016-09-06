---
ms.openlocfilehash: 19a9894b4a643c8c35df635f991781718538abe4
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="在应用程序逻辑中使用 FTP 接口 |Microsoft Azure 应用程序服务"
    description="如何创建和配置 FTP 接口或 API 的应用程序并在 Azure 应用程序服务中的一个逻辑应用程序中使用它"
    authors="rajram"
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
    ms.date="08/23/2015"
    ms.author="rajram"/>

# 开始使用 FTP 连接器并将其添加到您的逻辑应用程序
连接到 FTP 服务器移动数据或文件。 FTP 接头的关键功能包括︰

- 从 FTP 服务器根据需要提取文件
- 基于可配置的时间表运行轮询
- 轮询 FTP 服务器并触发 FTP 服务器中的新文档所基于的逻辑流程
- 指定 FTP 服务器的 IP 地址、 端口、 密码和主机名
- 能够运行按需发送
- 删除 FTP 服务器上根据需要的文件的能力

对业务工作流程和过程数据作为逻辑应用程序在此工作流的一部分，您可以添加 FTP 连接器。 

## 创建新的 FTP 连接器
若要创建新的 FTP 接口，按照下列步骤操作。
- 启动 Azure 门户
- 打开 Azure 市场使用 + （页面底部） 新建-> Web + 移动--> Azure 市场︰  
![启动 Azure 的市场][1]

- 在 API 的应用程序上单击
- 对于 FTP，搜索并选择 FTP 连接器︰  
![选择 FTP 连接器][2]

- 在上的单击创建
- 打开 FTP 连接器刀片中, 提供了以下数据︰  
![创建 FTP 连接器][3]

- **位置**-选择您想要进行部署的连接器的地理位置
- **预订**-选择您想要在中创建此连接器的订阅
- **资源组**的选择或创建资源组连接器所在的位置
- **Web 托管计划**-选择或创建 web 宿主计划
- **定价层**-选择连接器定价层
- **名称**-一个 FTP 连接器的名称为
- **程序包设置**
    - **服务器地址**-指定的 FTP 服务器名称或 IP 地址
    - **用户名**-指定要连接到 FTP 服务器的用户名
    - **密码**-指定的密码连接到 FTP 服务器
    - **根文件夹**-指定的根文件夹路径
    - **使用二进制**-指定为 true，则使用二进制传输模式，对于 ASCII
    - **使用 SSL** -指定为 true，则使用 FTP 通过安全的 SSL/TLS 通道
    - **服务器端口**-指定的 FTP 服务器端口号
- 在上的单击创建。 将创建新的 FTP 连接器。

## 在应用程序逻辑中使用 FTP 连接器
一旦创建 FTP 接口，可以从流使用它。

创建新的流通过 + 新-> 移动 Web + LogicApp->。 流程包括资源组提供的元数据︰  
![创建逻辑应用程序][4]

单击*触发器和操作*。 打开流设计器︰  
![逻辑应用程序空流设计器][5]

FTP 连接器可以用作触发器和操作。

### 触发器
在空流设计器中，单击 FTP 连接器从右库窗格中︰  
![选择 FTP 触发器][6]

FTP 连接器有一个触发器的 ' 文件可用 （读取然后删除）。 此触发器︰

- 轮询新文件的文件夹路径
- 实例化时为每个新的文件的逻辑流程
- 从文件夹路径中删除文件之后被实例化的逻辑流程

单击文件可用 （读取然后删除） 触发器︰  
![基本输入 FTP 触发器][7]

输入将帮助您配置要在计划的频率进行轮询特定文件夹路径。 基本的输入
- 频率-指定 FTP 轮询的频率
- 时间间隔-指定计划的频率的间隔
- 文件夹路径的 FTP 服务器中指定的文件夹路径
- 类型的文件-指定该文件类型是否是文本或二进制

单击椭圆上的... 显示高级的输入︰  
![基本输入 FTP 触发器][8]

高级的初始信息包括︰
- 文件掩码-指定轮询时的文件掩码
- 排除文件掩码 — 指定轮询时排除的文件掩码

提供输入，然后单击复选标记以完成输入的配置上︰  
![基本输入 FTP 触发器][9]

请注意，配置的 FTP 触发器显示两个输入参数和输出配置。

#### 在后续操作中使用 FTP 触发器的输出
FTP 连接器的输出可以用作流程中的一些其他操作的输入。

您可以单击... 在输入对话框中操作并选择从下拉框中直接输出的 FTP。

您还可以在操作的输入框中直接编写表达式。 下面给出了流表达式来引用 ftp 触发器的输出︰

    @triggers('ftpconnector').outputs.body.Content

### 操作
单击右侧窗格中从 FTP 连接器上。 按下操作支持 FTP 连接器列表︰  
![FTP 操作列表][10]

FTP 接口支持以下操作︰

- **获取文件**的获取特定文件的内容
- **上载文件**-将文件上载到 FTP 文件夹路径
- **删除文件**的 FTP 文件夹路径中删除文件
- **文件列表**-列出 FTP 文件夹路径中的所有文件

让我们看一个示例 — 上载文件。 单击上载文件。

首先显示基本的输入︰  
![上载文件操作的基本输入][11]


- **内容**-指定要上载的文件的内容
- **内容传输编码**-指定 none 或 base64
- **文件路径**-指定要上载的文件的文件路径

单击...对于高级的输入︰  
![上载文件操作的基本输入][12]


- **将追加如果存在**-真或假追加如果存在。 启用时，将数据追加到文件，如果有的话。 当禁用时，如果存在此文件会覆盖
- **临时文件夹**的可选。 如果提供，适配器将上载到临时文件夹路径文件，该文件完成上载后将移动到文件夹路径。 临时文件夹路径应为文件夹路径，以确保在移动操作是原子相同的物理磁盘上。 只有在禁用追加是否存在属性时，可以使用临时文件夹。

提供输入，然后单击复选标记以完成输入的配置上︰  
![已配置上载文件操作][13]

文件路径参数设置为︰

    @concat('/Output/',triggers().outputs.body.FileName)

请注意，配置的 FTP 上载文件操作显示两个输入参数为输出参数。

#### 使用上一个操作的输出作为输入 FTP 操作
请注意，在配置屏幕快照中，内容是值设置为的表达式。

    @triggers().outputs.body.Content


您可以将其设置为所需的任何值。 这只是一个示例。 表达式将逻辑应用触发器的输出，并使用它作为该文件的内容上载。 假设您要使用一步操作的输出，可以说转换。 在这种情况下，将该表达式

    @actions('transformservice').outputs.body.OutputXML

## 用做更多您的连接器
创建连接器时，可以将其添加到业务工作流使用的逻辑应用程序中。 请参阅[逻辑应用程序是什么？](app-service-logic-what-are-logic-apps.md)。

查看 Swagger REST API 参考[连接器](http://go.microsoft.com/fwlink/p/?LinkId=529766)和应用程序的 API 参考。

您还可以查看性能统计信息和控制安全接头。 请参阅[管理和监视您的内置 API 的应用程序和连接器](app-service-logic-monitor-your-connectors.md)。

<!-- Image reference -->
[1]: ./media/app-service-logic-connector-ftp/LaunchAzureMarketplace.PNG
[2]: ./media/app-service-logic-connector-ftp/SelectFTPConnector.PNG
[3]: ./media/app-service-logic-connector-ftp/CreateFTPConnector.PNG
[4]: ./media/app-service-logic-connector-ftp/CreateLogicApp.PNG
[5]: ./media/app-service-logic-connector-ftp/LogicAppEmptyFlowDesigner.PNG
[6]: ./media/app-service-logic-connector-ftp/ChooseFTPTrigger.PNG
[7]: ./media/app-service-logic-connector-ftp/BasicInputsFTPTrigger.PNG
[8]: ./media/app-service-logic-connector-ftp/AdvancedInputsFTPTrigger.PNG
[9]: ./media/app-service-logic-connector-ftp/ConfiguredFTPTrigger.PNG
[10]: ./media/app-service-logic-connector-ftp/ListOfFTPActions.PNG
[11]: ./media/app-service-logic-connector-ftp/BasicInputsUploadFile.PNG
[12]: ./media/app-service-logic-connector-ftp/AdvancedInputsUploadFile.PNG
[13]: ./media/app-service-logic-connector-ftp/ConfiguredUploadFile.PNG
 

测试
