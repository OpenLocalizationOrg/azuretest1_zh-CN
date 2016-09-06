---
ms.openlocfilehash: 7b3044db23ebf604fab42945eed75fcb013dd14d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用开放式并发 （Windows 应用商店） 处理数据库写入冲突 |Microsoft Azure" 
    description="了解如何处理数据库写入冲突这两个服务器上并在 Windows 应用商店应用程序。" 
    documentationCenter="windows" 
    authors="wesmc7777" 
    manager="dwrede" 
    editor="" 
    services="mobile-services"/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/18/2015" 
    ms.author="wesmc"/>

# 处理数据库写入冲突



##概述

本教程旨在帮助您更好地了解如何处理两个或多个客户端写入到同一个数据库记录在 Windows 应用商店应用程序时可能发生的冲突。 两个或多个客户端可能会更改写入相同的物料，在同一时间，在某些情况下。 如果不使用任何冲突检测，上次写入将覆盖任何以前的更新即使这不是所期望的结果。 Azure 的移动服务提供了用于检测和解决这些冲突的支持。 本主题将指导您通过允许您处理数据库写入冲突这两个服务器上，并在您的应用程序中的步骤。

在本教程中将添加功能，为快速启动应用程序来处理更新 TodoItem 数据库时可能发生的争用。 


##先决条件

本教程要求如下

+ Microsoft Visual Studio 2013年或更高版本。
+ 本教程基于移动服务快速入门。 在开始本教程之前，您必须先完成 [开始使用移动服务]。 
+ [Azure 帐户]
+ Azure 的移动服务 NuGet 程序包 1.1.0 或更高版本。 若要获取最新版本，请按照以下这些步骤︰
    1. 在 Visual Studio 中，打开该项目，请右击解决方案资源管理器中的项目，然后单击**管理 Nuget 程序包**。 

        ![] [19]

    2. 展开**联机**并单击**Microsoft 和.NET**。 在搜索文本框中输入**Azure 移动服务**。 单击在**Azure 的移动服务**NuGet 程序包的**安装**。

        ![] [20]


 

##更新应用程序允许更新

在本节中将更新任务列表用户界面以允许更新每个项的列表框控件中的文本。 列表框将包含用于数据库表中的每一项的复选框和文本框控件。 您将无法更新 TodoItem 的文本字段。 应用程序将处理`LostFocus`从该文本框以更新数据库中的项的事件。


1. 在 Visual Studio 中，打开任务列表项目中下载 [开始使用移动服务] 教程。
2. Visual Studio 解决方案资源管理器中，打开 MainPage.xaml 和替换`ListView`定义`ListView`显示下方并保存更改。

        <ListView Name="ListItems" Margin="62,10,0,0" Grid.Row="1">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <StackPanel Orientation="Horizontal">
                        <CheckBox Name="CheckBoxComplete" IsChecked="{Binding Complete, Mode=TwoWay}" Checked="CheckBoxComplete_Checked" Margin="10,5" VerticalAlignment="Center"/>
                        <TextBox x:Name="ToDoText" Height="25" Width="300" Margin="10" Text="{Binding Text, Mode=TwoWay}" AcceptsReturn="False" LostFocus="ToDoText_LostFocus"/>
                    </StackPanel>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>


4. Visual Studio 解决方案资源管理器中，打开共享项目中的 MainPage.cs。 将事件处理程序添加到主页，文本框`LostFocus`事件，如下所示。


        private async void ToDoText_LostFocus(object sender, RoutedEventArgs e)
        {
            TextBox tb = (TextBox)sender;
            TodoItem item = tb.DataContext as TodoItem;
            //let's see if the text changed
            if (item.Text != tb.Text)
            {
                item.Text = tb.Text;
                await UpdateToDoItem(item);
            }
        }

