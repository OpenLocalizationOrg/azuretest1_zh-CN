---
ms.openlocfilehash: 9781a2cb742eace1cafddcb0cde4f43581aabcdc
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="将资源添加到 Azure 的资源组"
   description="了解如何使用 Visual Studio 将资源添加到 Azure 的资源组。"
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

# 将资源添加到 Azure 的资源组

如果您需要向资源组中添加更多的资源，你可以在 Visual Studio 的 JSON 大纲窗口中。

## 将资源添加到资源组

### 将资源添加到资源组

1. 在解决方案资源管理器中选择 Azure 的资源组的 JSON 文件。 JSON 文件显示在编辑器和**JSON 大纲**窗口还会显示编辑器旁边。 如果您关闭 JSON 大纲窗口，它不会自动打开再次选择 JSON 模板文件 （而不是参数文件） 的上下文菜单上的显示轮廓命令之前。

    ![Azure 的资源组的 JSON 文件](./media/vs-azure-tools-resource-group-adding-resources/arm-json-file.png)

1. 在**JSON 大纲**窗口中，选择**资源**节点，然后选择**添加新的资源**命令在上下文菜单中，或选择 JSON 大纲窗口顶部的**添加资源**按钮。

    ![添加新的资源的资源组](./media/vs-azure-tools-resource-group-adding-resources/arm-add-resource.png)

1. 在**添加资源**对话框中的资源的列表中，选择您想要添加，请提供所需的资源，信息的资源，然后选择**添加**按钮。 例如，如果您添加一个存储帐户，您需要为您提供将创建参数的基名称。 
 
    ![添加资源对话框](./media/vs-azure-tools-resource-group-adding-resources/arm-add-resource-dialog.png)

    资源将添加到的资源列出在**JSON 大纲**窗口中，并在文件中出现的新的 JSON 表示的资源。 此外可以添加相关的参数和/或变量。


    ![JSON 文件中添加的资源](./media/vs-azure-tools-resource-group-adding-resources/arm-add-resource-json.png)

1. 将新的资源部署到 Azure 的资源组。 在解决方案资源管理器中的资源组项目的上下文菜单中，选择**部署**，然后选择资源组的名称。 

    ![部署的 azure 的资源组](./media/vs-azure-tools-resource-group-adding-resources/deploy-arm-resource-group.png)

    出现**部署到资源组**对话框。


1. 选择**部署**按钮。

1. 如果需要由您指定的任何参数，则会出现**编辑参数**对话框。 输入任何所需的值，然后选择**保存**按钮。 新的资源部署到 Azure 的资源组。

## 请参见

[创建和部署 Azure 资源组部署项目](http://go.microsoft.com/fwlink/p/?LinkID=623073)

[资源管理器的 azure Cmdlet](https://msdn.microsoft.com/library/dn654592.aspx)

[使用 Windows PowerShell 使用 Azure 资源管理器](../powershell-azure-resource-manager/)

[Channel9 视频︰ Azure 的资源管理器](http://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B224#fbid=)


测试
