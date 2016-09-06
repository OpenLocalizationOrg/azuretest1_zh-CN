---
ms.openlocfilehash: c2457fe27242df830cfcc0f8970093c97a520d67
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
# 当终止或更改 ACOM 技术文章的名称的步骤

本指南适用于 Sme 被列为需要从 azure.microsoft.com 的技术文档部分撤除一篇文章的作者。 如果重命名文件时，这些步骤也适用。

如果您在 Azure 社区的成员，并且您认为文章应出于任何原因退出，请留下评论让知道有问题文章的作者文章 Disqus 评论流中。

SME 作者必须遵守以下几步来从容撤出内容，使网站的用户不能坏的经验，我们撤出从网站的内容时。 删除文章或更改它的名称应该是最后一件事 ！

## 步骤 1︰ 管理入站的链接

确定是否有任何非 Microsoft 入站的链接到您的内容。 通常情况下，博客、 论坛和 web 上的其他内容点的文章。 通常情况下，您可以使用博客所有者可以更改这些链接，并可以删除或更新论坛文章的链接。 Web 分析工具可以告诉您是否存在任何高流量入站的链接，您可能需要以这种方式管理。

## 步骤 2︰ 删除所有交叉链接到文章技术内容存储库

1. 确保使用最新的本地分支 – 运行的`git pull upstream master`（或在该命令的相应变化。

2.  扫描的 azure 内容-pr/文章文件夹和任何的 azure 的内容-pr/包括文件夹文章，包括链接到您想停用，这篇文章和交叉链接删除或将其替换为相应的新交叉链接。 您可以使用搜索和替换查找交叉链接，如果您有一个安装的实用程序。 如果不这样做，您可以免费使用 Windows PowerShell ！ 下面介绍了如何使用 PowerShell 查找交叉链接︰

 一。 启动 Windows PowerShell。

 b。 在 PowerShell 提示符处，更改到 azure 的内容-pr\articles 文件夹︰

 `cd azure-content-pr\articles`

 c。 键入以下命令，将列出包含您要删除的文章引用的所有文件︰

 `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name`

  如果您想发送到文本文件 （在此种情况下，命名 psoutput.txt) 文件名的列表，您可以︰

  `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name | Out-File C:\Users\<your account>\psoutput.txt`

3. 添加并提交所有更改、 将它们推送到您分叉和创建提取请求以将更改从您分叉移动到主资料库的主分支。

## 步骤 3︰ 更新 FWLink 工具

检查有任何可能指向文章的 FWLinks FWLink 工具。 指向任何 FWLinks 替换内容;如果不在拥有该链接的别名，请将它加入。 所有者不更新链接，如果文件为 MSCOM 更改该链接的票证。 详细信息-[内部 wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Manage%20inbound%20links%20to%20retired%20topics.aspx)。

## 第 4 步︰ 从 azure.microsoft.com 上的其他页面文章删除所有交叉链接，并在适当情况下创建废弃页的重定向

您需要使用维护并更新文档解决方案登录页，您为此部件的服务的人。 如果您不知道此人是谁，请联系您的内容的团队伙伴。 维护和更新文档的登陆页面的人需要做两件事︰

1. 在 Visual Studio 中，扫描**整个**ACOM web 解决方案的交叉引用指向要废弃的文件。 删除交叉引用，或将它们替换为更新交叉引用。 您将需要删除 HTML 链接的 HTML 链接的相关的资源字符串。 详细信息-请参见[内部 wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Create%20or%20edit%20a%20service%20landing%20page%20or%20left%20nav.aspx)

2. 如果更换项目存在，则创建重定向。 详细信息-请参见[内部 wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Remove%20published%20pages%20and%20request%20redirects.aspx)。

3. 检查到存储库中的更改。

## 步骤 5︰ 废弃文章

之后已经完成之前的三个步骤，这些更改是实时的然后您可以从存储库删除文章。
## 第 6 步︰ 从 MSDN 中删除链接

检查断开的链接到已撤销或重命名主题内容的 QA 工具和受到影响的所有 MSDN 主题中删除/修复链接。

## 第 7 步︰ 从搜索引擎中删除缓存的页

转到这些网页从搜索引擎中删除缓存的网页︰ [Bing](https://www.bing.com/webmaster/tools/content-removal?rflid=1)
[Google](https://www.google.com/webmasters/tools/removals?pli=1)


### 贡献者指南链接

- [概述文章](./../README.md)
- [指南文章索引](./contributor-guide-index.md)
