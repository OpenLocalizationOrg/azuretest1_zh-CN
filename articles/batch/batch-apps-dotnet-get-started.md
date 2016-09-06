---
ms.openlocfilehash: 40ec551a335310edb809c648f16f848628611068
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="教程-开始使用.NET Azure 的批处理应用程序库"
    description="了解有关 Azure 批处理应用程序以及如何使用一个简单的方案开发的基本概念"
    services="batch"
    documentationCenter=".net"
    authors="yidingzhou"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="07/21/2015"
    ms.author="yidingz"/>




# 开始使用.NET Azure 的批处理应用程序库
本教程演示您如何批 Azure 上运行并行计算工作负载使用批处理应用程序服务。

批处理应用程序是 Azure 批地以应用程序为中心的管理和执行批处理工作负载的功能。  使用批处理应用程序 SDK，可以创建程序包添加到批次启用应用程序，并将它们部署在您自己的帐户或请其他批处理用户。  批次还提供了任务间的依赖关系的数据管理、 作业监控、 内置的诊断和记录和支持。  此外，它包括在其中管理作业、 查看日志，并下载而不需要编写自己的客户端的输出的管理门户。

在批处理应用程序方案中，您可以编写使用批处理应用程序云 SDK 并行任务划分作业，描述这些任务之间的任何依赖项并指定如何执行每个任务的代码。  此代码部署到的批次帐户。  客户机然后可以简单地通过指定作业和 REST api 的输入的文件的类型执行作业。

