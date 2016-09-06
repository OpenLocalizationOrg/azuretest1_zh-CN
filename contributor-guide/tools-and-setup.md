---
ms.openlocfilehash: 5137506f4dd0d82f30a111a6667788a2b4256162
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
pageTitle="安装和设置在 GitHub 创作工具" 
description="工具和步骤来获取为创作在 GitHub 的 Azure 内容设置。" 
services="contributor-guide" 
documentationCenter="" 
authors="tysonn"  
manager="carolz" />

<tags 
ms.service="contributor-guide"
 ms.devlang="" 
 ms.topic="article"
  ms.tgt_pltfrm="" 
  ms.workload="" 
  ms.date="01/19/2015" 
  ms.author="tysonn" />

#安装和设置在 GitHub 创作工具

按照设置刀具到 Azure 的技术文档做出贡献的这篇文章中的步骤。 休闲和偶尔参与者可能可以使用 GitHub 步骤 2 中介绍的用户界面。

如果您熟悉 Git，可能要查看一些 Git 术语︰ [https://help.github.com/articles/github-glossary](https://help.github.com/articles/github-glossary)。 此外，此 StackOverflow 线程包含您会遇到下面这组步骤中的 Git 术语表︰ [http://stackoverflow.com/questions/7076164/terminology-used-by-git](http://stackoverflow.com/questions/7076164/terminology-used-by-git)

## 内容

- [创建 GitHub 帐户并设置您的配置文件](#create-a-github-account-and-set-up-your-profile)
- [注册 Disqus](#sign-up-for-disqus)
- [确定是否确实需要按照其余步骤](#determine-whether-you-really-need-to-follow-the-rest-of-these-steps)
- [在 GitHub 的权限](#permissions-in-github)
- [对于 Windows 安装 Git](#install-git-for-windows)
- [启用双因素身份验证](#enable-two-factor-authentication)
- [安装减价编辑器](#install-a-markdown-editor)
- [将配置凌动](#configure-atom)
- [分叉存储库并将其复制到您的计算机](#fork-the-repository-and-copy-it-to-your-computer)
- [配置您的用户名称和本地电子邮件](#configure-your-user-name-and-email-locally)
- [下一步行动](#next-steps)

## 创建 GitHub 帐户并设置您的配置文件

参加 Azure 的技术内容，您将需要[GitHub](http://www.github.com)帐户。

如果您的 Microsoft 参与者，您需要设置 GitHub 帐户，所以您要清楚地标识为 Microsoft 员工。 设置您的配置文件，如下所示︰

- **资料图片**︰ 图片的您 （必需）
- **名称**︰ 第一个和最后一个名称 （必需）
- **电子邮件**︰ Microsoft 电子邮件地址 （必填）
- **公司**︰ 微软公司 （必填）
- **位置**︰ 列出您所在的位置 （必需）

您的配置文件应类似于此配置文件︰

<p align="center">
 ![GitHub 的配置文件示例](./media/tools-and-setup/githubprofile.png)

## 注册 Disqus

每个已发布 Azure 技术文章有注释流 Disqus 服务提供的。

 ![一讨论徽标](./media/tools-and-setup/discus.png)

如果您是 Microsoft 员工，并且如果您是作者或投稿人的文章，就需要对签名 Disqus 因此可以参与文章的评论流。

1. 在[http://www.disqus.com/](http://www.disqus.com/)的帐户注册
2. 填写您的配置文件，如下所示︰

 - **全名**︰ 您的全名显示于 Microsoft 通讯簿列表括在括号中的信息，即您的别名加 @MSFT plus。 格式︰*名字姓氏 [alias@MSFT]*
 - **位置**︰ 您所在的位置
 - **简短的个人简历**︰ 您的标题

## 确定是否确实需要按照其余步骤

您可能不需要按照本文中的步骤操作。 它取决于内容提供您想要或需要进行排序。

###提交对现有项目的纯文本更改

如果您仅需要或想要对现有项目进行文本的更新，您可能不需要执行其余的步骤。 您可以使用 GitHub 的基于 web 的减价编辑器来提交您的更改。 只需单击您想要修改的文章在 GitHub 链接︰

 ![GitHub 的配置文件示例](./media/tools-and-setup/contributetogit.png)

 然后，单击编辑图标在 GitHub 版本的文章

 ![GitHub 的配置文件示例](./media/tools-and-setup/editicon.PNG)

 可以轻松地将更改提交简单易用的 web 编辑器打开它。 不需要按照本文中的其他步骤。

###所有其他更改
您需要安装工具，如果要进行任何以下类型的更改︰

 - 文章对重大更改
 - 创建和发布新的文章
 - 添加新的映像或更新图像
 - 无需发布更改每个那段日子天内更新一篇文章

 转到下一节 ！

##在 GitHub 的权限

GitHub 帐户的任何人都可以参与到 Azure 的技术内容，通过在[https://github.com/Azure/azure-content](https://github.com/Azure/azure-content)我们公共存储库。 不不需要任何特殊的权限。

如果 Microsoft 员工从事 Azure 内容，您应该在我们的私人工作内容存储库，领料/收货 azure 内容。 请访问[http://aka.ms/azuregithub](http://aka.ms/azuregithub)以获取将让您作出贡献通过专用 repo-单击**加入一个团队**，并加入**azure 内容读取**的读取的权限。

## 对于 Windows 安装 Git

为 Windows 安装 Git，从[http://git-scm.com/download/win](http://git-scm.com/download/win)。 此下载安装 Git 版本控制系统，并安装 Git 大扫除，将使用与您本地的 Git 存储库进行交互命令行应用程序。

您可以接受默认设置;如果希望 Windows 命令行中可用的命令，请，选择启用该选项。

<p align="center">
 ![GitHub 的配置文件示例](./media/tools-and-setup/gitbashinstall.png)

(注意︰ 这不是"Github 的 Windows"相同。 "Windows Github"是不同的基于 GUI 的工具，如果要仔细读一下自己也适用。 [https://windows.github.com/](https://windows.github.com/)) 

## 启用双因素身份验证

您必须启用两因素身份验证 (2FA) 在 GitHub 帐户如果您在使用专用的内容存储库。 它需要在专用存储库中。

若要启用此功能，请按照这两个以下 GitHub 的帮助主题中的说明进行操作︰

- [大约两因素身份验证](https://help.github.com/articles/about-two-factor-authentication/)
- [创建命令行使用一个访问令牌](https://help.github.com/articles/creating-an-access-token-for-command-line-use/)

启用 2FA 之后，您必须输入而不是在命令提示符下 GitHub 密码的访问标记，当您尝试从命令行访问 GitHub 存储库。 访问令牌不设置 2FA 时进入一条文本消息身份验证代码。 它是一个长字符串，如下所示︰ fdd3b7d3d4f0d2bb2cd3d58dba54bd6bafcd8dee。 有关这几个注意事项︰

- 创建访问令牌时，请将其保存在文本文件中以使其在您需要时随时可以访问。

- 稍后，当您需要粘贴标记，知道有两种方法在命令行中粘贴︰

 - 单击命令行窗口左上角的图标 > 编辑 > 粘贴。
 - 右键单击窗口左上角的图标，然后单击属性 > 选项 > 快速编辑模式。 这使您可以通过在命令行窗口中右键单击粘贴将配置命令行。

## 安装减价编辑器

我们创作内容在文件中，而不使用简单的"减价"法比复杂"标记"（HTML、 XML 等）。 因此，您需要安装减价编辑器。

- **Atom**︰ 我们大多数人来说使用 GitHub 的 Atom 减价编辑器︰ [http://atom.io](http://atom.io)。 它不用于商业用途需要一个许可证。 它具有拼写检查。 

- **记事本**︰ 您可以使用记事本非常轻便的选项。

- **枯燥文字活泼起来**︰ 这是一种轻型、 简洁、 在线和开放源减价编辑器提供预览。 请访问[http://prose.io](http://prose.io)和授权规范您的存储库中的文本。

- **[Visual Studio 代码](https://www.visualstudio.com/products/code-vs.aspx)**-微软在这一领域的入口。

## 将配置凌动

如果您使用 Atom，您需要设置的几种方法。

- Atom 默认的选项卡上，使用 2 空格，但减价要求 4 个空格。 如果您将其保留在默认的两个，您的文章看起来非常棒，在本地的预览，但不是在它被导入到 Azure。 因此，配置凌动使用 4 个空格-您可以在文件下找到此设置 > 设置 > 编辑器设置 > 制表符长度。 
- 可能还需要打开软换行此部分中，其作用相同，记事本中的"自动换行"。 
- 若要打开减价预览，请单击包 > 减价预览 > 切换预览。 可以使用 Ctrl-Shift-M 可切换预览 HTML 视图。 

## 分叉存储库并将其复制到您的计算机

1. 在 GitHub 中创建存储库的分叉-转到的页面的右上角，单击分叉按钮。 如果出现提示，请选择您的帐户应在其中创建分叉的位置。 这将创建存储库中的 Git 中心帐户的副本。 一般来说，技术编写人员以及项目经理需要分叉 azure 内容 pr，专用 repo。 分叉 azure 内容，公共 repo 需要社区参与者。 您只需分叉一次;之后您首次设置，如果您想要将您分叉复制到另一台计算机，您只需运行命令，请按照本部分将 repo 复制到您的计算机中。  如果您选择创建派生的两个资料库，您需要创建派生的每个存储库。

2. 将复制的个人访问令牌从[https://github.com/settings/applications#personal-access-tokens](https://github.com/settings/applications#personal-access-tokens)获得。 您可以接受该标记的默认权限。  在以后重复使用的文本文件中保存个人的访问令牌。

3. 接下来，使用您的凭据在命令字符串中嵌入复制到计算机上的存储库。  若要执行此操作，请打开 GitBash。  在命令提示符下输入以下命令。  此命令创建在您的计算机上的 azure-content(-pr) drectory。  如果您正在使用默认的位置，它将在 c:\users<your Windows user name>\azure-content(-pr)。

公共 repo:

        git clone https://[your GitHub user name]:[token]@github.com/<your GitHub user name>/azure-content.git

专用的 repo:

        git clone https://[your GitHub user name]:[token]@github.com/<your GitHub user name>/azure-content-pr.git

例如，该克隆命令可能如下所示︰

        git clone https://smithj:b428654321d613773d423ef2f173ddf4a312345@github.com/smithj/azure-content-pr.git  

## 设置远程资源库中的连接和配置凭据

通过输入以下命令来创建到根存储库的引用。 此设置在 GitHub 的存储库的连接，以便可以获取到您的本地计算机上最新的更改并将更改推送回 GitHub。 本地，以便不需要输入用户名和密码每次尝试访问上游 repo 并在 GitHub 上的您分叉，此命令还会配置您的令牌。

公共 repo:

        cd azure-content
        git remote add upstream https://[your GitHub user name]:[token]@github.com/Azure/azure-content.git
        git fetch upstream

专用的 repo:

        cd azure-content-pr
        git remote add upstream https://[your GitHub user name]:[token]@github.com/Azure/azure-content-pr.git
        git fetch upstream

这通常需要一段时间。 执行此操作后，您不必再次分叉或重新输入您的凭据。 您只需要铲车到本地计算机重新复制如果工具设置另一台计算机上。


## 配置您的用户名称和本地电子邮件

若要确保正确作为参与者列出，您需要在 Git 中本地配置您的用户名称和电子邮件。

1. 启动 Git 大扫除，并切换到 azure 内容或领料/收货 azure 内容︰

   ````
   cd azure-content
   ````

 或

   ````
   cd azure-content-pr
   ````

2. 配置您的用户名，以便它按照您设置在 GitHub 配置文件匹配您的姓名︰

    ````
    git config --global user.name "John Doe"
    ````
3. 配置您的电子邮件，使其匹配主要的电子邮件，指定在 GitHub 配置文件;如果您在 MSFT 员工，它应该是 MSFT 电子邮件地址︰

    ````
    git config --global user.email "alias@example.com"
    ````
4. 类型`git config -l`和检查您的本地设置，以确保用户名称和电子邮件配置中的是正确的。

##下一步行动

- [创建一个本地工作分支](./git-commands-for-master.md)计算机，以便您可以开始工作。
- 复制[减价模板](../markdown templates/markdown-template-for-new-articles.md)，作为新项目的基础。





###贡献者指南链接

- [概述文章](./../README.md)
- [指南文章索引](./contributor-guide-index.md)



<!--Anchors-->
[使用客户友好的声音]: #use-a-customer-friendly-voice
[请考虑本地化和机器翻译]: #consider-localization-and-machine-translation
[若要监视的其他样式和语音的问题]: #other-style-and-voice-issues-to-watch-for


[创建 GitHub 帐户并设置您的配置文件]: #create-a-github-account-and-set-up-your-profile
[确定是否确实需要按照其余步骤]: #determine-whether-you-really-need-to-follow-the-rest-of-these-steps
[在 GitHub 的权限]: #permissions-in-github
[对于 Windows 安装 Git]: #install-git-for-windows
[启用双因素身份验证]: #enable-two-factor-authentication
[安装减价编辑器]: #install-a-markdown-editor
[分叉存储库并将其复制到您的计算机]: #fork-the-repository-and-copy-it-to-your-computer
[安装 git 的凭据-winstore]: #install-git-credential-winstore
[注册 Disqus]: #sign-up-for-disqus
[配置您的用户名称和本地电子邮件]: #configure-your-user-name-and-email-locally
[下一步行动]: #next-steps
