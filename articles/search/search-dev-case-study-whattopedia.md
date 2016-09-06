---
ms.openlocfilehash: b16dacae017926f1fd9ecf7acc929f1fa866dc70
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure 搜索开发案例研究︰ 如何 WhatToPedia 在 Microsoft Azure 上构建 infomedia 门户" 
    description="了解如何构建在 Microsoft Azure 上使用搜索服务的信息门户和元搜索引擎" 
    services="search, sql-database,  storage, web-sites" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="mblythe"/>

<tags 
    ms.service="search" 
    ms.devlang="NA" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="search" 
    ms.date="07/08/2015" 
    ms.author="heidist"/>

# Azure 搜索开发案例研究

## [WhatToPedia.com](http://whattopedia.com/)在 Microsoft Azure 上生成一个 infomedia 门户网站的方式

 ![][6]  &nbsp;&nbsp;&nbsp;  <font size="9">一个好主意</font> 


我们的想法是建立信息门户，用于帮助购物者与零售商通过高度相关、 范围搜索类似的体验，为如何随门户匹配 tourists 了旅馆、 饭店和在整合区域时的娱乐。 

我们将它设想的门户将对一个给定的市场中零售商数据提供极高质量的搜索体验，帮助查找基于位置和其他有用标志的零售商商店的购物者提供。 我们将要播种一个初始数据集，使用搜索引擎，但更深层次的价值将经过时，零售商订阅者发布关于其业务信息的帮助。 促销、 新商品、 流行的品牌、 内部专业服务---所有都将值添加到我们的站点的数据的示例。 自我报告此数据并将其集成到搜索 corpus，一旦零售商登记为订阅服务器。 广告，加上订阅模型，为我们新的业务提供了收入来源。

搜索将会主导的用户交互模型，在纯粹的云平台上。 对于规模和低成本的目的，几乎一切，从门户体验到源代码管理中，将通过一项在线服务。 开箱即用，使用搜索引擎提供了我们需要的功能，我们可以快速创建搜索应用程序，而无需构建和管理搜索引擎自己。

## 我们的构建

