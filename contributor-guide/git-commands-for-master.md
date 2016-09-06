---
ms.openlocfilehash: 04abcfb42d92177b201d226d4bf6bf6560c00fa5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties pageTitle="Git 命令创建新文章或更新现有项目" description="如何使用技术 Azure 内容 GitHub 存储库。" metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="carolz" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="01/16/2015" ms.author="tysonn" />

# Git 命令创建新文章或更新现有项目


## 标准过程 （从母版）
按照这篇文章在您的计算机上创建本地的工作分支，以便可以创建新文章的技术文档部分的 azure.microsoft.com 或更新现有项目中的步骤。

![](./media/git-commands-for-master/githubcommands1.png)

1. 启动 Git Bash （或使用 Git 的命令行工具）。

 **注意︰**如果您正在使用公用存储库中，更改领料/收货 azure 内容到 azure 内容中的所有命令。

2. 领料/收货 azure 内容改为︰

        cd azure-content-pr
3. 签出该主分支︰

        git checkout master

4. 创建新本地工作分支从主分支︰

        git pull upstream master:<working branch>


5. 将移动到新的工作分支中︰

        git checkout <working branch>

6. 让您知道您创建本地工作分支分叉︰

        git push origin <working branch>

7. 创建新的文章或对现有项目进行更改。 使用 Windows 资源管理器打开和创建新减价文件，并使用 Atom (http://atom.io) 作为减价编辑器。 创建或修改您的文章和图像后，请转到下一步。

8. 添加并提交您所做的更改︰

        git add .
        git commit –m "<comment>"
        
   或者，添加特定于仅可以修改文件︰

        git add <file path>
        git commit –m "<comment>"

9. 从上游的更改更新您的本地工作分支︰

        git pull upstream master

10. 将更改推送到您分叉，GitHub 上︰

        git push origin <working branch>

12. 当您准备好您的提交内容暂存，验证，和/或出版，上游主分支在 GitHub UI 中，拉请求从创建您分叉到主分支。

13. 拉请求接受者审阅请求申请、 提供反馈信息，和/或接受您请求的请求。 

14. 请验证您已发布的文章或更改

 http://azure.microsoft.com/documentation/articles/*name-of-your-article-without-the-MD-extension*

**注释︰**

- 这一次，一旦周围 10 上午太平洋标准时间 （太平洋标准时间） 星期一至星期五每日发布的技术文章。 请记住，您请求的请求已被接受之前更改将包含在运行下一个计划的发布。
- 如果是员工专用存储库中，拉的所有请求都都将遵守必须先解决，才可以接受拉请求的验证规则。 
- 在专用 repo，所做的更改在您提交自动转移和临时链接写入到拉请求。 请检查您暂存的内容并注销拉请求注释中指示所做的更改已准备好将实时。 如果您不希望拉请求被接受的如果只临时更改-，添加该注意到拉请求。

## 使用版本分支

当您正在使用版本分支时，从发行分支创建本地工作分支的最好办法是使用该命令语法︰

    git checkout upstream/<upstream branch name> -b <local working branch name>

这将直接从上游的分支，以避免任何本地合并创建本地分支。

