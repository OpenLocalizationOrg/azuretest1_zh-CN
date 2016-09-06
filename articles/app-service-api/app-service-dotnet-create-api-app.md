---
ms.openlocfilehash: 6640edb23da7425019c77efc238ae867ad27fbfa
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="创建 ASP.NET API 在 Azure 应用程序服务的应用程序 |Microsoft Azure"
    description="了解如何使用 Visual Studio 2013 Azure 应用程序服务，在创建 ASP.NET API 的应用程序。"
    services="app-service\api"
    documentationCenter=".net"
    authors="bradygaster"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/14/2015"
    ms.author="tdykstra"/>

# 创建 ASP.NET API 在 Azure 应用程序服务的应用程序

> [AZURE.SELECTOR]
- [Visual Studio 2015年或 2013](app-service-dotnet-create-api-app.md)
- [Visual Studio 代码](app-service-create-aspnet-api-app-using-vscode.md)

## 概述

本教程演示如何创建 ASP.NET Web API 项目配置[API 在 Azure 应用程序服务的应用程序](app-service-api-apps-why-best-platform.md)部署到云的。 有关如何将现有的 Web API 项目以进行部署配置为 API 的应用程序的信息，请参阅[配置 Web API 项目作为 API 的应用程序](app-service-dotnet-create-api-app-visual-studio.md)。

系列中的后续教程将说明如何为[部署](app-service-dotnet-deploy-api-app.md)和[调试](../app-service-dotnet-remotely-debug-api-app.md)API 的应用程序项目，您在本教程中创建。

[AZURE.INCLUDE [install-sdk-2015-2013](../../includes/install-sdk-2015-2013.md)]

本教程需要 2.6 或更高版本的 Azure sdk 版本的.NET。

## 创建一个 API 的应用程序项目

说明指示您输入项目的名称，请输入**ContactsList**。

[AZURE.INCLUDE [app-service-api-create](../../includes/app-service-api-create.md)]

[AZURE.INCLUDE [app-service-api-review-metadata](../../includes/app-service-api-review-metadata.md)]

[AZURE.INCLUDE [app-service-api-define-api-app](../../includes/app-service-api-define-api-app.md)]

[AZURE.INCLUDE [app-service-api-direct-deploy-metadata](../../includes/app-service-api-direct-deploy-metadata.md)]

## 下一步行动

API 应用程序现已准备好进行部署，您可以按照[部署 API 应用程序](app-service-dotnet-deploy-api-app.md)教程来做到这一点。

测试
