---
ms.openlocfilehash: 203641256624310e153ea287076eb39fe12f94b0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="轮询长时间运行的操作" 
    description="本主题演示如何轮询长时间运行的操作。" 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2015" 
    ms.author="juliako"/>


#提供实时流与 Azure 媒体服务

##概述

将请求发送到介质服务来启动操作的 Api 提供了 Microsoft Azure 媒体服务 (例如︰ 创建、 开始、 停止或删除通道)。 这些操作是运行时间较长。

媒体服务.NET SDK 提供了 Api 来发送请求并等待操作完成 （在内部，Api 轮询操作进度有些时间间隔）。 例如，当您调用通道。Start （），该方法返回后启动通道。 您还可以使用异步版本︰ 等待通道。StartAsync() （基于任务的异步模式的信息，请参阅[点击](https://msdn.microsoft.com/library/hh873175(v=vs.110).aspx)）。 Api 发送操作请求和操作完成之前然后轮询状态被称为"轮询方法"。 胖客户端应用程序和/或有状态服务推荐方法 （尤其是异步版本）。

有应用程序不能等待的长时间运行的 http 请求并且想要手动轮询操作进度的情况。 一个典型的例子是与无状态的 web 服务交互的浏览器︰ 当浏览器请求创建一个通道，web 服务启动长时间运行的操作，并返回到浏览器的操作 ID。 浏览器可以让 web 服务获取操作状态基于 id 的。 媒体服务.NET SDK 提供了 Api，对于这种情况下很有用。 这些 Api 被称为"非轮询方法"。
"非轮询方法"具有以下命名模式︰ 发送*操作名称*操作 (例如，SendCreateOperation)。 发送*操作名称*操作方法返回的**IOperation**对象;返回的对象包含可用于跟踪操作的信息。 发送*操作名称*OperationAsync 方法都返回**任务<IOperation>**。

目前，下面的类支持非轮询方法︰**通道**、 **StreamingEndpoint**和**程序**。

轮询操作状态，使用**OperationBaseCollection**类上的**GetOperation**方法。 使用下面的时间间隔检查操作状态︰ 对于**通道**和**StreamingEndpoint**操作，使用 30 秒;对于**程序**的操作，使用 10 秒钟。


##示例

下面的示例定义一个名为**ChannelOperations**类。 此类定义可能是 web 服务的类定义的起始点。 为简单起见，下面的示例使用方法的非异步版本。

该示例还演示如何，客户端可能会使用此类。

###ChannelOperations 类定义

    /// <summary> 
    /// The ChannelOperations class only implements 
    /// the Channel’s creation operation. 
    /// </summary> 
    public class ChannelOperations
    {
        // Read values from the App.config file.
        private static readonly string _mediaServicesAccountName =
            ConfigurationManager.AppSettings["MediaServicesAccountName"];
        private static readonly string _mediaServicesAccountKey =
            ConfigurationManager.AppSettings["MediaServicesAccountKey"];
    
        // Field for service context.
        private static CloudMediaContext _context = null;
        private static MediaServicesCredentials _cachedCredentials = null;
    
        public ChannelOperations()
        {
                _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName,
                    _mediaServicesAccountKey);
    
                _context = new CloudMediaContext(_cachedCredentials);    }
    
        /// <summary>  
        /// Initiates the creation of a new channel.  
        /// </summary>  
        /// <param name="channelName">Name to be given to the new channel</param>  
        /// <returns>  
        /// Operation Id for the long running operation being executed by Media Services. 
        /// Use this operation Id to poll for the channel creation status. 
        /// </returns> 
        public string StartChannelCreation(string channelName)
        {
            var operation = _context.Channels.SendCreateOperation(
                new ChannelCreationOptions
                {
                    Name = channelName,
                    Input = CreateChannelInput(),
                    Preview = CreateChannelPreview(),
                    Output = CreateChannelOutput()
                });
    
            return operation.Id;
        }
    
        /// <summary> 
        /// Checks if the operation has been completed. 
        /// If the operation succeeded, the created channel Id is returned in the out parameter.
        /// </summary> 
        /// <param name="operationId">The operation Id.</param> 
        /// <param name="channel">
        /// If the operation succeeded, 
        /// the created channel Id is returned in the out parameter.</param>
        /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
        public bool IsCompleted(string operationId, out string channelId)
        {
            IOperation operation = _context.Operations.GetOperation(operationId);
            bool completed = false;
    
            channelId = null;
    
            switch (operation.State)
            {
                case OperationState.Failed:
                    // Handle the failure. 
                    // For example, throw an exception. 
                    // Use the following information in the exception: operationId, operation.ErrorMessage.
                    break;
                case OperationState.Succeeded:
                    completed = true;
                    channelId = operation.TargetEntityId;
                    break;
                case OperationState.InProgress:
                    completed = false;
                    break;
            }
            return completed;
        }
    
    
        private static ChannelInput CreateChannelInput()
        {
            return new ChannelInput
            {
                StreamingProtocol = StreamingProtocol.RTMP,
                AccessControl = new ChannelAccessControl
                {
                    IPAllowList = new List<IPRange>
                    {
                        new IPRange
                        {
                            Name = "TestChannelInput001",
                            Address = IPAddress.Parse("0.0.0.0"),
                            SubnetPrefixLength = 0
                        }
                    }
                }
            };
        }
    
        private static ChannelPreview CreateChannelPreview()
        {
            return new ChannelPreview
            {
                AccessControl = new ChannelAccessControl
                {
                    IPAllowList = new List<IPRange>
                    {
                        new IPRange
                        {
                            Name = "TestChannelPreview001",
                            Address = IPAddress.Parse("0.0.0.0"),
                            SubnetPrefixLength = 0
                        }
                    }
                }
            };
        }
    
        private static ChannelOutput CreateChannelOutput()
        {
            return new ChannelOutput
            {
                Hls = new ChannelOutputHls { FragmentsPerSegment = 1 }
            };
        }
    }

###客户端代码

    ChannelOperations channelOperations = new ChannelOperations();
    string opId = channelOperations.StartChannelCreation("MyChannel001");
    
    string channelId = null;
    bool isCompleted = false;
    
    while (isCompleted == false)
    {
        System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
        isCompleted = channelOperations.IsCompleted(opId, out channelId);
    }
    
    // If we got here, we should have the newly created channel id.
    Console.WriteLine(channelId);
 
测试
