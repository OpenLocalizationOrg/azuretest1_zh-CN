---
ms.openlocfilehash: 51d48515a1ce40e9e751ac8841ed93f262d5b081
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

##<a name="update-app"></a>更新应用程序以调用自定义的 API

1. 在 Visual Studio 中，您的快速启动项目中打开 default.html 文件时，找到名为的**按钮**元素`buttonRefresh`，和它后面添加下面的新元素︰ 

        <button id="buttonCompleteAll" style="margin-left: 5px">Complete All</button>

    这向该页添加一个新按钮。 

2. 在中打开 default.js 代码文件`js`项目文件夹，查找**refreshTodoItems**函数，请确保该函数包含以下代码︰

        todoTable.where({ complete: false })
           .read()
           .done(function (results) {
               todoItems = new WinJS.Binding.List(results);
               listItems.winControl.itemDataSource = todoItems.dataSource;
           });            

    这项进行筛选，以便查询不会返回已完成的项目。

3. 之后的**refreshTodoItems**函数，添加以下代码︰

        var completeAllTodoItems = function () {
            var okCommand = new Windows.UI.Popups.UICommand("OK");
        
            // Asynchronously call the custom API using the POST method. 
            mobileService.invokeApi("completeall", {
                body: null,
                method: "post"
            }).done(function (results) {
                var message = results.result.count + " item(s) marked as complete.";
                var dialog = new Windows.UI.Popups.MessageDialog(message);
                dialog.commands.append(okCommand);
                dialog.showAsync().done(function () {
                    refreshTodoItems();
                });
            }, function (error) {
                var dialog = new Windows.UI.Popups
                    .MessageDialog(error.message);
                dialog.commands.append(okCommand);
                dialog.showAsync().done();
            });
        };

        buttonCompleteAll.addEventListener("click", function () {
            completeAllTodoItems();
        });

    此方法处理了新建按钮的**Click**事件。 在客户端，将 POST 请求发送到新的自定义 API 调用**InvokeApiAsync**方法。 由自定义的 API 返回的结果将显示在消息对话框中，如任何错误。

## <a name="test-app"></a>测试应用程序

1. 在 Visual Studio 中，请按**F5**键以重新生成该项目并启动应用程序。

2. 在应用程序中，在**插入 TodoItem**，请键入一些文本，然后单击**保存**。

3. 直到几个 todo 项添加到列表，请重复上述步骤。

4. 单击**全部完成**按钮。

    ![](./media/mobile-services-windows-store-javascript-call-custom-api/mobile-custom-api-windows-store-completed.png)

    指示标记为已完成的项目数，将显示一个消息对话框，筛选执行查询，以清除列表中的所有项目。