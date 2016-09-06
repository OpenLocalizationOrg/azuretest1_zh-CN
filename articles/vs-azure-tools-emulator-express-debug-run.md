---
ms.openlocfilehash: e34ab8b42a02f69de2fd74bbd2ab34cbcf7b3b65
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="使用仿真程序表达来运行和调试云服务在本地计算机上的 |Microsoft Azure"
   description="使用仿真程序表达来运行和调试云服务在本地计算机上"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="patshea123"
   manager="douge"
   editor="tlee" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/14/2015"
   ms.author="patshea" />


# 使用仿真程序表达来运行和调试云服务在本地计算机上

通过使用仿真程序表达，可以测试和调试不以管理员身份运行 Visual Studio 的云服务。 您可以设置您的项目设置，以便使用仿真程序表达或完整的仿真程序，具体取决于云服务的要求。 有关完整的仿真程序的详细信息，请参阅[Azure 应用程序在仿真程序中运行](https://msdn.microsoft.com/library/azure/hh403990.aspx)。 仿真程序快速最初包含在 Azure SDK 2.1，并从 Azure SDK 2.3，它已经是默认模拟器。

## 在 Visual Studio IDE 使用仿真器速成版

创建新的项目在 Azure SDK 2.3 或更高版本时，仿真程序表达已被选中。 使用早期版本的 SDK 创建的现有项目，请按照以下步骤选择仿真程序表达。

### 若要将项目配置为使用仿真程序表达

1. 在 Azure 项目的快捷菜单中，选择**属性**，然后选择**Web**选项卡。

1. 在**本地开发服务器**，选择**使用 IIS Express 选项**按钮。 速成版仿真程序不兼容 IIS Web 服务器。

1. 在**仿真程序**中，选择**使用仿真程序表达**选项按钮。

    ![仿真程序速成版](./media/vs-azure-tools-emulator-express-debug-run/IC673363.gif)

## 在命令提示符下启动仿真程序表达

在命令提示符处，您可以使用 /useemulatorexpress 选项启动计算仿真程序，csrun.exe 的 express 版本。

## 限制

使用仿真程序表达之前，应了解一些限制︰

- 速成版仿真程序不兼容 IIS Web 服务器。

- 云服务可以包含多个角色，但每个角色都限制在一个实例。

- 您不能访问 1000年以下的端口号。 例如，如果您使用的身份验证提供程序通常使用 1000年以下的端口，您可能需要将此值更改为上面 1000年的端口号。

- 适用于计算仿真程序的任何限制也适用于仿真程序表达。 例如，不能将每个部署的 50 多个角色实例。 [运行计算仿真程序中的 Azure 应用程序](http://go.microsoft.com/fwlink/p/?LinkId=623050)，请参阅

## 下一步行动

[调试的云服务](https://msdn.microsoft.com/library/azure/ee405479.aspx)


测试
