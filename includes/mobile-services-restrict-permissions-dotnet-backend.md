---
ms.openlocfilehash: 2feef9255cc01339d698425e4abbf06cfb2c7cef
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---


默认情况下，移动服务的资源的所有请求都会向客户端显示应用程序键，不严格保护资源的访问受到限制。 若要保护您的资源，必须限制对只有已通过身份验证的客户端的访问。

1. 在 Visual Studio 中，打开您的移动服务项目、 展开控制器文件夹，并打开**TodoItemController.cs**。 **TodoItemController**类实现的 TodoItem 表的数据访问。 添加以下`using`语句︰

        using Microsoft.WindowsAzure.Mobile.Service.Security;

2. 将下面的_AuthorizeLevel_属性应用于**TodoItemController**类。 

        [AuthorizeLevel(AuthorizationLevel.User)]

    这将确保对_TodoItem_表的所有操作都需要身份验证的用户。 您还可以应用在方法级别的*AuthorizeLevel*属性。

3. （可选）如果您想要调试本地身份验证，则展开`App_Start`文件夹中，打开**WebApiConfig.cs**，然后将以下代码添加到**注册**方法。  

        config.SetIsHosted(true);

    这会告诉本地移动服务项目运行一样被承载于 Azure，包括考虑*AuthorizeLevel*设置。 若无此设置，本地主机上的所有 HTTP 请求都获准无需尽管*AuthorizeLevel*设置的身份验证。 启用自主的模式时，您还需要设置的本地应用程序键的值。

4. （可选）在 web.config 项目文件中，将设置为一个字符串值`MS_ApplicationKey`应用程序设置。 

    这是您使用 （无用户名） 的密码在本地运行该服务时测试 API 的帮助页面。  在 Azure，活动站点不使用此字符串值，并不需要使用实际的应用程序键;任何有效的字符串值将起作用。
 
4. 重新发布您的项目。
