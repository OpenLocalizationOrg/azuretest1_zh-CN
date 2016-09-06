---
ms.openlocfilehash: c589ad68b4159a965b61619116111e5b54439571
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="教程︰ 创建管线与使用 Visual Studio 的复制活动" 
    description="在本教程中，将与复制活动创建 Azure 数据工厂管线，通过使用 Visual Studio。" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/27/2015" 
    ms.author="spelluru"/>

# 教程︰ 创建并监控使用 Visual Studio 的数据工厂
> [AZURE.SELECTOR]
- [教程概述](data-factory-get-started.md)
- [使用数据工厂编辑器](data-factory-get-started-using-editor.md)
- [使用 PowerShell](data-factory-monitor-manage-using-powershell.md)
- [使用 Visual Studio](data-factory-get-started-using-vs.md)


##在本教程中
在本教程中将首先创建名为**ADFTutorialDataFactoryVS** ，使用 Azure 预览门户 Azure 数据工厂，然后执行以下操作使用 Visual Studio 2013年:

1. 创建两个链接的服务︰ **AzureStorageLinkedService1**和**AzureSqlinkedService1**。 AzureStorageLinkedService1 链接 Azure 存储和 AzureSqlLinkedService1 链接到数据工厂的 SQL Azure 数据库︰ **ADFTutorialDataFactoryVS**。 管道的输入的数据驻留在 Azure blob 存储 blob 容器中，输出数据将存储在 SQL Azure 数据库中的表。 因此，您将添加下列两个数据存储为链接服务到数据工厂。
2. 创建两个数据表工厂︰ **EmpTableFromBlob**和**EmpSQLTable**，这表示数据存储区中存储的输入/输出数据。 EmpTableFromBlob，您需要指定包含 blob 与源数据和 EmpSQLTable 的 blob 容器，您将指定的 SQL 表中将存储输出的数据。 您还需要指定其他属性，如数据结构、 数据等的可用性。.
3. 创建名为**ADFTutorialPipeline**的 ADFTutorialDataFactoryVS 中的管道。 管道必须将输入数据从 Azure 斑点到输出 SQL Azure 表**复制活动** 


## <a name="CreateDataFactory"></a>创建 Azure 数据工厂
在此步骤中，您可以使用 Azure 预览门户创建名为**ADFTutorialDataFactoryVS**Azure 数据工厂。

