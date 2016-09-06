---
ms.openlocfilehash: 6ff90e16ed235aedbd90accdb61ac68ec415ab1a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
本指南将显示您如何使用[ClearDB]来从[Azure 存储]中创建一个 MySQL 数据库以及如何创建[waws] [Azure 网站]时作为链接的资源创建一个 MySQL 数据库。 [ClearDB]是容错的数据库作为服务提供程序允许您运行和管理 Azure 数据中心中的 MySQL 数据库，并从任何应用程序连接到它们。  

> [AZURE.NOTE] 为网站创建过程的一部分创建一个 MySQL 数据库时，您只能创建一个可用的数据库。 从 Azure 存储创建一个 MySQL 数据库使您可以创建一个可用的数据库或从付费的选项中选择。

## 如何︰ 从 Azure 存储中创建一个 MySQL 数据库

若要从[Azure 存储]创建一个 MySQL 数据库，执行下列操作︰

1. 登录到[Azure 管理门户]的[门户]。
2. 单击**+ 新建**底部的页上，然后选择**市场**。

    ![从存储库中选择加载项](./media/create-mysql-db/select-store.png)

3. 选择**ClearDB MySQL 数据库**，然后单击位于框架底部的箭头。

    ![选择 ClearDB MySQL 数据库](./media/create-mysql-db/select-cleardb-mysql.png)

4. 选择某一计划、 输入一个数据库名，选择一个区域，然后单击位于框架底部的箭头。

    ![从商店购买 MySQL 数据库](./media/create-mysql-db/purchase-mysql.png)

5. 单击复选标记来完成您的购买。

    ![查看并完成您的购买](./media/create-mysql-db/complete-mysql-purchase.png)

6. 在创建数据库之后，您可以从管理门户中的**加载项**选项卡来进行管理。

    ![管理 Azure 门户中的 MySQL 数据库](./media/create-mysql-db/manage-mysql-add-on.png)

7. 可以通过单击页面底部的 （如上所示） 的**连接信息**获取数据库连接信息。

    ![MySql 连接信息](./media/create-mysql-db/mysql-conn-info.png) 


## 如何︰ 创建一个 MySQL 数据库为 Azure 网站链接的资源

若要创建[waws] [Azure 网站]时，请创建一个 MySQL 数据库作为链接的资源，执行以下操作︰

1. 登录到[Azure 管理门户]的[门户]。
2. 单击**+ 新建**底部的页上，然后选择**计算**、**网站**，并**创建数据库**。

    ![创建与数据库的 web 站点](./media/create-mysql-db/custom_create.png)

3. 为您的网站提供一个**URL** ，选择为您的站点，该**地区**从**数据库**下拉列表中选择**创建新的 MySQL 数据库**。 （可选） 您可以替换连接字符串的默认名称。 单击页面底部的箭头。

    ![提供网站的详细信息](./media/create-mysql-db/provide-website-details.png) 

4. 提供数据库**名称**，选择**区域**数据库 （这应该是相同的区域为您的网站），同意 ClearDB 的法律条款，然后单击位于框架底部的复选标记。

    ![提供 MySQL 的详细信息](./media/create-mysql-db/provide-mysql-details.png)

5. 在创建网站之后，单击以转到您的站点的仪表板站点的名称。

    ![转到 web 站点的仪表板](./media/create-mysql-db/go-to-website-dashboard.png)

6. 单击**配置**。

    ![转到要配置选项卡](./media/create-mysql-db/go-to-configure-tab.png)

7. 向下滚动到**连接字符串**部分中，单击**显示的连接字符串**。 

    ![显示连接字符串](./media/create-mysql-db/show-conn-string.png)

8. 复制应用程序中使用的连接字符串。

    ![显示的连接字符串](./media/create-mysql-db/shown-conn-string.png)

> [AZURE.NOTE] 连接字符串都可以访问您网站的应用程序的连接字符串的名称。 在.NET 应用程序中，连接字符串是**connectionStrings**对象中的可用。 在其他编程语言，连接字符串是环境变量的可访问性。 有关详细信息，请参阅[如何配置 Web 站点][配置]。

[ClearDB]: http://www.cleardb.com/
[waws]: /documentation/services/web-sites/
[Azure 存储]: ../articles/store.md
[门户网站]: http://manage.windowsazure.com
[配置]: ../article/app-service-web/web-sites-configure.md
