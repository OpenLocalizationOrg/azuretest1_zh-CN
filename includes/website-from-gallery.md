---
ms.openlocfilehash: 654734106ec884593188dbaa032b0365aa08a6b7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
Azure 市场提供范围广泛的流行的 web 应用程序由 Microsoft、 第三方公司和开放源码软件计划开发。 从 Azure 市场创建的 web 应用程序不需要安装任何软件，不是用于连接到[Azure 预览门户网站](http://go.microsoft.com/fwlink/?LinkId=529715)的浏览器。 

在本教程中，您将学习︰

- 如何创建新 web 应用程序通过 Azure 市场。

- 若要部署 web 应用程序通过 Azure 预览门户的方式。
 
将构建 WordPress 博客使用的默认模板。 下图显示了完整的应用程序︰


![Wordpress 的博客][13]

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

## 在门户中创建 web 应用程序

1. 登录到 Azure 预览门户。

2. 通过单击**市场**图标打开 Azure 市场︰

    ![市场上图标][marketplace]

    或者单击仪表板的右上方上的**新建**图标并选择列表的 bottow 在**市场**。
    
    ![创建新的][5]
    
3. 选择**Web + 移动**。 **WordPress**中搜索，然后单击**WordPress**图标。

    ![从列表的 WordPress][7]
    
5. 阅读后 WordPress 的应用程序的说明，选择**创建**。

6. 单击**Web 应用程序**，然后配置您的 web 应用程序提供所需的值。
    
    ![配置您的应用程序][8]

7. 单击**数据库**，并提供用于配置 MySQL 数据库所需的值。 

    ![配置数据库][database]

8. 提供新的资源组的名称。

    ![设置资源组][groupname]

8. 如果有必要，请单击**订阅**，并指定订阅以使用。 

7. 完成定义 web 应用程序，单击**创建**并等待将在创建新的 web 应用程序。

   创建应用程序之后，您将看到包含 web 应用程序和数据库的资源组。

   ![显示组][resourcegroup]

## 启动和管理您的 WordPress web 应用程序
    
1. 有关您的应用程序的详细信息，请参阅您新的 web 应用程序上单击。

    ![发布仪表板][10]

2. 在**精要**页中，单击打开 web 应用程序的欢迎页面的**Url**下的**浏览**或链接。

    ![站点的 URL][browse]

3. 如果您没有安装 WordPress，输入所需的 WordPress 的适当的配置信息，然后单击**安装 WordPress**完成配置，然后打开该 web 应用程序的登录页。

4. 单击**登录**，然后输入您的凭据。  

5. 您将有一个新的 WordPress web 应用程序类似于 web 应用程序。    

    ![WordPress 站点][13]






[5]: ./media/website-from-gallery/start-marketplace.png
[6]: ./media/website-from-gallery/wordpressgallery-02.png
[7]: ./media/website-from-gallery/search-web-app.png
[8]: ./media/website-from-gallery/set-web-app.png
[9]: ./media/website-from-gallery/wordpressgallery-05.png
[10]: ./media/website-from-gallery/select-web.png
[13]: ./media/website-from-gallery/wordpressgallery-09.png
[webapps]: ./media/website-from-gallery/selectwebapps.png
[数据库]: ./media/website-from-gallery/set-db.png
[resourcegroup]: ./media/website-from-gallery/show-rg.png
[浏览]: ./media/website-from-gallery/browse-web.png
[市场]: ./media/website-from-gallery/marketplace-icon.png
[组名]: ./media/website-from-gallery/set-rg.png
