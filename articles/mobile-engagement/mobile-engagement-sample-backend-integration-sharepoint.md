---
ms.openlocfilehash: 1da6ffa7ea0b58b8e09048fab602e750c1f815b0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure 移动服务的后端集成" 
    description="Azure 移动服务与 SharePoint 后端以从 sharepoint 网站创建市场活动" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="05/12/2015" 
    ms.author="piyushjo" />

# Azure 移动服务的 API 集成

在市场营销自动化系统中，创建和激活市场营销活动还自动发生。 为此目的的 Azure 移动服务允许创建此类自动化的市场营销活动，以及使用 Api。 

通常客户使用移动服务前端界面创建公告/轮询等作为其市场营销活动的一部分。 但是随着市场营销活动变得成熟，还有需要利用后端系统 （如任何的 CRM 系统或 CMS 系统 SharePoint 等） 中被锁定，以便可以在移动服务基于从后端系统中的流动的数据动态地创建市场活动创建完全自动化的管道数据。 

![][5]

本教程将经历这样的情况下，SharePoint 业务用户填充 SharePoint 列表与市场数据和自动化的过程选取列表中的项目并连接与使用 REST Api 从 SharePoint 数据创建市场营销活动的移动服务系统。 
 
> [AZURE.IMPORTANT] 一般情况下，可以使用此示例作为起点对于了解如何调用任何移动接洽 REST API，它详细说明了调用 Api 的身份验证和传递参数的两个关键方面。 

## SharePoint 集成
1. 下面是示例 SharePoint 列表如下所示。 **标题**、**类别**、 **NotificationTitle**、**消息**和**URL**用于创建公告。 没有名为**IsProcessed**形式的控制台程序示例自动化过程所使用的列。 可以的运行此控制台程序作为 Azure WebJob，以便您可以计划其也可以直接使用 SharePoint 工作流程序创建和激活公告 SharePoint 列表中插入一项时。 在此示例中，我们使用遍历 SharePoint 列表中的项目和为每个在 Azure 移动服务中创建发布并得令人难以置信的成功发布创建的**IsProcessed**标志，最后将标记的控制台程序。

    ![][1]

2. 我们使用的*SharePoint 中的远程身份验证联机使用客户端对象模型*示例中的代码[在此处](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c)与 SharePoint 列表进行身份验证。
 
3. 一旦完成认证，我们循环遍历列表项，以查明任何新创建的项目 (这将具有**IsProcessed** = false)。 

        static async void CreateCampaignFromSharepoint()
        {
            using (ClientContext clientContext = ClaimClientContext.GetAuthenticatedContext(targetSharepointSite))
            {
                // Using https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c for authentication     
                Web site = clientContext.Web;
                List list = site.Lists.GetByTitle("VideoContent");
                CamlQuery query = new CamlQuery();
                query.ViewXml = "<View/>";
                ListItemCollection items = list.GetItems(query);

                // Load the SharePoint list
                clientContext.Load(list);
                clientContext.Load(items);
                clientContext.ExecuteQuery();

                // Loop through the list to go through all the items which are newly added
                foreach (ListItem item in items)
                    if (item["IsProcessed"].ToString() != "Yes")
                    {
                        string name = item["Title"].ToString();
                        string notificationTitle = item["NotificationTitle"].ToString();
                        string notificationMessage = item["Message"].ToString();
                        string category = item["Category"].ToString();
                        string actionURL = ((FieldUrlValue)item["URL"]).Url;

                        // Send an HTTP request to create AzME campaign
                        int campaignId = CreateAzMECampaign
                            (name, notificationTitle, notificationMessage, category, actionURL).Result;

                        if (campaignId != -1)
                        {
                            // If creating campaign is successful then set this to true
                            item["IsProcessed"] = "Yes";

                            // Now try to activate the campaign also
                            await ActivateAzMECampaign(campaignId);
                        }
                        else
                        {
                            item["IsProcessed"] = "Error";
                        }
                        item.Update();
                    }
                clientContext.ExecuteQuery();
            }
        }

## 移动服务集成
1.  我们一旦我们找到一个项目需要处理的提取需从列表项和调用创建公告的信息`CreateAzMECampaign`创建并随后`ActivateAzMECampaign`以将其激活。 这些都是实质上是 REST API 调用到移动服务后端调用。 

