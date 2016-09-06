---
ms.openlocfilehash: 67673fb2613aba8707c33bf5e0a4f9cd2bfb63b4
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="构建实时应用程序与 Pusher (iOS) 的移动服务"
    description="了解如何使用 Pusher 来将通知发送到 iOS 在 Azure 媒体服务应用程序。"
    services="mobile-services"
    documentationCenter="ios"
    authors="lindydonna"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/16/2015" 
    ms.author="donnam"/>


# 构建与移动服务和 Pusher 的实时应用程序

本主题演示如何可以添加到您的基于 Azure 移动服务的应用程序的实时功能。 当完成时，您应做事项列表数据同步，在实时，跨所有应用程序的运行实例。

[推式通知给用户][]教程演示了如何使用推式通知来通知用户新的项目，在 Todo 列表。 推式通知是显示偶尔发生变化的好方法。 但是，有时候应用程序需要频繁的实时通知。 可以将实时通知添加到您使用 Pusher API 的移动服务。 在本教程中，我们使用 Pusher 移动服务保持同步任何正在运行的应用程序实例中进行更改时的 Todo 列表。

Pusher 是一种基于云的服务，如移动服务，使得构建实时应用程序非常容易。 您可以使用 Pusher 来快速构建实时轮询、 聊天室、 多人游戏、 协作应用程序广播实时数据和内容，就是刚刚开始 ！ 有关详细信息，请参阅[http://pusher.com](http://pusher.com)。

本教程将引导您完成以下基本步骤来添加 Todo 列表应用程序的实时协作︰

1. [创建一个 Pusher 帐户][]
2. [更新您的应用程序][]
3. [安装服务器脚本][]
4. [测试您的应用程序][]

本教程基于移动服务快速入门。 在开始本教程之前，您必须首先完成[开始使用移动服务][]。

## <a name="sign-up"></a>创建一个新的 Pusher 帐户

[AZURE.INCLUDE [pusher-sign-up](../../includes/pusher-sign-up.md)]

## <a name="update-app"></a>更新您的应用程序

现在，您已经设置了您的 Pusher 帐户下, 一步是修改新功能的 iOS 应用程序代码。

###安装 libPusher 库

[LibPusher][]库使您从 iOS 访问 Pusher。

1. 下载 libPusher 库[从这里][libPusherDownload]。

2. 创建一个名为_libPusher_在您的项目组。

3. 在查找程序，解压缩已下载的 zip 文件选择**libPusher combined.a**和**/headers**文件夹中，然后将这些项目拖到项目中的**libPusher**组。

4. 请检查**复制项目到目标组的文件夹**，然后单击**完成**

    ![][add-files-to-group]

   这会将 libPusher 文件复制到您的项目。

5. 在项目资源管理器中的项目根，单击**生成阶段**，然后单击**添加生成阶段**和**添加复制文件**。

6. 从新生成阶段到项目资源管理器拖动的**libPusher combined.a**文件。

7. **目标**更改为**框架**，然后单击**复制安装时只**。

    ![][add-build-phase]

8. **链接二进制与库**区内添加以下库︰

    - libicucore.dylib
    - CFNetwork.framework
    - Security.framework
    - SystemConfiguration.framework

9. 最后在**生成设置**，找到目标生成设置**其他链接器标志**并添加**-all_load**标志。

    ![][add-linker-flag]

    这将显示**-all_load**标志设置为调试生成目标。

该库现已安装准备就绪。

### 将代码添加到应用程序

1. 在 Xcode，打开**QSTodoService.h**文件，并添加下面的方法声明︰

        // Allows retrieval of items by id
        - (NSUInteger) getItemIndex:(NSDictionary *)item;

        // To be called when items are added by other users
        - (NSUInteger) itemAdded:(NSDictionary *)item;

        // To be called when items are completed by other users
        - (NSUInteger) itemCompleted:(NSDictionary *)item;

2. 用以下内容替换现有**addItem**和**completeItem**的声明︰

        - (void) addItem:(NSDictionary *) item;
        - (void) completeItem: (NSDictionary *) item;

3. 在**QSTodoService.m**中，添加以下代码以实现的新方法︰

        // Allows retrieval of items by id
        - (NSUInteger) getItemIndex:(NSDictionary *)item
        {
            NSInteger itemId = [[item objectForKey: @"id"] integerValue];

            return [items indexOfObjectPassingTest:^BOOL(id currItem, NSUInteger idx, BOOL *stop)
                 {
                     return ([[currItem objectForKey: @"id"] integerValue] == itemId);
                 }];
        }

        // To be called when items are added by other users
        -(NSUInteger) itemAdded:(NSDictionary *)item
        {
            NSUInteger index = [self getItemIndex:item];

            // Only complete action if item not already in list
            if(index == NSNotFound)
            {
                NSUInteger newIndex = [items count];
                [(NSMutableArray *)items insertObject:item atIndex:newIndex];
                return newIndex;
            }
            else
                return -1;
        }

        // To be called when items are completed by other users
        - (NSUInteger) itemCompleted:(NSDictionary *)item
        {
            NSUInteger index = [self getItemIndex:item];

            // Only complete action if item exists in items list
            if(index != NSNotFound)
            {
                NSMutableArray *mutableItems = (NSMutableArray *) items;
                [mutableItems removeObjectAtIndex:index];
            }
            return index;
        }

    QSTodoService 现在允许您按**id**查找项并添加和本地的情况下将显式的请求发送到远程服务完成的项目。

4. 现有**addItem**和**completeItem**方法替换为以下代码︰

        -(void) addItem:(NSDictionary *)item
        {
            // Insert the item into the TodoItem table and add to the items array on completion
            [self.table insert:item completion:^(NSDictionary *result, NSError *error) {
                [self logErrorIfNotNil:error];
            }];
        }

        -(void) completeItem:(NSDictionary *)item
        {
            // Set the item to be complete (we need a mutable copy)
            NSMutableDictionary *mutable = [item mutableCopy];
            [mutable setObject:@(YES) forKey:@"complete"];

            // Update the item in the TodoItem table and remove from the items array on completion
            [self.table update:mutable completion:^(NSDictionary *item, NSError *error) {
                [self logErrorIfNotNil:error];
            }];
        }


    请注意项现在添加和完成，以及更新的用户界面，而不是更新数据表格时收到从 Pusher 事件时。

5. 在**QSTodoListViewController.h**文件中，添加下面的导入语句︰

        #import "PTPusherDelegate.h"
        #import "PTPusher.h"
        #import "PTPusherEvent.h"
        #import "PTPusherChannel.h"

6. 修改接口声明添加**PTPusherDelegate** ，如下所示︰

        @interface QSTodoListViewController : UITableViewController<UITextFieldDelegate, PTPusherDelegate>

7. 添加以下新的属性︰

        @property (nonatomic, strong) PTPusher *pusher;

8. 添加下面的代码声明一个新的方法︰

        // Sets up the Pusher client
        - (void) setupPusher;

9. 在**QSTodoListViewController.m**中，添加以下行另**@synthesise**线实现新属性︰

        @synthesize pusher = _pusher;

10. 现在，添加以下代码以实现的新方法︰

        // Sets up the Pusher client
        - (void) setupPusher {

            // Create a Pusher client, using your Pusher app key as the credential
            // TODO: Move Pusher app key to configuration file
            self.pusher = [PTPusher pusherWithKey:@"**your_app_key**" delegate:self encrypted:NO];
            self.pusher.reconnectAutomatically = YES;

            // Subscribe to the 'todo-updates' channel
            PTPusherChannel *todoChannel = [self.pusher subscribeToChannelNamed:@"todo-updates"];

            // Bind to the 'todo-added' event
            [todoChannel bindToEventNamed:@"todo-added" handleWithBlock:^(PTPusherEvent *channelEvent) {

                // Add item to the todo list
                NSUInteger index = [self.todoService itemAdded:channelEvent.data];

                // If the item was not already in the list, add the item to the UI
                if(index != -1)
                {
                    NSIndexPath *indexPath = [NSIndexPath indexPathForRow:index inSection:0];
                    [self.tableView insertRowsAtIndexPaths:@[ indexPath ]
                                  withRowAnimation:UITableViewRowAnimationTop];
                }
            }];

            // Bind to the 'todo-completed' event
            [todoChannel bindToEventNamed:@"todo-completed" handleWithBlock:^(PTPusherEvent *channelEvent) {

                // Update the item to be completed
                NSUInteger index = [self.todoService itemCompleted:channelEvent.data];

                // As long as the item did exit in the list, update the UI
                if(index != NSNotFound)
                {
                    NSIndexPath *indexPath = [NSIndexPath indexPathForRow:index inSection:0];
                    [self.tableView deleteRowsAtIndexPaths:@[ indexPath ]
                                  withRowAnimation:UITableViewRowAnimationTop];
                }
            }];
        }

11. 更换`**your_app_key**`app_key 值的占位符连接信息对话框中从先前复制。

12. **OnAdd**方法替换为以下代码︰

        - (IBAction)onAdd:(id)sender
        {
            if (itemText.text.length  == 0) {
                return;
            }

            NSDictionary *item = @{ @"text" : itemText.text, @"complete" : @(NO) };
            [self.todoService addItem:item];

            itemText.text = @"";
        }

13. 在**QSTodoListViewController.m**文件中，找到 (void) viewDidLoad 方法并添加对**setupPusher**方法的调用，这样的前几行是︰

        - (void)viewDidLoad
        {
            [super viewDidLoad];
            [self setupPusher];

14. 在**tableView:commitEditingStyle:forRowAtIndexPath**方法的末尾，用下面的代码替换对**completeItem**的调用︰

        // Ask the todoService to set the item's complete value to YES
        [self.todoService completeItem:item];

应用程序现已能够从 Pusher，接收事件并相应地更新本地 Todo 列表。



##<a name="install-scripts"></a>安装服务器脚本



剩下的工作建立您服务器的脚本。 插入或更新到任务列表表项目时，我们将插入的脚本。



1. 登录到[Azure 管理门户网站]上，单击**移动服务**，然后单击移动服务。


2. 在管理门户中，单击**数据**选项卡，然后单击**TodoItem**表。

    ![][1]



3. 在**TodoItem**，单击**脚本**选项卡，然后选择**插入**。


    ![][2]



    这将显示插入发生**TodoItem**表中时将调用该函数。


4. 插入函数替换为以下代码︰


        var Pusher = require('pusher');

        function insert(item, user, request) {

            request.execute({
                success: function() {
                    // After the record has been inserted, trigger immediately to the client
                    request.respond();

                    // Publish event for all other active clients
                    publishItemCreatedEvent(item);
                }
            });

            function publishItemCreatedEvent(item) {

                // Ideally these settings would be taken from config
                var pusher = new Pusher({
                  appId: '**your_app_id**',
                  key: '**your_app_key**',
                  secret: '**your_app_secret**'
                });

                // Publish event on Pusher channel
                pusher.trigger( 'todo-updates', 'todo-added', item );
            }
        }



5. 上面的脚本中的占位符替换为先前复制从连接信息对话框中的值︰

    - **`**your_app_id**`**︰ 应用程序 & #95; id 值
    - **`**your_app_key**`**︰ 应用程序 & #95; 键值
    - **`**your_app_key_secret**`**︰ 应用程序 & #95; 键 & #95; 秘密


6. 单击**保存**按钮。 您现在已经配置脚本以将事件发布到 Pusher，每次**TodoItem**表中插入一个新项。


7. 从**操作**下拉列表中选择**更新**。


8. 更新函数替换为以下代码︰

        var Pusher = require('pusher');

        function update(item, user, request) {

            request.execute({
                success: function() {
                    // After the record has been updated, trigger immediately to the client
                    request.respond();

                    // Publish event for all other active clients
                    publishItemUpdatedEvent(item);
                }
            });

            function publishItemUpdatedEvent(item) {

                // Ideally these settings would be taken from config
                var pusher = new Pusher({
                  appId: '**your_app_id**',
                  key: '**your_app_key**',
                  secret: '**your_app_secret**'
                });

                // Publish event on Pusher channel
                pusher.trigger( 'todo-updates', 'todo-completed', item );

            }
        }



9. 重复步骤 5，此脚本来替换占位符。


10. 单击**保存**按钮。 您现在已经配置脚本以将事件发布到 Pusher，每次更新新项目时。



##<a name="test-app"></a>测试您的应用程序



若要测试该应用程序将需要运行两个实例。 您可以在 iOS 设备，另一个在 iOS 模拟器上运行一个实例。

1. IOS 设备连接，请按**运行**按钮 （或命令 + R 键） 来启动该应用程序在设备上，然后停止调试。

    现在，您有您的应用程序安装在您的设备上。

2. 运行该应用程序在 iOS 模拟器，并在同一时间开始应用程序在 iOS 设备上。

    现在，您有两个运行的应用程序。

3. 在一个应用程序实例中添加新的 Todo 项。

    验证所添加的项出现在另一个实例。

4. 检查一个 Todo 项标记为一个应用程序实例中完成。

    验证项目从其他实例就会消失。

祝贺您，您已经成功地配置了您的移动服务应用程序可跨所有客户端实时同步。

## <a name="nextsteps"> </a>下一步行动

现在，您已看到多么容易它是使用 Pusher 服务具有移动服务，请按照这些链接以了解更多关于 Pusher。

-   Pusher API 文档︰ <http://pusher.com/docs>
-   Pusher 教程︰ <http://pusher.com/tutorials>

若要了解有关注册和使用服务器脚本的详细信息，请参阅[移动服务服务器脚本引用]。

<!-- Anchors. -->
[创建一个 Pusher 帐户]: #sign-up
[更新您的应用程序]: #update-app
[安装服务器脚本]: #install-scripts
[测试您的应用程序]: #test-app

<!-- Images. -->
[1]: ./media/mobile-services-ios-build-realtime-apps-pusher/mobile-portal-data-tables.png
[2]: ./media/mobile-services-ios-build-realtime-apps-pusher/mobile-insert-script-push2.png

[添加文件-与组]: ./media/mobile-services-ios-build-realtime-apps-pusher/pusher-ios-add-files-to-group.png
[添加生成阶段]: ./media/mobile-services-ios-build-realtime-apps-pusher/pusher-ios-add-build-phase.png
[添加链接器标志]: ./media/mobile-services-ios-build-realtime-apps-pusher/pusher-ios-add-linker-flag.png

<!-- URLs. -->
[推式通知给用户]: /develop/mobile/tutorials/push-notifications-to-users-ios
[开始使用移动服务]: /develop/mobile/tutorials/get-started
[libPusher]: http://go.microsoft.com/fwlink/p?LinkId=276999
[libPusherDownload]: http://go.microsoft.com/fwlink/p/?LinkId=276998


[Azure 的管理门户]: https://manage.windowsazure.com/

[移动服务服务器脚本引用]: http://go.microsoft.com/fwlink/p/?LinkId=262293

测试