4. 添加主页的定义为共享项目 MainPage.cs，`UpdateToDoItem()`方法引用的事件处理程序如下所示。

        private async Task UpdateToDoItem(TodoItem item)
        {
            Exception exception = null;         
            try
            {
                //update at the remote table
                await todoTable.UpdateAsync(item);
            }
            catch (Exception ex)
            {
                exception = ex;
            }           
            if (exception != null)
            {
                await new MessageDialog(exception.Message, "Update Failed").ShowAsync();
            }
        }

现在应用程序写入的文本更改回数据库的每个项文本框失去焦点时。

##您的应用程序中启用了冲突检测

两个或多个客户端可能会更改写入相同的物料，在同一时间，在某些情况下。 如果不使用任何冲突检测，上次写入将覆盖任何以前的更新即使这不是所期望的结果。 [乐观并发控制] 假定每个事务可以提交，因此没有使用任何资源锁定。 提交事务之前, 乐观并发控制验证，任何其他事务都已修改数据。 如果数据已被修改，则回滚提交的事务。 Azure 的移动服务支持通过修订为每个物料使用乐观并发控制`__version`系统将添加到每个表的属性列。 在本节中，我们将使应用程序检测这些写入冲突通过`__version`的系统属性。 通过将通知应用程序`MobileServicePreconditionFailedException`期间如果自上一次查询以来记录发生了更改的更新尝试。 它将能够进行选择，要将其更改提交到数据库或数据库上一次更改保持不变。 移动服务的系统属性的详细信息，请参阅系统属性。

1. 共享的项目和更新中打开 TodoItem.cs`TodoItem`类定义中使用下面的代码包含`__version`启用支持写入冲突检测的系统属性。

        public class TodoItem
        {
            public string Id { get; set; }          
            [JsonProperty(PropertyName = "text")]
            public string Text { get; set; }            
            [JsonProperty(PropertyName = "complete")]
            public bool Complete { get; set; }          
            [JsonProperty(PropertyName = "__version")]
            public string Version { set; get; }
        }

    > [AZURE.NOTE] 在使用非类型化的表时，表的 SystemProperties 加上版本标记启用开放式并发。  
    >
    >````` 
    //Enable optimistic concurrency by retrieving __version
todoTable.SystemProperties |= MobileServiceSystemProperties.Version;
`````


2. By adding the `Version` property to the `TodoItem` class, the application will be notified with a `MobileServicePreconditionFailedException` exception during an update if the record has changed since the last query. This exception includes the latest version of the item from the server. In MainPage.cs for the shared project, add the following code to handle the exception in the `UpdateToDoItem()` method.

        private async Task UpdateToDoItem(TodoItem item)
        {
            Exception exception = null;         
            try
            {
                //update at the remote table
                await todoTable.UpdateAsync(item);
            }
            catch (MobileServicePreconditionFailedException<TodoItem> writeException)
            {
                exception = writeException;
            }
            catch (Exception ex)
            {
                exception = ex;
            }           
            if (exception != null)
            {
                if (exception is MobileServicePreconditionFailedException)
                {
                    //Conflict detected, the item has changed since the last query
                    //Resolve the conflict between the local and server item
                    await ResolveConflict(item, ((MobileServicePreconditionFailedException<TodoItem>) exception).Item);
                }
                else
                {
                    await new MessageDialog(exception.Message, "Update Failed").ShowAsync();
                }
            }
        }


