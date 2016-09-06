---
ms.openlocfilehash: f4b373a49722bca8d13afb28a5f3e4e6f46dd14f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="分析计算仿真程序在本地云服务" 
    services="cloud-services"
    description="与 Visual Studio 探查器研究云服务中的性能问题" 
    documentationCenter=""
    authors="patshea123" 
    manager="douge" 
    editor="tglee"
    tags="" 
    />

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="05/27/2015" 
    ms.author="patshea"/>

# Azure 计算仿真程序使用 Visual Studio 的事件探查器中的本地测试云服务的性能

各种工具和技术都可用于测试云服务的性能。
当您发布到 Azure 的云服务时，可以让 Visual Studio 收集分析数据，然后对其进行分析本地[分析 Azure 应用程序][1]所述。
您可以使用诊断跟踪多个性能计数器[在 Azure 中的使用性能计数器][2]所述。
您可能希望分析再将其部署到云计算仿真程序中的本地应用程序。

本文介绍了 CPU 采样方法的分析，它可以在仿真器中本地执行。 CPU 取样是一种分析的方法不是很令人讨厌。 指定的采样间隔，探查器将调用堆栈中的快照。 通过一段时间，收集数据并将其显示在报表中。 这种分析方法往往指示位置计算密集型操作的应用程序中的大部分 CPU 工作正在进行。  这使您有机会聚焦"热路径"，您的应用程序花费大部分时间。


## 先决条件

您可以在 Visual Studio 津贴或 Visual Studio 最终只有本地运行探查器。 

## <a name="step1"> </a> 第 1 步︰ 配置 Visual Studio 进行分析

首先，有几个 Visual Studio 的配置选项进行分析时可能会有所帮助。 以有意义的分析报告，您需要为您应用程序和系统库的符号符号 （.pdb 文件）。 您需要确保您引用可用符号服务器。 为此，请在 Visual Studio 中的**工具**菜单上，选择**选项**，然后选择**调试**，然后**符号**。 请确保**符号 (.pdb) 文件位置**下列出了 Microsoft 符号服务器。  您也可以引用 http://referencesource.microsoft.com/symbols，可能有其他的符号文件。

![][4]

如果需要，您可以简化通过设置仅我的代码的探查器生成的报告。 通过启用仅我的代码时，函数调用堆栈得到简化，以便调用的库和.NET Framework 完全内部处于隐藏状态的报告。 在**工具**菜单上选择**选项**。 展开**性能工具**节点，然后选择**常规**。 选中的复选框**启用仅我的代码的探查器报告**。

![][17]

与现有的项目或新项目，可以使用这些说明。  如果您创建一个新项目，尝试使用下面描述的方法，选择 C# **Azure 云服务**项目，然后选择**Web 角色**和**辅助角色**。

![][5]

例如目的，添加一些代码到您的项目，需要大量的时间和演示一些明显的性能问题。 例如，向工作人员角色项目中添加以下代码︰

    public class Concatenator
    {
        public static string Concatenate(int number)
        {
            int count;
            string s = "";
            for (count = 0; count < number; count++)
            {
                s += "\n" + count.ToString();
            }
            return s;
        }
    }

从辅助角色的 RoleEntryPoint 派生类中的 RunAsync 方法中调用此代码。 （忽略有关同步运行的方法的警告）。

        private async Task RunAsync(CancellationToken cancellationToken)
        {
            // TODO: Replace the following with your own logic.
            while (!cancellationToken.IsCancellationRequested)
            {
                Trace.TraceInformation("Working");
                Concatenator.Concatenate(10000);
            }
        }

生成并运行您的云服务本地，而不调试 (Ctrl + F5) 设置为**发布**的解决方案配置。 这可确保所有文件和文件夹创建以本地方式运行应用程序，并确保所有仿真程序已启动。 从任务栏启动计算仿真程序用户界面，若要验证您的工作角色正在运行。

## <a name="step2"> </a> 步骤 2︰ 将附加到进程

而不是从 Visual Studio 2010 IDE 分析应用程序，必须将探查器附加到正在运行的进程。 

将探查器附加到过程中，在**分析**菜单中，选择**事件探查器**和**附加/分离**。

![][6]

对于一个辅助的角色，找到 WaWorkerHost.exe 过程。

![][7]

如果您的项目文件夹所在的网络驱动器，探查器将要求您提供其他位置来保存分析报告。

 您还可以通过将附加到 WaIISHost.exe 附加到 web 角色。
如果您的应用程序中有多个工作角色进程，您需要使用流程 Id 来区分它们。 您可以通过访问进程对象以编程方式查询流程 Id。 例如，如果您将此代码添加到角色中的 RoleEntryPoint 派生类的 Run 方法时，您可以查看计算仿真程序用户界面中知道要连接到哪个进程的日志。

    var process = System.Diagnostics.Process.GetCurrentProcess();
    var message = String.Format("Process ID: {0}", process.Id);
    Trace.WriteLine(message, "Information");

