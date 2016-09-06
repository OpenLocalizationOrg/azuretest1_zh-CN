---
ms.openlocfilehash: 2572e74d9853a22c14c6ed493f923b973d6b5aaf
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
## 查看 API 的应用程序元数据

使 Web API 项目以将其部署为 API 的应用程序的元数据包含在一个*apiapp.json*文件和*元数据*文件夹中。

![](./media/app-service-api-review-metadata/metadatainse.png)

*Apiapp.json*文件中的默认内容类似于下面的示例︰

        {
            "$schema": "http://json-schema.org/schemas/2014-11-01/apiapp.json#",
            "id": "ContactsList",
            "namespace": "microsoft.com",
            "gateway": "2015-01-14",
            "version": "1.0.0",
            "title": "ContactsList",
            "summary": "",
            "author": "",
            "endpoints": {
                "apiDefinition": "/swagger/docs/v1",
                "status": null
            }
        }

请注意`apiDefinition`端点`/swagger/docs/v1`︰ 默认情况下，API 的应用程序项目使用[Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet 程序包提供[Swagger](http://swagger.io/)自动生成元数据。 

对于本教程中，您可以接受默认值。 本教程后面的[API 的应用程序元数据](#api-app-metadata)一节介绍了如何自定义此元数据。
