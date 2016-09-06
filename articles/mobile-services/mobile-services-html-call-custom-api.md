---
ms.openlocfilehash: 608a0bea0f702cacf582e00dfc46c15b23aa41ea
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="从 HTML 客户端-移动服务调用一个自定义的 API"
    description="了解如何定义一个自定义的 API，然后调用它从一个 HTML 应用程序，使用 Azure 移动服务。"
    services="mobile-services"
    documentationCenter=""
    authors="ggailey777"  
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html" 
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="06/16/2015"
    ms.author="glenga"/>

# 从一个 HTML 应用程序调用一个自定义的 API

[AZURE.INCLUDE [mobile-services-selector-call-custom-api](../../includes/mobile-services-selector-call-custom-api.md)]

本主题演示如何从一个 HTML 应用程序调用一个自定义的 API。 自定义的 API 使您能够定义自定义终结点公开服务器功能的不映射到插入、 更新、 删除或读取操作。 通过使用自定义的 API，您可以更好地控制消息，包括读取和设置 HTTP 消息头定义非 JSON 消息正文格式。

本主题中创建的自定义 API 使您能够发送一个 POST 请求，在已完成的标志设置为`true`的表中的所有 todo 项。 如果没有此自定义的 API，客户端必须发送单个请求来更新表中每个 todo 项标志。

本教程基于移动服务快速入门。 在开始本教程之前，您必须首先完成[移动服务入门]或[添加到现有应用程序的移动服务]。

## <a name="define-custom-api"></a>定义自定义的 API

[AZURE.INCLUDE [mobile-services-create-custom-api](../../includes/mobile-services-create-custom-api.md)]

##<a name="update-app"></a>更新应用程序以调用自定义的 API

1. 使用文本编辑器，打开 index.html 文件，找到名为的**按钮**元素`buttonRefresh`，和它后面添加下面的新元素︰

        <button id="buttonCompleteAll">Complete All</button>

    这向该页添加一个新按钮。

2. 在 page.js，和**refreshTodoItems**函数之后，添加以下代码︰

        var completeAllTodoItems = function () {
            // Asynchronously call the custom API using the POST method.
            client.invokeApi("completeall", {
                body: null,
                method: "post"
            }).done(function (results) {
                var message = results.result.count + " item(s) marked as complete.";
                alert(message);
                refreshTodoItems();
            }, function(error) {
                alert(error.message);
            });
        };

        $('#buttonCompleteAll').click(function () {
            completeAllTodoItems();
        });

    此方法处理了新建按钮的**Click**事件。 在客户端，将 POST 请求发送到新的自定义 API 调用**invokeApi**方法。 由自定义的 API 返回的结果将显示在消息对话框中，如任何错误。

## <a name="test-app"></a>测试应用程序

1. 请刷新浏览器。

2. 在应用程序中，在**插入 TodoItem**，请键入一些文本，然后单击**保存**。

3. 直到几个 todo 项添加到列表，请重复上述步骤。

4. 单击**全部完成**按钮。 指示标记为已完成的项目数，将显示一个消息对话框，筛选执行查询，以清除列表中的所有项目。

## 下一步行动

本主题介绍了如何使用**invokeApi**函数来调用 HTML/JavaScript 应用程序从一个非常简单的自定义 API。 若要了解有关使用**invokeApi**函数的详细信息，请参阅发布[自定义 API 在 Azure 移动服务](http://blogs.msdn.com/b/carlosfigueira/archive/2013/06/19/custom-api-in-azure-mobile-services-client-sdks.aspx)。  

另外，还要考虑找出更多的以下移动服务主题︰

* [移动服务服务器脚本引用]
  <br/>了解有关创建自定义的 Api 的详细信息。

* [在源代码管理存储服务器脚本]
  <br/> 了解如何使用源代码控制功能可以更容易地和安全地制定并发布 API 的自定义脚本代码。

<!-- Anchors. -->
[定义自定义的 API]: #define-custom-api
[更新应用程序以调用自定义的 API]: #update-app
[测试应用程序]: #test-app
[下一步行动]: #next-steps

<!-- URLs. -->
[移动服务服务器脚本引用]: http://go.microsoft.com/fwlink/?LinkId=262293
[开始使用移动服务]: mobile-services-html-get-started.md
[添加到现有应用程序的移动服务]: mobile-services-html-get-started-data.md
[在源代码管理存储服务器脚本]: mobile-services-store-scripts-source-control.md

测试
