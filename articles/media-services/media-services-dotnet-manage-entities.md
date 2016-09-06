---
ms.openlocfilehash: aa61f074a6f8e2f5c41a581bcc2025890f41aa31
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties 
    pageTitle="管理资产和与介质服务.NET SDK 的相关的实体" 
    description="了解如何为.NET 管理资产和与媒体服务 SDK 相关的实体。" 
    authors="juliako" 
    manager="dwrede" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2015"
    ms.author="juliako"/>


#管理资产和与介质服务.NET SDK 的相关的实体


> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-manage-entities.md)
- [REST](media-services-rest-manage-entities.md)


本主题演示如何完成以下介质服务管理任务︰

- 获取资产引用 
- 获取作业引用 
- 列出所有资产 
- 列表的作业和资产 
- 列出所有的访问策略 
- 列出所有定位器 
- 删除资源 
- 删除作业 
- 删除访问策略 

##先决条件 

请参阅[设置您的环境](media-services-set-up-computer.md)

##获取资产引用

频繁的任务是媒体服务中获取的现有资产的引用。 下面的代码示例演示如何获取资产引用服务器上的资产集合从上下文对象，根据资产 id。
下面的代码示例使用 Linq 查询来获取对已有的 IAsset 对象的引用。

    static IAsset GetAsset(string assetId)
    {
        // Use a LINQ Select query to get an asset.
        var assetInstance =
            from a in _context.Assets
            where a.Id == assetId
            select a;
        // Reference the asset as an IAsset.
        IAsset asset = assetInstance.FirstOrDefault();
    
        return asset;
    }

##获取作业引用

当您处理介质服务代码中的任务与工作时，经常需要获取对现有作业基于 id 的引用 下面的代码示例演示如何获取对对象的引用 IJob 作业集合中。
WarningWarning 您可能需要获取作业引用时开始运行时间较长的编码工作，而且需要检查作业状态的线程上。 在这样的情况下，当此方法返回从一个线程，您需要检索到作业的刷新的引用。

    static IJob GetJob(string jobId)
    {
        // Use a Linq select query to get an updated 
        // reference by Id. 
        var jobInstance =
            from j in _context.Jobs
            where j.Id == jobId
            select j;
        // Return the job reference as an Ijob. 
        IJob job = jobInstance.FirstOrDefault();
    
        return job;
    }

##列出所有资产

随着资产存储中有数量的增长，因此最好列出您的资产。 下面的代码示例演示如何循环访问服务器上下文对象上的资产集合。 与每个资产，该代码示例还将其属性值的一些写入控制台。 例如，每个资产可以包含多个媒体文件。 代码示例写出每个资产与相关联的所有文件。



    static void ListAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);
    
        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();
    
        foreach (IAsset asset in _context.Assets)
        {
            // Display the collection of assets.
            builder.AppendLine("");
            builder.AppendLine("******ASSET******");
            builder.AppendLine("Asset ID: " + asset.Id);
            builder.AppendLine("Name: " + asset.Name);
            builder.AppendLine("==============");
            builder.AppendLine("******ASSET FILES******");
    
            // Display the files associated with each asset. 
            foreach (IAssetFile fileItem in asset.AssetFiles)
            {
                builder.AppendLine("Name: " + fileItem.Name);
                builder.AppendLine("Size: " + fileItem.ContentFileSize);
                builder.AppendLine("==============");
            }
        }
    
        // Display output in console.
        Console.Write(builder.ToString());
    }

##列表的作业和资产

与媒体服务及其关联的作业的列表资产是一项重要相关的任务。 下面的代码示例演示如何列出每个 IJob 对象，然后为每个作业，它将显示有关该作业的属性，所有相关的任务，所有输入和输出的所有资产。 在此示例中的代码可用于许多其他任务。 例如，如果您想要列出从先前运行的一个或多个编码作业的输出资产，此代码将演示如何访问输出资产。 参考输出资产后，您可以再提供的内容给其他用户或应用程序下载，或提供的 Url。 

提供资产的选项的详细信息，请参见[.NET 媒体服务 sdk 提供的资产](media-services-deliver-streaming-content.md)。

    // List all jobs on the server, and for each job, also list 
    // all tasks, all input assets, all output assets.

    static void ListJobsAndAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);
    
        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();
    
        foreach (IJob job in _context.Jobs)
        {
            // Display the collection of jobs on the server.
            builder.AppendLine("");
            builder.AppendLine("******JOB*******");
            builder.AppendLine("Job ID: " + job.Id);
            builder.AppendLine("Name: " + job.Name);
            builder.AppendLine("State: " + job.State);
            builder.AppendLine("Order: " + job.Priority);
            builder.AppendLine("==============");
    
    
            // For each job, display the associated tasks (a job  
            // has one or more tasks). 
            builder.AppendLine("******TASKS*******");
            foreach (ITask task in job.Tasks)
            {
                builder.AppendLine("Task Id: " + task.Id);
                builder.AppendLine("Name: " + task.Name);
                builder.AppendLine("Progress: " + task.Progress);
                builder.AppendLine("Configuration: " + task.Configuration);
                if (task.ErrorDetails != null)
                {
                    builder.AppendLine("Error: " + task.ErrorDetails);
                }
                builder.AppendLine("==============");
            }
    
            // For each job, display the list of input media assets.
            builder.AppendLine("******JOB INPUT MEDIA ASSETS*******");
            foreach (IAsset inputAsset in job.InputMediaAssets)
            {
    
                if (inputAsset != null)
                {
                    builder.AppendLine("Input Asset Id: " + inputAsset.Id);
                    builder.AppendLine("Name: " + inputAsset.Name);
                    builder.AppendLine("==============");
                }
            }
    
            // For each job, display the list of output media assets.
            builder.AppendLine("******JOB OUTPUT MEDIA ASSETS*******");
            foreach (IAsset theAsset in job.OutputMediaAssets)
            {
                if (theAsset != null)
                {
                    builder.AppendLine("Output Asset Id: " + theAsset.Id);
                    builder.AppendLine("Name: " + theAsset.Name);
                    builder.AppendLine("==============");
                }
            }
    
        }
    
        // Display output in console.
        Console.Write(builder.ToString());
    }

