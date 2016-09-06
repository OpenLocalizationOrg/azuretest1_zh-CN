---
ms.openlocfilehash: 86f016d3778bfa1b518ce86a9f30ea127bbec5fc
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用表格存储 (Node.js) 的 web 应用程序 |Microsoft Azure" 
    description="通过添加 Azure 存储服务和 Azure 模块建立速成教程与 Web 应用程序的教程。" 
    services="cloud-services, storage" 
    documentationCenter="nodejs" 
    authors="TomArcher" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="09/01/2015" 
    ms.author="tarcher"/>






# 使用存储的 Node.js Web 应用程序

## 概述

在本教程中，您将扩展通过 Node.js Microsoft Azure 客户端库来处理数据管理服务的[使用速成版的 Node.js Web 应用程序]教程中创建的应用程序。 您将扩展您的应用程序创建一个基于 web 的任务列表应用程序，您可以将它们部署到 Azure。 任务列表允许用户检索任务、 添加新任务，并将任务标记为已完成。

在 Azure 存储中存储的任务项。 Azure 存储提供了容错和高可用性的非结构化的数据存储。 Azure 存储包含几个数据结构，您可以存储和访问数据，并且您可以利用从 Api Node.js 或通过 REST Api Azure SDK 中包含的存储服务。 有关详细信息，请参阅[存储和 Azure 中访问数据]。

本教程假设您已经完成了[Node.js Web 应用程序]和[快速与 Node.js][Node.js Web 应用程序的使用速成]教程。

您将了解︰

-   如何处理 Jade 模板引擎
-   如何使用 Azure 数据管理服务

下面是完整的应用程序的屏幕快照︰

![在 internet explorer 中已完成的 web 页](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## 在 Web.Config 中设置存储凭据

若要访问 Azure 存储，您需要在存储凭据传递。 若要执行此操作，您可以使用 web.config 应用程序设置。
这些设置将作为传递环境变量到节点上，然后读取的 Azure SDK 的。

> [AZURE.NOTE] 存储凭据只用于在应用程序部署到 Azure。 仿真程序中运行时，应用程序将使用存储仿真器。

执行以下步骤来检索存储帐户凭据并将它们添加到 web.config 设置︰

1.  如果它已不打开，通过展开**所有程序，Azure**从**「 开始 」**菜单启动 Azure PowerShell、 **Azure PowerShell**，用鼠标右键单击，然后选择**以管理员身份运行**。

2.  将目录更改为包含您的应用程序的文件夹。 例如，c:\\节点\\任务列表\\WebRole1。

3.  从 Azure Powershell 窗口中输入以下 cmdlet 检索存储帐户信息︰

        PS C:\node\tasklist\WebRole1> Get-AzureStorageAccounts

    这将检索存储帐户和帐户与您托管服务相关联的键的列表。

    > [AZURE.NOTE] 由于 Azure SDK 创建存储帐户，当您部署服务，存储帐户应该存在从以前的指南中将应用程序部署。

4.  打开包含在应用程序部署到 Azure 时使用的环境设置的**ServiceDefinition.csdef**文件︰

        PS C:\node\tasklist> notepad ServiceDefinition.csdef

5.  插入下面的块下**环境**元素，替换 {存储帐户} 和 {存储访问键} 与帐户名称和您要用于部署的存储帐户的主键︰

        <Variable name="AZURE_STORAGE_ACCOUNT" value="{STORAGE ACCOUNT}" />
        <Variable name="AZURE_STORAGE_ACCESS_KEY" value="{STORAGE ACCESS KEY}" />

    ![Web.cloud.config 文件的内容](./media/storage-nodejs-use-table-storage-cloud-service-app/node37.png)

6.  保存文件并关闭记事本。

### 安装附加模块

2. 使用下面的命令来安装 [azure] [节点的 uuid] [nconf] 和 [异步] 模块以及本地保存一项为他们保存的**package.json**文件︰

        PS C:\node\tasklist\WebRole1> npm install azure-storage node-uuid async nconf --save

    此命令的输出应类似于以下内容︰

        node-uuid@1.4.1 node_modules\node-uuid

        nconf@0.6.9 node_modules\nconf
        ├── ini@1.1.0
        ├── async@0.2.9
        └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.8)

        azure-storage@0.1.0 node_modules\azure-storage
        ├── extend@1.2.1
        ├── xmlbuilder@0.4.3
        ├── mime@1.2.11
        ├── underscore@1.4.4
        ├── validator@3.1.0
        ├── node-uuid@1.4.1
        ├── xml2js@0.2.7 (sax@0.5.2)
        └── request@2.27.0 (json-stringify-safe@5.0.0, tunnel-agent@0.3.0, aws-sign@0.3.0, forever-agent@0.5.2, qs@0.6.6, oauth-sign@0.3.0, cookie-jar@0.3.0, hawk@1.0.0, form-data@0.1.3, http-signature@0.10.0)

