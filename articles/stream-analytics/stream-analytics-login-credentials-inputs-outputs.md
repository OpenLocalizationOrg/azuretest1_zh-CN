---
ms.openlocfilehash: fc46687194f12e037c8885c170ac3cef222311f9
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="流分析︰ 旋转的输入和输出登录凭据 |Microsoft Azure" 
    description="了解如何更新流分析输入和输出的凭据。" 
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="08/19/2015" 
    ms.author="jeffstok"/>

#旋转的输入/输出凭据

##摘要
Azure 流分析目前不允许作业正在运行时替换输入/输出上的凭据。

虽然 Azure 流分析支持恢复从最后输出的作业，我们希望共享最小化滞后于停止和启动作业的整个过程。

##第 1 部分-准备新的一套凭证︰
这就是适用于以下的输入/输出︰

* Blob 存储
* 事件集线器
* SQL 数据库
* 表存储

对于其他输入/输出，继续第 2 部分。

###网络日志存储/表存储
1.  在 Azure 管理门户网站，请转到存储扩展︰  
![graphic1][graphic1]
2.  找到您的作业所使用的存储器并进入它︰  
![页图 2][graphic2]
3.  请单击管理访问键命令︰  
![graphic3][graphic3]
4.  之间的主键访问辅助访问键，**选择不使用您的工作**。
5.  命中再生︰  
![graphic4][graphic4]
6.  将复制新生成的密钥︰  
![graphic5][graphic5]
7.  请转到第 2 部分。

###事件集线器
1.  在 Azure 管理门户网站，请转到服务总线扩展︰  
![graphic6][graphic6]
2.  找到您的作业使用服务总线 Namespace 并进入它︰  
![graphic7][graphic7]
3.  如果您的工作在服务总线 Namespace 上使用共享的访问策略，跳转到步骤 6  
4.  转到事件集线器选项卡︰  
![graphic8][graphic8]
5.  找到您作业使用事件中心并进入它︰  
![graphic9][graphic9]
6.  转至配置选项卡︰  
![graphic10][graphic10]
7.  策略名称下拉列表，查找您的作业所使用的共享的访问策略︰  
![graphic11][graphic11]
8.  之间为主键和辅助键，**选择不使用您的工作**。  
9.  命中再生︰  
![graphic12][graphic12]
10. 将复制新生成的密钥︰  
![graphic13][graphic13]
11. 请转到第 2 部分。  

###SQL 数据库

>[AZURE.NOTE] 注意︰ 您需要连接到 SQL 数据库服务。 我们将显示如何使用 Azure 管理门户上的管理经验，但您可以选择使用一些客户端工具，如 SQL Server 管理 Studio 也。

1.  在 Azure 管理门户网站，请转到 SQL 数据库扩展︰  
![graphic14][graphic14]
2.  找到在同一行上链接作业，**请单击服务器上**所使用的 SQL 数据库︰  
![graphic15][graphic15]
3.  请单击管理命令︰  
![graphic16][graphic16]
4.  类型数据库的主服务器︰  
![graphic17][graphic17]
5.  键入您的用户名，密码，并单击登录:  
![graphic18][graphic18]
6.  单击新建查询:  
![graphic19][graphic19]
7.  在下面的查询将 < login_name > 替换为您的用户名和替换类型<enterStrongPasswordHere>与您的新密码︰  
`CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'`
8.  单击运行:  
![graphic20][graphic20]
9.  请返回到步骤 2 并此时间单击数据库︰  
![graphic21][graphic21]
10. 请单击管理命令︰  
![graphic22][graphic22]
11. 键入您的用户名，密码，并单击登录:  
![graphic23][graphic23]
12. 单击新建查询:  
![graphic24][graphic24]
13. 在下面的查询替换您要标识此登录名的数据库 （您可以提供您，例如给予 < login_name >，为相同的值） 的上下文中的名称为 < 用户名 > 和 < login_name > 替换为新的用户名，请键入︰  
`CREATE USER <user_name> FROM LOGIN <login_name>`
14. 单击运行:  
![graphic25][graphic25]
15. 现在，您应该提供新的用户具有相同的角色和您原始的用户具有的权限。
16. 请转到第 2 部分。

##第 2 部分︰ 停止流分析作业
1.  在 Azure 管理门户网站，请转到流分析扩展︰  
![graphic26][graphic26]
2.  找到您的工作并进入它︰  
![graphic27][graphic27]
3.  转到输入选项卡或输出选项卡基于是否旋转一个输入或输出上的凭据。  
![graphic28][graphic28]
4.  单击停止命令并确认作业已停止︰  
![graphic29][graphic29]等待要停止的作业。
5.  找到您想要上旋转的凭据并进入它的输入/输出︰  
![graphic30][graphic30]
6.  进入第 3 部分。

