---
ms.openlocfilehash: fb877e4f3bacc2a0d95ec14546cf3c24d156658a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="安排后端任务计划程序 |Microsoft Azure"
    description="使用 Azure 移动服务计划程序来计划您的移动应用程序的作业。"
    services="mobile-services"
    documentationCenter=""
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="06/16/2015"
    ms.author="glenga"/>

# 计划中移动服务的定期作业

> [AZURE。选择器列表 (平台 |后端）]
- [(任何 |.NET)](mobile-services-dotnet-backend-schedule-recurring-tasks.md)
- [(任何 |) Javascript](mobile-services-schedule-recurring-tasks.md)

本主题演示如何使用在管理门户的作业计划程序功能定义服务器根据您定义的计划执行的脚本代码。 在这种情况下，脚本定期检查使用的远程服务，Twitter，这种情况下，存储在新表中的结果。 其他一些可以安排的定期任务包括︰

+ 旧的或重复数据的记录存档。
+ 请求和存储外部数据，如 tweets、 RSS 项和位置信息。
+ 处理或调整大小的图像存储。

本教程展示如何使用作业计划程序创建的计划的作业请求从 Twitter 的 tweet 数据并存储新的更新表中的 tweets。

##<a name="get-oauth-credentials"></a>注册为 1.1 版 Twitter Api 访问和存储凭据

[AZURE.INCLUDE [mobile-services-register-twitter-access](../../includes/mobile-services-register-twitter-access.md)]

##<a name="create-table"></a>创建新的更新表

接下来，您需要创建一个新表中存储 tweets。

2. 在管理门户中，单击您的移动服务，**数据**选项卡，然后单击**+ 创建**。

3. 在**表名称**的类型_的更新_，然后单击检查按钮。

##<a name="add-job"></a>创建新的计划的作业  

现在，您可以创建计划访问 Twitter 的作业和商店 tweet 新更新表中的数据。

2. 单击**计划**选项卡，然后单击**+ 创建**。

    >[AZURE.NOTE]当您运行您的移动服务<em>自由</em>层，只是再一次运行一个计划的作业。 在带薪级，可以一次运行最多十台已排定的作业。

3. 在计划程序对话框中，输入**作业名称** _getUpdates_ ，设置计划时间间隔和单位，然后单击检查按钮。

    这将创建名为**getUpdates**的新作业。

4. 单击刚创建的新作业，单击**脚本**选项卡，占位符函数**getUpdates**替换为以下代码︰

        var updatesTable = tables.getTable('Updates');
        var request = require('request');
        var twitterUrl = "https://api.twitter.com/1.1/search/tweets.json?q=%23mobileservices&result_type=recent";

        // Get the service configuration module.
        var config = require('mobileservice-config');

        // Get the stored Twitter consumer key and secret.
        var consumerKey = config.twitterConsumerKey,
            consumerSecret = config.twitterConsumerSecret
        // Get the Twitter access token from app settings.
        var accessToken= config.appSettings.TWITTER_ACCESS_TOKEN,
            accessTokenSecret = config.appSettings.TWITTER_ACCESS_TOKEN_SECRET;

        function getUpdates() {
            // Check what is the last tweet we stored when the job last ran
            // and ask Twitter to only give us more recent tweets
            appendLastTweetId(
                twitterUrl,
                function twitterUrlReady(url){
                    // Create a new request with OAuth credentials.
                    request.get({
                        url: url,
                        oauth: {
                            consumer_key: consumerKey,
                            consumer_secret: consumerSecret,
                            token: accessToken,
                            token_secret: accessTokenSecret
                        }},
                        function (error, response, body) {
                        if (!error && response.statusCode == 200) {
                            var results = JSON.parse(body).statuses;
                            if(results){
                                console.log('Fetched ' + results.length + ' new results from Twitter');
                                results.forEach(function (tweet){
                                    if(!filterOutTweet(tweet)){
                                        var update = {
                                            twitterId: tweet.id,
                                            text: tweet.text,
                                            author: tweet.user.screen_name,
                                            date: tweet.created_at
                                        };
                                        updatesTable.insert(update);
                                    }
                                });
                            }
                        } else {
                            console.error('Could not contact Twitter');
                        }
                    });

                });
         }
        // Find the largest (most recent) tweet ID we have already stored
        // (if we have stored any) and ask Twitter to only return more
        // recent ones
        function appendLastTweetId(url, callback){
            updatesTable
            .orderByDescending('twitterId')
            .read({success: function readUpdates(updates){
                if(updates.length){
                    callback(url + '&since_id=' + (updates[0].twitterId + 1));
                } else {
                    callback(url);
                }
            }});
        }

        function filterOutTweet(tweet){
            // Remove retweets and replies
            return (tweet.text.indexOf('RT') === 0 || tweet.to_user_id);
        }


    此脚本会调用使用存储的凭据来请求最近的 tweets 包含 hashtag Twitter 查询 API `#mobileservices`。 重复的 tweets 和答复是从结果中删除之前它们存储在表中。

    >[AZURE.NOTE]此示例假定只有几行插入运行期间每一个计划表。 在循环中插入许多行的情况下您可能会用尽连接时可用层上运行。 在这种情况下，您应在批处理中执行插入。 有关详细信息，请参阅[方法︰ 执行大容量插入](mobile-services-how-to-use-server-scripts.md#bulk-inserts)。

6. 单击**运行一次**测试脚本。

    这样可以节省并保持禁用计划程序中执行该作业。

7. 单击后退按钮，单击**数据**，单击**更新**表格，单击**浏览**并验证 Twitter 的数据已被插入到表。

8. 单击后退按钮，单击**计划**，选择**getUpdates**，然后单击**启用**。

    这使运行指定的计划，在这种情况下每小时的作业。

祝贺您，您已成功创建您的移动服务中的新计划的作业。 此作业将作为执行计划之前禁用或对其进行修改。

## <a name="nextsteps"> </a>请参见

* [移动服务服务器脚本引用]
  <br/>了解更多有关注册和使用服务器脚本。

<!-- Anchors. -->
[注册 Twitter 访问和存储凭据]: #get-oauth-credentials
[创建新的更新表]: #create-table
[创建新的计划的作业]: #add-job
[下一步行动]: #next-steps

<!-- Images. -->

<!-- URLs. -->
[移动服务服务器脚本引用]: http://go.microsoft.com/fwlink/?LinkId=262293
[WindowsAzure.com]: http://www.windowsazure.com/
[Azure 的管理门户]: https://manage.windowsazure.com/
[移动服务中注册 Twitter 登录应用程序]: /develop/mobile/how-to-guides/register-for-twitter-authentication
[Twitter 的开发人员]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[应用程序设置]: http://msdn.microsoft.com/library/windowsazure/b6bb7d2d-35ae-47eb-a03f-6ee393e170f7

测试
