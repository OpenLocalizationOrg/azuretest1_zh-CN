---
ms.openlocfilehash: 3cc689ff7d1615fd527e477647d531c8d65226ed
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---


1. 在 Visual Studio 中，打开修改**数据入门**教程完成的项目。

2. 按**F5**键运行应用程序，然后在**插入 TodoItem**中键入文本，单击**保存**。

3. 重复上面的步骤至少三次，以使您拥有三个以上项目的 TodoItem 表中存储。 

2. 在 default.js 文件中，使用以下代码替换的**RefreshTodoItems**方法︰

        var refreshTodoItems = function () {
            // Define a filtered query that returns the top 3 items.
            todoTable.where({ complete: false })
                .take(3)
                .read()
                .done(function (results) {
                    todoItems = new WinJS.Binding.List(results);
                    listItems.winControl.itemDataSource = todoItems.dataSource;
                });
        };

    此查询时，查询执行数据绑定时，将返回未标记为已完成前三项。

3. 按**F5**键以运行该应用程序。

    请注意，仅从 TodoItem 表的前三个结果显示。 

4. （可选）查看通过使用消息检查软件，如浏览器开发人员工具或[Fiddler]发送到移动服务的请求的 URI。 

    请注意， **take(3)**方法被转换为查询选项**$top = 3**中查询的 URI。

5. 再次更新的**RefreshTodoItems**方法，用下面的代码︰
            
        var refreshTodoItems = function () {
            // Define a filtered query that skips the first 3 items and 
            // then returns the next 3 items.
            todoTable.where({ complete: false })
                .skip(3)
                .take(3)
                .read()
                .done(function (results) {
                    todoItems = new WinJS.Binding.List(results);
                    listItems.winControl.itemDataSource = todoItems.dataSource;
                });
        };

    此查询跳过前三个结果，并返回后，接下来三。 这实际上是第二个"页面"的数据，其中的页面大小是三个项目。

    > [AZURE.NOTE] 本教程使用通过将硬编码的分页值传递给了**花**和**跳过**方法的简化的方案。 在实际应用程序中，您可以使用查询类似于以上页导航控件或可比较的用户界面让用户导航到上一页和下一页。  您还可以调用**includeTotalCount**方法在服务器上，以及分页的数据获取可用项的总计数。

6. （可选）再次查看发送到移动服务的请求的 URI。 

    请注意， **skip(3)**方法被转换为查询选项**$skip = 3**中查询的 URI。

<!-- URLs -->
[Fiddler]: http://go.microsoft.com/fwlink/?LinkID=262412