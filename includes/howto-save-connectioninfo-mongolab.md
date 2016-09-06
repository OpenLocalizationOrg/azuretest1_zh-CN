---
ms.openlocfilehash: 9d1fc0daa1af56387a601ca9d5b29685b55d30db
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
虽然可以将 MongoLab URI 粘贴到您的代码，我们建议为便于管理的环境中对其进行配置。 这样一来，如果更改 URI，则可以更新它通过 Azure 门户不用的代码。


1. 在 Azure 门户中，选择**Web 应用程序**。
1. 单击 Web 应用程序列表中的 web 应用程序的名称。  
![WebAppEntry][entry-website]  
Web 应用程序的仪表板显示。

1. 在菜单栏中，单击**配置**。  
![WebAppDashboardConfig][focus-mongolab-websitedashboard-config]

1. 向下滚动到连接字符串部分。  
![WebAppConnectionStrings][focus-mongolab-websiteconnectionstring]

1. 对于**名称**，输入 MONGOLAB_URI。
1. **值**，将粘贴上一节中我们获得的连接字符串。
1. （而不是默认**SQLAzure**）**类型**下拉列表中选择**自定义**。
1. 在工具栏上，单击**保存**。  
![SaveWebApp][button-website-save]

**注意︰**Azure 添加**CUSTOMCONNSTR\_**为此变量，这就是为什么上面引用的代码前缀**CUSTOMCONNSTR\_MONGOLAB_URI。**

[入口网站]: ./media/howto-save-connectioninfo-mongolab/entry-website.png
[焦点-mongolab-websitedashboard-配置]: ./media/howto-save-connectioninfo-mongolab/focus-mongolab-websitedashboard-config.png
[焦点-mongolab-websiteconnectionstring]: ./media/howto-save-connectioninfo-mongolab/focus-mongolab-websiteconnectionstring.png
[按钮的网站保存]: ./media/howto-save-connectioninfo-mongolab/button-website-save.png
