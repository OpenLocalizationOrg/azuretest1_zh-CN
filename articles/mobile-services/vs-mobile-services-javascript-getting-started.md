---
ms.openlocfilehash: de95549be0626aa793ba0a6fd7fd60a555b811af
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="" 
    description="如何在 Visual Studio 中的 JavaScript 项目开始使用移动服务" 
    services="mobile-services" 
    documentationCenter="" 
    authors="patshea123" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="vs-getting-started" 
    ms.devlang="JavaScript" 
    ms.topic="article" 
    ms.date="08/12/2015" 
    ms.author="patshea"/>

# 移动服务入门

> [AZURE.SELECTOR]
> - [入门教程](vs-mobile-services-javascript-getting-started.md)
> - [发生了什么事](vs-mobile-services-javascript-what-happened.md)

哪种类型的移动服务连接到您取决于您需要为了遵循这些示例中的代码执行的第一步。

对于 JavaScript 后端移动服务，创建名为 TodoItem 的表。  创建表，定位移动节点下的 Azure 服务在服务器资源管理器中，右键单击移动服务的节点，可以打开上下文菜单，选择**创建表**。 输入表名为"TodoItem"。

如果改为有.NET 后端移动服务，尚 TodoItem 表中的默认项目模板的 Visual Studio 创建为您，但您需要将其发布到 Azure。 若要发布它，请在解决方案资源管理器中打开的移动服务项目的上下文菜单，选择**发布网站**。 接受默认设置，然后选择**发布**按钮。

##获得对表的引用

客户端对象已添加到您的项目。  它的名称是您使用"客户端"追加到该日志的移动服务的名称。 下面的代码获取 TodoItem，可以在后续操作中使用读取和更新数据的表中包含数据的表的引用。

    var todoTable = yourMobileServiceClient.getTable('TodoItem');

##添加条目 

将新项插入到数据表。 Id (GUID 的字符串类型) 自动创建新行的主键。 不变 id 列的类型，因为移动服务基础结构会使用它。

    var todoTable = client.getTable('TodoItem');
    var todoItems = new WinJS.Binding.List();
    var insertTodoItem = function (todoItem) {
        todoTable.insert(todoItem).done(function (item) {
            todoItems.push(item);
        });
    };

##读取/查询表

下面的代码查询所有物料表、 更新本地集合并将结果绑定到用户界面元素列表项目。

        // This code refreshes the entries in the list view 
        // by querying the TodoItems table.
        todoTable.where()
            .read()
            .done(function (results) {
                todoItems = new WinJS.Binding.List(results);
                listItems.winControl.itemDataSource = todoItems.dataSource;
            });

您可以使用 where 方法修改该查询。 下面是一个示例筛选已完成的项目。

    todoTable.where(function () {
        return (this.complete === false && this.createdAt !== null);
    })
    .read()
    .done(function (results) {
        todoItems = new WinJS.Binding.List(results);
        listItems.winControl.itemDataSource = todoItems.dataSource;
    });

查询可以使用的更多示例，请参阅[查询对象](http://msdn.microsoft.com/library/azure/jj613353.aspx)。

##更新条目

更新数据表格中的行。 在此示例中， *todoItem*是更新后的项目中，并从移动服务中返回*项*是同一项。 当移动服务响应时，请使用[拼接](http://msdn.microsoft.com/library/windows/apps/Hh700810.aspx)方法的本地 todoItems 列表中更新项目。 **完成**方法调用返回的[承诺](https://msdn.microsoft.com/library/dn802826.aspx)对象，以获取插入的对象的一个副本，并处理任何错误。

        todoTable.update(todoItem).done(function (item) {
            todoItems.splice(todoItems.indexOf(item), 1, item);
        });

#####删除条目

删除数据表中的行。 [完成]()方法调用返回的[承诺](https://msdn.microsoft.com/library/dn802826.aspx)对象，以获取插入的对象的一个副本，并处理任何错误。

    todoTable.del(todoItem).done(function (item) {
        todoItems.splice(todoItems.indexOf(item), 1);
    }



[了解有关移动服务的详细信息](http://azure.microsoft.com/documentation/services/mobile-services/) 
