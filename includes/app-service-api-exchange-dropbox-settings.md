---
ms.openlocfilehash: f454ba44c98a415a0cd7b324dfcbccb94373ce35
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
4. 在另一个浏览器窗口或选项卡，请转到[Azure 预览门户](https://portal.azure.com)。

3. 转到收存箱连接器**应用程序 API**刀片式服务器。 （如果你仍在**资源组**刀片式服务器，只需单击该关系图中的收存箱接头。）

4. 单击**设置**，单击**设置**刀片式服务器中的**身份验证**。

    ![单击设置](./media/app-service-api-exchange-dropbox-settings/clicksettings.png)

    ![单击验证](./media/app-service-api-exchange-dropbox-settings/clickauth.png)

5. 在身份验证刀片式服务器，输入客户机 ID 和秘密从收存箱站点，客户端，然后单击**保存**。

    ![输入设置，然后单击保存](./media/app-service-api-exchange-dropbox-settings/authblade.png)

3. 复制**重定向 URI** （客户机 ID 和客户端密钥上方的灰色框），并将值添加到您留在上一步中打开页面。 

    重定向 URI 遵循以下模式︰

        [gatewayurl]/api/consent/redirect/[connectorname]

    例如︰

        https://dropboxrgaeb4ae60b7.azurewebsites.net/api/consent/redirect/DropboxConnector

    ![获取重定向 URI](./media/app-service-api-exchange-dropbox-settings/redirecturi.png)

    ![创建收存箱的应用程序](./media/app-service-api-exchange-dropbox-settings/dbappsettings2.png)