##在一个节点的应用程序中使用表服务

在本节中，您将扩展**明示**命令由添加一个**task.js**文件，其中包含为任务模型的基本应用程序。 此外将修改现有的**app.js** ，并创建一个新的**tasklist.js**文件使用的模型。

### 创建模型

1. 在**WebRole1**目录中，创建一个名为**模型**的新目录。

2. 在**模型**目录中，创建一个名为**task.js**的新文件。 此文件将包含您的应用程序创建的任务模型。

3. **Task.js**文件的开头添加以下代码，以引用所需的库︰

        var azure = require('azure-storage');
        var uuid = require('node-uuid');
        var entityGen = azure.TableUtilities.entityGenerator;

4. 接下来，您将添加代码以定义和导出任务对象。 此对象负责连接的表。

        module.exports = Task;

        function Task(storageClient, tableName, partitionKey) {
          this.storageClient = storageClient;
          this.tableName = tableName;
          this.partitionKey = partitionKey;
          this.storageClient.createTableIfNotExists(tableName, function tableCreated(error) {
            if(error) {
              throw error;
            }
          });
        };

5. 接下来，添加以下代码以定义其他方法对任务的对象允许在表中存储的数据的交互︰

        Task.prototype = {
          find: function(query, callback) {
            self = this;
            self.storageClient.queryEntities(query, function entitiesQueried(error, result) {
              if(error) {
                callback(error);
              } else {
                callback(null, result.entries);
              }
            });
          },

          addItem: function(item, callback) {
            self = this;
            // use entityGenerator to set types
            // NOTE: RowKey must be a string type, even though
            // it contains a GUID in this example.
            var itemDescriptor = {
              PartitionKey: entityGen.String(self.partitionKey),
              RowKey: entityGen.String(uuid()),
              name: entityGen.String(item.name),
              category: entityGen.String(item.category),
              completed: entityGen.Boolean(false)
            };

            self.storageClient.insertEntity(self.tableName, itemDescriptor, function entityInserted(error) {
              if(error){  
                callback(error);
              }
              callback(null);
            });
          },

          updateItem: function(rKey, callback) {
            self = this;
            self.storageClient.retrieveEntity(self.tableName, self.partitionKey, rKey, function entityQueried(error, entity) {
              if(error) {
                callback(error);
              }
              entity.completed._ = true;
              self.storageClient.updateEntity(self.tableName, entity, function entityUpdated(error) {
                if(error) {
                  callback(error);
                }
                callback(null);
              });
            });
          }
        }

6. 保存并关闭**task.js**文件。

### 创建控制器

1. 在**WebRole1 中的路由**目录中，创建一个名为**tasklist.js**的新文件，在文本编辑器中打开它。

2. 将以下代码添加到**tasklist.js**中。 这样会加载 azure 和异步模块，由**tasklist.js**。 这还将定义**任务列表**功能，传递我们前面定义的**任务**对象的实例︰

        var azure = require('azure-storage');
        var async = require('async');

        module.exports = TaskList;

        function TaskList(task) {
          this.task = task;
        }