##第 3 部分︰ 编辑流分析作业中的凭据

###Blob 存储/表存储
1.  查找存储帐户密钥字段并将新生成的密钥粘贴到其中︰  
![graphic31][graphic31]
2.  单击保存命令，并确认将保存您的更改︰  
![graphic32][graphic32]
3.  当您保存所做的更改时，将自动启动连接测试，它是确保已成功通过。
4.  进入第 4 部分。

###事件集线器
1.  查找事件中心策略注册表项中的字段，并将新生成的密钥粘贴到其中︰  
![graphic33][graphic33]
2.  单击保存命令，并确认将保存您的更改︰  
![graphic34][graphic34]
3.  连接测试将自动启动，当您保存更改，请确保已成功通过。
4.  进入第 4 部分。

###双电源
1.  请单击更新授权︰  
* ![graphic35][graphic35]
* 您将收到以下确认︰  
* ![graphic36][graphic36]
2.  单击保存命令，并确认将保存您的更改︰  
![graphic37][graphic37]
3.  当您保存更改时，将自动开始连接测试，请确保它已经成功通过。
4.  进入第 4 部分。

###SQL 数据库
1.  查找用户名称和密码字段并粘贴到您新创建的一套凭证︰  
![graphic38][graphic38]
2.  单击保存命令，并确认将保存您的更改︰  
![graphic39][graphic39]
3.  连接测试将自动启动，当您保存更改，请确保已成功通过。  
4.  进入第 4 部分。

##第 4 部分︰ 从上次停止的时间开始您的工作
1.  离开输入/输出︰  
![graphic40][graphic40]
2.  请单击开始命令︰  
![graphic41][graphic41]
3.  拿最近的停止时间并单击确定:  
 ![graphic42][graphic42]
4.  请继续第 5 部分。  

##第 5 部分︰ 删除旧的一套凭证
这就是适用于以下的输入/输出︰
* Blob 存储
* 事件集线器
* SQL 数据库
* 表存储

###Blob 存储/表存储
对于以前曾被您的作业更新现在未使用的访问键访问键，重复第 1 部分。

###事件集线器
对于以前曾被您的作业更新现在未使用的键的键，重复第 1 部分。

###SQL 数据库
1.  请返回到查询窗口部件 1 第 7 步和类型在以下查询中，< previous_login_name > 替换以前曾被您的作业的用户名︰  
`DROP LOGIN <previous_login_name>`  
2.  单击运行:  
    ![graphic43][graphic43]  

您应收到以下确认︰ 

    Command(s) completed successfully.

