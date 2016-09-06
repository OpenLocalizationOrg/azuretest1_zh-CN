---
ms.openlocfilehash: 618e4e80cf30c530541fae71432afff15bf9e5af
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="配置您的 Azure 项目使用多个服务配置"
   description="配置您的 Azure 项目使用多个服务配置"
   services="visual-studio-online"
   documentationCenter="na"
   authors="kempb"
   manager="douge"
   editor="tlee" />
<tags 
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2015"
   ms.author="kempb" />

# Azure 项目配置

Azure 的云服务项目包括两个配置文件︰ ServiceDefinition.csdef 和 ServiceConfiguration.cscfg。 这些文件与 Azure 的云服务应用程序打包并部署到 Azure。

- **ServiceDefinition.csdef**文件中包含的元数据所需的环境 Azure 的云服务应用程序，包括它包含哪些角色的要求。 此文件还包含将应用于所有实例的配置设置。 可以在运行时使用 Azure 服务承载运行时 API 读取这些配置设置。 在 Azure 中运行您的服务时，不能更新此文件。

- **ServiceConfiguration.cscfg**文件服务定义文件中定义的配置设置中设置的值，并指定要运行的每个角色的实例数。 在 Azure 运行云服务时，可以更新此文件。

对于 Microsoft Visual Studio Azure 工具提供了属性页，您可以使用它来设置存储在这些文件中的配置设置。 要访问属性页，请双击下面 Azure 的云服务项目在解决方案资源管理器中的角色引用或右键单击角色引用和选择**属性**，如下图中所示。

![VS_Solution_Explorer_Roles_Properties](./media/vs-azure-tools-multiple-services-project-configurations/IC784076.png)