3. In MainPage.cs, add the definition for the `ResolveConflict()` method referenced in `UpdateToDoItem()`. Notice that in order to resolve the conflict, you set the local item's version to the updated version from the server before committing the user's decision. Otherwise, you will continually encounter the conflict.


        private async Task ResolveConflict(TodoItem localItem, TodoItem serverItem)
        {
            //Ask user to choose the resolution between versions
            MessageDialog msgDialog = new MessageDialog(String.Format("Server Text: \"{0}\" \nLocal Text: \"{1}\"\n", 
                                                        serverItem.Text, localItem.Text), 
                                                        "CONFLICT DETECTED - Select a resolution:");
            UICommand localBtn = new UICommand("Commit Local Text");
            UICommand ServerBtn = new UICommand("Leave Server Text");
            msgDialog.Commands.Add(localBtn);
            msgDialog.Commands.Add(ServerBtn);          
            localBtn.Invoked = async (IUICommand command) =>
            {
                // To resolve the conflict, update the version of the 
                // item being committed. Otherwise, you will keep
                // catching a MobileServicePreConditionFailedException.
                localItem.Version = serverItem.Version;             
                // Updating recursively here just in case another 
                // change happened while the user was making a decision
                await UpdateToDoItem(localItem);
            };          
            ServerBtn.Invoked = async (IUICommand command) =>
            {
                RefreshTodoItems();
            };          
            await msgDialog.ShowAsync();
        }



##Test database write conflicts in the application

In this section you will build a Windows Store app package to install the app on a second machine or virtual machine. Then you will run the app on both machines generating a write conflict to test the code. Both instances of the app will attempt to update the same item's `text` property requiring the user to resolve the conflict.


1. Create a Windows Store app package to install on second machine or virtual machine. To do this, click **Project**->**Store**->**Create App Packages** in Visual Studio.

    ![][0]

2. On the Create Your Packages screen, click **No** as this package will not be uploaded to the Windows Store. Then click **Next**.

    ![][1]

3. On the Select and Configure Packages screen, accept the defaults and click **Create**.

    ![][10]

4. On the Package Creation Completed screen, click the **Output location** link to open the package location.

    ![][11]

5. Copy the package folder, "todolist_1.0.0.0_AnyCPU_Debug_Test", to the second machine. On that machine, open the package folder and right click on the **Add-AppDevPackage.ps1** PowerShell script and click **Run with PowerShell** as shown below. Follow the prompts to install the app.

    ![][12]
  
5. Run instance 1 of the app in Visual Studio by clicking **Debug**->**Start Debugging**. On the Start screen of the second machine, click the down arrow to see "Apps by name". Then click the **todolist** app to run instance 2 of the app. 

    App Instance 1  
    ![][2]

    App Instance 2  
    ![][2]


6. In instance 1 of the app, update the text of the last item to **Test Write 1**, then click another text box so that the `LostFocus` event handler updates the database. The screenshot below shows an example.
    
    App Instance 1  
    ![][3]

    App Instance 2  
    ![][2]

7. At this point the corresponding item in instance 2 of the app has an old version of the item. In that instance of the app, enter **Test Write 2** for the `text` property. Then click another text box so the `LostFocus` event handler attempts to update the database with the old `_version` property.

    App Instance 1  
    ![][4]

    App Instance 2  
    ![][5]

8. Since the `__version` value used with the update attempt didn't match the server `__version` value, the Mobile Services SDK throws a `MobileServicePreconditionFailedException` allowing the app to resolve this conflict. To resolve the conflict, you can click **Commit Local Text** to commit the values from instance 2. Alternatively, click **Leave Server Text** to discard the values in instance 2, leaving the values from instance 1 of the app committed. 

    App Instance 1  
    ![][4]

    App Instance 2  
    ![][6]



##Automatically handling conflict resolution in server scripts

You can detect and resolve write conflicts in server scripts. This is a good idea when you can use scripted logic instead of user interaction to resolve the conflict. In this section, you will add a server side script to the TodoItem table for the application. The logic this script will use to resolve conflicts is as follows:

+  If the TodoItem's ` complete` field is set to true, then it is considered completed and `text` can no longer be changed.
+  If the TodoItem's ` complete` field is still false, then attempts to update `text` will be comitted.

The following steps walk you through adding the server update script and testing it.

1. Log into the [Azure Management Portal], click **Mobile Services**, and then click your app. 

    ![][7]

2. Click the **Data** tab, then click the **TodoItem** table.

    ![][8]

