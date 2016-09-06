---
ms.openlocfilehash: a56552da2a978790cc8c8ab019c65ab0ccb0bbae
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
##<a name="update-app"></a>更新应用程序以调用自定义的 API

1. 在 Visual Studio 中，您的快速启动项目中打开 MainPage.xaml 文件，找到名为的**按钮**元素`ButtonRefresh`，并将其替换为下面的 XAML 代码︰ 

        <StackPanel Grid.Row="3" Grid.ColumnSpan="2" Orientation="Horizontal">
            <Button Width="225" Name="ButtonRefresh" 
                Click="ButtonRefresh_Click">Refresh</Button>
            <Button Width="225"  Name="ButtonCompleteAll" 
                Click="ButtonCompleteAll_Click">Complete All</Button>
        </StackPanel>

    这向该页添加一个新按钮。 

2. 打开 MainPage.xaml.cs 文件，并添加下面的类定义代码︰

        public class MarkAllResult
        {
            public int Count { get; set; }
        }

    此类用于保存自定义的 API 返回的行计数值。 

3. 在**主页**类中，找到**RefreshTodoItems**方法并确保`query`通过下面**的**方法定义︰

        .Where(todoItem => todoItem.Complete == false)

    这项进行筛选，以便查询不会返回已完成的项目。

3. 在**主页**类中，添加下面的方法︰

        private async void ButtonCompleteAll_Click(object sender, RoutedEventArgs e)
        {
            string message;
            try
            {
                // Asynchronously call the custom API using the POST method. 
                var result = await App.MobileService
                    .InvokeApiAsync<MarkAllResult>("completeAll", 
                    System.Net.Http.HttpMethod.Post, null);
                message =  result.Count + " item(s) marked as complete.";
                RefreshTodoItems();
            }
            catch (MobileServiceInvalidOperationException ex)
            {
                message = ex.Message;                
            }
        
            MessageBox.Show(message);  
        }

    此方法处理了新建按钮的**Click**事件。 客户端将请求发送到新的自定义 API 调用**InvokeApiAsync**方法。 通过自定义的 API 返回的结果将显示在一个消息对话框。

## <a name="test-app"></a>测试应用程序

1. 在 Visual Studio 中，请按**F5**键以重新生成该项目并启动应用程序。

2. 在应用程序中，在**插入 TodoItem**，请键入一些文本，然后点击**保存**。

3. 直到几个 todo 项添加到列表，请重复上述步骤。

4. 点击**完成全部**按钮。

    ![](./media/mobile-services-windows-phone-call-custom-api/mobile-custom-api-windows-phone-completed.png)

    显示一个消息框，指示标记为已完成的项目数和筛选的查询会被再次执行，以清除列表中的所有项目。