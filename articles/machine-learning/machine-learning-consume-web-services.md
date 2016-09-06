---
ms.openlocfilehash: 355d75af0335292e80fc7485bc57d305c6e520ee
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用机器学习 web 服务 |Microsoft Azure" 
    description="一旦发布机器学习服务，可以使用 rest 风格的 web 服务，可作为请求-响应服务或作为批处理执行服务。" 
    services="machine-learning" 
    solutions="big-data" 
    documentationCenter="" 
    authors="bradsev" 
    manager="paulettm" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="tbd" 
    ms.date="06/29/2015" 
    ms.author="bradsev" />


# 如何使用机器学习实验从已发布 Azure 机器学习 web 服务

## 简介

当发布为 web 服务，Azure 机器学习实验提供可以由范围广泛的设备和平台使用 REST API。 这是因为简单 REST API 接受并响应使用 JSON 格式的邮件。 Azure 机器学习门户提供了可用于调用 R C# 和 Python 中的 web 服务的代码。 但可以使用任何编程语言调用这些服务，并且从任何设备满足三个条件︰

* 具有网络连接
* 具有 SSL 的 HTTPS 请求执行能力
* 有能力分析 JSON （由手或支持库）

这意味着可以从 web 应用程序、 移动应用程序、 自定义的桌面应用程序消耗和甚至是从 Excel 中的服务。

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  

Azure 机器学习 web 服务可以使用两种方式，作为一种请求-响应服务或作为批处理执行服务。 在每种情况下通过发布实验之后均可以消耗 rest 风格的 web 服务提供的功能。 部署使用 Azure 的 web 服务端点，被自动缩放该服务基于使用 Azure 中机器学习 web 服务，可以避免硬件资源的前期和日常成本。

<!-- When this article gets published, fix the link and uncomment
For more information on how to manage Azure Machine Learning web service endpoints using the REST API, see **Azure machine learning web service endpoints**. 
-->

有关如何创建和发布 Azure 机器学习 web 服务的信息，请参阅[发布 Azure 机器学习 web 服务][发布]。 创建机器学习实验和发布它的分步演练，请参见[开发预防性解决方案使用 Azure 机器学习][演练]。

[发布]: machine-learning-publish-a-machine-learning-web-service.md
[演练]: machine-learning-walkthrough-develop-predictive-solution.md


## 请求-响应服务 (RR)

请求-响应服务 (RR) 是用于提供已创建和发布 Azure 机器学习 Studio 实验中的无状态模型接口的低延迟、 高可扩展的 web 服务。

RR 接受单个行的输入参数，并生成输出为单个行。 输出行可以包含多个列。

RR 的示例验证应用程序的真实性。 在这种情况下可以预期数百至数以百万计的应用程序的安装。 当应用程序启动时，它调用到 RR 服务相关的输入。 然后，应用程序接收来自该服务允许或阻止应用程序执行验证响应。


## 批处理执行服务 (BES)
 
批处理执行服务 (BES) 是一种服务，处理高容量、 异步、 评分的一批数据记录。 BE 的输入包含一批从各种各样的来源，如 blob，Azure，SQL Azure，HDInsight （查询结果的配置单元，例如） 和 HTTP 源中的表的记录。 BE 的输出包含评分的结果。 结果输出到 Azure blob 存储中的文件，并在响应中返回数据从存储终结点。

BE 都有用时响应不需要立即进行，如为定期安排评分为个人或事物 (IOT) 设备的互联网。

## 示例
若要显示 RR 和 BE 的工作方式，我们使用 Azure Web 服务示例。 IOT （互联网内容） 方案中，将使用此服务。 为了简单起见，我们设备只发送一个值时， `cog_speed`，并重新获取一种回答。 

