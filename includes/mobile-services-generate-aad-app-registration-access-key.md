---
ms.openlocfilehash: 2261e49992fce7b2351b22ea28a5baf575fcdae0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
1. 单击目录在[Azure 管理门户](https://manage.windowsazure.com/)页面上的**应用程序**选项卡。
  
2. 单击您的集成的应用程序注册。

3. 在应用程序页上单击**配置**，然后向下滚动页面的**键**部分。 
4. 单击新的密钥持续**1 年**时间。 然后单击**保存**，门户将显示新的键值。
5. 复制**客户机 ID**和**密钥**保存后显示。 请注意，注册表项值将仅显示给您一次保存之后。 

    ![](./media/mobile-services-generate-aad-app-registration-access-key/client-id-and-key.png)

6. 向下滚动到页面底部的集成的应用程序配置和启用应用程序**读取目录数据**权限并单击**保存**。

    ![](./media/mobile-services-generate-aad-app-registration-access-key/app-perms.png)


7. 在[Azure 管理门户](https://manage.windowsazure.com/)中向后定位到您的移动服务，请单击**配置**选项卡。 向下滚动到**应用程序设置**部分中添加下面的应用程序设置并单击**保存**。 

    <table border="1">
    <tr>
    <th>应用程序设置名称</th><th>说明</th>
    </tr>
    <tr>
    <td>AAD_CLIENT_ID</td><td>从上面的步骤中您集成的应用程序复制的客户机 id。</td>
    </tr>
    <tr>
    <td>AAD_CLIENT_KEY</td><td>在您 AAD 生成的应用程序键集成应用程序在执行上面的步骤。</td>
    </tr>
    <tr>
    <td>AAD_TENANT_DOMAIN</td><td>您 AAD 的域名。 应类似于"mydomain.onmicrosoft.com"</td>
    </tr>
    </table><br/>

 
    ![](./media/mobile-services-generate-aad-app-registration-access-key/aad-app-settings.png)
  