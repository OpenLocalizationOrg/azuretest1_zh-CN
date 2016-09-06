---
ms.openlocfilehash: 0acf706088dcd178e51d419f5b8bf92546647990
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="Azure 备份限制表"
   description="描述用于 Azure 备份系统的限制。"
   services="backup"
   documentationCenter="NA"
   authors="Jim-Parker"
   manager="jwhit"
   editor="" />
<tags
   ms.service="backup"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="07/01/2015"
   ms.author="jimpark"; "aashishr" />


以下限制适用于 Azure 备份。

| 限制标识符 | 默认限制 |
|---|---|
|可以针对每个存储库注册/服务器计算机的数目|50|
|在 Azure 存储库存储中存储的数据的数据源的大小|1.65 TB max<sup>1</sup>|
|备份可以在 Azure 的每个订阅中创建的电子仓库的数量|25|
|可以每日计划备份的次数|对于 Windows 服务器/客户端每天三次 <br/> 每个工作日 SCDPM 2|
|可以创建的恢复点的数量|366<sup>2</sup>|
|附加到 Azure 的虚拟机的备份数据磁盘|5|

- <sup>1</sup>1.65 TB 的限制不适用于 IaaS VM 备份。
- <sup>2</sup>可以使用任意排列到达这是小于 366 号。
