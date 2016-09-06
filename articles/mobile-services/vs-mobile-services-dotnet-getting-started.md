---
ms.openlocfilehash: e207da447d22fe77673e8e820cd33a29337798be
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle=""
    description="如何在 Visual Studio.NET 项目中开始使用 Azure 移动服务"
    services="mobile-services"
    documentationCenter=""
    authors="patshea123"
    manager="douge"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/16/2015" 
    ms.author="patshea123"/>

# 要开始使用移动服务 （.NET 项目）

> [AZURE.SELECTOR]
> - [入门教程](vs-mobile-services-dotnet-getting-started.md)
> - [发生了什么事](vs-mobile-services-dotnet-what-happened.md)

哪种类型的移动服务连接到您取决于您需要为了遵循这些示例中的代码执行的第一步。

- 对于 JavaScript 后端移动服务，创建名为 TodoItem 的表。  创建表，定位移动节点下的 Azure 服务在服务器资源管理器中，右键单击移动服务的节点，可以打开上下文菜单，选择**创建表**。 输入表名为"TodoItem"。

- 如果您有.NET 后端移动服务，尚 TodoItem 表中的默认项目模板的 Visual Studio 创建为您，但您需要将其发布到 Azure。 若要发布它，请在解决方案资源管理器中打开的移动服务项目的上下文菜单，选择**发布网站**。 接受默认设置，然后选择**发布**按钮。

#####获得对表的引用

下面的代码创建参考表 (`todoTable`)，其中包含 TodoItem，可以在后续操作中使用读取和更新数据表格数据。 您将需要 TodoItem 类使用属性将设置为 interpet 移动服务将发送到您的查询响应中的 JSON。

    public class TodoItem
    {
        public string Id { get; set; }

        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }

        [JsonProperty(PropertyName = "complete")]
        public bool Complete { get; set; }
    }

    IMobileServiceTable<TodoItem> todoTable = App.<yourClient>.GetTable<TodoItem>();

如果您的表具有权限设置为**与应用程序键的任何人**，此代码可以正常工作。 如果您更改权限，以保护您的移动服务，您将需要添加用户身份验证支持。 请参阅[开始使用身份验证](mobile-services-dotnet-backend-windows-universal-dotnet-get-started-users.md)。

#####添加一个表项

将新项插入到数据表。

    TodoItem todoItem = new TodoItem() { Text = "My first to do item", Complete = false };
    await todoTable.InsertAsync(todoItem);

#####读取或查询表

下面的代码查询一个表的所有项。 请注意，它返回仅数据，即默认情况下 50 项的第一页。 您可以传递所需，因为它是一个可选参数的页面大小。

    List<TodoItem> items;
    try
    {
        // Query that returns all items.
        items = await todoTable.ToListAsync();
    }
    catch (MobileServiceInvalidOperationException e)
    {
        // handle exception
    }


#####更新表项

更新数据表格中的行。 参数的产品要进行更新的 TodoItem 对象。

    await todoTable.UpdateAsync(item);

#####删除表项

删除数据库中的行。 参数文件是 TodoItem 对象被删除。

    await todoTable.DeleteAsync(item);


[了解有关移动服务的详细信息](http://azure.microsoft.com/documentation/services/mobile-services/)
