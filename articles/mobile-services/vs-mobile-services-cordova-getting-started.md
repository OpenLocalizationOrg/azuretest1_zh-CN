---
ms.openlocfilehash: f0cb8ff05ad9602397f3b724f418f76e7d7ee5be
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="" 
    description="描述您可以开始使用 Azure 移动服务 Cordova 项目中的第一步" 
    services="mobile-services" 
    documentationCenter="" 
    authors="patshea123" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="vs-getting-started" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/12/2015" 
    ms.author="patshea"/>

# 要开始使用移动服务 （Cordova 项目）

> [AZURE.SELECTOR]
> - [入门教程](vs-mobile-services-cordova-getting-started.md)
> - [发生了什么事](vs-mobile-services-cordova-what-happened.md)

##第一步
哪种类型的移动服务连接到您取决于您需要为了遵循这些示例中的代码执行的第一步。

- 对于 JavaScript 后端移动服务，创建名为 TodoItem 的表。  创建表，定位移动节点下的 Azure 服务在服务器资源管理器中，右键单击移动服务的节点，可以打开上下文菜单，选择**创建表**。 输入表名为"TodoItem"。

- 如果您有.NET 后端移动服务，尚 TodoItem 表中的默认项目模板的 Visual Studio 创建为您，但您需要将其发布到 Azure。 若要发布它，请在解决方案资源管理器中打开的移动服务项目的上下文菜单，选择**发布网站**。 接受默认设置，然后选择**发布**按钮。



##创建参考表

下面的代码获取 TodoItem，可以在后续操作中使用读取和更新数据的表中包含数据的表的引用。 创建移动服务时，将自动创建的 TodoItem 表。

    var todoTable = mobileServiceClient.getTable('TodoItem');

有关使用这些示例，表上的权限必须设置为**应用程序键的任何人**。 稍后，您可以设置身份验证。 请参阅[开始使用身份验证](mobile-services-html-get-started-users.md)。

##向表中添加项

将新项插入到数据表。 Id (GUID 的字符串类型) 自动创建新行的主键。 **完成**方法调用返回的[承诺](https://msdn.microsoft.com/library/dn802826.aspx)对象，以获取插入的对象的一个副本，并处理任何错误。

    function TodoItem(text) {
        this.text = text;
        this.complete = false;
    }
    
    var items = new Array();
    var insertTodoItem = function (todoItem) {
        todoTable.insert(todoItem).done(function (item) {
            items.push(item)
        });
    };

##读取或查询表

下面的代码查询所有项目，请按文本字段的表。 您可以添加代码来处理成功处理程序中的查询结果。 在这种情况下，会更新本地项的数组。

    todoTable.orderBy('text')
        .read().done(function (results) {
            items = results.slice();
        });

您可以使用 where 方法修改该查询。 下面是一个示例筛选已完成的项目。

    todoTable.where(function () {
            return (this.complete === false);
        })
        .read().done(function (results) {
            items = results.slice();
        });

有关更多示例的查询可以使用，请参阅[查询]((http://msdn.microsoft.com/library/azure/jj613353.aspx))对象。

##更新表项

更新数据表格中的行。 在此代码中，当移动服务的响应，该项目从列表中删除。 **完成**方法调用返回的[承诺](https://msdn.microsoft.com/library/dn802826.aspx)对象，以获取插入的对象的一个副本，并处理任何错误。

    todoTable.update(todoItem).done(function (item) {
        // Update a local collection of items.
        items.splice(items.indexOf(todoItem), 1, item);
    });

##删除表项

删除使用**del**方法模拟运算表中的行。 **完成**方法调用返回的[承诺](https://msdn.microsoft.com/library/dn802826.aspx)对象，以获取插入的对象的一个副本，并处理任何错误。

    todoTable.del(todoItem).done(function (item) {
        items.splice(items.indexOf(todoItem), 1);
    });

[了解有关移动服务的详细信息](http://azure.microsoft.com/documentation/services/mobile-services/)
