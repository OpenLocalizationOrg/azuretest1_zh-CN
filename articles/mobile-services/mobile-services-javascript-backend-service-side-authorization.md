---
ms.openlocfilehash: d9a8199218a231240c9d3b746a41f5ed7944411b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="服务端授权的 JavaScript 后端移动服务中的用户 |Microsoft Azure"
    description="了解如何在 JavaScript 的 Azure 移动服务的后端用户授权"
    services="mobile-services"
    documentationCenter=""
    authors="krisragh"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.topic="article"
    ms.devlang="javascript"
    ms.date="08/25/2015"
    ms.author="krisragh"/>

# 中移动服务的用户的服务端授权

> [AZURE。选择器列表 (平台 |后端）]
- [(任何 |.NET)](mobile-services-dotnet-backend-service-side-authorization.md)
- [(任何 |) Javascript](mobile-services-javascript-backend-service-side-authorization.md)

本主题演示如何使用服务器端脚本来为用户授权。 在本教程中，您使用 Azure 移动服务注册脚本，筛选器查询基于用户 Id，并让他们自己的数据的用户访问。 筛选用户的用户 id 的查询结果是授权的最基本形式。 这取决于您特定的情况下，您可能还要创建用户或角色的表来跟踪更详细的用户身份验证信息，如哪些终结点指定的用户有权访问。

本教程中基于移动快速启动服务以及基于[添加到现有的移动服务应用程序的身份验证]本教程。 请先完成[添加到现有的移动服务应用程序的身份验证]。

## <a name="register-scripts"></a>注册脚本

1. 登录到[Azure 管理门户网站]上，单击**移动服务**，然后单击移动服务上。 单击**数据**选项卡，然后单击**TodoItem**表。

2. 单击**脚本**，选择**插入**操作、 现有脚本替换下面的函数，然后单击**保存**。 

        function insert(item, user, request) {
          item.userId = user.userId;
          request.execute();
        }

    此脚本将已通过身份验证的用户的用户 ID 添加到之前插入的项。

    >[AZURE.NOTE] 请确保已启用该[动态架构](https://msdn.microsoft.com/library/azure/jj193175.aspx)。 否则，不会自动添加*用户 Id*列。 此设置是默认启用的新的移动服务。

3. 同样，下面的函数替换现有的**读取**操作。 此脚本筛选器返回 TodoItem 对象，以便用户可以收到他们插入自己的项目。

        function read(query, user, request) {
           query.where({ userId: user.userId });
           request.execute();
        }

## <a name="test-app"></a>测试应用程序

1. 请注意当您现在运行客户端应用程序，尽管项目中已有的_TodoItem_表中前面的教程，将返回任何项。 这是因为以前的项插入用户 ID 列没有，现在具有 null 值。 验证新添加的项具有关联的用户 Id 值_TodoItem_表中。

2. 如果您有额外的登录帐户，请验证用户只可以通过关闭并删除该应用程序并再次运行它查看他们自己的数据。 显示登录凭据对话框时，请输入不同的登录和验证输入下先前登录的项将不显示。

<!-- Anchors. -->
[注册服务器脚本]: #register-scripts
[下一步行动]:#next-steps

<!-- Images. -->

<!-- URLs. -->

[Windows 推式通知和实时连接]: http://go.microsoft.com/fwlink/p/?LinkID=257677
[移动服务服务器脚本引用]: http://go.microsoft.com/fwlink/p/?LinkId=262293
[我的应用程序的仪表板]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[添加到现有的移动服务应用程序的身份验证]: /develop/mobile/tutorials/get-started-with-users-ios

[Azure 的管理门户]: https://manage.windowsazure.com/
 

测试