若要查看日志，请启动计算仿真程序用户界面。

![][8]

控制台窗口的标题栏上单击，在计算仿真程序用户界面中打开辅助角色日志控制台窗口。 您可以查看日志中的进程 ID。

![][9]

一个贴，执行中的步骤应用程序的 UI （如果需要） 重现方案。

当您想要停止分析时，选择**停止分析**链接。

![][10]

## <a name="step3"> </a> 步骤 3︰ 查看性能报告

将显示应用程序的性能报告。

在这种情况下，探查器停止执行、 将数据保存在.vsp 文件中，并显示一个报表，显示这些数据的分析。

![][11]


如果您看到 String.wstrcpy 热路径中的，单击仅我的代码以更改视图，以显示用户的代码。  如果您看到 String.Concat，请尝试按显示所有代码按钮。

您应该看到的连接方法和 String.Concat 占用了大部分执行时间。

![][12]

如果您在本文中添加字符串串联代码，您应该对此看到任务列表中的警告。 您还可能看到警告是过量的垃圾回收，这是由于创建和释放的字符串的数量。

![][14]

## <a name="step4"> </a> 第 4 步︰ 进行更改并比较性能

此外可以将代码更改前后的性能进行比较。  停止正在运行的进程中，并编辑 StringBuilder 使用替换字符串串联操作的代码︰

    public static string Concatenate(int number)
    {
        int count;
        System.Text.StringBuilder builder = new System.Text.StringBuilder("");
        for (count = 0; count < number; count++)
        {
             builder.Append("\n" + count.ToString());
        }
        return builder.ToString();
    }

另一个性能运行，并比较性能。 在性能资源管理器中，如果运行在同一会话中，您可以只选择两个报表，打开快捷菜单，然后选择**比较性能报告**。 如果您想要与另一个性能会话中运行，**分析**菜单上，选择**比较性能报告**。 在出现的对话框中指定这两个文件。

![][15]

这些报告突出显示两个运行之间的差异。

![][16]

祝贺您 ！ 您已经引起了启动探查器。

## <a name="troubleshooting"> </a> 疑难解答

- 确保您要分析发布版本并不调试的情况下启动。

- 如果探查器菜单上未启用附加/分离选项，运行性能向导。

- 使用计算仿真程序用户界面以查看您的应用程序的状态。 

- 如果您有问题的仿真程序中启动应用程序或附加探查器，关闭向下计算仿真程序，然后重新启动它。 如果这样不能解决此问题，请尝试重新启动。 如果您使用计算仿真程序挂起和删除运行部署可以发生此问题。

- 如果您已使用的任何性能分析命令从命令行，尤其是全局设置，确保已调用 VSPerfClrEnv /globaloff 和 VsPerfMon.exe 已经被关闭。

- 如果采样，当您看到消息"PRF0025︰ 未收集任何数据，"检查附加到的进程的 CPU 活动。 应用程序不执行任何计算的工作可能不会产生任何的采样数据。  还有可能的进程退出之前未进行任何采样。 请检查您要分析一个角色的运行方法不会终止。

## <a name="nextSteps"> </a> 下一步行动

在 Visual Studio 的探查器不支持检测 Azure 仿真程序中的二进制文件，但如果您想要测试内存分配，在进行分析时，可以选择该选项。 此外可以选择并发分析，从而帮助您确定线程是否争用的锁或层交互分析，可帮助您跟踪性能问题时进行交互的应用程序、 数据层和辅助角色之间最常用的各层之间的浪费时间。  您可以查看您的应用程序生成的数据库查询和使用分析数据来提高您的数据库使用。 层交互分析有关信息，请参阅[演练︰ 使用 Visual Studio 的团队系统 2010 年的层交互探查器][3]。


[第 1 步︰ 配置 Visual Studio 进行分析]: #step1
[步骤 2︰ 将附加到进程]: #step2
[步骤 3︰ 查看分析报告]: #step3
[第 4 步︰ 进行更改并比较性能]: #step4
[疑难解答]: #troubleshooting
[下一步行动]: #nextSteps

[1]: http://msdn.microsoft.com/library/azure/hh369930.aspx
[2]: http://msdn.microsoft.com/library/azure/hh411542.aspx
[3]: http://blogs.msdn.com/b/habibh/archive/2009/06/30/walkthrough-using-the-tier-interaction-profiler-in-visual-studio-team-system-2010.aspx
[4]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally09.png
[5]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally10.png
[6]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally02.png
[7]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally05.png
[8]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally010.png
[9]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally07.png
[10]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally06.png
[11]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally03.png
[12]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally011.png
[14]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally04.png 
[15]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally013.png
[16]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally012.png
[17]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally08.png
 
测试