1.  之后登录到[Azure 预览门户](http://portal.azure.com)，从左下角中单击**新建**，选择在**创建**刀片式服务器，**数据分析**和单击**数据分析**刀片式服务器中的**数据工厂**。 

    ![新-> DataFactory](./media/data-factory-get-started-using-vs/NewDataFactoryMenu.png)   

6. 在**新数据工厂**刀片式服务器︰
    1. **名称**中输入**ADFTutorialDataFactoryVS** 。 
    
        ![新数据工厂刀片式服务器](./media/data-factory-get-started-using-vs/getstarted-new-data-factory.png)
    2. 单击**资源组的名称**，请执行下列操作︰
        1. 单击**创建新的资源组**。
        2. 在**创建资源组**刀片式服务器， **ADFTutorialResourceGroup**输入资源组的**名称**并单击**确定**。 

            ![创建资源组](./media/data-factory-get-started-using-vs/CreateNewResourceGroup.png)

        部分在本教程中的步骤假定您使用的名称︰ **ADFTutorialResourceGroup**资源组。 若要了解有关资源组，请参阅[使用资源组管理 Azure 资源](resource-group-overview.md)。  
7. 在**新数据工厂**刀片式服务器，请注意，选择**添加到 Startboard** 。
8. 在**新数据工厂**刀片式服务器，请单击**创建**。

    Azure 数据工厂的名称必须是全局唯一的。 如果您收到错误︰**数据工厂名称"ADFTutorialDataFactoryVS"是不可用**的更改数据工厂 (例如，yournameADFTutorialDataFactoryVS) 的名称，然后尝试重新创建。 在本教程中执行其余步骤时使用此名称来代替 ADFTutorialFactory。 请参阅对数据工厂项目命名规则 [数据工厂的命名规则] [数据-工厂-命名的规则] 主题。  
     
    ![数据工厂名称不可用](./media/data-factory-get-started-using-vs/getstarted-data-factory-not-available.png)

9. 单击左侧的**通知**中心并查看来自创建过程的通知。 单击**X**关闭**通知**刀片式服务器，如果它是打开的。 
10. 完成创建后，您将看到**数据工厂**刀片式服务器，如下所示。

    ![数据工厂主页](./media/data-factory-get-started-using-vs/getstarted-data-factory-home-page.png)

## 创建和部署数据工厂使用 Visual Studio 的实体 

### 必备组件
您必须在您的计算机上安装下列程序︰ 
- Visual Studio 2013 年
- 下载有关 Visual Studio 2013 Azure SDK。 导航到[Azure 下载页](http://azure.microsoft.com/downloads/)，然后单击**VS 2013 安装** **.NET**节。

### 演练

#### 创建 Visual Studio 项目 
1. 启动**Visual Studio 2013年**。 单击**文件**，指向**新建**，单击**项目**。 您应该看到**新建项目**对话框。  
2. 在**新建项目**对话框中，选择**DataFactory**模板，并单击**空数据工厂项目**。 如果您看不到 DataFactory 模板，关闭 Visual Studio 安装 Visual Studio 2013，Azure SDK，重新打开 Visual Studio。  

    ![新建项目对话框](./media/data-factory-get-started-using-vs/new-project-dialog.png)

3. 为项目、**位置**和**解决方案**名称输入一个**名称**，并单击**确定**。

    ![解决方案资源管理器](./media/data-factory-get-started-using-vs/solution-explorer.png)   

#### 创建链接的服务
链接的服务链接的数据存储区或计算到 Azure 数据工厂服务。 Azure 存储、 SQL Azure 数据库或内部的 SQL Server 数据库，可以是一个数据存储区。

在此步骤中，您将创建两个链接的服务︰ **AzureStorageLinkedService1**和**AzureSqlLinkedService1**。 AzureStorageLinkedService1 链接服务链接 Azure 存储帐户和 AzureSqlLinkedService 链接到数据工厂的 SQL Azure 数据库︰ **ADFTutorialDataFactory**。 

##### 创建链接的 Azure 存储服务

4. 在解决方案资源管理器中右击**链接服务**，指向**添加**，并单击**新的项目**。      
5. 在**添加新项**对话框中，从列表中选择**Azure 存储链接服务**并单击**添加**。 

    ![新链接的服务](./media/data-factory-get-started-using-vs/new-linked-service-dialog.png)
 
3. **帐户名**和**accountkey**替换为 Azure 存储帐户和它的键的名称。 

    ![Azure 存储链接服务](./media/data-factory-get-started-using-vs/azure-storage-linked-service.png)

4. 将**AzureStorageLinkedService1.json**文件保存。

#### 创建链接的 Azure SQL 服务

5. 再次右击**解决方案资源管理器**中的**链接服务**节点上，指向**添加**，并单击**新项**。 
6. 这一次，选择**Azure SQL 链接服务**，然后单击**添加**。 
7. 在**AzureSqlLinkedService1.json 文件**中，**服务器名称**、**数据库名称**、 **username@servername**和**密码**名称替换 SQL Azure 服务器、 数据库、 用户帐户和密码。    
8.  将**AzureSqlLinkedService1.json**文件保存。 


### 创建输入和输出表
在前面的步骤中，您创建了链接的服务**AzureStorageLinkedService1**和**AzureSqlLinkedService1**来链接到数据工厂的 Azure 存储帐户和 SQL Azure 数据库︰ **ADFTutorialDataFactory**。 在此步骤中，您将定义两个工厂数据表 （ **EmpTableFromBlob**和**EmpSQLTable** ） 表示通过 AzureStorageLinkedService1 和 AzureSqlLinkedService1 分别引用数据存储区中存储的输入/输出数据。 EmpTableFromBlob，您需要指定包含 blob 与源数据和 EmpSQLTable 的 blob 容器，您将指定的 SQL 表中将存储输出的数据。

#### 创建输入的表

9. 右击**解决方案资源管理器**中的**表**，指向**添加**，并单击**新的项目**。
10. 在**添加新项**对话框中，选择**Azure 斑点**，并单击**添加**。   
10. JSON 文本替换为下面的文本并保存的**AzureBlobLocation1.json**文件。 

        {
            "name": "EmpTableFromBlob",
            "properties":
            {
                "structure":  
                [ 
                    { "name": "FirstName", "type": "String"},
                    { "name": "LastName", "type": "String"}
                ],
                "location": 
                {
                    "type": "AzureBlobLocation",
                    "folderPath": "adftutorial/",
                    "format":
                    {
                        "type": "TextFormat",
                        "columnDelimiter": ","
                    },
                    "linkedServiceName": "AzureStorageLinkedService1"
                },
                "availability": 
                {
                    "frequency": "Hour",
                    "interval": 1,
                    "waitOnExternal": {}
                }
            }
        }

#### 创建输出表

11. 再次右击**解决方案资源管理器**中的**表**，指向**添加**，单击**新建项目**。
12. 在**添加新项**对话框中，选择**SQL Azure**中，并单击**添加**。 
13. JSON 文本替换为下面的 JSON 并保存的**AzureSqlTableLocation1.json**文件。

        {
            "name": "EmpSQLTable",
            "properties":
            {
                "structure":
                [
                    { "name": "FirstName", "type": "String"},
                    { "name": "LastName", "type": "String"}
                ],
                "location":
                {
                    "type": "AzureSqlTableLocation",
                    "tableName": "emp",
                    "linkedServiceName": "AzureSqlLinkedService1"
                },
                "availability": 
                {
                    "frequency": "Hour",
                    "interval": 1            
                }
            }
        }

#### 创建管线 
到目前为止已经创建输入/输出链接的服务和表。 现在，将管线创建要复制的**复制活动**从 Azure 数据 blob 到 SQL Azure 数据库。 


1. **管线**在**解决方案资源管理器**中用鼠标右键单击，指向**添加**，单击**新项**。  
15. 在**添加新项**对话框中选择**复制数据管道**并单击**添加**。 
16. 替换为下面的 JSON JSON，保存的**CopyActivity1.json**文件。.
            
         {
            "name": "ADFTutorialPipeline",
            "properties":
            {   
                "description" : "Copy data from a blob to Azure SQL table",
                "activities":   
                [
                    {
                        "name": "CopyFromBlobToSQL",
                        "description": "Push Regional Effectiveness Campaign data to Azure SQL database",
                        "type": "CopyActivity",
                        "inputs": [ {"name": "EmpTableFromBlob"} ],
                        "outputs": [ {"name": "EmpSQLTable"} ],     
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "BlobSource"
                            },
                            "sink":
                            {
                                "type": "SqlSink",
                                "writeBatchSize": 10000,
                                "writeBatchTimeout": "60:00:00"
                            }   
                        },
                        "Policy":
                        {
                            "concurrency": 1,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 0,
                            "timeout": "01:00:00"
                        }       
                    }
                ],
        
                "start": "2015-07-12T00:00:00Z",
                "end": "2015-07-13T00:00:00Z",
                "isPaused": false
            }
        } 

