---
ms.openlocfilehash: c044baba2dfffaecb4923145b645f57dfab97bfb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
## 设置存储连接字符串

使用存储的连接字符串配置终结点和凭据以访问存储服务的.NET 支持 Azure 存储客户端库。 我们建议您保留在配置文件中，而不是将它硬编码到应用程序中的存储连接字符串。 您有两个选项来保存您的连接字符串︰

- 如果您的应用程序运行在 Azure 的云服务，保存连接字符串使用 Azure 服务配置系统 (`*.csdef` ，`*.cscfg`的文件)。 Azure 的云服务配置的详细信息，请参阅[如何创建和部署云服务](../articles/cloud-services/cloud-services-how-to-create-deploy.md)。
- 如果 Azure 的虚拟机上运行的应用程序或者如果您正在构建.NET 应用程序将运行在 Azure 之外，保存连接字符串使用.NET 配置系统 (例如`web.config`或`app.config`文件)。

稍后在本指南中，我们将展示如何从您的代码中检索连接字符串。

### 配置连接字符串从 Azure 的云服务

Azure 的云服务具有唯一的服务配置机制，使您可以动态地从 Azure 管理门户更改配置设置，而无需重新部署您的应用程序。

在 Azure 服务配置中配置您的连接字符串︰

1.  在解决方案资源管理器中的 Visual Studio，Azure 部署项目中，**角色**文件夹中用鼠标右键单击您的 web 角色或辅助角色，然后单击**属性**。  
    ![在 Visual Studio 中选择上一个云服务角色的属性][connection-string1]

2.  单击**设置**选项卡，然后按**添加设置**按钮。  
    ![在 visual Studio 中添加一个云服务设置][connection-string2]

    新的**Setting1**条目然后将显示在设置网格中。

3.  在**类型**下拉列表的新**Setting1**项，选择**连接字符串**。  
    ![设置连接字符串类型][connection-string3]

4.  单击****... **Setting1**项的右端。
    将会打开**存储帐户连接字符串**对话框。

5.  选择想要的目标存储仿真器 （Microsoft Azure 存储在本地计算机上模拟） 或在云中的存储帐户。 本指南中的代码适用于这两个选项。 

    > [AZURE.NOTE] 您可以将存储仿真程序，以避免招致任何使用 Azure 存储相关的成本。 但是，如果您选择目标云在 Azure 存储帐户，执行本教程中的成本是微不足道的。

    如果您的目标在云中存储帐户，然后输入该存储帐户的主访问键。 若要了解如何将复制您通过 Azure 管理门户网站的主要访问密钥，请参阅[查看、 复制和重新生成存储访问键](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)。

    > [AZURE.NOTE] 您的存储帐户密钥是类似于您的存储帐户的根密码。 一定要保护您的密钥。 避免将其分发给其他用户，或将其保存在纯文本格式的文件，可供其他人访问。 重新生成密钥使用管理门户，如果您认为它可能已被破坏。
    
    ![选择目标环境][connection-string4]

6.  从**Setting1**的条目**名称**更改为更友好的名称，如**StorageConnectionString**。 将引用此以后在本指南中的代码中的连接字符串。  
    ![更改连接字符串的名称][connection-string5]
    
### 配置您的连接字符串使用.NET 配置

如果您要编写的应用程序不是 Azure 的云服务，（请参阅上一节），则建议使用.NET 配置系统 (例如`web.config`或`app.config`)。 这包括 Azure 网站或 Azure 的虚拟机，以及应用程序设计为在 Azure 之外运行。 您将存储连接字符串使用`<appSettings>`元素，如下所示。 更换`account-name`您的存储帐户的名称和`account-key`与您的帐户的访问密钥︰

    <configuration>
        <appSettings>
            <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key" />
        </appSettings>
    </configuration>

例如，在配置文件中的配置设置可能类似于︰

    <configuration>
        <appSettings>
            <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=nYV0gln9fT7bvY+rxu2iWAEyzPNITGkhM88J8HUoyofpK7C8fHcZc2kIZp6cKgYRUM74lHI84L50Iau1+9hPjB==" />
        </appSettings>
    </configuration>

现在您就可以执行本指南中的操作任务。

[连接字符串 1]: ./media/storage-configure-connection-string-include/connection-string1.png
[连接字符串 2]: ./media/storage-configure-connection-string-include/connection-string2.png
[连接 string3]: ./media/storage-configure-connection-string-include/connection-string3.png
[连接 string4]: ./media/storage-configure-connection-string-include/connection-string4.png
[连接 string5]: ./media/storage-configure-connection-string-include/connection-string5.png

[配置连接字符串]: http://msdn.microsoft.com/library/azure/ee758697.aspx