3. Click **Script**, then select the **Update** operation.

    ![][9]

4. Replace the existing script with the following function, and then click **Save**.

        function update(item, user, request) { 
            request.execute({ 
                conflict: function (serverRecord) {
                    // Only committing changes if the item is not completed.
                    if (serverRecord.complete === false) {
                        //write the updated item to the table
                        request.execute();
                    }
                    else
                    {
                        request.respond(statusCodes.FORBIDDEN, 'The item is already completed.');
                    }
                }
            }); 
        }   
5. Run the **todolist** app on both machines. Change the TodoItem `text` for the last item in instance 2. Then click another text box so the `LostFocus` event handler updates the database.

    App Instance 1  
    ![][4]

    App Instance 2  
    ![][5]

6. In instance 1 of the app, enter a different value for the last text property. Then click another text box so the `LostFocus` event handler attempts to update the database with an incorrect `__version` property.

    App Instance 1  
    ![][13]

    App Instance 2  
    ![][14]

7. Notice that no exception was encountered in the app since the server script resolved the conflict allowing the update since the item is not marked complete. To see that the update was truly successful, click **Refresh** in instance 2 to re-query the database.

    App Instance 1  
    ![][15]

    App Instance 2  
    ![][15]

8. In instance 1, click the check box to complete the last Todo item.

    App Instance 1  
    ![][16]

    App Instance 2  
    ![][15]

9. In instance 2, try to update the last TodoItem's text and trigger the `LostFocus` event. In response to the conflict, the script resolved it by refusing the update because the item was already completed. 

    App Instance 1  
    ![][17]

    App Instance 2  
    ![][18]

##Next steps

This tutorial demonstrated how to enable a Windows Store app to handle write conflicts when working with data in Mobile Services. Next, consider completing one of the following Windows Store tutorials:

* [Add authentication to your app] 
  <br/>Learn how to authenticate users of your app.

* [Add push notifications to your app] 
  <br/>Learn how to send a very basic push notification to your app with Mobile Services.
 


<!-- Images. -->
[0]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-create-app-package1.png
[1]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-create-app-package2.png
[2]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-app1.png 
[3]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-app1-write1.png
[4]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-app1-write2.png
[5]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-app2-write2.png
[6]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-app2-write2-conflict.png
[7]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/mobile-services-selection.png
[8]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/mobile-portal-data-tables.png
[9]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/mobile-insert-script-users.png
[10]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-create-app-package3.png
[11]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-create-app-package4.png
[12]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-install-app-package.png
[13]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-app1-write3.png
[14]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-app2-write3.png
[15]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-write3.png
[16]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-checkbox.png
[17]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-2-items.png
[18]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-already-complete.png
[19]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/mobile-manage-nuget-packages-VS.png
[20]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/mobile-manage-nuget-packages-dialog.png

<!-- URLs. -->
[Optimistic Concurrency Control]: http://go.microsoft.com/fwlink/?LinkId=330935
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Azure Account]: http://www.windowsazure.com/pricing/free-trial/
[Validate and modify data with scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-dotnet
[Refine queries with paging]: /develop/mobile/tutorials/add-paging-to-data-dotnet
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-dotnet
[Add authentication to your app]: /develop/mobile/tutorials/get-started-with-users-dotnet
[Add push notifications to your app]: /develop/mobile/tutorials/get-started-with-push-dotnet

[Azure Management Portal]: https://manage.windowsazure.com/
[Management Portal]: https://manage.windowsazure.com/
[Windows Phone 8 SDK]: http://go.microsoft.com/fwlink/p/?LinkID=268374
[Mobile Services SDK]: http://go.microsoft.com/fwlink/p/?LinkID=268375
[Developer Code Samples site]:  http://go.microsoft.com/fwlink/p/?LinkId=271146
[System Properties]: http://go.microsoft.com/fwlink/?LinkId=331143
 