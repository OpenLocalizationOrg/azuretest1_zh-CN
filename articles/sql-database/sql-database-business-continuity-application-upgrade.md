---
ms.openlocfilehash: 64066f5e5696b522cda0af90e9785b54ef8a243b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="在应用程序升级过程中的 SQL 数据库业务连续性" 
   description="本部分提供了业务连续性，以防止应用程序在升级期间的停机时间的指南。" 
   services="sql-database"
   documentationCenter="" 
   authors="elfisher" 
   manager="jeffreyg" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="07/14/2015"
   ms.author="elfish"/>

#在不停机的情况下升级应用程序

在应用程序一词指组件如前端的 Microsoft Azure 的上下文中，部署在云服务中，服务和数据层用于保存应用程序数据或元数据。 云应用程序通常用于提供不间断的全天候服务。 推出新版本的应用程序，在实际站点中更改数据层中的应用时可能有可能造成一些中断，例如减少可用的功能或甚至完全的停机时间。 

在设计应用程序升级过程的主要目标应消除或大大减少的持续时间时应用程序功能将减少。 若要实现过程通常涉及创建要用作备份，以防升级失败的应用程序的临时副本。 在设计和规划升级时，应考虑以下因素︰

1.  当应用程序将功能较差的最大可接受的时间 
2.  最小的一组将在升级过程中提供的功能
3.  若要回滚任何错误发生在升级过程中的能力。
4.  所涉及的总成本。  这包括创建 （例如，活动的各地区复制的其他高级数据库） 的临时副本所需的其他应用程序组件的成本和增量成本对于升级过程所使用的临时部署。 

如果应用程序暂时可以在只读模式下运行升级工作流可以用于有效地完全消除停机时间。 若要了解如何实现升级您特定的应用程序的拓扑结构的工作流请参考[到 Azure SQL 数据库应用程序的滚动升级过程的中断降至最低的最佳做法](https://msdn.microsoft.com/library/azure/dn790385.aspx)
 
 
