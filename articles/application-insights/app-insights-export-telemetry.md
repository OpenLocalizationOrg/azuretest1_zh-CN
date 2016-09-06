---
ms.openlocfilehash: 90fa8df24194f3669f35f3eec4d0beec6e6b995c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="连续导出的遥测从应用程序的见解" 
    description="将诊断和使用状况的数据导出到 Microsoft Azure 中的存储，并从那里下载它。" 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/31/2015" 
    ms.author="awills"/>
 
# 从应用程序的见解导出遥测

想要做一些定制您遥测分析？ 或者，您是否愿意在具有特定属性的事件电子邮件警报？ 连续的输出非常适合于此。 您在应用程序的见解门户中看到的事件导出到 Microsoft Azure 中以 JSON 格式存储。 在这里下载数据并编写任何代码需要对其进行处理。  

连续的导出即可，在免费试用期结束[标准和特优价格计划](http://azure.microsoft.com/pricing/details/application-insights/)。

（如果您只是希望不要看到的指标[一次性出口](app-insights-metrics-explorer.md#export-to-excel)或是搜索刀片式服务器，请单击顶部的刀片式服务器导出。）

## <a name="setup"></a> 设置连续导出

在应用程序的应用程序理解门户中概述刀片式服务器，打开连续导出: 

![向下滚动并单击导出连续](./media/app-insights-export-telemetry/01-export.png)

添加一个导出，并选择要用来放置数据[Azure 存储帐户](../storage-introduction.md)︰

![单击添加将导出的目标存储帐户，然后创建新的存储或选择现有的存储](./media/app-insights-export-telemetry/02-add.png)

选择您想要导出的事件类型︰

![单击选择事件类型](./media/app-insights-export-telemetry/03-types.png)


创建输出后，它会开始转。 （您只能获得数据到达后创建导出的。） 

可能会有大约一小时前在 blob 数据显示的延迟。

如果您想要在以后更改的事件类型，只需编辑导出︰

![单击选择事件类型](./media/app-insights-export-telemetry/05-edit.png)

若要停止流，请单击禁用。 再次单击启用，将用新数据重新启动流。 不会导出已禁用时到达在门户中的数据。

若要永久地停止流，删除导出。 这样做不会从存储中删除数据。

#### 不能添加或更改导出？

* 要添加或更改导出，您需要所有者、 参与者或应用程序理解参与者的访问权限。 [关于角色][角色]。

## <a name="analyze"></a> 您得到的是什么事件？

导出的数据的原始的遥测数据，我们收到您的应用程序，只是我们添加我们计算的位置数据从客户端的 IP 地址。 

不包括其他计算的指标的。 例如，我们不导出平均 CPU 利用率，但我们不要导出原始的遥测从中计算平均值。

数据还包括任何已设置的[可用性 web 测试](app-insights-monitor-web-app-availability.md)的结果。 

## <a name="get"></a> 检查数据

要检查在 Visual Studio 中的 Azure 存储，请打开**查看**，**云资源管理器**。 (如果您没有该菜单命令，您需要安装 Azure SDK︰ 打开**新建项目**对话框中，展开 C# / 云并选择**.net 获取 Microsoft Azure 的 SDK**。)

当您打开您的 blob 存储时，您将看到具有一套 blob 文件的容器。 从您的应用程序理解资源名称、 其检测键、 遥测-/ 日期/时间类型派生的每个文件的 URI。 （资源名称都是小写，和检测项省略虚线）。

![检查使用合适的工具的 blob 存储](./media/app-insights-export-telemetry/04-data.png)

日期和时间是 UTC，当遥测被放入存储的不是它所生成的时间。 因此如果您编写的代码下载数据，它可以线性数据中移动。


## <a name="format"></a> 数据格式

* 每个 blob 是一个文本文件，包含多个 \n'-separated 行。
* 每一行都是一个未格式化的 JSON 文档。 如果您想要坐下来观察，在 Visual Studio 中打开并选择编辑，高级，格式文件︰

![查看使用合适的工具遥测](./media/app-insights-export-telemetry/06-json.png)

在刻度，其中 10000 刻度是持续时间 = 1 毫秒。 例如，这些值显示 10ms 30ms 接收它，并 1.8s 来处理浏览器中的页面从浏览器发送的请求的时间︰

    "sendRequest": {"value": 10000.0},
    "receiveRequest": {"value": 30000.0},
    "clientProcess": {"value": 17970000.0}

[详细的数据模型的属性类型和属性值的引用。](app-insights-export-data-model.md)

## 处理数据

在规模较小时，您可以编写一些代码来提取分离数据，读入一个电子表格，等等。 例如︰

    private IEnumerable<T> DeserializeMany<T>(string folderName)
    {
      var files = Directory.EnumerateFiles(folderName, "*.blob", SearchOption.AllDirectories);
      foreach (var file in files)
      {
         using (var fileReader = File.OpenText(file))
         {
            string fileContent = fileReader.ReadToEnd();
            IEnumerable<string> entities = fileContent.Split('\n').Where(s => !string.IsNullOrWhiteSpace(s));
            foreach (var entity in entities)
            {
                yield return JsonConvert.DeserializeObject<T>(entity);
            }
         }
      }
    }

更大的代码示例，请参阅[使用辅助角色]的[exportasa]。



## <a name="delete"></a>删除旧数据
请注意，您负责管理的存储容量和在必要时删除旧数据。 

## 如果重新生成存储键...

如果您对您的存储更改键，连续出口将停止工作。 在 Azure 帐户中，您将看到一个通知。 

打开连续导出刀片式服务器并编辑您导出。 编辑导出目标，但只是将所选的相同存储。 单击确定以确认。

![你导出目标编辑连续导出，打开和关闭。](./media/app-insights-export-telemetry/07-resetstore.png)

持续的出口将重新启动。

## 将导出到电源 BI

[Microsoft 电源 BI](https://powerbi.microsoft.com/)提供数据丰富多变的视觉效果，在整合来自多个源的信息的能力。 您可以传输遥测数据有关的性能和用法的电源业务智能应用程序从应用程序的见解。

[业务智能电源流应用程序见解](app-insights-export-power-bi.md)

![电源 BI 应用程序理解使用数据视图的示例](./media/app-insights-export-telemetry/210.png)

## 将导出到 SQL

另一个选项是将数据移到 SQL 数据库中，在那里可以执行功能更强大的分析功能。

我们有示例演示了两种备选方法︰ 将数据从 blob 存储到数据库︰

* [将导出到 SQL 使用辅助角色][exportcode]
* [将导出到 SQL 使用流分析][exportasa]


在较大的比例，考虑[HDInsight](http://azure.microsoft.com/services/hdinsight/) -云中的 Hadoop 群集。 HDInsight 提供了多种用于管理和分析重要数据的技术。



## 问与答

* *但我想的只是一次性下载图表。*  
 
    是的您可以这样做。 在刀片式服务器的顶部，单击[导出数据](app-insights-metrics-explorer.md#export-to-excel)。

* *设置一个导出，但在我的存储区中没有任何数据。*

    是否应用程序的见解收到任何遥测从您的应用程序由于设置导出？ 您将仅收到新的数据。

* *我试图建立一个导出，但被拒绝访问*

    如果您的组织拥有帐户，您必须是所有者或 contributors （参与者） 组的成员。

    <!-- Your account has to be either a paid-for account, or in the free trial period. -->

* *可以导出直接到我自己的内部存储？* 

    不，对不起。 我们的出口引擎目前只适用于 Azure 存储这一次。  

* *有任何的 my 存储区中的数据量的限制吗？* 

    No. 我们将保持推送数据删除导出之前。 如果我们命中 blob 存储的外部限制，但这是非常巨大，我们将停止。 这取决于您控制您使用多少存储空间。  

* *在存储中应该看到多少 blob？*

 * 对于您选定要导出的每个数据类型，新 blob 创建每分钟 （如果有可用的数据）。 
 * 此外，对高流量的应用程序，其他分区单位被分配。 在这种情况下每个部门创建一个 blob 每一分钟。


* *我再生到我存储的密钥，或更改容器的名称和导出现在不起作用。*

    编辑导出并打开导出目标刀片式服务器。 保持同一存储与上面一样，选择然后单击确定以确认。 导出将重新启动。 如果修订是在过去的几天内，您不会丢失数据。

* *我是否可以暂停出口？*

    是。 单击禁用。

## 代码示例

* [业务智能电源流应用程序见解](app-insights-export-power-bi.md)
* [分析导出 JSON 使用辅助角色][exportcode]
* [将导出到 SQL 使用流分析][exportasa]

* [详细的数据模型的属性类型和属性值的引用。](app-insights-export-data-model.md)

<!--Link references-->

[exportcode]: app-insights-code-sample-export-telemetry-sql-database.md
[exportasa]: app-insights-code-sample-export-sql-stream-analytics.md
[角色]: app-insights-resources-roles-access-control.md

 

测试