有四个方面的信息来调用的 RR 或 BES 服务所需。 已发布实验后，此信息是随时可从[Azure 机器学习服务页](https://studio.azureml.net)中的服务页面。 单击位于屏幕左侧的 WEB 服务链接，您将看到已发布的服务。 若要查找与特定服务有关的信息，有 API 帮助页链接 RR 和 BE。

1.  该**服务 API 密钥**，可用在服务主页面
2.  该**服务 URI**，可用的 api 帮助选服务页
3.  预期的中**API 请求正文**，可用的 api 帮助选服务页
4.  预期的**响应正文 API**，可用的 api 帮助选服务页

在下面的两个示例中，使用 C# 语言来说明所需的代码和目标的平台是 Windows 8 桌面。 

### RR 的示例
在 API 的帮助页面上，除了 URI，您将输入和输出定义和代码示例。 API 输入称为出，此服务特别地，而且是 API 调用的负载。 

**示例请求**

    {
      "Inputs": {
        "input1": {
          "ColumnNames": [
            "cog_speed"
          ],
          "Values": [
            [
              "0"
            ],
            [
              "1"
            ]
          ]
        }
      },
      "GlobalParameters": {}
    }


同样，API 响应还出，再次调用此服务的专门。

**响应示例**

    {
      "Results": {
        "output1": {
          "type": "DataTable",
          "value": {
            "ColumnNames": [
              "cog_speed"
            ],
            "ColumnTypes": [
              "Numeric"
            ].
          "Values": [
            [
              "0"
            ],
            [
              "1"
            ]
          ]
        }
      },
      "GlobalParameters": {}
    }

页面的底部，您将找到的代码示例。 下面是 C# 实现的代码示例 
                   
**示例代码**

    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Net.Http;
    using System.Net.Http.Formatting;
    using System.Net.Http.Headers;
    using System.Text;
    using System.Threading.Tasks;
    
    namespace CallRequestResponseService
    {
        public class StringTable
        {
            public string[] ColumnNames { get; set; }
            public string[,] Values { get; set; }
        }
    
        class Program
        {
            static void Main(string[] args)
            {
                InvokeRequestResponseService().Wait();
            }
    
            static async Task InvokeRequestResponseService()
            {
                using (var client = new HttpClient())
                {
                    var scoreRequest = new
                    {
                        Inputs = new Dictionary<string, StringTable> () { 
                            { 
                                "input1", 
                                new StringTable() 
                                {
                                    ColumnNames = new string[] {"cog_speed"},
                                    Values = new string[,] {  { "0"},  { "1"}  }
                                }
                            },
                        GlobalParameters = new Dictionary<string, string>() { }
                    };
                    
                    const string apiKey = "abc123"; // Replace this with the API key for the web service
                    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue( "Bearer", apiKey);
    
                    client.BaseAddress = new Uri("https://ussouthcentral.services.azureml.net/workspaces/<workspace id>/services/<service id>/execute?api-version=2.0&details=true");
                    
                    // WARNING: The 'await' statement below can result in a deadlock if you are calling this code from the UI thread of an ASP.Net application.
                    // One way to address this would be to call ConfigureAwait(false) so that the execution does not attempt to resume on the original context.
                    // For instance, replace code such as:
                    //      result = await DoSomeTask()
                    // with the following:
                    //      result = await DoSomeTask().ConfigureAwait(false)

                    HttpResponseMessage response = await client.PostAsJsonAsync("", scoreRequest);
    
                    if (response.IsSuccessStatusCode)
                    {
                        string result = await response.Content.ReadAsStringAsync();
                        Console.WriteLine("Result: {0}", result);
                    }
                    else
                    {
                        Console.WriteLine("Failed with status code: {0}", response.StatusCode);
                    }
                }
            }
        }
    }

### BES 示例
在 API 的帮助页面上，除了 URI，您会发现几个可用的调用有关的信息。 RR 与服务不同，BES 服务是异步的。 这意味着 BES API 只需排队等候一个作业来执行，并且调用方轮询该作业的状态，以查看完成的。 以下是当前支持的批处理作业的操作︰

1. 创建 （提交） 的批处理作业
1. 启动此批处理作业
1. 获取状态的批处理作业的结果
1. 取消正在运行的批处理作业

**1.创建批处理执行作业**

在创建批处理作业的 Azure 机器学习服务终结点时，可以指定多个参数，将定义执行此批处理︰

* **输入**︰ 表示存储 blob 对批处理作业的输入位置。
* **GlobalParameters**︰ 表示一个可定义为其实验的全局参数的集合。 Azure 机器学习实验可以具有两个必需的和可选参数的自定义服务的执行，并且调用方应提供所有必需的参数，如果适用的话。 这些参数被指定为键 / 值对的集合。
* **输出**︰ 如果该服务定义了一个或多个输出，我们允许调用方将其中的任何重定向到所选的 Azure 斑点位置。 这将允许您保存该服务的输出以首选位置并在可预知名称，否则为输出 blob 名称随机生成。 **注意**服务期望的输出内容，基于其类型，若要另存为支持的格式︰
  - 数据集输出︰ 可以另存为**.csv、.tsv、.arff**
  - 模型输出的培训︰ 可以另存为**.ilearner**
  
  输出位置重写被指定为*< 输出名称、 斑点参考 >*对，其中*输出的名称*是用户定义名称 （服务的 API 帮助页还显示） 特定的输出节点，，*斑点引用*是指输出将被重定向到 Azure 斑点位置的集合。
  
所有这些作业创建参数可以是服务的可选的取决于你的性质。 例如，服务的定义，没有输入节点不需要*输入*的参数中传递和输出位置重写功能完全是可选的因为否则将输出存储在设置为 Azure 机器学习工作区的默认存储帐户。 下面，我们介绍的示例请求负载，传递到 REST API，在其中只能输入的信息传入的服务︰

**示例请求**

    {
      "Input": {
        "ConnectionString":     
        "DefaultEndpointsProtocol=https;AccountName=mystorageacct;AccountKey=mystorageacctKey",
        "RelativeLocation": "/mycontainer/mydatablob.csv",
        "BaseLocation": null,
        "SasBlobToken": null
      },
      "Outputs": null,
      "GlobalParameters": null
    }

对批处理作业创建 API 的响应是对您的工作相关联的唯一作业 id。 此 id 是非常重要的因为它提供了您可以在系统中的其他操作引用此作业的唯一方法。  
  
**响应示例**

    "539d0bc2fde945b6ac986b851d0000f0" // The JOB_ID

**2.开始执行批处理作业**

创建批处理作业只注册其在系统中，并将其放置在一个*未启动*状态。 要实际计划执行作业，必须调用**启动**API 服务终结点的 API 帮助页所述，并提供在创建作业时获得的作业 id。
  
**3.获得执行批处理作业的状态**

通过将该作业的 id 传递给 GetJobStatus API，可以在任何时间轮询异步批处理作业的状态。 API 响应将包含作业的当前状态以及批处理作业的实际结果指示符，如果这已成功完成。 出现错误时，在*详细信息*属性返回有关实际失败原因的详细信息。
 
**有效载荷的响应**

    {
        "StatusCode": STATUS_CODE,
        "Results": RESULTS,
        "Details": DETAILS
    }

*状态代码*可以是下列项之一︰

* 未启动
* 运行
* 失败
* Cancelled
* 完成

作业已成功完成的情况下，才会填充*结果*属性 （它是**空**否则）。 基于作业的完成和服务是否至少一个定义的输出节点，结果将作为返回*[输出名称，blob 引用]*对的集合，其中 blob 引用是对 blob 包含实际结果的 SAS 只读引用。 

**响应示例**

    {
        "Status Code": "Finished",
        "Results":
        {
            "dataOutput":
            {              
                "ConnectionString": null,
                "RelativeLocation": "outputs/dataOutput.csv",
                "BaseLocation": "https://mystorageaccount.blob.core.windows.net/",
                "SasBlobToken": "?sv=2013-08-15&sr=b&sig=ABCD&st=2015-04-04T05%3A39%3A55Z&se=2015-04-05T05%3A44%3A55Z&sp=r"              
            },
            "trainedModelOutput":
            {              
                "ConnectionString": null,
                "RelativeLocation": "models/trainedModel.ilearner",
                "BaseLocation": "https://mystorageaccount.blob.core.windows.net/",
                "SasBlobToken": "?sv=2013-08-15&sr=b&sig=EFGH%3D&st=2015-04-04T05%3A39%3A55Z&se=2015-04-05T05%3A44%3A55Z&sp=r"              
            },           
        },
        "Details": null
    }

**4.取消批处理执行作业**

通过调用指定的 CancelJob API 和传递的作业的 id，可以随时取消正在运行的批处理作业。 这将完成出于各种原因如，作业时间太长而无法完成。 



#### 使用[SDK BE](machine-learning-consume-web-services.md#batch-execution-service-sdk)

[BES SDK 金块包](http://www.nuget.org/packages/Microsoft.Azure.MachineLearning/)提供简化调用 BES 得分在批处理模式下的功能。 若要安装该 Nuget 程序包，在 Visual Studio 中，转到工具，选择 Nuget 程序包管理器，然后单击程序包管理器控制台。 

发布为 web 服务可以包含 web 服务输入的模块即表示他们期望的输入，通过 web 服务提供的 AzureML 试验调用斑点位置引用的形式。 也是不使用 web 服务输入的模块和改用读卡器模块的选项。 在这种情况下，读取器通常会读取从 SQL 数据库在运行时使用一个查询获取数据。 Web 服务参数可以用于动态指向其他服务器或表，等等。SDK 支持这两种模式。

下面的代码示例演示如何将提交并监视对 Azure 机器学习服务终结点使用 BES SDK 的批处理作业。 请注意有关的设置和调用的详细信息的注释。

#### **示例代码**

    // This code requires the Nuget package Microsoft.Azure.MachineLearning to be installed.
    // Instructions for doing this in Visual Studio:
    // Tools -> Nuget Package Manager -> Package Manager Console
    // Install-Package Microsoft.Azure.MachineLearning 
    
      using System;
      using System.Collections.Generic;
      using System.Threading.Tasks;
      
      using Microsoft.Azure.MachineLearning;
      using Microsoft.Azure.MachineLearning.Contracts;
      using Microsoft.Azure.MachineLearning.Exceptions;
    
    namespace CallBatchExecutionService
    {
        class Program
        {
            static void Main(string[] args)
            {               
                InvokeBatchExecutionService().Wait();
            }
    
            static async Task InvokeBatchExecutionService()
            {
                // First collect and fill in the URI and access key for your web service endpoint.
                // These are available on your service's API help page.
                var endpointUri = "https://ussouthcentral.services.azureml.net/workspaces/YOUR_WORKSPACE_ID/services/YOUR_SERVICE_ENDPOINT_ID/";
                string accessKey = "YOUR_SERVICE_ENDPOINT_ACCESS_KEY";
    
                // Create an Azure Machine Learning runtime client for this endpoint
                var runtimeClient = new RuntimeClient(endpointUri, accessKey);
    
                // Define the request information for your batch job. This information can contain:
                // -- A reference to the AzureBlob containing the input for your job run
                // -- A set of values for global parameters defined as part of your experiment and service
                // -- A set of output blob locations that allow you to redirect the job's results
    
                // NOTE: This sample is applicable, as is, for a service with explicit input port and
                // potential global parameters. Also, we choose to also demo how you could override the
                // location of one of the output blobs that could be generated by your service. You might 
                // need to tweak these features to adjust the sample to your service.
                //
                // All of these properties of a BatchJobRequest shown below can be optional, depending on
                // your service, so it is not required to specify all with any request.  If you do not want to
                // use any of the parameters, a null value should be passed in its place.
                
                // Define the reference to the blob containing your input data. You can refer to this blob by its
                    // connection string / container / blob name values; alternatively, we also support references 
                    // based on a blob SAS URI
                    
                    BlobReference inputBlob = BlobReference.CreateFromConnectionStringData(connectionString:                                         "DefaultEndpointsProtocol=https;AccountName=YOUR_ACCOUNT_NAME;AccountKey=YOUR_ACCOUNT_KEY",
                        containerName: "YOUR_CONTAINER_NAME",
                        blobName: "YOUR_INPUT_BLOB_NAME");
                              
                    // If desired, one can override the location where the job outputs are to be stored, by passing in
                    // the storage account details and name of the blob where we want the output to be redirected to.
                    
                    var outputLocations = new Dictionary<string, BlobReference>
                        {
                          {
                           "YOUR_OUTPUT_NODE_NAME", 
                           BlobReference.CreateFromConnectionStringData(                                     connectionString: "DefaultEndpointsProtocol=https;AccountName=YOUR_ACCOUNT_NAME;AccountKey=YOUR_ACCOUNT_KEY",
                                containerName: "YOUR_CONTAINER_NAME",
                                blobName: "YOUR_DESIRED_OUTPUT_BLOB_NAME")
                           }
                        };
                
                // If applicable, you can also set the global parameters for your service
                var globalParameters = new Dictionary<string, string>
                {
                    { "YOUR_GLOBAL_PARAMETER", "PARAMETER_VALUE" }
                };
                    
                var jobRequest = new BatchJobRequest
                {
                    Input = inputBlob,
                    GlobalParameters = globalParameters,
                    Outputs = outputLocations
                };
    
                try
                {
                    // Register the batch job with the system, which will grant you access to a job object
                    BatchJob job = await runtimeClient.RegisterBatchJobAsync(jobRequest);
    
                    // Start the job to allow it to be scheduled in the running queue
                    await job.StartAsync();
    
                    // Wait for the job's completion and handle the output
                    BatchJobStatus jobStatus = await job.WaitForCompletionAsync();
                    if (jobStatus.JobState == JobState.Finished)
                    {
                        // Process job outputs
                        Console.WriteLine(@"Job {0} has completed successfully and returned {1} outputs", job.Id, jobStatus.Results.Count);
                        foreach (var output in jobStatus.Results)
                        {
                            Console.WriteLine(@"\t{0}: {1}", output.Key, output.Value.AbsoluteUri);
                        }
                    }
                    else if (jobStatus.JobState == JobState.Failed)
                    {
                        // Handle job failure
                        Console.WriteLine(@"Job {0} has failed with this error: {1}", job.Id, jobStatus.Details);
                    }
                }
                catch (ArgumentException aex)
                {
                    Console.WriteLine("Argument {0} is invalid: {1}", aex.ParamName, aex.Message);
                }
                catch (RuntimeException runtimeError)
                {
                    Console.WriteLine("Runtime error occurred: {0} - {1}", runtimeError.ErrorCode, runtimeError.Message);
                    Console.WriteLine("Error details:");
                    foreach (var errorDetails in runtimeError.Details)
                    {
                        Console.WriteLine("\t{0} - {1}", errorDetails.Code, errorDetails.Message);
                    }
                }
                catch (Exception ex)
                {
                    Console.WriteLine("Unexpected error occurred: {0} - {1}", ex.GetType().Name, ex.Message);
                }
            }
        }
    }

 

测试