##列出所有的访问策略

媒体服务中，您可以定义策略访问有关某项资产或其文件。 访问策略定义文件或某项资产 （哪些类型的访问和持续时间） 的权限。 在媒体服务代码，通常通过创建 IAccessPolicy 对象，然后将其与现有资产关联定义访问策略。 然后您将创建一个 ILocator 对象，它可以提供对介质服务中的资产的直接访问。 伴随本文档系列的 Visual Studio 项目包含多个代码示例，说明如何创建和访问策略和定位器给资产。

下面的代码示例演示如何列出服务器上的所有访问策略，并显示与每个相关联的权限的类型。 列出所有的 ILocator 对象在服务器上，就是另一种有用的方法以查看访问策略，然后对于每个定位器，您可以列出其关联的访问策略使用的 AccessPolicy 属性。

    static void ListAllPolicies()
    {
        foreach (IAccessPolicy policy in _context.AccessPolicies)
        {
            Console.WriteLine("");
            Console.WriteLine("Name:  " + policy.Name);
            Console.WriteLine("ID:  " + policy.Id);
            Console.WriteLine("Permissions: " + policy.Permissions);
            Console.WriteLine("==============");
    
        }
    }

##列出所有定位器

定位程序是提供直接的路径来访问资产，以及资产权的定位程序关联的访问策略所定义的 URL。 每个资产可以有 ILocator 上的定位器属性与其关联的对象的集合。 将服务器上下文还有一个定位器集合包含所有的定位器。

下面的代码示例列出了服务器上的所有定位器。 每个定位程序，它显示的 Id 相关的资产和访问策略。 它还显示资产的权限、 到期日期和完整路径的类型。

请注意，资产定位器路径仅资产的基 URL。 以创建对单独文件的用户或应用程序可以浏览到直接的路径，您的代码必须将特定的文件路径添加到定位路径。 有关如何执行此操作的详细信息，请参阅主题[提供与.NET 媒体服务 SDK 的资产](media-services-deliver-streaming-content.md)。

    static void ListAllLocators()
    {
        foreach (ILocator locator in _context.Locators)
        {
            Console.WriteLine("***********");
            Console.WriteLine("Locator Id: " + locator.Id);
            Console.WriteLine("Locator asset Id: " + locator.AssetId);
            Console.WriteLine("Locator access policy Id: " + locator.AccessPolicyId);
            Console.WriteLine("Access policy permissions: " + locator.AccessPolicy.Permissions);
            Console.WriteLine("Locator expiration: " + locator.ExpirationDateTime);
            // The locator path is the base or parent path (with included permissions) to access  
            // the media content of an asset. To create a full URL to a specific media file, take 
            // the locator path and then append a file name and info as needed.  
            Console.WriteLine("Locator base path: " + locator.Path);
            Console.WriteLine("");
        }
    }


##删除资源

下面的示例删除某项资产。

    static void DeleteAsset( IAsset asset)
    {
        // delete the asset
        asset.Delete();
    
        // Verify asset deletion
        if (GetAsset(asset.Id) == null)
            Console.WriteLine("Deleted the Asset");
    
    }

##删除作业

若要删除作业，必须检查作业的状态的状态属性中所示。 可以删除已完成或取消作业，同时作业的特定状态，例如排队、 计划，或处理，在必须先取消，然后删除它们。
下面的代码示例演示通过检查作业状态，然后在删除时的状态是已完成或取消删除作业的方法。 此代码取决于为获得对某个作业本主题的上一节︰ 获取作业引用。

    static void DeleteJob(string jobId)
    {
        bool jobDeleted = false;
    
        while (!jobDeleted)
        {
            // Get an updated job reference.  
            IJob job = GetJob(jobId);
    
            // Check and handle various possible job states. You can 
            // only delete a job whose state is Finished, Error, or Canceled.   
            // You can cancel jobs that are Queued, Scheduled, or Processing,  
            // and then delete after they are canceled.
            switch (job.State)
            {
                case JobState.Finished:
                case JobState.Canceled:
                case JobState.Error:
                    // Job errors should already be logged by polling or event 
                    // handling methods such as CheckJobProgress or StateChanged.
                    // You can also call job.DeleteAsync to do async deletes.
                    job.Delete();
                    Console.WriteLine("Job has been deleted.");
                    jobDeleted = true;
                    break;
                case JobState.Canceling:
                    Console.WriteLine("Job is cancelling and will be deleted "
                        + "when finished.");
                    Console.WriteLine("Wait while job finishes canceling...");
                    Thread.Sleep(5000);
                    break;
                case JobState.Queued:
                case JobState.Scheduled:
                case JobState.Processing:
                    job.Cancel();
                    Console.WriteLine("Job is scheduled or processing and will "
                        + "be deleted.");
                    break;
                default:
                    break;
            }
    
        }
    }


##删除访问策略

下面的代码示例演示如何获取引用访问策略基于策略 Id，然后删除该策略。

    static void DeleteAccessPolicy(string existingPolicyId)
    {
        // To delete a specific access policy, get a reference to the policy.  
        // based on the policy Id passed to the method.
        var policyInstance =
                from p in _context.AccessPolicies
                where p.Id == existingPolicyId
                select p;
        IAccessPolicy policy = policyInstance.FirstOrDefault();
    
        policy.Delete();
    
    }
    

测试