WhatToPedia 是启动 infomedia 公司。 我们构建了[WhatToPedia.com](http://whattopedia.com/) --正在测试市场在北欧投入于 2015 年 2 月 2 日的日期。 我们的客户群是主要是砖和灰泥店需要可以很容易地管理和维护合理的联机状态。

我们的任务是吸引购物者，通过很好的联机搜索体验，提高基于结果在市/县或邻居，品牌，存储名称，或存储类型。 吸引顾客有连锁反应，激发零售商要订阅我们的门户网站。 订阅是收费的以合理的速度。

 ![][7] 

用于对订阅进行注册之后, 零售商接管其创建现有配置文件 （最初由我们从购买数据），与促销活动、 特色的品牌或宣布有关其他数据对其进行更新。 内部功能，如所说的语言接受货币，免税购物，可以自我报告，以更好地吸引购物者正在寻找那些款待。

## 我们是谁

我的名字是托马斯 Segato （Microsoft 咨询），我与 Jesper Boelling，导致在 WhatToPedia，设计解决方案的开发人员合作。 

WhatToPedia 是启动时，市场新门户业务在瑞典，其中大部分 60000 零售商是砖和灰泥 Sme （中小型企业） 的测试。 因为我们知道在欧洲购物的人讲多种语言，并享有多种货币，我们构建的解决方案，满足多语言的购物者。 我们需要以及在发现，搜索引擎在 Azure 搜索支持我们多语言需求。

Azure 搜索是我们项目的游戏变换器。 之前的 Azure 搜索可用性，我们花费相当多的能源通过构建我们自己的搜索引擎的困难工作。 将 Azure 搜索作为一种联机服务将最大的技术和管理障碍从我们的解决方案，这意味着进入市场更快，并具有更强大的搜索体验。  

## 我们如何做到了

我们的使命就是构建一个完整的基础架构，基于云的服务只。 Microsoft 已被选作战略平台，因为它的提供程序提供必要的服务 （用于协作和开发），扩展需求和价格的降低。
 
### 高级别组件

我们构建了一个企业，而不仅仅是网站。 支持整个工作所需的各种工具和应用程序。 我们采用 Visual Studio 和 Visual Studio 在线开发团队基础服务 (TFS) 在线获取源代码管理和 scrum 管理、 沟通和协作，Office 365 和 Microsoft Azure 当然所有站点相关运算和存储。 Visual Studio，IDE 提供直接调配到 Azure，集成到 TFS 在线提供附加的工作效率大大提高。

下图所示的 WhatToPedia 基础结构中使用的高级别组件。

   ![][8]

### 我们如何使用 Microsoft Azure

查看上图中的绿色框，您将看到 WhatToPedia 解决方案基于这些服务︰

- [Azure 搜索](http://azure.microsoft.com/services/search/)
- [使用 MVC 4 azure 网站](http://azure.microsoft.com/services/websites/)
- [计划任务的 azure WebJobs](../websites-webjobs-resources.md)
- [SQL azure 数据库](http://azure.microsoft.com/services/sql-database/)
- [Azure BLOB 存储](http://azure.microsoft.com/services/storage/)
- [SendGrid 电子邮件传递](http://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/)

该解决方案的核心是数据和搜索。 转销商到最终客户的数据的流程如下所示︰

  ![][9]

主数据存储是转销商和 Azure SQL 数据库中的记帐数据。 这包括初始数据集，再加上随着时间的推移添加特定于零售商的数据。 我们使用 Azure WebJob 从 SQL 数据库的更新发送到 Azure 搜索中搜索 corpus。

### 表示层

门户是 Azure 网站，在 MVC 4 和[使用 Twitter 的引导](http://en.wikipedia.org/wiki/Bootstrap_%28front-end_framework%29)中实现。 我们选择了 MVC，因为它比 ASP.NET 基于窗体的开发提供更简洁的方法为 HTML。 为了避免无需创建多个设备的应用程序和维护多个移动平台，选择了使用 Twitter，引导支持所有设备和平台。

### 身份验证、 权限和敏感数据

购物者匿名浏览该网站。 在这种情况下，购物者，对于没有登录要求也我们存储的所有使用者数据。 

零售商是另外一回事。 在这里，我们将存储面向公众的个人资料、 计费信息和他们想要在网站上公开的媒体内容。 每个零售商预订网站获得用于进行身份验证，用户之前对订阅服务器的配置文件进行更新的用户登录。  我们安全地存储在 SQL Azure 数据库和 Azure BLOB 存储的所有订阅服务器的数据。
我们之所以选择了一个基于.NET 的基于表单的身份验证的身份验证模型。 我们选择了以简洁性著称; 这种方法我们并不需要的角色、 用户界面支持和其他无关的功能，附带了其他方法。 

若要确保零售商只能看到它们所属的数据，我们创建了的零售商随后使用所有每个零售商的 ID 读取和写入操作涉及零售商特有的数据。 使用此方法时，我们发现，我们没有不需要各个零售商授予数据库权限。 所有零售商与下一个数据库角色，作为我们的数据隔离技术的零售商 ID 与系统进行都交互。

因为我们的业务是所有关于后续的影响 （给零售商带来更多业务，创建激励广告和订阅），我们可以绘制的线条在 web 上处理采购。 在这种情况下，您不会找到我们简化了我们的安全需求的站点上购物车。 

我们采用另一种简化是外包的付费和帐户应付运营。 通过路由的客户付款信息直接发送到第三方 ([SveaWebPay](http://www.sveawebpay.se/))，我们可以减少与存储和保护我们的数据存储区中的敏感数据相关联的风险。 

### 搜索引擎

我们的核心是解决方案的基于 Azure 搜索服务的搜索引擎。 最初，我们建立了一个自定义搜索引擎，但在此过程中，我们意识到复杂性和工作量已非常高事实上，和这促使我们要考虑其他备选方案。 

已向包括我们最重要的基本功能︰

- 筛选器
- 多面导航
- 跃进的结果
- 通过 AJAX 分页

互联网搜索引导我们到下面的视频，我们可以试一试的 Azure 搜索提出了︰ [TechEd 欧洲在深入探究](http://channel9.msdn.com/events/TechEd/Europe/2014/DBI-B410) 

观看视频之后, 我们已准备好建立基于我们所看到的原型。 因为数据包含可搜索的术语，而且我们已经有合作，我们想要排序方面，和筛选的数据的要求，因为我们已经有一个数据模型，在 MVC 中，创建原型相当简单。 

这是我们如何构建原型。

**配置 Azure 搜索服务**

1. 登录到 Azure 的门户网站，为我们订阅添加搜索服务。 我们使用的共享的版本 （我们订阅是免费的）。
2. 创建索引。 对于原型中，我们可以使用门户 UI 定义搜索字段并创建记分的配置文件。 我们计分的配置文件基于位置数据︰ 国家/地区 |城市 | 地址 (请参见︰ 添加评分的配置文件)。
3. 复制服务 URL 和管理我们的配置文件的 api 密钥。 这个关键字是在门户中，搜索服务页面上，它用于对服务进行身份验证。
    
**开发搜索索引器作业 – Windows 控制台**

1. 从数据库中读取所有转销商。
2. 要上载转销商通过一个一个的 Azure 搜索服务 API 的调用 (请参见︰ http://msdn.microsoft.com/library/azure/dn798930.aspx)。
3. 在数据库中设置一个属性转销商增量索引进行索引。 我们通过存储每个配置文件 （或不编入索引） 的索引状态的索引字段中添加了该操作。 

请参阅生成索引器作业代码片段的附录。

**MVC 开发搜索 Web 门户 –**

1. 调用 Azure 搜索服务，可以从搜索所有文档 (请参见︰ http://msdn.microsoft.com/library/azure/dn798927.aspx)
2. 从搜索服务响应后 （通过使用 json.net http://james.newtonking.com/json） 的提取
   - 结果
   - 小平面
   - 结果计数
   - 开发用户界面显示搜索结果、 方面和计数 （我们已经有这样）。

这是我们用来从 Azure 搜索得到的结果的代码︰

    string requestUrl = 
    string.Format("https://{0}.search.windows.net/indexes/profiles/docs?searchMode=all&$count=true&search={1}&facet=city,count:20&facet=category&$top=10&$skip={2}&api-version=2014-07-31-Preview{3}", Config.SearchServiceName, EscapeODataString(q), skip, filter);
      var response = client.GetAsync(requestUrl).Result; //  + Uri.EscapeDataString("hennes")
      response.EnsureSuccessStatusCode();
     dynamic json = JsonConvert.DeserializeObject(response.Content.ReadAsStringAsync().Result);

**提高位置**

可能最重要的要求，以验证原型中包括向查询中添加一个位置搜索关键字。 非常重要，如果用户在搜索中输入城市名称查询，我们门户中给定城市的转销商将排名要高于转销商有城市关键字的说明中。 这一要求，对于我们使用计分的配置文件来进行排序比其他字段更高城市字段。

**支持多种语言**

我们需要以正确的语言显示正确的搜索结果，并提供一个选项在不同语言中查找相同的结果。 对此问题的两个方面是︰ 

- 多种语言中的词的搜索
- 在正确的语言显示搜索结果

我们的解决方法的演示文稿部分通过添加本地化文本与每种语言和与语言属性的文档。 当用户输入搜索词，我们用户`$filter`表达式进行筛选语言用户已选择。

每个文档都有隐藏的属性，名为"城市"，生成的集合类型上。 此属性存储所有的语言，使用户可以搜索多种语言中的城市名称。

###数据存储

（配置文件、 订阅和记帐） 的所有数据都存储在 SQL 数据库中。 所有媒体文件都存储在 Azure BLOB 存储，包括图像和视频由零售商提供。 使用单独的 BLOB 存储隔离上载文件; 的效果文件是永远不会合并与网站，因此我们不需要重建站点，只要我们添加文件。

我们存储设计的一个重要优点是多个开发人员可以共享单个开发存储。 WhatToPedia 项目的要求之一是能够在 15 分钟内，包括分销商的数据、 图像和视频创建一个开发环境。 通过从 TFS 在线获取最新的数据、 运行 SQL 脚本，并运行导入作业，整个环境可以将经受很快根本。 这种做法还提高了转移的过程。

###WebJobs

我们使用 Azure WebJobs 到索引更新的数据。 通过创建搜索索引器作业，索引的一部分时很容易集成到我们的解决方案。 我们所做的唯一的代码更改已以容纳索引器作业已添加`Indexed`字段来指示索引的数据模型。 只要添加或更新，新的配置文件就`Indexed`字段被设置为 false。 这同样适用，如果零售商改变他或她通过门户的配置文件数据。  

作业中遇到的所有行都寻找`Indexed`设置为 false。 当它找到该行时，单据过帐到 Azure 搜索，然后`Indexed`字段设置为 true。 我们没有计划而不更新数据，因为 Azure 搜索服务实际上负责此添加。 如果添加已存在的文档时，该服务将自动执行更新。

作为控制台应用程序，可以上载到 Azure 的 web 站点为 ZIP 文件，解压缩，然后安排制定了所有 web 作业。

作业计划作为计划的 web 任务每隔 5 分钟运行一次。 我们计算服务使大约三分钟传 3000 文档，这是在我们的要求。 

> [AZURE.NOTE] 没有在 Azure 搜索最近引入了一个原型索引功能。 此功能是太晚我们用我们的第一个版本，但看起来似乎是解决同样的问题，我们使用我们的索引器作业，这是自动执行数据加载操作。


###备份策略

我们设计了多层备份策略进行恢复的一系列方案，从灾难性故障，甚至个别事务的恢复。 若要保护的资产包括三种类型的数据 （网站，订阅服务器数据和媒体文件）。 

首先，通过保持在 TFS 联机 web 站点的源代码，我们知道，是否网站出现故障，我们可以重建它从 TFS 重新发布。 

订阅服务器在 SQL Azure 数据库中的数据是最敏感的资产。 我们重新使用内置该功能 （请参阅[Azure SQL 数据库备份和还原](http://msdn.microsoft.com/library/azure/jj650016.aspx)）。 备份的计划是每周一次的完整数据库备份、 差异数据库备份，每天一次，以及事务日志备份内每 5 分钟。  给出的数据的大小，此解决方案就足够我们即时的和预期的数据量。

第三，我们在 Azure BLOB 存储中存储图像和视频文件。 我们仍在评估最终备份计划为这些数据，考虑 Cloudberry Azure 作为潜在的解决方案资源管理器。 现在，我们将使用 WebJob 来将图像和视频复制到另一个位置。

##我们学习了

因为我们已经有了数据，它很容易建立概念证明。 小时内, 我们有方面与原型和计数器，分页，排序配置文件，并搜索结果。 搜索结果是如此精确，我们决定去呈现给最终客户的筛选器。 

对我们最大的惊喜就是速度，我们无法了解 Azure 搜索和多少得到。 确切地说，我们建立了概念证明中 （请参阅下面的注释） 的几个小时，500 行代码替换 3 中的代码行的前端应用程序 （加上新的 WebJob），并获得更好的结果。 

以前，我们的代码实现分页、 次数和标准进行搜索的其他行为。 使用 Azure 搜索时，我们得到的结果包括搜索点击量、 小平面、 分页数据，计数-我们需要而无需提供自己的所有东西。 它还包括提高并内置多面导航，我们没有在我们的原始解决方案。

在实施过程中的最大挑战时，这是一个预览版本，查找信息和共享的经验是很困难。 一旦我们连接很少的点，我们发现使用 Azure 搜索服务是由于其 REST API 和 JSON 数据格式非常简单。 我们可以直接从类似 JQuery JSON.Net 最开放源插件调用框架，我们可以进行快速的试验和调试使用 Fiddler 之类的工具。 

> [AZURE.NOTE] 除了有准备的数据，它帮助我们在已经构建原型的理解如何搜索技术的工作原理，使我们提高工作效率，而且更 appreciative 的内置功能。 如果您需要在构造搜索查询量产，多面导航、 筛选等可以预见，原型设计，要花更长的时间。 

###在搜索演示文稿页控制方面

我们在概念验证过程中的经验教训之一就是规划方面提前仔细。 将大量数据加载到解决方案之后, 我们看到巨大的方面是向用户提供过高。 

我们解决这个通过约束 facet count 参数。 Count 参数实施硬限制的数目方面返回给用户。 包括计数参数讨论的链接可以找到[这里](search-faceted-navigation.md)。

###WebJobs 用于计划任务

Azure 的搜索并不为我们只有令人兴奋。 我们发现，使用 WebJobs 自动到 Azure 搜索我们数据装载操作大大优于我们以前的做法，需要执行使用专用虚拟机运行 Windows 计划程序与更新搜索索引的计划任务。 WebJobs 的配置更简单并更易于调试和当然很多价格低于无需支付一个专用的虚拟机。

###Azure BLOB 存储资源管理器中更新图像

我们发现，使用[Azure BLOB 存储浏览器](https://azurestorageexplorer.codeplex.com/)（可在 codeplex 上） 将管理图像和视频网站更新很有帮助。 我们使用它作为开发工具来手动更新图像和视频是我们主站点的一部分。 我们发现它是比将更改部署到门户，更灵活，消除了完整的测试迭代，每当我们需要更新图像。 

##最后的几个字

为工作人员在 WhatToPedia 为使我们能够共享他们的故事，谢谢 ！  

我们希望您发现此案例研究中非常有用。 如果您继续使用 Azure 搜索，我建议有一些资源可以加快沿您︰

- [专用于 Azure 搜索 MSDN 论坛](https://social.msdn.microsoft.com/forums/azure/home?forum=azuresearch)
- [StackOverflow 还有一个标记](http://stackoverflow.com/questions/tagged/azure-search)
- [在 Azure.com 上的文档页](http://azure.microsoft.com/documentation/services/search/)
- [在 MSDN 上的 azure 搜索文档](http://msdn.microsoft.com/library/azure/dn798933.aspx)


##附录︰ 搜索索引器 WebJob

下面的代码生成构建原型上一节中提到的索引器。

        static void Main(string[] args)
        {
            int success = 0;
            int errors = 0;

            Log.Write("Starting job","", System.Diagnostics.TraceLevel.Info);

            var serviceName = ConfigurationManager.AppSettings["SearchServiceName"];
            var serviceKey = ConfigurationManager.AppSettings["SearchServiceKey"];

            HttpClient client = new HttpClient();
            client.DefaultRequestHeaders.Add("api-key", serviceKey);

            var db = new DB(Config.ConectionString);

            var recreateIndex = false;
            Boolean.TryParse(ConfigurationManager.AppSettings["RecreateIndex"], out recreateIndex);

            if(recreateIndex)
            {
                Log.Write("Recreating index and set all to no index", "", System.Diagnostics.TraceLevel.Info);
                db.SetAllToNotIndexed();
                RecreateIndex(serviceName, client);
            }            
            
            var profiles = db.Profiles.Where(p=>!p.Indexed).ToList();

            Log.Write(string.Format("Indexing {0} profiles",profiles.Count),"", System.Diagnostics.TraceLevel.Info);

            var cities = db.Cities.ToList();
            var categories = db.Tags.Where(p=>p.ParentId==null).ToList();            

            foreach (var profile in profiles)
            {
                Log.Write(string.Format("Indexing profile {0}", profile.Name),"",profile.ProfileId,0,System.Diagnostics.TraceLevel.Verbose);

                try
                {
                    var city = cities.Where(p => p.CityId == profile.CityId);
                    var category = categories.Where(p => p.TagId == profile.CategoryId);

                    var cityse = city.Where(p => p.Lang == "se").FirstOrDefault();
                    var cityen = city.Where(p => p.Lang == "en").FirstOrDefault();
                    var categoryse = category.Where(p => p.Lang == "se").FirstOrDefault();
                    var categoryen = category.Where(p => p.Lang == "en").FirstOrDefault();

                    var citysename = cityse == null ? "" : cityse.Name;
                    var cityenname = cityen == null ? "" : cityen.Name;
                    var categorysename = categoryse == null ? "" : categoryse.Name;
                    var categoryenname = categoryen == null ? "" : categoryen.Name;

                    var tags = db.GetTagsFromProfile(profile.ProfileId);

                    var batch = new
                    {
                        value = new[] 
                    { 
                        new 
                        { 
                            id = profile.ProfileId.ToString()+"_en",
                            profileid = profile.ProfileId.ToString(),
                            city = cityenname,
                            category = categoryenname,
                            address = profile.Adress1,
                            email = profile.Email,
                            name = profile.Name,
                            lang = "en",
                            brands = profile.Brands,
                            descen=profile.DescEn,
                            descse=profile.DescSe,
                            orgnumber=profile.OrgNumber,
                            phone=profile.Phone,
                            zip=profile.Zip,
                            cities = city.Select(p=>p.Name).ToArray(),
                            categories = category.Select(p=>p.Name).ToArray(),
                            cityid = profile.CityId.ToString(),
                            tags=tags.ToArray()
                        },
                        new 
                        { 
                            id = profile.ProfileId.ToString()+"_se",
                            profileid = profile.ProfileId.ToString(),
                            city = citysename,
                            category = categorysename,
                            address = profile.Adress1,
                            email = profile.Email,
                            name = profile.Name,
                            lang = "se",
                            brands = profile.Brands,
                            descen=profile.DescEn,
                            descse=profile.DescSe,
                            orgnumber=profile.OrgNumber,
                            phone=profile.Phone,
                            zip=profile.Zip,
                            cities = city.Select(p=>p.Name).ToArray(),
                            categories = category.Select(p=>p.Name).ToArray(),
                            cityid = profile.CityId.ToString(),
                            tags=tags.ToArray()
                        }
                    },
                    };

                    var response = client.PostAsync("https://" + serviceName + ".search.windows.net/indexes/profiles/docs/index?api-version=2014-10-20-Preview", new StringContent(JsonConvert.SerializeObject(batch), Encoding.UTF8, "application/json")).Result;
                    response.EnsureSuccessStatusCode();

                    db.Entry(profile).State = System.Data.Entity.EntityState.Modified;
                    profile.Indexed = true;
                    db.SaveChanges();
                    success++;
                }
                catch(Exception ex)
                {
                    Log.Write("Error indexing profile", ex.Message, profile.ProfileId, 0, System.Diagnostics.TraceLevel.Verbose);
                    errors++;
                }
            }
            if(errors > 0)
            {
                Log.Write(string.Format("Job ended success ({0}), errors ({1})", success, errors), "", System.Diagnostics.TraceLevel.Error);
            }
            else
            {
                Log.Write(string.Format("Job ended success ({0}), errors ({1})", success, errors), "", System.Diagnostics.TraceLevel.Info);
            }
            
        }

        static void RecreateIndex( string ServiceName, HttpClient client)
        {
            var index = new
            {
                name = "profiles",
                fields = new[] 
                { 
                    new { name = "id",              type = "Edm.String",         key = true,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "profileid",       type = "Edm.String",         key = false,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "cityid",          type = "Edm.String",         key = false,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "city",            type = "Edm.String",         key = false, searchable = true,  filterable = true, sortable = true,  facetable = true, retrievable = true,  suggestions = true  },
                    new { name = "category",        type = "Edm.String",         key = false, searchable = true,  filterable = true, sortable = false, facetable = true, retrievable = true,  suggestions = false  },
                    new { name = "address",         type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "email",           type = "Edm.String",         key = false, searchable = true,  filterable = false, sortable = true, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "name",            type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true, suggestions = true },
                    new { name = "lang",            type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = false,  facetable = false,  retrievable = true, suggestions = false },
                    new { name = "brands",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "descen",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "descse",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "orgnumber",       type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "phone",           type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "zip",             type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "cities",          type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false },
                   new { name = "categories",      type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false },
                    new { name = "tags",            type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false }
                    
                }
            };

            var url = "https://" + ServiceName + ".search.windows.net/indexes/?api-version=2014-10-20-Preview";

            var deleteUrl = "https://" + ServiceName + ".search.windows.net/indexes/profiles?api-version=2014-10-20-Preview";

            try
            {
                var deleteResponseIndex = client.DeleteAsync(deleteUrl).Result;
                deleteResponseIndex.EnsureSuccessStatusCode();
            }
            catch (Exception ex)
            {

            }

            var responseIndex = client.PostAsync(url, new StringContent(JsonConvert.SerializeObject(index), Encoding.UTF8, "application/json")).Result;
            responseIndex.EnsureSuccessStatusCode();            
          


<!--Anchors-->
[子标题 1]: #subheading-1
[2 小标题]: #subheading-2
[副标题 3]: #subheading-3
[副标题 4]: #subheading-4
[副标题 5]: #subheading-5
[下一步行动]: #next-steps

<!--Image references-->
[6]: ./media/search-dev-case-study-whattopedia/lightbulb.png
[7]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Search-Bageri.png
[8]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Stack.png
[9]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Archicture.png


<!--Link references-->
[链接 1 到另一个 azure.microsoft.com 文档主题]: ../virtual-machines-windows-tutorial.md
[另一 azure.microsoft.com 文档主题链接 2]: ../web-sites-custom-domain-name.md
[链接到另一个 azure.microsoft.com 文档主题的 3]: ../storage-whatis-account.md
 