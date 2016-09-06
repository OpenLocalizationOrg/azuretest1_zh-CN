---
ms.openlocfilehash: 9541bfb0f5db815a484e2e431c0326b39004d038
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
在此步骤中，您将测试可用性组侦听器使用同一网络上运行的客户端应用程序。

对于客户端连接，请注意下列要求︰

- 客户端连接到侦听程序必须来自承载 AlwaysOn 可用性复制副本驻留在不同的云服务比的机器。

- 如果 AlwaysOn 副本处于不同的子网中，则客户端必须指定"MultisubnetFailover = True"中的连接字符串。 这会导致尝试并行连接到不同的子网中的副本。 请注意这种情况下，包括跨地区 AlwaysOn 可用性组部署。

例如，可以从一个相同的 Azure VNet （但不是承载副本） 中的虚拟机连接到侦听程序。 完成此测试的简单方法是尝试连接到的可用性组侦听器的 SSMS。 另一种简单的方法是运行[SQLCMD.exe](https://technet.microsoft.com/library/ms162773.aspx) ，如下所示︰

    sqlcmd -S "<ListenerName>,<EndpointPort>" -d "<DatabaseName>" -Q "select @@servername, db_name()" -l 15

> [AZURE.NOTE] 如果 EndpointPort 的值为 1433年，它不必在调用中指定它。 前面的调用还假定客户机加入相同的域和调用方已被授予使用 windows 身份验证对数据库的权限。

当测试侦听器，请确保故障转移可用性组，以确保客户端可以连接到侦听程序在故障转移期间。