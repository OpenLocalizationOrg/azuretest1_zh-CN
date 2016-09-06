---
ms.openlocfilehash: ac966809e600a3f1775da22c8a130f35a2ff60d9
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
## 添加 Web API 代码

在下面的步骤中，您将添加代码的一个简单的 HTTP Get 方法返回的联系人的硬编码列表。 

1. 在解决方案资源管理器中，用鼠标右键单击**模型**文件夹，并选择**添加 > 类**。 

    ![](./media/app-service-api-define-api-app/03-add-new-class-v3.png) 

2. 命名新文件*Contact.cs*。 

    ![](./media/app-service-api-define-api-app/0301-add-new-class-dialog-v3.png) 

3. 单击**添加**。

4. 一旦创建*Contact.cs*文件，下面的代码替换该文件的全部内容。 

        namespace ContactsList.Models
        {
            public class Contact
            {
                public int Id { get; set; }
                public string Name { get; set; }
                public string EmailAddress { get; set; }
            }
        }

5. 用鼠标右键单击**控制器**文件夹，并选择**添加 > 控制器**。 

    ![](./media/app-service-api-define-api-app/05-new-controller-v3.png)

6. 在**添加构架**对话框中，选择**Web API 2 控制器-空**选项，单击**添加**。 

    ![](./media/app-service-api-define-api-app/06-new-controller-dialog-v3.png)

7. 命名的控制器**ContactsController**，然后单击**添加**。 

    ![](./media/app-service-api-define-api-app/07-new-controller-name-v2.png)

8. 一旦创建 ContactsController.cs 文件，下面的代码替换该文件的全部内容。 

        using ContactsList.Models;
        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Net;
        using System.Net.Http;
        using System.Threading.Tasks;
        using System.Web.Http;
        
        namespace ContactsList.Controllers
        {
            public class ContactsController : ApiController
            {
                [HttpGet]
                public IEnumerable<Contact> Get()
                {
                    return new Contact[]{
                        new Contact { Id = 1, EmailAddress = "barney@contoso.com", Name = "Barney Poland"},
                        new Contact { Id = 2, EmailAddress = "lacy@contoso.com", Name = "Lacy Barrera"},
                        new Contact { Id = 3, EmailAddress = "lora@microsoft.com", Name = "Lora Riggs"}
                    };
                }
            }
        }

## 启用 Swagger 用户界面

默认情况下，API 的应用程序项目与[Swagger]自动启用生成(http://swagger.io/ "正式的 Swagger 信息")元数据，以及当您使用**添加 API 应用程序 SDK**菜单项转换 Web API 项目，默认情况下还启用 API 测试页。  

但是，Azure API 应用程序新项目模板会禁用 API 测试页。 使用 API 的应用程序项目模板创建 API 的应用程序项目时，请执行以下步骤来启用测试页。

**注意︰**如果为*公用匿名*和 Swagger 用户界面启用部署 API 的应用程序，任何人都将能够使用 Swagger 用户界面查找并调用您的 Api。 

1. 打开*App_Start/SwaggerConfig.cs*文件，然后搜索为**EnableSwaggerUI**:

    ![](./media/app-service-api-define-api-app/12-enable-swagger-ui-with-box.png)

2. 取消注释下面的代码行︰

            })
        .EnableSwaggerUi(c =>
            {

3. 当您完成时，该文件应如下所示︰

    ![](./media/app-service-api-define-api-app/13-enable-swagger-ui-with-box.png)

## 测试 Web API

若要查看 API 测试页，请执行以下步骤。

1. 运行应用程序本地 (CTRL + F5)。

    浏览器将打开并显示 HTTP 403 错误，因为基 URL 不是有效的 web 页或 API 方法 URL 为此项目。
 
3.  通过添加导航到 Swagger 页`/swagger`的基 URL 的末尾。 

    ![](./media/app-service-api-define-api-app/swaggerhome.png)

2. 单击**联系人 > 获取 > 试一下**，并查看 API 是否正常运行，并返回预期的结果。 

    ![](./media/app-service-api-define-api-app/swaggertry.png)

3. 在 Visual Studio 中，单击**调试 > 停止调试**。