2. 继续添加到**tasklist.js**文件添加到**showTasks**、 **addTask**和**completeTasks**所使用的方法︰

        TaskList.prototype = {
          showTasks: function(req, res) {
            self = this;
            var query = azure.TableQuery()
              .where('completed eq ?', false);
            self.task.find(query, function itemsFound(error, items) {
              res.render('index',{title: 'My ToDo List ', tasks: items});
            });
          },

          addTask: function(req,res) {
            var self = this      
            var item = req.body.item;
            self.task.addItem(item, function itemAdded(error) {
              if(error) {
                throw error;
              }
              res.redirect('/');
            });
          },

          completeTask: function(req,res) {
            var self = this;
            var completedTasks = Object.keys(req.body);
            async.forEach(completedTasks, function taskIterator(completedTask, callback) {
              self.task.updateItem(completedTask, function itemsUpdated(error) {
                if(error){
                  callback(error);
                } else {
                  callback(null);
                }
              });
            }, function goHome(error){
              if(error) {
                throw error;
              } else {
               res.redirect('/');
              }
            });
          }
        }

3. 将**tasklist.js**文件保存。

### 修改 app.js

1. 在**WebRole1**目录中，打开一个文本编辑器中的**app.js**文件。 

2. 该文件的开头添加以下项加载 azure 的模块并将表名和分区键设置︰

        var azure = require('azure-storage');
        var tableName = 'tasks';
        var partitionKey = 'hometasks';

3. 在 app.js 文件中，向下滚动位置看到以下行︰

        app.use('/', routes);
        app.use('/users', users);

    下面显示的代码替换上面的行。 这将初始化的实例<strong>任务</strong>连接到您的存储帐户。 这传递给<strong>任务列表</strong>，它将用它来与表服务进行通信︰

        var TaskList = require('./routes/tasklist');
        var Task = require('./models/task');
        var task = new Task(azure.createTableService(), tableName, partitionKey);
        var taskList = new TaskList(task);

        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
    
4. 将**app.js**文件保存。

### 修改索引视图

1. 将目录更改到目录**视图**和文本编辑器中打开**index.jade**文件。

2. 下面的代码替换**index.jade**文件的内容。 这将定义视图，以显示现有任务，以及用于添加新任务，将现有的标记为已完成的表单。

        extends layout

        block content
          h1= title
          br
        
          form(action="/completetask", method="post")
            table.table.table-striped.table-bordered
              tr
                td Name
                td Category
                td Date
                td Complete
              if tasks != []
                tr
                  td 
              else
                each task in tasks
                  tr
                    td #{task.name._}
                    td #{task.category._}
                    - var day   = task.Timestamp._.getDate();
                    - var month = task.Timestamp._.getMonth() + 1;
                    - var year  = task.Timestamp._.getFullYear();
                    td #{month + "/" + day + "/" + year}
                    td
                      input(type="checkbox", name="#{task.RowKey._}", value="#{!task.completed._}", checked=task.completed._)
            button.btn(type="submit") Update tasks
          hr
          form.well(action="/addtask", method="post")
            label Item Name: 
            input(name="item[name]", type="textbox")
            label Item Category: 
            input(name="item[category]", type="textbox")
            br
            button.btn(type="submit") Add item

3. 保存并关闭**index.jade**文件。

### 修改全局布局

