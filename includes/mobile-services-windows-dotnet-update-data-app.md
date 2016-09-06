---
ms.openlocfilehash: 9628a131492c2b4826afd2d7ab77caa43a4f11ee
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

1. 在 MainPage.xaml.cs 文件中，添加或取消注释以下使用语句︰ 

        using Microsoft.WindowsAzure.MobileServices;

2. TodoItem 类定义替换为以下代码︰ 

        public class TodoItem
        {
            public string Id { get; set; }
    
            [Newtonsoft.Json.JsonProperty(PropertyName = "text")]  
            public string Text { get; set; }
    
            [Newtonsoft.Json.JsonProperty(PropertyName = "complete")]  
            public bool Complete { get; set; }
        }
    
    **JsonPropertyAttribute**用来定义中为基础数据表中的列名称的客户端类型的属性名称之间的映射。

    >[AZURE.NOTE] 在通用的 Windows 应用程序项目中，TodoItem 类共享的数据模型文件夹中的单独的代码文件中定义。

3. 在 MainPage.xaml.cs，注释出或删除的行，定义现有的项集合，然后取消注释或添加以下行，替换_&lt;yourClient&gt;_与`MobileServiceClient`字段添加到 App.xaml.cs 文件中，当您连接到移动服务的项目︰ 

        private MobileServiceCollection<TodoItem, TodoItem> items;
        private IMobileServiceTable<TodoItem> todoTable = 
            App.<yourClient>.GetTable<TodoItem>();
          
    此代码将创建数据库表 (todoTable) 的代理类和移动感知服务绑定集合 （的项目）。 

4. 在**InsertTodoItem**方法中，移除设置**TodoItem.Id**属性，属性的代码行添加到该方法中，**异步**修饰符和取消注释以下代码行︰ 

        await todoTable.InsertAsync(todoItem);


    此代码在表格中插入一个新项。 

5. **RefreshTodoItems**方法替换为以下代码︰ 

        private async void RefreshTodoItems()
        {
            MobileServiceInvalidOperationException exception = null;
            try
            {
                // Query that returns all items.   
                items = await todoTable.ToCollectionAsync();             
            }
            catch (MobileServiceInvalidOperationException e)
            {
                exception = e;
            }
            if (exception != null)
            {
                await new MessageDialog(exception.Message, "Error loading items").ShowAsync();
            }
            else
            {
                ListItems.ItemsSource = items;
                this.ButtonSave.IsEnabled = true;
            }    
        }

    这将设置绑定到集合中的项目`todoTable`，其中包含所有从移动服务返回的**TodoItem**对象。 如果执行查询的问题，引发一个消息框，显示的错误。 

6. 在**UpdateCheckedTodoItem**方法中，添加到该方法中，**异步**修饰符和取消注释以下代码行︰ 

        await todoTable.UpdateAsync(item);

    将项更新发送给移动服务。 

现在，该应用程序已更新为使用移动服务的后端存储，就可以测试对移动服务的应用程序。