---
ms.openlocfilehash: f147f60ee474aa88a2d0a2dc8900a675a77012ec
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="服务端授权的.NET 后端移动服务中的用户 |Microsoft Azure"
    description="了解如何将访问权限限制为授权用户使用在.NET 后端移动服务"
    services="mobile-services"
    documentationCenter="windows"
    authors="krisragh"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.topic="article"
    ms.devlang="dotnet"
    ms.date="07/02/2015"
    ms.author="krisragh"/>

# 中移动服务的用户的服务端授权

> [AZURE。选择器列表 (平台 |后端）]
- [(任何 |.NET)](mobile-services-dotnet-backend-service-side-authorization.md)
- [(任何 |) Javascript](mobile-services-javascript-backend-service-side-authorization.md)

本主题演示如何使用服务器端逻辑来授权用户。  在本教程中，您修改表控制器、 筛选基于用户 Id 的查询和授予用户访问自己的数据。 筛选用户的用户 id 的查询结果是授权的最基本形式。 这取决于您特定的情况下，您可能还要创建用户或角色的表来跟踪更详细的用户身份验证信息，如哪些终结点指定的用户有权访问。

本教程中基于移动快速启动服务以及基于[添加到现有的移动服务应用程序的身份验证]本教程。 请先完成[添加到现有的移动服务应用程序的身份验证]。

## <a name="register-scripts"></a>修改数据访问方法

1. 在 Visual Studio，打开移动项目，展开 DataObjects 文件夹中，并打开**TodoItem.cs**。 **TodoItem**类定义的数据对象，并且您需要添加要用于筛选的**用户 Id**属性。 **TodoItem**类中加入以下新的用户 Id 属性︰

        public string UserId { get; set; }

    >[AZURE.NOTE] 若要使此数据模型更改和维护数据库中的现有数据，则必须使用[代码第一个迁移](mobile-services-dotnet-backend-how-to-use-code-first-migrations.md)。

2. 在 Visual Studio 中，展开控制器文件夹，请打开**TodoItemController.cs**然后添加以下 using 语句︰ 

        using Microsoft.Azure.Mobile.Server.Security;

3. 找到的**PostTodoItem**方法，该方法的开始处添加以下代码。 

        // Get the logged in user
        var currentUser = User as ServiceUser;
    
        // Set the user ID on the item
        item.UserId = currentUser.Id;
    
    插入到 TodoItem 表中之前，此代码会将身份验证的用户的用户 ID 添加到项。

3. 找到的**GetAllTodoItems**方法和现有的**return**语句替换为以下代码行︰ 

        // Get the logged in user
        var currentUser = User as ServiceUser;

        return Query().Where(todo => todo.UserId == currentUser.Id);
        
    此查询筛选返回的 TodoItem 对象，这样每个用户只接收它们插入的项目。

4. 重新发布到 Azure 的移动服务项目。


## <a name="test-app"></a>测试应用程序

1. 请注意当您现在运行客户端应用程序，尽管有项目已经从前面的教程数据库中，将返回任何项。 这是因为以前的项插入用户 ID 列没有，现在具有 null 值。

2. 如果您有额外的登录帐户，请验证用户只可以通过关闭并删除该应用程序并再次运行它查看他们自己的数据。 显示登录凭据对话框时，请输入不同的登录和验证输入下先前登录的项将不显示。



<!-- Anchors. -->
[注册服务器脚本]: #register-scripts
[下一步行动]:#next-steps

<!-- Images. -->

[3]: ./media/mobile-services-dotnet-backend-ios-authorize-users-in-scripts/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[添加到现有的移动服务应用程序的身份验证]: mobile-services-dotnet-backend-ios-get-started-users.md
 

测试
