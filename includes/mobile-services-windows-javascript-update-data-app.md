---
ms.openlocfilehash: b828059e131eca38ee5ba180e1dbd17850ad6a41
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---


1. 接下来，请取消注释或添加下面的代码行和替换`<yourClient>`与变量添加到 service.js 文件中，当您连接到移动服务的项目︰

        var todoTable = <yourClient>.getTable('TodoItem');

    此代码将创建新的数据库表中，使用缓存的筛选器的代理对象 (**todoTable**)。 

2. **InsertTodoItem**函数替换为以下代码︰

        var insertTodoItem = function (todoItem) {
            // Inserts a new row into the database. When the operation completes
            // and Mobile Services has assigned an id, the item is added to the binding list.
            todoTable.insert(todoItem).done(function (item) {
                todoItems.push(item);
            });
        };

    此代码在表格中插入一个新项。

3. **RefreshTodoItems**函数替换为以下代码︰

        var refreshTodoItems = function () {
            // This code refreshes the entries in the list by querying the table.
            // Results are filtered to remove completed items.
            todoTable.where({ complete: false })
                .read().done(function (results) {
                todoItems = new WinJS.Binding.List(results);
                listItems.winControl.itemDataSource = todoItems.dataSource;
            });
        };

    这将设置绑定到 todoTable，其中包含所有从移动服务返回的**TodoItem**对象中的项的集合。 

4. **UpdateCheckedTodoItem**函数替换为以下代码︰
        
        var updateCheckedTodoItem = function (todoItem) {
            // This code takes a freshly completed TodoItem and updates the database. 
            todoTable.update(todoItem);
            // Remove the completed item from the filtered list.
            todoItems.splice(todoItems.indexOf(todoItem), 1);
        };

    将项更新发送给移动服务。

现在，该应用程序已更新为使用移动服务的后端存储，就可以测试对移动服务的应用程序。
