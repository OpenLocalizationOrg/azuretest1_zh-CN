---
ms.openlocfilehash: b0c15b01789b76e72b8e1d19ae954d10e2f044f6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
## 生成的 API 的应用程序客户端 

在 Visual Studio 中的 API 应用程序工具便于生成到 Azure API 应用程序调用从桌面、 存储和移动应用程序的 C# 代码。 

1. 在 Visual Studio 中，打开包含 API 应用程序中[创建 API 的应用程序](../article/app-service-api/app-service-dotnet-create-api-app.md)主题的解决方案。 

2. 从**解决方案资源管理器中**，右击解决方案并选择**添加** > **新的项目**。

    ![添加新项目](./media/app-service-dotnet-debug-api-app-gen-api-client/01-add-new-project-v3.png)

3. 在**添加新项目**对话框中，请执行以下步骤︰

    1. 选择的**Windows 桌面**类别。
    
    2. 选择**控制台应用程序**项目模板。
    
    3. 命名该项目。
    
    4. 单击**确定**以生成新项目在现有解决方案中。
    
    ![添加新项目](./media/app-service-dotnet-debug-api-app-gen-api-client/02-contact-list-console-project-v3.png)

4. 右键单击新创建的控制台应用程序项目，然后选择**添加** > **Azure API 应用程序客户端**。 

    ![添加新客户端](./media/app-service-dotnet-debug-api-app-gen-api-client/03-add-azure-api-client-v3.png)
    
5. 在**添加 Microsoft Azure API 应用程序客户端**对话框中，请执行以下步骤︰ 

    1. 选择**下载**选项。 
    
    2. 从下拉列表中，选择您在前面创建的 API 应用程序。 
    
    3. 单击**确定**。 

    ![生成屏幕](./media/app-service-dotnet-debug-api-app-gen-api-client/04-select-the-api-v3.png)

    该向导将下载 API 的元数据文件并生成一个类型化的界面，用于调用 API 的应用程序。

    ![发生生成](./media/app-service-dotnet-debug-api-app-gen-api-client/05-metadata-downloading-v3.png)

    代码生成完成后，您将看到 API 的应用程序的名称在解决方案资源管理器中的新文件夹。 此文件夹包含实现客户端和数据模型的代码。 

    ![生成已完成](./media/app-service-dotnet-debug-api-app-gen-api-client/06-code-gen-output-v3.png)

6. 从项目根打开**Program.cs**文件并使用下面的代码替换**Main**方法︰ 

        static void Main(string[] args)
        {
            var client = new ContactsList();
    
            // Send GET request.
            var contacts = client.Contacts.Get();
            foreach (var c in contacts)
            {
                Console.WriteLine("{0}: {1} {2}",
                    c.Id, c.Name, c.EmailAddress);
            }
    
            // Send POST request.
            client.Contacts.Post(new Models.Contact
            {
                EmailAddress = "lkahn@contoso.com",
                Name = "Loretta Kahn",
                Id = 4
            });
    
            Console.WriteLine("Finished");
            Console.ReadLine();
        }

## 测试 API 的应用程序客户端

一旦编码的 API 的应用程序，就可以测试代码。

1. 打开**解决方案资源管理器**。

2. 用鼠标右键单击在上一节中创建的控制台应用程序。

3. 从控制台应用程序的上下文菜单中，选择**调试 > 启动新实例**。 

4. 控制台窗口应打开并显示所有的联系人。 

    ![运行的控制台应用程序](./media/app-service-dotnet-debug-api-app-gen-api-client/running-console-app.png)

5. 按**enter 键**以关闭控制台窗口。          
