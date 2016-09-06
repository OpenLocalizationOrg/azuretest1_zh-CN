---
ms.openlocfilehash: 8ad8688fe481498643b5fbd1eb0c9fe13d3193c8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="使用 Visual Studio 的 Azure 的云服务项目中的管理角色"
   description="了解如何向 Azure 的云服务项目添加新的角色或从中删除现有角色，通过使用 Visual Studio。"
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

# 使用 Visual Studio 的 Azure 的云服务项目中的管理角色

您创建了 Azure 的云服务项目后，可以向其中添加新的角色或移除现有角色。 此外可以导入现有项目，并将其转换为一个角色。 例如，您可以导入 ASP.NET web 应用程序并将其指定为 web 角色。

## 添加或删除角色

**添加角色**

在解决方案资源管理器，云服务项目中打开快捷菜单上的**角色**节点并选择**添加**。 您可以从当前解决方案中选择一个现有的 web 角色或辅助角色或创建新的 web 或辅助角色项目。 或者，您可以选择适当的项目，如 ASP.NET web 应用程序项目，并将其与一个角色的项目。

**若要移除角色关联**

在解决方案资源管理器中的云服务项目**角色**节点中，打开要删除并选择**删除**的角色的快捷菜单。

##删除并在云服务中添加角色

如果从云服务项目中删除一个角色，但后来又决定将角色添加回项目中，添加角色声明和基本特性，如终结点和诊断信息。 没有额外的资源或引用被添加到 ServiceDefinition.csdef 文件或 ServiceConfiguration.cscfg 文件。 如果您想要添加此信息，您必须手动将其添加到这些文件。

例如，可能会删除 web 服务角色而稍后将此角色添加回解决方案。 如果您这样做时，将发生错误。 若要避免此错误，您需要添加<LocalResources>到 ServiceDefinition.csdef 文件显示在下面的 XML 中的元素。 使用 web 服务角色的名称属性的一部分添加回项目的名称**<LocalStorage>**元素。 在此示例 web 服务角色的名称是 WCFServiceWebRole1:

    <WebRole name="WCFServiceWebRole1">
        <Sites>
          <Site name="Web">
            <Bindings>
              <Binding name="Endpoint1" endpointName="Endpoint1" />
            </Bindings>
          </Site>
        </Sites>
        <Endpoints>
          <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
          <Import moduleName="Diagnostics" />
        </Imports>
       <LocalResources>
          <LocalStorage name="WCFServiceWebRole1.svclog" sizeInMB="1000" cleanOnRoleRecycle="false" />
       </LocalResources>
    </WebRole>


测试
