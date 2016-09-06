---
ms.openlocfilehash: 94b5aa8c5dcf3c32f3283cfcd809f435b84b3241
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
## API 的应用程序元数据

本节提供有关您可以自定义的 API 的应用程序元数据的其他信息。

在*apiapp.json*文件中，并在*元数据*文件夹中，文件属性中的大多数影响提供一个 API 的应用程序软件包的 Azure 市场的方式。 以下各节说明了哪些属性和文件影响 API 的应用程序时将您的代码到 API 的应用程序部署在 Azure 订购。 

### API 的应用程序 ID 

`id`属性确定 API 的应用程序的名称。  例如︰

        "id": "ContactsList",

![](./media/app-service-api-direct-deploy-metadata/apiappname.png)

### 命名空间

设置`namespace`Azure Active Directory 租户的域的属性。 要查找您的域，请打开您的浏览器到[Azure 的传统门户网站](https://manage.windowsazure.com/)，浏览**活动目录**，选择**域**选项卡。 例如︰

        "namespace": "contoso.onmicrosoft.com",

### 动态 Swagger API 定义

如果 API 的应用程序可以返回动态[Swagger](http://swagger.io/) API 定义，存储返回 JSON 的 API 定义的 GET 请求的相对 URL 中`endpoints.apiDefinition`属性。 例如︰  

        "endpoints": {
            "apiDefinition": "/swagger/docs/v1"
        }

> **注意︰**如果您正在使用 Swashbuckle 生成 Swagger API 定义，Web API 控制器中的 HTTP 方法重载将导致重复操作 id。 有关详细信息，请参阅[自定义 Swashbuckle 生成操作标识符](../article/app-service-api/app-service-api-dotnet-swashbuckle-customize.md)。
  
### 静态的 Swagger API 定义

提供一个静态[Swagger](http://swagger.io/) 2.0 API 定义文件，将文件存储在*元数据*文件夹并命名文件*apiDefinition.swagger.json*

![](./media/app-service-api-direct-deploy-metadata/apidefinmetadata.png)

离开`endpoints.apiDefinition`出的*apiapp.json*文件或将其值设置为 null。 如果同时包括`endpoints.apiDefinition`URL 和一个*apiDefinition.swagger.json*文件，该 URL 将优先，文件将被忽略。
