---
ms.openlocfilehash: 01861bfc2a3f877da5383d3b720c1df68be22428
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何使用 Blitline 图像处理-Azure 功能指南" 
    description="了解如何使用 Azure 应用程序中处理图像的 Blitline 服务。" 
    services="" 
    documentationCenter=".net" 
    authors="blitline-dev" 
    manager="jason@blitline.com" 
    editor="jason@blitline.com"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/09/2014" 
    ms.author="support@blitline.com"/>
# 如何使用 Blitline Azure 和 Azure 存储

本指南将解释如何访问 Blitline 服务以及如何将作业提交到 Blitline。

## Blitline 是什么？

Blitline 是价格的基于云的图像处理提供了企业级图像处理它所需的成本来构建自己的一小部分的服务。

事实是，图像经过处理之后反复，通常从一开始为每个网站进行重建。 我们认识到这一点因为我们构建这些无数次太。 我们决定，也许它是时间的一天我们就是要做每个人。 我们知道如何去做，做到快速、 高效，并同时保存所有工作的人。

有关详细信息，请参阅[http://www.blitline.com](http://www.blitline.com)。

## Blitline 是什么不...

为了阐明 Blitline 是有用的通常很容易识别 Blitline 不什么然后再向前移动。

- Blitline 并没有要上载的图像的 HTML 小部件。 公开或使用受限权限可用于 Blitline 到达，您必须具有可用的图像。

- Blitline 不进行实时图像处理，如 Aviary.com

- Blitline 不接受图像上载，您不能直接将图像推送到 Blitline。 必须将它们推送到 Azure 存储位置或其他位置的 Blitline 支持，然后告诉 Blitline 哪里去得到它们。

- Blitline 是大规模应用平行，不进行任何同步处理。 意味着您必须给我们的 postback_url，我们可以告诉您当我们完成处理。

## 创建一个 Blitline 帐户

[AZURE.INCLUDE [blitline-signup](../includes/blitline-signup.md)]

## 如何创建一个 Blitline 作业

Blitline 使用 JSON 来定义要在图像上执行的操作。 此 JSON 是由几个简单的字段组成。

最简单的示例如下所示︰

        json : '{
       "application_id": "MY_APP_ID",
       "src" : "http://cdn.blitline.com/filters/boys.jpeg",
       "functions" : [ {
           "name": "resize_to_fit",
           "params" : { "width": 240, "height": 140 },
           "save" : { "image_identifier" : "external_sample_1" }
       } ]
    }'

现在，我们有需要"src"图像的 JSON"...boys.jpeg"，然后重新调整到 240 x 140。

应用程序 ID 是您可以在您在 Azure 上**连接信息**或**管理**选项卡上找到。 它是您机密标识符，您可以在 Blitline 上运行作业。

"保存"参数标识要将该图像放一旦处理完它的位置有关的信息。 在这微不足道的情况下，我们还没有定义一个。 如果不定义位置 Blitline 会将它存储本地 （和临时） 唯一云位置处。 您将能够获得 JSON 时使 Blitline，Blitline 返回该位置。 "图像"标识符是必需的以识别该特定保存图像时返回给您。

您可以找到有关我们在这里支持的*函数*的详细信息︰ <http://www.blitline.com/docs/functions>

您还可以找到有关的作业选项的文档︰ <http://www.blitline.com/docs/api>

一旦您 JSON 所有您需要做是**文章**到 `http://api.blitline.com/jobs`

您将获得 JSON 返回如下所示︰

    {
     "results":
         {"images":
            [{
              "image_identifier":"external_sample_1",
              "s3_url":"https://s3.amazonaws.com/dev.blitline/2011110722/YOUR_APP_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg"
            }],
          "job_id":"4eb8c9f72a50ee2a9900002f"
         }
    }


这说明，Blitline 已经收到您的请求，它已将其放在处理队列中，和完成后的图像都将显示在︰ **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_应用程序\_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg**

## 如何将图像保存到您的 Azure 存储帐户

如果您有一个 Azure 存储帐户，可轻易使 Blitline 将已处理的图像推入到 Azure 的容器。 通过添加"azure_destination"可以定义的位置和 Blitline 推到的权限。

下面是一个示例︰

    job : '{
      "application_id": "YOUR_APP_ID",
      "src" : "http://www.google.com/logos/2011/houdini11-hp.jpg",
         "functions" : [{
         "name": "blur",
         "save" : {
             "image_identifier" : "YOUR_IMAGE_IDENTIFIER",
             "azure_destination" : {
                 "account_name" : "YOUR_AZURE_CONTAINER_NAME",
                 "shared_access_signature" : "SAS_THAT_GIVES_BLITLINE_PERMISSION_TO_WRITE_THIS_OBJECT_TO_CONTAINER",
               }
           }
         }]
       }'


通过填写您自己的 CAPITALIZED 值，您可以提交到 http://api.blitline.com/job 此 JSON 和"src"图像将使用模糊滤镜处理，然后被推送到 Azure 目标。

###请注意︰

SA 必须包含完整的 SAS url，包括目标文件的文件名。

示例：

    http://blitline.blob.core.windows.net/sample/image.jpg?sr=b&sv=2012-02-12&st=2013-04-12T03%3A18%3A30Z&se=2013-04-12T04%3A18%3A30Z&sp=w&sig=Bte2hkkbwTT2sqlkkKLop2asByrE0sIfeesOwj7jNA5o%3D


您还可以阅读 Blitline 的 Azure 存储文档的最新版本[在此处](http://www.blitline.com/docs/azure_storage)。


## 下一步行动

请访问 blitline.com 来了解我们其他所有功能︰

* Blitline API 端点文档<http://www.blitline.com/docs/api>
* Blitline API 函数<http://www.blitline.com/docs/functions>
* Blitline API 示例<http://www.blitline.com/docs/examples>
* 第三部分 Nuget 库<http://nuget.org/packages/Blitline.Net>

测试
