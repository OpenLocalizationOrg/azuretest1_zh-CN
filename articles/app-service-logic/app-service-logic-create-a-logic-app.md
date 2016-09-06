---
ms.openlocfilehash: c5ddffde64e91e7f26aa95302b1478b568f36892
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="创建一个逻辑应用程序 |Microsoft Azure"
    description="了解如何创建一个基本的应用程序服务的逻辑应用程序"
    authors="stepsic-microsoft-com"
    manager="dwrede"
    editor=""
    services="app-service\logic"
    documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/10/2015"
    ms.author="stepsic"/>

# 创建新的逻辑应用程序

| 快速参考 |
| --------------- |
| [逻辑应用程序定义语言](https://msdn.microsoft.com/library/azure/dn948512.aspx?f=255&MSPPError=-2147217396) |
| [逻辑应用程序连接器文档](https://azure.microsoft.com/en-us/documentation/articles/app-service-logic-connectors-list/) |
| [逻辑应用程序论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) |

本主题演示如何，只需几分钟时间，您可以开始使用[应用程序服务的逻辑应用程序](app-service-logic-what-are-logic-apps.md)。 我们将逐步介绍一个工作流，可让您提供您感兴趣的收存箱文件夹的 Tweets 一套。

要使用此方案中，您将需要︰

- Azure 的订阅
- 一个 Twitter 帐户
- 收存箱帐户

<!--- TODO: Add try it now information here -->

## 获取您的连接器

首先，您需要创建将使用两个接口︰[收存箱接头](app-service-logic-connector-dropbox.md)和[使用 Twitter，接头](app-service-logic-connector-twitter.md)。 由于使用 Twitter API 的限制，我们还需要注册一个免费的应用程序与 Twitter。 若要创建它们︰

1. 登录到 Azure 的门户。

2. 单击主屏幕和搜索 Twitter （或[单击此处](https://portal.azure.com/#create/microsoft_com.TwitterConnector.0.2.2)）[市场](https://portal.azure.com/#blade/HubsExtension/GalleryFeaturedMenuItemBlade/selectedMenuItemId/apiapps)。

3. 选择**使用 Twitter 的连接器**，然后单击**创建**。 您将获得查看您的所有设置。 可以**使用 Twitter，连接器**为保留名称。  
4. 选择**包设置**--此处您将需要输入您使用 Twitter 的应用程序中的信息。  您可以设置免费的应用程序，使用以下步骤︰
    1. 转到[使用 Twitter 的应用程序注册页](http://apps.twitter.com)。
    2. 创建新的应用程序。
    3. 为其指定的名称和说明。  您可以输入任何 URL 的网站，以及回调 URL 为空。
    4. 注册后，将复制**使用者密钥**从 Twitter 到 Azure 中的**客户机 Id**字段和从到 Twitter**使用者密码** **clientSecret。**
    5. 要返回的其他 API 设置 Azure 窗格中单击**确定**。

5. 键入计划名称**创建新的应用程序服务计划**中。

    >[AZURE.NOTE]本节中的步骤假定您要创建一个新的应用程序服务计划。 如果您正在使用现有的应用程序服务计划，请单击**现有选择**、 选择现有的计划，然后跳到最后这一节中的步骤。 您需要来承载您的应用程序的所有计划。

6.  选择新计划**定价层**。

    >[AZURE.NOTE]默认情况下，显示只有逻辑应用程序建议的计划。 单击**查看所有**以查看所有可用的计划。 当您运行逻辑应用程序可用的层中时，只能每小时运行一次和最多每个月 1000年操作使用。

7. 创建您的流的**资源组**。

    资源组作为容器的应用程序。 所有您的应用程序的资源将住在同一个资源组。

8. 如果您有多个 Azure 的订阅，请选择您将使用。

9. 选择要运行的逻辑应用程序的**位置**。

    ![创建 API 的应用程序视图](./media/app-service-logic-create-a-logic-app/gallery.png)

10. 单击**创建**。 设置步骤可能需要一分钟或两个。

11. 现在重复[收存箱](https://portal.azure.com/#create/microsoft_com.DropboxConnector.0.2.2)的过程。

## 逻辑应用程序启动

现在，您需要创建新的逻辑应用程序︰

1. 单击**+ 新**屏幕的左下方，展开**Web + 移动**，然后单击**逻辑的应用程序**。

    这将显示创建逻辑应用程序视图，提供入门的一些基本设置的位置。

    ![创建逻辑应用程序视图](./media/app-service-logic-create-a-logic-app/createlogicapp.png)

2. 在**名称**中，键入为您的逻辑应用程序有意义的名称。

3. 选择创建连接器时所使用的**应用程序服务计划**。 这自动为您选择的位置、 订阅和资源组。

这负责基本的设置，但是不单击**创建**还为时尚早。 接下来，您将向工作流添加触发器和操作。

## 添加触发器

触发器是什么使您的逻辑应用程序运行。 接下来，您将添加定期触发器，在预定的时间表上启动工作流。

1. 仍在**创建逻辑应用程序**视图中，单击**触发器和操作**。

    这将显示全屏幕设计器显示您的流。 右手边是造成触发器的所有服务的列表。

2. 在上方的区域中，单击**重复周期**。

    这将添加一个框，在其中可以指定的定期设置。

    ![定期](./media/app-service-logic-create-a-logic-app/recurrence.png)

3.  选择的重复**频率**和**时间间隔**（例如一次每 1 小时），然后单击绿色的选中标记。

现在，您将添加到流中的操作。

## 添加一个 Twitter 操作

操作是工作流。 可以有任意数量的操作，并可以来组织它们，以便将信息从一个操作传递到下一步。

1. 在右侧窗格中，单击**使用 Twitter，连接器**。

2. 加载后，请单击**授权**登录到 Twitter 帐户，单击**授权的应用程序**。

    这将授予对您的 Twitter 帐户的连接器访问。 显示由 Twitter 连接器的可能操作的列表。

    ![操作](./media/app-service-logic-create-a-logic-app/actions.png)

    > [AZURE.NOTE] **授权**按钮使用 OAUTH 安全连接到 SaaS 服务，像 Twitter 一样。 更在 OAUTH [OAUTH](app-service-logic-oauth-security.md)安全性。

3. 单击**搜索 tweets**，然后在**指定查询**中，键入类似`#MicrosoftAzure`，然后单击绿色的选中标记。

    ![Twitter 搜索](./media/app-service-logic-create-a-logic-app/twittersearch.png)

Twitter 连接器现在是工作流的一部分。

## 添加收存箱操作并创建应用程序

最后一步是添加到收存箱文件上载 tweets 的操作。

1. 在右侧窗格中，单击**收存箱连接线**。

2. 完成设置后，单击**授权**、 收存箱帐户和**允许**登录。

    ![授权收存箱接头](./media/app-service-logic-create-a-logic-app/authorize.png)

    这将授予收存箱帐户连接器访问。 将显示提供的收存箱接头的可能操作的列表。

4. 单击**上载文件**。  

    这将显示收存箱接头设置，必须设置以将数据从 Twitter 搜索传递到收存箱。

    ![收存箱](./media/app-service-logic-create-a-logic-app/dropbox.png)

3. 在**文件路径**字段中，键入 `/tweet.txt`

4. 在**内容**字段中，单击`...`按钮，再单击**Tweet 文本**选项。

    此输入的值`@first(body('twitterconnector')).TweetText`的文本框。 此生成的值包含以下部分︰

    内容的一部分                               | 说明
    ------------------------------------------ | ------------
     `@`                                       | 指示您正在输入一个函数，而不是实际值。
    `actions('twitterconnector').outputs.body` | 获取由 Twitter 连接器查询返回了 tweets。
    `first()`                                  | 搜索 tweets 操作返回一个列表，但您只想要上载一个文件
    `.TweetText`                               | 选择的 tweet 消息属性。

5. 单击绿色的复选标记，以保存连接器设置。

5. 设计过程完成之后，单击顶部的**代码视图**从右向左，请注意这是用于定义的工作流的 JSON 代码与设计器中，您只被创建设计器中。 我们将讨论此代码中的[下一个主题][使用逻辑应用程序功能]更多。

6. 单击**确定**按钮在屏幕的底部，然后单击**创建**按钮。

    这将创建新的逻辑应用程序。

## 在创建后管理逻辑应用程序

现在，您的逻辑应用程序启动并运行。 每次计划的工作流运行时，它会检查与特定 hashtag tweets。 如果找到匹配的 tweet，它可以将它放在您收存箱。 最后，将了解如何禁用该应用程序，或看到它如何做。

1. 单击屏幕左侧的**浏览**并选择**逻辑的应用程序**。

2. 单击您刚创建以当前状态和一般信息，请参阅新逻辑应用程序。

3. 若要编辑您新的逻辑应用程序，请单击**触发器和操作**。

5. 若要关闭该应用程序，单击命令栏中的**禁用**。

在 5 分钟之内，您就能够设置在云环境中运行一个简单的逻辑应用程序。 若要了解有关使用逻辑应用程序功能的详细信息，请参阅[使用逻辑应用程序功能]。 要了解自身的逻辑应用程序定义，请参阅[创作逻辑应用程序定义](app-service-logic-author-definitions.md)。

<!-- Shared links -->
[Azure 门户]: https://portal.azure.com
[使用逻辑应用程序功能]: app-service-logic-use-logic-app-features.md

测试