有关这些基础架构的服务定义和服务配置文件的信息，请参阅[架构引用](https://msdn.microsoft.com/library/azure/dd179398.aspx)。 服务配置有关的详细信息，请参阅[配置应用程序](https://msdn.microsoft.com/library/azure/gg432977.aspx)。

## 配置角色属性

尽管有一些差异，在下面的部分所指出，是类似，web 角色和辅助角色的属性页。

从**缓存**页中，您可以配置缓存服务正在预览的 Azure。 有关详细信息，请参阅[如何︰ 配置 Azure 角色中缓存](https://msdn.microsoft.com/library/azure/jj131263.aspx)。

### 配置页

在**配置**页中，您可以设置这些属性︰

**实例**

将**实例**计数属性设置此角色用于运行该服务的实例数。

**VM 大小**属性设置为**额外小**、**小**、**中**、**大**，或**特大**。  有关详细信息，请参阅[配置的大小的云服务](https://msdn.microsoft.com/library/azure/ee814754.aspx)。

**启动操作**（仅适用于 web 角色）

设置此属性，以指定在开始调试时，Visual Studio 应启动 web 浏览器的 HTTP 端点或 HTTPS 的终结点，或这两种。

仅当您已经为您的角色定义 HTTPS 终结点 HTTPS 端点选项不可用。 在**终结点**属性页上，您可以定义 HTTPS 终结点。

如果您已经添加的 HTTPS 终结点，HTTPS 端点选项默认启用的并且在开始调试，除了浏览器的 HTTP 端点时，Visual Studio 将启动浏览器为此终结点。 本示例假定这两个启动选项已启用。

**Diagnostics**

默认情况下，Web 角色启用诊断。 Azure 的云服务项目和存储帐户已设置为使用本地存储仿真器。 当准备好部署到 Azure 时，您可以单击生成器按钮 （**...**） 更新在云中使用 Azure 存储的存储帐户。 可以根据需要或定期自动诊断数据传送到存储帐户。 Azure 的诊断程序的详细信息，请参阅[通过使用 Azure 诊断收集日志数据](https://msdn.microsoft.com/library/azure/gg433048.aspx)。

## 设置页面

在**设置**页中，可以添加您的服务配置设置。 配置设置是名称-值对。 在角色中运行的代码可以读取使用[Azure 托管库](http://go.microsoft.com/fwlink?LinkID=171026)提供的类在运行时配置设置的值。 具体来说， [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx)方法返回的值在运行时命名的配置设置。

配置连接到存储帐户连接字符串

连接字符串是提供连接和身份验证信息存储仿真器或者 Azure 存储帐户的配置设置。 只要您的代码必须访问 Azure 存储服务数据 – 即 blob、 队列或表数据--从角色中运行的代码，您必须定义为该存储帐户连接字符串。

指向 Azure 存储帐户连接字符串必须使用已定义的格式。 有关如何创建连接字符串的信息，请参阅[如何配置连接字符串](https://msdn.microsoft.com/library/azure/ee758697.aspx)。

当准备好测试您的服务对 Azure 存储服务中，或当您准备部署到 Azure 的云服务时，可以更改为指向您的 Azure 存储帐户的任何连接字符串的值。 单击 （**...**） 中，选择**输入存储帐户凭据**。 请输入您的帐户信息，包括您的用户名和帐户密码。 在**存储帐户连接字符串**对话框中，您还可以指示您是否要使用默认 HTTPS 终结点 （默认选项），默认 HTTP 端点或自定义的终结点。 您可能决定使用自定义的终结点，如果您注册您的服务，自定义域名所述[配置在 Azure 存储帐户中的 blob 数据的自定义域名](./storage//storage-custom-domain-name/)。

>[AZURE.IMPORTANT] 您必须修改连接字符串以指向一个 Azure 存储帐户，然后再部署您的服务。 如果不这样做可能会导致您不能启动，或者初始化、 忙、 和停止状态之间循环的角色。

## 终结点页

辅助角色可以有任意数量的 HTTP、 HTTPS 或 TCP 终结点。 终结点可以是输入终结点，为外部客户端不可用或内部终结点，可供其他正在运行的服务中的角色。

- 要使 HTTP 端点可供外部客户端和 Web 浏览器中，更改终结点类型输入，并指定名称和公共的端口号。

- 要对外部客户端和 Web 浏览器进行 HTTPS 终结点可用，将终结点的类型更改为**输入**，并指定的名称、 公用端口号，以及管理证书名称。

    请注意，您可以指定一个管理证书之前，必须定义证书的证书属性页上。

- 若要使一个终结点可用于内部访问在云服务中的其他角色，将终结点的类型更改为内部的并指定的名称和可能为此终结点的专用端口。

## 本地存储页

可以使用**本地存储**属性页来保留一个或多个角色的本地存储资源。 本地存储资源是 Azure 角色的实例正在运行中的虚拟机的文件系统中保留的目录。 有关如何使用本地存储资源的详细信息，请参阅[配置本地存储资源](https://msdn.microsoft.com/library/azure/ee758708.aspx)。

## 页上的证书

在**证书**页上，您可以与您的角色关联的证书。 您添加的证书可以用于配置 HTTPS 的终结点的**终结点**属性页上。

**证书**属性页将您的证书的信息添加到您的服务配置。 请注意您的证书不附带服务;必须将您的证书上载到 Azure 通过[Azure 平台管理门户](http://go.microsoft.com/fwlink/?LinkID=213885)的分别。

若要将证书与您的角色相关联，提供证书的名称。 此名称用于**终结点**属性页上配置 HTTPS 终结点时，请参考该证书。 接下来，指定证书存储区是否**本地计算机**或**当前用户**和存储的名称。 最后，输入证书的指纹。 如果证书是 （我） 存储在当前用户 \ 个人中，您可以通过从已填充的列表中选择证书输入证书的指纹。 如果它驻留在任何其他位置，请手动输入的指纹值。

当您从证书存储区中添加证书时，所有中间证书将自动添加到您的配置设置。 这些中间证书还需要上载到 Azure 才能正确配置 ssl 服务。

只有当在云环境中运行时，您与您的服务关联的任何管理证书应用到您的服务。 当在本地开发环境中运行您的服务时，它使用由计算仿真程序的标准证书。

## Azure 的云服务项目配置

若要配置设置应用于整个 Azure 的云服务项目，第一次打开该项目节点的快捷菜单，然后选择属性以打开其属性页。 下表显示了这些属性页。

|属性页|说明|
|---|---|
|应用程序	|在此页面中，可以显示 Azure 工具，使用此云服务项目，并可以升级到当前版本的工具的版本信息。|
|生成事件|在此页上，您可以设置预先生成和后期生成事件。|
|开发|在此页面中，您可以指定生成配置说明和运行任何生成后事件的条件。|
|Web|在此页面中，您可以配置与 web 服务器的设置。|

## 请参见

[Azure Microsoft Visual Studio 工具](https://msdn.microsoft.com/library/azure/ee405484.aspx)


测试
