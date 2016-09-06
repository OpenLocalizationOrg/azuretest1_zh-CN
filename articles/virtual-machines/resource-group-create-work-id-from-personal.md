---
ms.openlocfilehash: 6b5040ffd583a33289b5b6164cafbc8299646c57
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="在 Azure Active Directory 中创建工作或学校的身份"
   description="描述如何从您的个人身份与资源组模板或基于角色的访问权限，在其他功能使用创建工作或学校的标识。"
   services="virtual-machines"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure"
   ms.date="09/01/2015"
   ms.author="rasquill"/>

# 在 Azure Active Directory 中创建工作或学校的身份

如果您创建了一个个人的 Azure 帐户或个人 MSDN 订阅并创建了 Azure 帐户利用 MSDN Azure 之旅 — 使用*Microsoft 帐户*标识来创建它。 许多强大功能的 Azure —[资源组模板](../resource-group-overview.md)是一个例子--（由 Azure Active Directory 管理标识） 的工作或学校帐户需要的工作。

幸运的是，关于您个人的 Azure 帐户的最佳措施之一就是它配备有可用于创建新的工作或学校帐户，您可以使用 Azure 需要的功能，它使用一个默认 Azure Active Directory 域。

> [AZURE.NOTE] 如果由管理员提供的用户名和密码，则很可能已拥有某一工作或学校 （有时也称为*组织 ID*） 的 ID。 如果是这样，您可以立即开始使用 Azure 帐户访问 Azure 需要的资源。 如果您发现您不能使用这些资源，您可能需要返回到本文的帮助。 有关详细信息，请参阅[帐户可用于登录](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SignInAccounts)和[Azure 如何订阅相关的 Azure 的广告](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SubRelationToDir)。

步骤很简单。 您需要在 Azure 的门户中，身份定位您签名发现您默认的 Azure Active Directory 域，并作为 Azure 联管理员向其中添加新用户。

## 在 Azure 门户中找到您的默认目录

