---
ms.openlocfilehash: 29296ffe67784265ef72856cbb3a6db01ad5bb08
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用.NET 后端创建 Windows 应用商店排行榜应用程序 |Microsoft Azure" 
    description="了解如何构建一个 Windows 应用商店排行榜应用程序使用.NET 后端使用 Azure 移动服务。" 
    documentationCenter="windows" 
    authors="MikeWasson" 
    manager="dwrede" 
    editor="" 
    services="mobile-services"/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/24/2015" 
    ms.author="glenga"/>

# 使用 Azure 移动创建排行榜应用程序服务.NET 后端

本教程演示如何生成 Windows 应用商店应用程序使用.NET 后端使用 Azure 移动服务。 Azure 的移动服务提供了监视，推式通知和其他功能，再加上用于构建移动应用程序的跨平台客户端库使用内置的身份验证，可扩展性和安全性的后端。 移动服务的.NET 后端基于[ASP.NET Web API](http://asp.net/web-api)，并为.NET 开发人员提供了创建 REST Api 的一类方法。   

## 概述

Web API 是开放源代码框架，为.NET 开发人员提供了创建 REST Api 的一类方法。 可以承载 Web API 解决方案在 Azure 网站，在 Azure 移动服务使用.NET 后端，或甚至自我承载自定义过程中。 移动服务是专门设计用于移动应用程序的宿主环境。 承载 Web API 服务上移动服务时，将获得以下优点以及数据存储︰

- 具有社会提供商和 Azure 活动目录 (AAD) 的内置身份验证。 
- 将通知发送到使用特定于设备的通知服务的应用程序。
- 一套完整的客户端库，使其可以轻松地从任何应用程序访问您的服务。 
- 内置的日志和诊断。

在本教程中，您将能够︰

- 创建 REST API，使用 Azure 移动服务。
- 将该服务发布到 Azure。
- 创建一个 Windows 应用商店应用程序使用该服务。
- 使用实体框架 (EF) 创建外键关系和数据传输对象 (Dto)。
- 使用 ASP.NET Web API 定义一个自定义的 API。

本教程使用[Visual Studio 2013年最新的更新](http://go.microsoft.com/fwlink/p/?LinkID=390465)。 


## 关于示例应用程序

*排行榜*显示列表中的玩家进行游戏，以及他们分数和每个运动员的排名。 排行榜可能是一个更大的游戏的一部分，或可能是一个单独的应用程序。 排行榜是现实世界的应用程序，但是相当简单的教程。 下面是该应用程序的屏幕快照︰

![][1]

若要使应用程序保持简单，没有任何实际的游戏。 相反，您可以添加玩家和提交每个队员的成绩。 当您提交分数时，移动服务计算新的排名。 在后端，移动服务与两个表中创建一个数据库︰

- 播放机。 包含播放机 ID 和名称。
- PlayerRank。 包含玩家的分数和排名。

PlayerRank 有到播放机的外键。 每个运动员都有零个或一个 PlayerRank。

在真实的排行榜应用程序中，PlayerRank 可能还有游戏 ID，以便玩家可以提交多个游戏的成绩。

![][2]

客户端应用程序可以执行 CRUD 操作对玩家的完整集。 它可以读取或删除现有的 PlayerRank 实体，但它不能创建或直接对其进行更新。 这是因为排位值的计算服务。 相反，客户端提交分数，和服务的所有播放机更新通道。

下载已完成的项目[在此处](http://code.msdn.microsoft.com/Leaderboard-App-with-Azure-9acf63af)。


## 创建项目

启动 Visual Studio 并创建新的 ASP.NET Web 应用程序项目。 命名项目排行榜。

![][3]

在 Visual Studio 2013，ASP.NET Web 应用程序项目包括 Azure 手机信息服务的模板。 选择此模板并单击**确定**。

![][4]
 
项目模板包括一个示例控制器和数据对象。  

![][5]
 
这些不需要此教程中，以便您可以从项目中删除它们。 此外在 WebApiConfig.cs 和 LeaderboardContext.cs 中删除对 TodoItem 的引用。

## 添加数据模型

您将使用[EF 代码优先](http://msdn.microsoft.com/data/ee712907#codefirst)来定义数据库表。 在 DataObjects 文件夹中，添加名为的类`Player`。

    using Microsoft.WindowsAzure.Mobile.Service;
    
    namespace Leaderboard.DataObjects
    {
        public class Player : EntityData
        {
            public string Name { get; set; }
        }
    }

添加另一个类名为`PlayerRank`。

    using Microsoft.WindowsAzure.Mobile.Service;
    using System.ComponentModel.DataAnnotations.Schema;
    
    namespace Leaderboard.DataObjects
    {
        public class PlayerRank : EntityData
        {
            public int Score { get; set; }
            public int Rank { get; set; }
    
            [ForeignKey("Id")]
            public virtual Player Player { get; set; }
        }
    }

请注意，这两个类从类继承的**EntityData** 。 从**EntityData**使应用程序更容易消耗数据，使用 Azure 移动服务的跨平台客户端库。 **EntityData**还可以简化应用程序处理[数据库写入冲突](mobile-services-windows-store-dotnet-handle-database-conflicts.md)。

`PlayerRank`类具有一个指向相关的[导航属性](http://msdn.microsoft.com/data/jj713564.aspx)`Player`实体。 **[ForeignKey]**属性告诉 EF`Player`属性表示外键。

## 添加 Web API 控制器

接下来，您将添加 Web API 控制器的`Player`， `PlayerRank`。 而不是普通的 Web API 控制器，您将添加一种特殊的控制器调用*表控制器*，专门针对 Azure 移动服务。

右键单击控制器文件夹 >**添加** > **启用了基架的新项**。

![][6] 

在**添加构架**对话框中，展开左边的**常见**并选择**Azure 移动服务**。 然后，选择**Azure 移动服务表控制器**。 单击**添加**。

![][7] 
 
在**添加控制器**对话框中︰

1.  在**模型类**中，选择播放机。 
2.  **数据上下文类**中，在选择 MobileServiceContext。
3.  命名为"PlayerController"的控制器。
4.  单击**添加**。


此步骤将添加一个名为 PlayerController.cs，到项目文件。

![][8] 

从派生的控制器**TableController<T>**。 此类继承**ApiController**，但专用于 Azure 移动服务。
 
- 路由︰ **TableController**的默认路由是`/tables/{table_name}/{id}`，其中*table_name*匹配的实体名称。 因此播放机控制器的工艺路线是*/tables/播放器 / {id}*。 此路由的约定使得**TableController**符合[REST API](http://msdn.microsoft.com/library/azure/jj710104.aspx)移动服务。
- 数据访问︰ **TableController**类的数据库操作，使用**IDomainManager**接口，定义了数据访问的抽象。  基架使用**EntityDomainManager**，它是**IDomainManager**包装 EF 上下文的具体实现。 

现在，添加第二个控制器用于 PlayerRank 的实体。 按照同样的步骤，但选择 PlayerRank 模型的类。 使用相同的数据上下文类;不要创建一个新。 命名为"PlayerRankController"的控制器。

## 使用 DTO 返回相关的实体

请回想一下，`PlayerRank`具有相关`Player`实体︰ 

    public class PlayerRank : EntityData
    {
        public int Score { get; set; }
        public int Rank { get; set; }

        [ForeignKey("Id")]
        public virtual Player Player { get; set; }
    }

手机信息服务客户端库不支持导航属性，它们将不进行序列化。 例如，以下是获取的原始 HTTP 响应`/tables/PlayerRank`:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Pragma: no-cache
    Content-Length: 97
    Content-Type: application/json; charset=utf-8
    Expires: 0
    Server: Microsoft-IIS/8.0
    Date: Mon, 21 Apr 2014 17:58:43 GMT
    
    [{"id":"1","rank":1,"score":150},{"id":"2","rank":3,"score":100},{"id":"3","rank":1,"score":150}]

请注意，`Player`不包括的对象关系图中。 若要包含播放机，我们可以通过定义*数据传输对象*(DTO) 拼合的对象图。 

DTO 是一个对象，定义在网络上发送数据的方式。 Dto 是有用的如果想要看起来不同于您的数据库模型的线格式。 若要创建 DTO 的`PlayerRank`，添加一个名为的新类`PlayerRankDto`DataObjects 文件夹中。

    namespace Leaderboard.DataObjects
    {
        public class PlayerRankDto
        {
            public string Id { get; set; }
            public string PlayerName { get; set; }
            public int Score { get; set; }
            public int Rank { get; set; }
        }
    }

在`PlayerRankController`类，我们将使用 LINQ**选择**方法来转换`PlayerRank`实例的`PlayerRankDto`实例。 更新`GetAllPlayerRank`和`GetPlayerRank`控制器方法，如下所示︰

    // GET tables/PlayerRank
    public IQueryable<PlayerRankDto> GetAllPlayerRank()
    {
        return Query().Select(x => new PlayerRankDto()
        {
            Id = x.Id,
            PlayerName = x.Player.Name,
            Score = x.Score,
            Rank = x.Rank
        });
    }
    
    // GET tables/PlayerRank/48D68C86-6EA6-4C25-AA33-223FC9A27959
    public SingleResult<PlayerRankDto> GetPlayerRank(string id)
    {
        var result = Lookup(id).Queryable.Select(x => new PlayerRankDto()
        {
            Id = x.Id,
            PlayerName = x.Player.Name,
            Score = x.Score,
            Rank = x.Rank
        });
    
        return SingleResult<PlayerRankDto>.Create(result);
    }

进行这些更改，这两种的 GET 方法返回`PlayerRankDto`给客户端的对象。 `PlayerRankDto.PlayerName`属性设置为播放机名称。 进行此更改后，下面是一个示例响应︰

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Pragma: no-cache
    Content-Length: 160
    Content-Type: application/json; charset=utf-8
    Expires: 0
    Server: Microsoft-IIS/8.0
    Date: Mon, 21 Apr 2014 19:57:08 GMT
    
    [{"id":"1","playerName":"Alice","score":150,"rank":1},{"id":"2","playerName":"Bob","score":100,"rank":3},{"id":"3","playerName":"Charles","score":150,"rank":1}]

请注意，JSON 负载现在包括运动员姓名。

而不是使用 LINQ Select 语句，另一种方法是使用 AutoMapper。 此选项需要一些附加的设置代码，但可以从域实体到 Dto 的自动映射。 有关详细信息，请参阅[数据库类型与在.NET 后端使用 AutoMapper 中的客户端类型之间的映射](http://blogs.msdn.com/b/azuremobile/archive/2014/05/19/mapping-between-database-types-and-client-type-in-the-net-backend-using-automapper.aspx)。

## 定义一个自定义的 API 提交成绩

`PlayerRank`实体包括`Rank`属性。 此值的服务器上，通过计算，我们不希望将其设置的客户端。 相反，客户端将使用一个自定义的 API 提交运动员的成绩。  当服务器获取新的成绩时，它将更新所有的播放机通道。

首先，添加名为的类`PlayerScore`到 DataObjects 文件夹。

    namespace Leaderboard.DataObjects
    {
        public class PlayerScore
        {
            public string PlayerId { get; set; }
            public int Score { get; set; }
        }
    }

在`PlayerRankController`类，移动`MobileServiceContext`变量从构造函数与类变量︰

    public class PlayerRankController : TableController<PlayerRank>
    {
        // Add this:
        MobileServiceContext context = new MobileServiceContext();

        protected override void Initialize(HttpControllerContext controllerContext)
        {
            base.Initialize(controllerContext);

            // Delete this:
            // MobileServiceContext context = new MobileServiceContext();
            DomainManager = new EntityDomainManager<PlayerRank>(context, Request, Services);
        }


删除以下方法从`PlayerRankController`:

- `PatchPlayerRank` 
- `PostPlayerRank` 
- `DeletePlayerRank`

然后添加以下代码为`PlayerRankController`:

    [Route("api/score")]
    public async Task<IHttpActionResult> PostPlayerScore(PlayerScore score)
    {
        // Does this player exist?
        var count = context.Players.Where(x => x.Id == score.PlayerId).Count();
        if (count < 1)
        {
            return BadRequest();
        }

        // Try to find the PlayerRank entity for this player. If not found, create a new one.
        PlayerRank rank = await context.PlayerRanks.FindAsync(score.PlayerId);
        if (rank == null)
        {
            rank = new PlayerRank { Id = score.PlayerId };
            rank.Score = score.Score;
            context.PlayerRanks.Add(rank);
        }
        else
        {
            rank.Score = score.Score;
        }

        await context.SaveChangesAsync();

        // Update rankings
        // See http://stackoverflow.com/a/575799
        const string updateCommand =
            "UPDATE r SET Rank = ((SELECT COUNT(*)+1 from {0}.PlayerRanks " +
            "where Score > (select score from {0}.PlayerRanks where Id = r.Id)))" +
            "FROM {0}.PlayerRanks as r";

        string command = String.Format(updateCommand, ServiceSettingsDictionary.GetSchemaName());
        await context.Database.ExecuteSqlCommandAsync(command);

        return Ok();
    }

`PostPlayerScore`方法接受`PlayerScore`实例作为输入。 (客户端将发送`PlayerScore`在 HTTP POST 请求。)该方法实现以下功能︰

1.  添加一个新的`PlayerRank`播放器，如果没有在数据库中已。
2.  更新玩家的分数。
3.  运行该批处理更新播放机通道的所有 SQL 查询。

**[路由]**属性可以定义此方法的自定义路由︰

    [Route("api/score")]

您还可以放入单独的控制器方法。 没有任何独特的优势，两种方法，它只取决于您希望如何组织代码。
若要了解有关**[路由]**属性的详细信息，请参阅[Web API 中路由属性](http://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2)。

## 创建 Windows 应用商店应用程序

在此部分中，我将介绍的 Windows 应用商店应用程序需要使用移动服务。 但是，我不会集中很大程度上 XAML 或用户界面。 相反，我希望可以专注于应用程序逻辑。 您可以下载完整的项目[这里](http://code.msdn.microsoft.com/Leaderboard-App-with-Azure-9acf63af)。

向解决方案中添加一个新的 Windows 应用商店应用程序项目。 我使用了空白的应用程序 (Windows) 模板。 

![][10]
 
使用 NuGet 程序包管理器添加移动服务客户端库。 在 Visual Studio 中，从**工具**菜单中，选择**NuGet 程序包管理器**。 然后，选择**程序包管理器控制台**。 在程序包管理器控制台窗口中，键入下面的命令。

    Install-Package WindowsAzure.MobileServices -Project LeaderboardApp

-此开关可指定要安装的软件包的项目。

## 添加模型类

创建一个名为模型，添加以下类︰

    namespace LeaderboardApp.Models
    {
        public class Player
        {
            public string Id { get; set; }
            public string Name { get; set; }
        }
    
        public class PlayerRank
        {
            public string Id { get; set; }
            public string PlayerName { get; set; }
            public int Score { get; set; }
            public int Rank { get; set; }
        }
    
        public class PlayerScore
        {
            public string PlayerId { get; set; }
            public int Score { get; set; }
        }
    }

这些类直接对应于移动服务中的数据实体。
 
## 创建视图模型

模型-视图-模型 (MVVM) 是一个变量的模型-视图-控制器 (MVC)。 MVVM 模式有助于演示文稿中的单独的应用程序逻辑。

- 模型表示域数据 （唱机、 玩家等级和分数的玩家）。
- 视图模型是视图的抽象表示形式。 
- 视图显示视图模型，并将发送到视图模型的用户输入。 为 Windows 应用商店应用程序中，视图是在 XAML 中定义的。

![][11] 

添加一个类名为`LeaderboardViewModel`。

    using LeaderboardApp.Models;
    using Microsoft.WindowsAzure.MobileServices;
    using System.ComponentModel;
    using System.Net.Http;
    using System.Threading.Tasks;
    
    namespace LeaderboardApp.ViewModel
    {
        class LeaderboardViewModel : INotifyPropertyChanged
        {
            MobileServiceClient _client;
    
            public LeaderboardViewModel(MobileServiceClient client)
            {
                _client = client;
            }
        }
    }

视图模型，实现**INotifyPropertyChanged** ，因此视图模型可以参与数据绑定。 

    class LeaderboardViewModel : INotifyPropertyChanged
    {
        MobileServiceClient _client;

        public LeaderboardViewModel(MobileServiceClient client)
        {
            _client = client;
        }

        // New code:
        // INotifyPropertyChanged implementation
        public event PropertyChangedEventHandler PropertyChanged;

        public void NotifyPropertyChanged(string propertyName)
        {
            if (PropertyChanged != null)
            {
                PropertyChanged(this,
                    new PropertyChangedEventArgs(propertyName));
            }
        }    
    }

接下来，添加可观察的属性。 XAML 将数据绑定到这些属性。 

    class LeaderboardViewModel : INotifyPropertyChanged
    {
        // ...

        // New code:
        // View model properties
        private MobileServiceCollection<Player, Player> _Players;
        public MobileServiceCollection<Player, Player> Players
        {
            get { return _Players; }
            set
            {
                _Players = value;
                NotifyPropertyChanged("Players");
            }
        }

        private MobileServiceCollection<PlayerRank, PlayerRank> _Ranks;
        public MobileServiceCollection<PlayerRank, PlayerRank> Ranks
        {
            get { return _Ranks; }
            set
            {
                _Ranks = value;
                NotifyPropertyChanged("Ranks");
            }
        }

        private bool _IsPending;
        public bool IsPending
        {
            get { return _IsPending; }
            set
            {
                _IsPending = value;
                NotifyPropertyChanged("IsPending");
            }
        }

        private string _ErrorMessage = null;
        public string ErrorMessage
        {
            get { return _ErrorMessage; }
            set
            {
                _ErrorMessage = value;
                NotifyPropertyChanged("ErrorMessage");
            }
        }
    }

`IsPending`属性为 true 时异步操作是挂起的服务上。 `ErrorMessage`属性包含该服务从任何错误消息。 

最后，将通过调用方法添加到服务层。 

    class LeaderboardViewModel : INotifyPropertyChanged
    {
        // ...

        // New code:
        // Service operations
        public async Task GetAllPlayersAsync()
        {
            IsPending = true;
            ErrorMessage = null;

            try
            {
                IMobileServiceTable<Player> table = _client.GetTable<Player>();
                Players = await table.OrderBy(x => x.Name).ToCollectionAsync();
            }
            catch (MobileServiceInvalidOperationException ex)
            {
                ErrorMessage = ex.Message;
            }
            catch (HttpRequestException ex2)
            {
                ErrorMessage = ex2.Message;
            }
            finally
            {
                IsPending = false;
            }
        }

        public async Task AddPlayerAsync(Player player)
        {
            IsPending = true;
            ErrorMessage = null;

            try
            {
                IMobileServiceTable<Player> table = _client.GetTable<Player>();
                await table.InsertAsync(player);
                Players.Add(player);
            }
            catch (MobileServiceInvalidOperationException ex)
            {
                ErrorMessage = ex.Message;
            }
            catch (HttpRequestException ex2)
            {
                ErrorMessage = ex2.Message;
            }
            finally
            {
                IsPending = false;
            }
        }

        public async Task SubmitScoreAsync(Player player, int score)
        {
            IsPending = true;
            ErrorMessage = null;

            var playerScore = new PlayerScore()
            {
                PlayerId = player.Id,
                Score = score
            }; 
            
            try
            {
                await _client.InvokeApiAsync<PlayerScore, object>("score", playerScore);
                await GetAllRanksAsync();
            }
            catch (MobileServiceInvalidOperationException ex)
            {
                ErrorMessage = ex.Message;
            }
            catch (HttpRequestException ex2)
            {
                ErrorMessage = ex2.Message;
            }
            finally
            {
                IsPending = false;
            }
        }

        public async Task GetAllRanksAsync()
        {
            IsPending = true;
            ErrorMessage = null;

            try
            {
                IMobileServiceTable<PlayerRank> table = _client.GetTable<PlayerRank>();
                Ranks = await table.OrderBy(x => x.Rank).ToCollectionAsync();
            }
            catch (MobileServiceInvalidOperationException ex)
            {
                ErrorMessage = ex.Message;
            }
            catch (HttpRequestException ex2)
            {
                ErrorMessage = ex2.Message;
            }
            finally
            {
                IsPending = false;
            }
         }    
    }

## 添加一个 MobileServiceClient 实例

打开*App.xaml.cs*文件，并添加到一个**MobileServiceClient**实例`App`类。

    // New code:
    using Microsoft.WindowsAzure.MobileServices;
    
    namespace LeaderboardApp
    {
        sealed partial class App : Application
        {
            // New code.
            // TODO: Replace 'port' with the actual port number.
            const string serviceUrl = "http://localhost:port/";
            public static MobileServiceClient MobileService = new MobileServiceClient(serviceUrl);
    
    
            // ...
        }
    }

本地调试时，移动服务运行在 IIS express 上。 Visual Studio 将分配一个随机端口号，这样的本地 URL 是 http://localhost︰*端口*，*端口*所在的端口号。 若要获取该端口号，请通过按 F5 来调试在 Visual Studio 中启动该服务。 Visual Studio 将启动浏览器，然后定位到服务 URL。  您还可以在项目属性中，在**Web**下找到的本地 URL。

## 创建主页

在主页面中，添加视图模型的实例。 然后设置为**DataContext**页面的视图模型。

    public sealed partial class MainPage : Page
    {
        // New code:
        LeaderboardViewModel viewModel = new LeaderboardViewModel(App.MobileService);

        public MainPage()
        {
            this.InitializeComponent();
            // New code:
            this.DataContext = viewModel;
        }

       // ...


正如我前面提到的我不会显示所有应用程序 XAML。 MVVM 模式的好处之一是如果您不喜欢该示例应用程序可以很容易地更改用户界面，以便将与应用程序逻辑的演示文稿。

玩家的列表将显示在一个**列表框**︰

    <ListBox Width="200" Height="400" x:Name="PlayerListBox" 
        ItemsSource="{Binding Players}" DisplayMemberPath="Name"/>

在**列表视图**中显示的排名︰

    <ListView x:Name="RankingsListView" ItemsSource="{Binding Ranks}" SelectionMode="None">
        <!-- Header and styles not shown -->
        <ListView.ItemTemplate>
            <DataTemplate>
                <Grid>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="*"/>
                        <ColumnDefinition Width="2*"/>
                        <ColumnDefinition Width="*"/>
                    </Grid.ColumnDefinitions>
                    <TextBlock Text="{Binding Path=Rank}"/>
                    <TextBlock Text="{Binding Path=PlayerName}" Grid.Column="1"/>
                    <TextBlock Text="{Binding Path=Score}" Grid.Column="2"/>
                </Grid>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>

所有的数据绑定是通过查看模型。

## 发布您的移动服务

在此步骤中，您将您的移动服务发布到 Microsoft Azure 并修改应用程序以使用实时服务。

在解决方案资源管理器中右击该排行榜项目并选择**发布**。
 
![][12]

在**发布**对话框中，单击**Azure 移动服务**。

![][13]
 
如果您没有登录到 Azure 帐户已，单击**登录**。

![][14]


选择一个现有的移动服务，或单击**新建**以创建一个新。 然后单击**确定**以发布。

![][15]
 
发布过程会自动创建数据库。 您不需要配置一个连接字符串。

现在，您就连接到实时服务的排行榜应用程序了。 您需要以下两种情况︰

- 服务的 URL
- 应用程序键

您可以同时从 Azure 管理门户。 在管理门户中，单击**移动服务**，然后单击移动服务。 在仪表板选项卡上列出的服务 URL。 要获取的应用程序键，请单击**管理密钥**。

![][16]
 
在**管理访问键**对话框中，复制的应用程序键的值。

![][17]

 
将服务 URL 和应用程序键传递给**MobileServiceClient**构造函数。

    sealed partial class App : Application
    {
        // TODO: Replace these strings with the real URL and key.
        const string serviceUrl = "https://yourapp.azure-mobile.net/";
        const string appKey = "YOUR ACCESSS KEY";

        public static MobileServiceClient MobileService = new MobileServiceClient(serviceUrl, appKey);

       // ...

现在当您运行该应用程序，它与实际服务通信。 

## 下一步行动

* [了解有关 Azure 移动服务的详细信息]
* [了解有关 Web API 的详细信息]
* [处理数据库写入冲突]
* [添加推式通知];例如，当某人添加新玩家或更新分数。
* [开始使用身份验证]

<!-- Anchors. -->
[概述]: #overview
[关于示例应用程序]: #about-the-sample-app
[创建项目]: #create-the-project
[添加数据模型]: #add-data-models
[添加 Web API 控制器]: #add-web-api-controllers
[使用 DTO 返回相关的实体]: #use-a-dto-to-return-related-entities
[定义一个自定义的 API 提交成绩]: #define-a-custom-api-to-submit-scores
[创建 Windows 应用商店应用程序]: #create-the-windows-store-app
[添加模型类]: #add-model-classes
[创建视图模型]: #create-a-view-model
[添加一个 MobileServiceClient 实例]: #add-a-mobileserviceclient-instance
[创建主页]: #create-the-main-page
[发布您的移动服务]: #publish-your-mobile-service
[下一步行动]: #next-steps

<!-- Images. -->

[1]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/01leaderboard.png
[2]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/02leaderboard.png
[3]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/03leaderboard.png
[4]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/04leaderboard.png
[5]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/05leaderboard.png
[6]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/06leaderboard.png
[7]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/07leaderboard.png
[8]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/08leaderboard.png
[9]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/09leaderboard.png
[10]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/10leaderboard.png
[11]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/11leaderboard.png
[12]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/12leaderboard.png
[13]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/13leaderboard.png
[14]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/14leaderboard.png
[15]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/15leaderboard.png
[16]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/16leaderboard.png
[17]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/17leaderboard.png

<!-- URLs. -->

[了解有关 Azure 移动服务的详细信息]: /develop/mobile/resources/
[了解有关 Web API 的详细信息]: http://asp.net/web-api
[处理数据库写入冲突]: mobile-services-windows-store-dotnet-handle-database-conflicts.md
[添加推式通知]: ../notification-hubs-windows-store-dotnet-get-started.md
[开始使用身份验证]: /develop/mobile/tutorials/get-started-with-users-dotnet

 
测试
