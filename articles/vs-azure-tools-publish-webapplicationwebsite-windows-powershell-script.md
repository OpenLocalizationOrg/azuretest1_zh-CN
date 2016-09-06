---
ms.openlocfilehash: e423282325e568f83b8998d9bc219b084c31239d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="发布-WebApplicationWebSite （Windows PowerShell 脚本）"
   description="了解如何将 web 项目发布到 Azure 网站。 如果它们不存在，该脚本在 Azure 订阅创建所需的资源。"
   services="visual-studio-online"
   documentationCenter="na"
   authors="kempb"
   manager="douge"
   editor="tglee" />
<tags 
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/13/2015"
   ms.author="kempb" />

# 发布-WebApplicationWebSite （Windows PowerShell 脚本）

##语法

将 web 项目发布到 Azure 网站。 如果它们不存在，该脚本在 Azure 订阅创建所需的资源。

    Publish-WebApplicationWebSite
    –Configuration <configuration> 
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose
    

## 配置

描述部署的详细信息的 JSON 配置文件路径。

|参数|默认值|
|---|---|
|别名|无|
|必需？|true|
|位置|名为|
|默认值|无|
|接受管道输入？|false|
|接受通配符？|false|

## SubscriptionName

您想要创建的网站将 Azure 订阅的名称。

|参数|默认值|
|---|---|
|别名|无|
|必需？|false|
|位置|名为|
|默认值|无|
|接受管道输入？|false|
|接受通配符？|false|

## WebDeployPackage

Web 部署包发布到网站的路径。 可以通过在 Visual Studio 中使用 Web 发布向导来创建此包。 请参阅[如何︰ 在 Visual Studio 中创建 Web 部署包](http://go.microsoft.com/fwlink/p/?LinkID=623089)。

|参数|默认值|
|---|---|
|别名|无|
|必需？|false|
|位置|名为|
|默认值|无|
|接受管道输入？|false|
|接受通配符？|false|
    
## DatabaseServerPassword

用户名和密码在 Azure 中的 SQL 数据库。

|参数 |默认值 |
|别名|无|
|---|---|
|必需？|false|
|位置|名为|
|默认值|无|
|接受管道输入？|false|
|接受通配符？|false|

## SendHostMessagesToOutput

如果为 true，则打印消息从脚本到输出流。

|参数|默认值|
|---|---|
|别名|无|
|必需？|false|
|位置|名为|
|默认值|false|
|接受管道输入？|false|
|接受通配符？|false|

## 备注

详细解释如何使用脚本来创建开发和测试环境中，请参阅[发布到开发和测试环境中使用 Windows PowerShell 脚本](https://msdn.microsoft.com/library/azure/dn642480.aspx)。

JSON 配置文件指定了什么是要部署的详细信息。 它包括您在创建项目时，例如名称和用户名的网站时指定的信息。 如果有的话，它还包括设置，数据库。 下面的代码显示了 JSON 配置文件的示例︰

    {
        "environmentSettings": {
            "webSite": {
                "name": "WebApplication10554",
                "location": "West US"
            },
            "databases": [
                {
                    "connectionStringName": "DefaultConnection",
                    "databaseName": "WebApplication10554_db",
                    "serverName": "iss00brc88",
                    "user": "sqluser2",
                    "password": "",
                    "edition": "",
                    "size": "",
                    "collation": "",
                    "location": "West US"
                }
            ]
        }
    }

您可以编辑 JSON 配置文件来更改部署什么。 网站部分是必需的但数据库部分是可选的。

## 下一步行动

[发布-WebApplicationVM （Windows PowerShell 脚本）](https://msdn.microsoft.com/library/azure/dn689112.aspx)




测试