从开始到[Azure 门户](https://manage.windowsazure.com)与 Microsoft 个人帐户身份登录。 您登录后，沿左侧的蓝色面板向下滚动，然后单击**ACTIVE DIRECTORY**。

![Azure 的活动目录](./media/resource-group-create-work-id-from-personal/azureactivedirectorywidget.png)

让我们先在 Azure 中查找某些有关您身份的信息。 您应该看到类似下面的主窗格中，显示有一个默认的目录。

![](./media/resource-group-create-work-id-from-personal/defaultaadlisting.png)

我们来看一些有关它的详细信息。 单击默认目录行，使您在默认的目录属性。  

![](./media/resource-group-create-work-id-from-personal/defaultdirectorypage.png)

若要查看默认域的名称，请单击**域**。

![](./media/resource-group-create-work-id-from-personal/domainclicktoseeyourdefaultdomain.png)

在此处您应该能够看到 Azure 帐户在创建时，Azure Active Directory 创建个人默认域，则哈希值 （从一个文本字符串生成一个数字） 您的个人标识用作 onmicrosoft.com 的子域。 这就是现在将向其添加新的用户的域。

## 在默认域中创建新用户

单击**用户**并查找单个个人的帐户。 您应该看到**源于从**列中它是**Microsoft 帐户**。 我们想要创建一个用户在默认。 onmicrosoft.com Azure Active Directory 域。

![](./media/resource-group-create-work-id-from-personal/defaultdirectoryuserslisting.png)

我们要按照[这些说明](https://technet.microsoft.com/library/hh967632.aspx#BKMK_1)在以下几个步骤，但使用一个具体的例子。

在页面的底部，单击**+ 添加用户**。 中显示的页上，键入新用户名，并将该**类型的用户****在您的组织中的新用户**。 在此示例中，新的用户名为`ahmet`。 选择默认域发现以前，将其作为 ahmet 的电子邮件地址的域。 单击完成后下, 一步的箭头。

![](./media/resource-group-create-work-id-from-personal/addingauserwithdirectorydropdown.png)

对 Ahmet，添加更多详细信息，但一定要选择适当的**角色**值。 它的易于使用**全局管理**以确保所有步骤都工作正常，但是如果您可以使用较小的角色，这就是一个不错的主意。 此示例使用**用户**角色。 (了解更多有关这些角色[这里](https://msdn.microsoft.com/library/azure/dn468213.aspx#BKMK_1)。)除非您想要为每个日志操作中使用多因素身份验证，则不启用多因素身份验证。 当您完成时，请单击下箭头。

![](./media/resource-group-create-work-id-from-personal/userprofileuseradmin.png)

单击**创建**按钮以生成并显示对 Ahmet 临时密码。

![](./media/resource-group-create-work-id-from-personal/gettemporarypasswordforuser.png)

复制用户名称的电子邮件地址，或使用**发送密码在电子邮件**。 您将需要很快登录的信息。

![](./media/resource-group-create-work-id-from-personal/receivedtemporarypassworddialog.png)

现在您应该看到新用户**Ahmet 开发人员**，从 Azure Active Directory 来指明其来源。 与 Azure Active Directory，您已创建的新工作或学校标识。 但是，这个身份没有使用 Azure 的资源的权限。

![](./media/resource-group-create-work-id-from-personal/defaultdirectoryusersaftercreate.png)

如果您使用**发送密码在电子邮件**，发送下面的电子邮件类型。

![](./media/resource-group-create-work-id-from-personal/emailreceivedfromnewusercreation.png)

## 添加订阅的 Azure 联管理员权限

现在，您需要添加新用户作为共同管理员的您的订购，以便新用户可以登录到管理门户。 为此，在左面板中单击**设置**。

![](./media/resource-group-create-work-id-from-personal/thesettingswidget.png)

在设置主区域中，单击**管理员**在顶部，您应该会看到只有您个人的 Microsoft 帐户标识。 在页面的底部，单击**+ 添加**指定联管理员。 在此处，输入新用户创建了，包括默认域的电子邮件地址。 下一个屏幕快照中所示，一个绿色的复选标记旁边有用户提供的默认目录。 请务必选择您希望该用户能够管理订阅的所有。

![](./media/resource-group-create-work-id-from-personal/addingnewuserascoadmin.png)

操作完成后，您现在应该看到两个用户，包括您新的共同的管理员身份。 注销该门户。

![](./media/resource-group-create-work-id-from-personal/newuseraddedascoadministrator.png)

## 日志记录和更改新用户的密码

您创建的新用户登录。

![](./media/resource-group-create-work-id-from-personal/signinginwithnewuser.png)

您将立即会提示您创建一个新密码。

![](./media/resource-group-create-work-id-from-personal/mustupdateyourpassword.png)

您应该获得回报与如下所示的成功。

![](./media/resource-group-create-work-id-from-personal/successtourdialog.png)


## 下一步行动

您现在可以使用新的 Azure Active Directory 标识使用[Azure 资源组模板](xplat-cli-azure-resource-manager.md)。

     azure login
    info:    Executing command login
    warn:    Please note that currently you can login only via Microsoft organizational account or service principal. For instructions on how to set them up, please read http://aka.ms/Dhf67j.
    Username: ahmet@aztrainpassxxxxxoutlook.onmicrosoft.com
    Password: *********
    /info:    Added subscription Azure Pass
    info:    Setting subscription Azure Pass as default
    +
    info:    login command OK
    ralph@local:~$ azure config mode arm
    info:    New mode is arm
    ralph@local:~$ azure group list
    info:    Executing command group list
    + Listing resource groups
    info:    No matched resource groups were found
    info:    group list command OK
    ralph@local:~$ azure group create newgroup westus
    info:    Executing command group create
    + Getting resource group newgroup
    + Creating resource group newgroup
    info:    Created resource group newgroup
    data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/newgroup
    data:    Name:                newgroup
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags:
    data:
    info:    group create command OK