在**视图**目录中的**layout.jade**文件用作其他**.jade**文件共用模板。 在此步骤中，您将修改即可使用[使用 Twitter 的引导](https://github.com/twbs/bootstrap)，这是一个工具包，可以轻松地设计好外观网站。

1. 下载并解压缩这些文件[使用 Twitter，引导](http://getbootstrap.com/)的。 将**bootstrap.min.css**文件从复制**引导\\dist\\css**文件夹**公用\\样式表**任务列表应用程序的目录。

2. 从 [**视图**] 文件夹，在您的文本编辑器中打开**layout.jade**和内容替换为以下︰

        doctype html
        html
          head
            title= title
            link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')
            link(rel='stylesheet', href='/stylesheets/style.css')
          body.app
            nav.navbar.navbar-default
              div.navbar-header
                a.navbar-brand(href='/') My Tasks
            block content

3. 将**layout.jade**文件保存。

### 在模拟器中运行应用程序

使用下面的命令来启动该应用程序在仿真程序中。

    PS C:\node\tasklist\WebRole1> start-azureemulator -launch

浏览器将打开，并显示以下页面︰

![Web 页面的标题我的任务列表带有表包含任务和字段来添加新任务。](./media/storage-nodejs-use-table-storage-cloud-service-app/node44.png)

使用窗体中添加项目，或将其标记为已完成移除现有项。

## 发布到 Azure 应用程序


在 Windows PowerShell 窗口调用以下 cmdlet 重新部署到 Azure 托管的服务。

    PS C:\node\tasklist\WebRole1> Publish-AzureServiceProject -name myuniquename -location datacentername -launch

此应用程序的唯一名称替换**myuniquename** 。 Azure 数据中心，如**美国西部**的名称替换**datacentername** 。

完成部署后，您应看到类似下面的响应︰

    PS C:\node\tasklist> publish-azureserviceproject -servicename tasklist -location "West US"
    WARNING: Publishing tasklist to Microsoft Azure. This may take several minutes...
    WARNING: 2:18:42 PM - Preparing runtime deployment for service 'tasklist'
    WARNING: 2:18:42 PM - Verifying storage account 'tasklist'...
    WARNING: 2:18:43 PM - Preparing deployment for tasklist with Subscription ID: 65a1016d-0f67-45d2-b838-b8f373d6d52e...
    WARNING: 2:19:01 PM - Connecting...
    WARNING: 2:19:02 PM - Uploading Package to storage service larrystore...
    WARNING: 2:19:40 PM - Upgrading...
    WARNING: 2:22:48 PM - Created Deployment ID: b7134ab29b1249ff84ada2bd157f296a.
    WARNING: 2:22:48 PM - Initializing...
    WARNING: 2:22:49 PM - Instance WebRole1_IN_0 of role WebRole1 is ready.
    WARNING: 2:22:50 PM - Created Website URL: http://tasklist.cloudapp.net/.

像以前一样，因为指定的**-启动**选项，浏览器将打开，并显示应用程序运行在 Azure 发布完成后。

![浏览器窗口，显示了我的任务列表的页面。 该 URL 指示页面现在位于 Azure。](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## 停止和删除应用程序

在部署您的应用程序之后, 可能要禁用它，以便您可以避免成本或生成和部署的免费试用版的时间段内的其他应用程序。

Azure 帐单的 web 角色实例的服务器消耗的时间每小时。
一旦部署您的应用程序，即使这些实例未运行且处于停止状态时才占用服务器的时间。

以下步骤演示如何停止并删除您的应用程序。

1.  在 Windows PowerShell 窗口停止以下 cmdlet 使用上一节中创建的部署服务︰

        PS C:\node\tasklist\WebRole1> Stop-AzureService

    停止该服务，则可能需要几分钟。 停止该服务后，您会收到一条消息，指示它已停止。

3.  若要删除该服务，请调用以下 cmdlet:

        PS C:\node\tasklist\WebRole1> Remove-AzureService contosotasklist

    出现提示时，输入**Y**以删除该服务。

    删除该服务可能需要几分钟的时间。 已删除该服务后您将收到一条消息，表明该服务已删除。

  [使用快速的 Node.js Web 应用程序]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/
  [存储和访问 Azure 中的数据]: http://msdn.microsoft.com/library/azure/gg433040.aspx
  [Node.js Web 应用程序]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/
 
 

测试