2.  移动接洽 REST Api 要求**基本身份验证方案授权 HTTP 标头**由`ApplicationId`和`ApiKey`获得从 Azure 的门户。 请确保您的**api 键**部分使用密钥而*不*从**sdk 键**部分。 

    ![][2]

        static string CreateAuthZHeader()
        {
            string BasicAuthzHeaderString = "Basic " + EncodeTo64(ApplicationId + ":" + ApiKey);
            return BasicAuthzHeaderString;
        }

        static public string EncodeTo64(string toEncode)
        {
            byte[] toEncodeAsBytes = System.Text.ASCIIEncoding.ASCII.GetBytes(toEncode);
            string returnValue = System.Convert.ToBase64String(toEncodeAsBytes);
            return returnValue;
        }  

3. 创建公告类型市场 — — 请参阅[文档](https://msdn.microsoft.com/library/dn913754.aspx)。 您需要确保您指定的市场活动`kind`*宣布*和[负载](https://msdn.microsoft.com/library/dn913749.aspx)以及将其作为 FormUrlEncodedContent 传递。 

        static async Task<int> CreateAzMECampaign(string campaignName, string notificationTitle, 
            string notificationMessage, string notificationCategory, string actionURL)
        {
            string createURIFragment = "/reach/1/create";

            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());
                
                // Create the payload to send the content
                // Reference -> https://msdn.microsoft.com/en-us/library/dn913749.aspx
                string data =
                    @"{""name"":""" + campaignName + @"""," + 
                    @"""type"":""only_notif""," + 
                    @"""deliveryTime"":""any"","" + 
                    @""notificationTitle"":""" + notificationTitle + 
                    @""",""notificationMessage"":""" + notificationMessage + 
                    @""",""actionUrl"":""" + actionURL + @"""}";
                
                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("data", data),
                });

                // Send the POST request
                var response = await client.PostAsync(url + createURIFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                int campaignId = -1;
                if (response.StatusCode.ToString() == "OK")
                {
                    // This is the campaign id
                    campaignId = Convert.ToInt32(responseString);
                    Console.WriteLine("Campaign successfully created with id {0}", campaignId);
                }
                else
                {
                    Console.WriteLine("Campaign creation failed with error - '{0}'", responseString);
                }
                return campaignId;
            }
        }

4. 创建发布之后，您将看到类似下面的内容上移动服务门户 (请注意，状态草稿和已激活 = = n/A)

    ![][3]

5. `CreateAzMECampaign` 创建发布市场活动并将其 Id 返回给调用方。 `ActivateAzMECampaign` 需要此 Id 作为参数来激活市场活动。 

        static async Task<bool> ActivateAzMECampaign(int campaignId)
        {
            string activateUriFragment = "/reach/1/activate";
            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());

                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("id", campaignId.ToString()),
                });

                // Send the POST request
                var response = await client.PostAsync(url + activateUriFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                if (response.StatusCode.ToString() == "OK")
                {
                    Console.WriteLine("Campaign successfully activated");
                    return true;
                }
                else
                {
                    Console.WriteLine("Campaign activation failed with error - '{0}'", responseString);
                    return false;
                }
            }
        }

6. 一旦激活通知，您将看到类似下面的内容移动服务门户上︰

    ![][4]

7. 一旦激活的市场活动，满足市场上的标准的任何设备将开始看到通知。 

8. 您还会注意到的列表项，标上 IsProcessed = false 设置为 True 后创建发布市场活动。  

此示例创建简单发布市场活动指定主要是必需的属性。 您可以自定义此尽可能使用的信息可以从门户网站[这里](https://msdn.microsoft.com/library/dn913749.aspx)。 

<!-- Images. -->
[1]: ./media/mobile-engagement-sample-backend-integration-sharepoint/sharepointlist.png
[2]: ./media/mobile-engagement-sample-backend-integration-sharepoint/properties.png
[3]: ./media/mobile-engagement-sample-backend-integration-sharepoint/new-announcement.png
[4]: ./media/mobile-engagement-sample-backend-integration-sharepoint/activate-announcement.png
[5]: ./media/mobile-engagement-sample-backend-integration-sharepoint/diagram.png


 
测试
