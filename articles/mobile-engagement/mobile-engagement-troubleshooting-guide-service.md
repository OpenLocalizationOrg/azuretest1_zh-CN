---
ms.openlocfilehash: 636ec707ec5a5fede2781fbb235ba0b1ad9c9535
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="故障排除指南-服务的 azure 移动服务" 
   description="故障排除指南 Azure 的移动服务" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="06/18/2015"
   ms.author="piyushjo"/>

# 服务问题的故障排除指南

下面是可能出现的问题，您可能会遇到与 Azure 移动服务的运行方式。

## 服务中断

### 问题
- 看起来由 Azure 移动签约服务中断的问题。

### 原因
- 似乎由 Azure 移动签约服务中断的问题可以导致不同的几个问题︰
    - 最初出现系统性 Azure 移动服务的所有的独立的问题
    - 导致的服务器停机时间 （不总是显示在服务器的状态） 的已知的问题︰
    - 计划延迟、 目标错误、 工牌更新问题、 统计停止收集，强制停止工作，API 的停止工作，新应用程序或用户无法创建，DNS 错误和超时错误在用户界面、 API 或应用程序在设备上。
    - 云的从属关系中断[Azure 服务状态](http://status.azure.com/)
    - 推送通知服务 （企业型） 相关性停机
    - 应用程序商店停机

1) 进行测试以查看问题是否是系统可以测试相同的功能，从不同
   
   - Azure 移动服务集成应用程序
   - 测试设备
   - 测试设备操作系统版本
   - 市场活动
   - 管理员用户帐户
   - 浏览器 （IE，Firefox、 镶边等）
   - 计算机

2) 如果该问题只影响用户界面或 API 的，测试︰

   - 测试从 Azure 的移动服务用户界面和 Azure 移动服务 API 相同的功能。

3) 如果问题与您的移动电话网络，测试︰

   - 在连接到互联网通过 WIFI，而通过 3g 手机网络连接进行测试。
   - 确认，您的防火墙不会阻止任一 Azure 移动服务的 IP 地址或端口。

4) 若要测试问题是否与您的设备︰

   - 测试您的设备能够与其他 Azure 移动服务集成应用程序连接到 Azure 移动服务。
   - 您可以在 Azure 的移动服务用户界面的电话从生成事件、 作业和崩溃的测试。 
   - 测试是否可以将 Azure 的移动服务用户界面的推式通知发送到设备中基于其设备 id。 

5) 若要测试问题是否与您的应用程序︰

   - 安装和测试您的应用程序，而不是从物理设备仿真程序中︰
   
6) 若要测试是否与操作系统升级到最终用户设备，需要 SDK 升级来解决的问题是︰

   - 测试不同版本的操作系统的不同设备上的应用程序。
   - 请确认您使用的最新版本的 sdk。
 
## 连接和不正确的信息问题

### 问题
- 登录到 Azure 的移动服务用户界面的问题。
- 使用 Azure 移动服务 API 的连接错误。
- 上传通过设备 API 的应用程序信息标记时出现问题。
- 从 Azure 移动服务下载日志或导出的数据时出现问题。
- 在 Azure 移动服务用户界面中显示的信息不正确。
- 在 Azure 移动服务日志中显示的信息不正确。

### 原因
* 确认您的用户帐户具有足够权限来执行该任务。
* 确认问题未发生在一台计算机或本地网络。
* 确认 Azure 移动签约服务没有报告停机。
* 确认您的应用程序信息标记文件遵循所有这些规则︰
    - 使用仅 UTF8 字符集 （ANSI 字符集不受支持）。
    - 使用逗号"，"作为分隔符 (您可以打开一个服务请求以请求，将.csv 分隔符字符从逗号"，"到另一个字符，例如以分号";")。
    - 使用布尔值全部小写，"真正的"和"假"。
    - 使用小于 35 MB 的最大文件大小的文件。
 
测试