## 获取帮助
进一步的帮助，请尝试我们的[Azure 流分析论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## 下一步行动

- [Azure 流分析简介](stream-analytics-introduction.md)
- [开始使用 Azure 流分析](stream-analytics-get-started.md)
- [小数位数 Azure 流分析作业](stream-analytics-scale-jobs.md)
- [Azure 流分析查询语言参考](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure 流分析管理 REST API 参考](https://msdn.microsoft.com/library/azure/dn835031.aspx)


[graphic1]: ./media/stream-analytics-login-credentials-inputs-outputs/1-stream-analytics-login-credentials-inputs-outputs.png
[页图 2]: ./media/stream-analytics-login-credentials-inputs-outputs/2-stream-analytics-login-credentials-inputs-outputs.png
[graphic3]: ./media/stream-analytics-login-credentials-inputs-outputs/3-stream-analytics-login-credentials-inputs-outputs.png
[graphic4]: ./media/stream-analytics-login-credentials-inputs-outputs/4-stream-analytics-login-credentials-inputs-outputs.png
[graphic5]: ./media/stream-analytics-login-credentials-inputs-outputs/5-stream-analytics-login-credentials-inputs-outputs.png
[graphic6]: ./media/stream-analytics-login-credentials-inputs-outputs/6-stream-analytics-login-credentials-inputs-outputs.png
[graphic7]: ./media/stream-analytics-login-credentials-inputs-outputs/7-stream-analytics-login-credentials-inputs-outputs.png
[graphic8]: ./media/stream-analytics-login-credentials-inputs-outputs/8-stream-analytics-login-credentials-inputs-outputs.png
[graphic9]: ./media/stream-analytics-login-credentials-inputs-outputs/9-stream-analytics-login-credentials-inputs-outputs.png
[graphic10]: ./media/stream-analytics-login-credentials-inputs-outputs/10-stream-analytics-login-credentials-inputs-outputs.png
[graphic11]: ./media/stream-analytics-login-credentials-inputs-outputs/11-stream-analytics-login-credentials-inputs-outputs.png
[graphic12]: ./media/stream-analytics-login-credentials-inputs-outputs/12-stream-analytics-login-credentials-inputs-outputs.png
[graphic13]: ./media/stream-analytics-login-credentials-inputs-outputs/13-stream-analytics-login-credentials-inputs-outputs.png
[graphic14]: ./media/stream-analytics-login-credentials-inputs-outputs/14-stream-analytics-login-credentials-inputs-outputs.png
[graphic15]: ./media/stream-analytics-login-credentials-inputs-outputs/15-stream-analytics-login-credentials-inputs-outputs.png
[graphic16]: ./media/stream-analytics-login-credentials-inputs-outputs/16-stream-analytics-login-credentials-inputs-outputs.png
[graphic17]: ./media/stream-analytics-login-credentials-inputs-outputs/17-stream-analytics-login-credentials-inputs-outputs.png
[graphic18]: ./media/stream-analytics-login-credentials-inputs-outputs/18-stream-analytics-login-credentials-inputs-outputs.png
[graphic19]: ./media/stream-analytics-login-credentials-inputs-outputs/19-stream-analytics-login-credentials-inputs-outputs.png
[graphic20]: ./media/stream-analytics-login-credentials-inputs-outputs/20-stream-analytics-login-credentials-inputs-outputs.png
[graphic21]: ./media/stream-analytics-login-credentials-inputs-outputs/21-stream-analytics-login-credentials-inputs-outputs.png
[graphic22]: ./media/stream-analytics-login-credentials-inputs-outputs/22-stream-analytics-login-credentials-inputs-outputs.png
[graphic23]: ./media/stream-analytics-login-credentials-inputs-outputs/23-stream-analytics-login-credentials-inputs-outputs.png
[graphic24]: ./media/stream-analytics-login-credentials-inputs-outputs/24-stream-analytics-login-credentials-inputs-outputs.png
[graphic25]: ./media/stream-analytics-login-credentials-inputs-outputs/25-stream-analytics-login-credentials-inputs-outputs.png
[graphic26]: ./media/stream-analytics-login-credentials-inputs-outputs/26-stream-analytics-login-credentials-inputs-outputs.png
[graphic27]: ./media/stream-analytics-login-credentials-inputs-outputs/27-stream-analytics-login-credentials-inputs-outputs.png
[graphic28]: ./media/stream-analytics-login-credentials-inputs-outputs/28-stream-analytics-login-credentials-inputs-outputs.png
[graphic29]: ./media/stream-analytics-login-credentials-inputs-outputs/29-stream-analytics-login-credentials-inputs-outputs.png
[graphic30]: ./media/stream-analytics-login-credentials-inputs-outputs/30-stream-analytics-login-credentials-inputs-outputs.png
[graphic31]: ./media/stream-analytics-login-credentials-inputs-outputs/31-stream-analytics-login-credentials-inputs-outputs.png
[graphic32]: ./media/stream-analytics-login-credentials-inputs-outputs/32-stream-analytics-login-credentials-inputs-outputs.png
[graphic33]: ./media/stream-analytics-login-credentials-inputs-outputs/33-stream-analytics-login-credentials-inputs-outputs.png
[graphic34]: ./media/stream-analytics-login-credentials-inputs-outputs/34-stream-analytics-login-credentials-inputs-outputs.png
[graphic35]: ./media/stream-analytics-login-credentials-inputs-outputs/35-stream-analytics-login-credentials-inputs-outputs.png
[graphic36]: ./media/stream-analytics-login-credentials-inputs-outputs/36-stream-analytics-login-credentials-inputs-outputs.png
[graphic37]: ./media/stream-analytics-login-credentials-inputs-outputs/37-stream-analytics-login-credentials-inputs-outputs.png
[graphic38]: ./media/stream-analytics-login-credentials-inputs-outputs/38-stream-analytics-login-credentials-inputs-outputs.png
[graphic39]: ./media/stream-analytics-login-credentials-inputs-outputs/39-stream-analytics-login-credentials-inputs-outputs.png
[graphic40]: ./media/stream-analytics-login-credentials-inputs-outputs/40-stream-analytics-login-credentials-inputs-outputs.png
[graphic41]: ./media/stream-analytics-login-credentials-inputs-outputs/41-stream-analytics-login-credentials-inputs-outputs.png
[graphic42]: ./media/stream-analytics-login-credentials-inputs-outputs/42-stream-analytics-login-credentials-inputs-outputs.png
[graphic43]: ./media/stream-analytics-login-credentials-inputs-outputs/43-stream-analytics-login-credentials-inputs-outputs.png
 