>[AZURE.NOTE] 若要完成本教程，您需要一个 Azure 帐户。 您可以免费试用帐户中只需几分钟。 有关详细信息，请参阅[Azure 免费试用版](http://azure.microsoft.com/pricing/free-trial/)。 您可以使用 NuGet 获取两个<a href="http://www.nuget.org/packages/Microsoft.Azure.Batch.Apps.Cloud/">批处理应用程序云</a>程序集和<a href="http://www.nuget.org/packages/Microsoft.Azure.Batch.Apps/">批处理应用程序客户端</a>程序集。 在 Visual Studio 中创建您的项目后，右击**解决方案资源管理器**中的项目，然后选择**管理 NuGet 程序包**。 您还可以为批处理应用程序，其中包括云启用应用程序，并能够将应用程序部署到一个项目模板下载 Visual Studio 扩展<a href="https://visualstudiogallery.msdn.microsoft.com/8b294850-a0a5-43b0-acde-57a07f17826a">这里</a>或通过搜索 Visual Studio 中的**批处理应用程序**通过扩展和更新菜单项。  您还可以找到<a href="https://go.microsoft.com/fwLink/?LinkID=512183&clcid=0x409">在 MSDN 上的端到端示例。</a>
>

###Azure 的批处理应用程序的基础知识
批次旨在与现有计算密集型应用程序的工作。 它利用了您现有的应用程序代码并运行动态、 虚拟化、 通用的环境中。 若要使应用程序能够使用批处理应用程序有一些需要执行的操作︰

1.  准备好您现有应用程序的可执行文件 – 将在传统的服务器场或群集 – 运行同一可执行文件的 zip 文件和它所需要的所有支持文件。 此压缩文件然后上载到批帐户使用 REST API 的管理门户。
2.  创建一个 zip 文件发送到该应用程序，您的工作负载的"云集"，并将其上载通过管理门户或 REST API。 是云的程序集包含生成针对云 SDK 中的两个组件︰
    1.  作业分隔 – 作业分解为多个可以单独处理的任务。 例如，在动画方案中，作业拆分器将影片求职和将其分解为各个帧。
    2.  任务处理器 – 调用给定任务的可执行文件的应用程序。 例如，在动画方案中，任务处理器将调用呈现单帧手头的任务指定呈现程序。
3.  提供一种将作业提交到 Azure 中启用的应用程序的方法。 这可能是在您的应用程序用户界面或 web 门户或者甚至无人参与的服务作为执行管道的一部分的插件。 请参阅<a href="https://go.microsoft.com/fwLink/?LinkID=512183&clcid=0x409">样本</a>在 MSDN 示例。



###批处理应用程序的主要概念
批处理应用程序的编程和使用模型围绕以下关键概念︰

####作业
**作业**是用户提交的一项工作。 提交作业时，用户指定的作业，该作业，并且作业所需的数据的任何设置的类型。 已启用的实现可以计算出这些用户的详细信息的名义或在某些情况下用户可以提供此信息通过客户端显式。 作业已返回结果。 每个作业有主输出，并可以选择预览输出。 如果需要，作业也可以返回额外的输出。

####任务
**任务**是一项工作将作为作业的一部分执行。 当用户提交作业时，它被划分为更小的任务。 该服务然后处理下列各项任务，则任务结果到整体的作业输出的程序集。 任务的性质取决于作业的类型。 作业拆分定义如何作业分解为执行任务，有的工作应用程序旨在处理哪些区块的知识作为指导。 任务输出也可以下载单独，可在某些情况下，例如当用户想要下载动画作业中的各项任务。

####合并的任务
**合并任务**是任务的一种特殊，将各项任务的结果汇编成最终的工作结果。 对于影片 rending 作业，合并任务可能装配呈现的帧转换为电影或要压缩到一个文件的所有呈现的帧。 每个作业都有合并任务即使没有实际需要合并。

####文件
**文件**是一种使用作为作业的输入数据。 一个作业可以没有输入与之相关联的文件或具有一个或多个。 同一文件可用于在多个作业，例如电影呈现作业，这些文件可能是纹理、 模型等。数据分析工作，这些文件可能是观察或测量的一组。

###启用云应用程序
您的应用程序必须包含静态字段或属性，包含您的应用程序的详细信息。 它指定应用程序的作业类型或应用程序处理的作业类型的名称。 可以通过 Visual Studio 库下载 SDK 中使用该模板时，提供该参数。

下面是一个并行的工作负荷的云应用程序声明示例︰

    public static class ApplicationDefinition
    {
        public static readonly CloudApplication App = new ParallelCloudApplication
        {
            ApplicationName = "ApplicationName",
            JobType = "ApplicationJobType",
            JobSplitterType = typeof(MyJobSplitter),
            TaskProcessorType = typeof(MyTaskProcessor),
        };
    }

####实施作业拆分和任务处理器
对于 embarrassingly 并行工作负荷，您必须实现作业拆分和任务处理器。

####实现 JobSplitter.SplitIntoTasks
SplitIntoTasks 实现必须返回任务的序列。 每项任务都代表单独的一部分工作将排队等待处理，通过计算节点。 每个任务必须独立并且必须设置任务处理器将需要的所有信息。

指定的作业拆分任务表示为 TaskSpecifier 对象。 TaskSpecifier 具有大量的属性，您可以设置此之前返回任务。


-   TaskIndex 将被忽略，但供任务处理器。 您可以使用此将索引传递给任务处理器。 如果您不需要传递索引，不需要设置该属性。
-   参数是按默认值为空集合。 您可以向其中添加或将其替换为一个新的集合。 可以将项目从作业参数集合并使用 WithJobParameters 或 WithAllJobParameters 方法进行复制。  


RequiredFiles 是默认情况下为空集合。 您可以向其中添加或将其替换为一个新的集合。 可以将项目从作业文件集合并使用 RequiringJobFiles 或 RequiringAllJobFiles 方法进行复制。

您可以指定取决于特定的任务另一任务。 为此，设置 TaskSpecifier.DependsOn 属性，通过它取决于任务的 ID:

    new TaskSpecifier {
        DependsOn = TaskDependency.OnId(5)
    }

可用输出的 depended 上的任务之前，该任务将不会运行。

此外可以指定一组任务应启动处理，直到完全完成另一个组。 在这种情况下，您可以设置 TaskSpecifier.Stage 属性。 用给定的值阶段的任务不会开始处理，直到完成所有任务值较低阶段;例如，第 3 阶段的任务不会开始处理与阶段 0、 1 或 2 的所有任务都完成之前。 从 0 开始阶段必须、 阶段的顺序必须有没有差别，和 SplitIntoTasks 必须返回任务阶段顺序︰ 是错误阶段 1 的任务之后，将返回与阶段 0 任务等。

与特殊任务调用合并任务结束每个并行作业。 合并任务的工作就是装配并行处理任务的结果为最终结果。 批处理应用程序会自动为您创建合并任务。  

在极少数情况下，您可能需要显式控制该合并任务，例如自定义其参数。 在这种情况下，您可以指定合并任务通过返回 TaskSpecifier 的 IsMerge 属性设为 true。 这将覆盖自动合并任务。 如果您创建一个显式合并任务︰  

-   您可以创建一个显式合并任务
-   它必须是序列中的最后一个任务
-   它必须具有 IsMerge 设置为 true，并且每个其他任务必须将 IsMerge 设置为 false  


但是，请记住，通常情况下您不需要显式地创建合并任务。  

下面的代码演示了 SplitIntoTasks 的一个简单实现。  

    protected override IEnumerable<TaskSpecifier> SplitIntoTasks(
        IJob job,
        JobSplitSettings settings)
    {
        int start = Int32.Parse(job.Parameters["startIndex"]);
        int end = Int32.Parse(job.Parameters["endIndex"]);
        int count = (end - start) + 1;

        // Processing tasks
        for (int i = 0; i < count; ++i)
        {
            yield return new TaskSpecifier
            {
                TaskIndex = start + i,
            }.RequiringAllJobFiles();
        }
    }
####实现 ParallelTaskProcessor.RunExternalTaskProcess

从作业拆分器返回的每个非合并任务就称为 RunExternalTaskProcess。 应调用您的应用程序使用适当的参数，并返回集合的需要保留以供将来使用的输出。

以下片段演示如何调用一个名为 application.exe 的程序。 注意您可以使用 ExecutablePath 帮助器方法来创建绝对文件路径。

云 SDK 中的 ExternalProcess 类提供有用的帮助程序逻辑运行您的应用程序可执行文件。 ExternalProcess 可以关照的取消，标准输出和标准错误捕获并设置环境变量，将退出代码转化为例外。 另外还可以使用.NET Process 类直接以运行您的程序，如果您喜欢。

RunExternalTaskProcess 方法返回 TaskProcessResult，它包含的输出文件的列表。 这必须包括至少合并; 所需的所有文件您还可以返回日志文件，预览文件和中间文件 （例如用于诊断目的任务失败时）。  请注意您的方法返回的文件路径，而不是文件内容。

每个文件都必须与它所包含的输出的类型标识︰ 输出 （即最终作业输出的一部分）、 预览、 日志或中间。  这些值来自于 TaskOutputFileKind 枚举。 下面这段代码返回一个任务的输出，没有预览或日志。 TaskProcessResult.FromExternalProcessResult 方法简化了捕获退出代码、 处理器输出和输出文件，从命令行程序的常见方案︰

下面的代码演示了 ParallelTaskProcessor.RunExternalTaskProcess 的一个简单实现。

    protected override TaskProcessResult RunExternalTaskProcess(
        ITask task,
        TaskExecutionSettings settings)
    {
        var inputFile = LocalPath(task.RequiredFiles[0].Name);
        var outputFile = LocalPath(task.TaskId.ToString() + ".jpg");

        var exePath = ExecutablePath(@"application\application.exe");
        var arguments = String.Format("-in:{0} -out:{1}", inputFile, outputFile);

        var result = new ExternalProcess {
            CommandPath = exePath,
            Arguments = arguments,
            WorkingDirectory = LocalStoragePath,
            CancellationToken = settings.CancellationToken
        }.Run();

        return TaskProcessResult.FromExternalProcessResult(result, outputFile);
    }
####实现 ParallelTaskProcessor.RunExternalMergeProcess

这就称为合并任务。 它应调用应用程序组合中任何方式是适合于您的应用程序的前一个任务的输出，并返回组合的输出。

RunExternalMergeProcess 的实现是非常类似于 RunExternalTaskProcess，唯一不同的是︰  

-   RunExternalMergeProcess 使用前一个任务的输出，而不是用户输入的文件。 因此，您应当制订要处理基于任务 ID，而不是从 Task.RequiredFiles 集合的文件的名称。
-   RunExternalMergeProcess 返回一个 JobOutput 文件，和一个 JobPreview 文件 （可选）。


下面的代码演示了 ParallelTaskProcessor.RunExternalMergeProcess 的一个简单实现。

    protected override JobResult RunExternalMergeProcess(
        ITask mergeTask,
        TaskExecutionSettings settings)
    {
        var outputFile =  "output.zip";

        var exePath =  ExecutablePath(@"application\application.exe");
        var arguments = String.Format("a -application {0} *.jpg", outputFile);

        new ExternalProcess {
            CommandPath = exePath,
            Arguments = arguments,
            WorkingDirectory = LocalStoragePath
        }.Run();

        return new JobResult {
            OutputFile = outputFile
        };
    }

###将作业提交到批处理应用程序
作业描述要运行的工作负荷，并需要包括除文件内容以外的工作负荷有关的所有信息。 例如，作业包含从客户端通过作业拆分并到任务流的配置设置。 在 MSDN 上提供的示例是如何提交作业，并提供多个客户端包括一个 web 门户和命令行客户端的示例。

测试
