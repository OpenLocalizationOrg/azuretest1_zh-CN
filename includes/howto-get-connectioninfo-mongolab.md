---
ms.openlocfilehash: e586e5222d49b26d3d50895badefb6daa9208fac
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
当您配置的 MongoLab 数据库时，MongoLab 传输 MongoDB 的标准连接字符串格式的连接到 Azure 的 URI。 此值用于启动 MongoDB 驱动程序通过您选择的 MongoDB 连接。 有关连接字符串的详细信息，请参阅在 mongodb.org 的[连接](http://www.mongodb.org/display/DOCS/Connections)。

**此 URI 包含您的数据库用户名和密码。  将其视为敏感信息并且不共享。**

您可以检索此 URI 在 Azure 门户中使用以下步骤︰

1. 选择**加载项**。  
![AddonsButton][button-addons]
1. 在加载项列表中找到您的 MongoLab 服务。  
![MongolabEntry][entry-mongolabaddon]
1. 单击加载项的名称，以达到加载项页。
1. 单击**连接信息**。  
![ConnectionInfoButton][button-connectioninfo]  
MongoLab URI 显示︰  
![ConnectionInfoScreen][screen-connectioninfo]  
1.  单击剪贴板按钮右侧的 MONGOLAB_URI 值复制到剪贴板的全部价值。

[输入 mongolabaddon]: ./media/howto-get-connectioninfo-mongolab/entry-mongolabaddon.png
[按钮 connectioninfo]: ./media/howto-get-connectioninfo-mongolab/button-connectioninfo.png
[屏幕 connectioninfo]: ./media/howto-get-connectioninfo-mongolab/dialog-mongolab_connectioninfo.png
[按钮加载项]: ./media/howto-get-connectioninfo-mongolab/button-addons.png