#### 发布/部署数据工厂实体
  
18. 在工具栏区域中，用鼠标右键单击并选择**数据工厂**以启用数据工厂工具栏，如果未启用。 
19. 在**数据工厂工具栏**中，单击**下拉列表框中**，查看您 Azure 的预订中的所有数据工厂。 如果您看到**登录到 Visual Studio**对话框中︰ 
    20. 输入与您要在其中创建数据工厂，输入**密码**，请单击**签名中**的 Azure 订阅关联的**电子邮件帐户**。
    21. 登录成功后，您应该看到在 Azure 预订中的所有数据工厂。 在本教程中，您将创建新的数据 facotry。       
22. 在下拉列表中，选择**ADFTutorialFactoryVS**，然后单击**发布**按钮来部署/发布链接的服务、 数据集和管道。    

    ![发布按钮](./media/data-factory-get-started-using-vs/publish.png)

23. 您应该看到上面图片中显示的数据工厂任务列表窗口中的发布的状态。 发布已成功完成的确认。

## 使用服务器资源管理器中查看数据工厂实体

1. 在**Visual Studio**中，单击**视图**菜单上，单击**服务器资源管理器**。
2. 在服务器资源管理器窗口中，展开**Azure** ，展开**数据工厂**。 如果看到**登录到 Visual Studio**，输入**帐户**与 Azure 订阅，然后单击**继续**。 输入**密码**，然后单击**登录**。 Visual Studio 将尝试获取有关您的订阅中的所有 Azure 数据工厂信息。 您将看到该操作在**数据工厂任务列表**窗口中的状态。
    ![服务器资源管理器](./media/data-factory-get-started-using-vs/server-explorer.png)
3. 您可以的数据工厂，右键单击，并选择导出到新的项目来创建基于现有的数据工厂 Visual Studio 项目的数据工厂。
    ![VS 项目导出数据工厂](./media/data-factory-get-started-using-vs/export-data-factory-menu.png)  

## Visual Studio 为更新数据工厂工具
若要更新有关 Visual Studio Azure 数据工厂工具，执行下列操作︰

1. 单击**工具**菜单上，选择**扩展和更新**。 
2. 在左窗格中选择**更新程序**，然后选择**Visual Studio 库**。
4. 选择**Visual Studio 的 Azure 数据工厂工具**，然后单击**更新**。 如果看不到此条目，则已经有最新版本的工具。 

您在本教程中创建的说明如何使用 Azure 预览门户来监视管道和数据集，请参阅[监视数据集和管道](data-factory-get-started-using-editor.md/#MonitorDataSetsAndPipeline)。


## 发送反馈
我们真诚欢迎您的反馈意见，在这篇文章。 请花几分钟的时间来提交您的反馈意见通过[电子邮件](mailto:adfdocfeedback@microsoft.com?subject=data-factory-get-started-using-vs.md)。

测试
