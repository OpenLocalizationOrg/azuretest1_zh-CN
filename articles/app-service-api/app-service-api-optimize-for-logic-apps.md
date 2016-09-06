---
ms.openlocfilehash: 22ecab3ba98d7005fb3f9afd02a20aa50011f227
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties
    pageTitle="增强您的 API 应用程序的逻辑应用程序"
    description="本文演示如何来装饰您的 API 应用程序与应用程序逻辑可以很好地工作"
    services="app-service\api"
    documentationCenter=".net"
    authors="sameerch"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/09/2015"
    ms.author="sameerch;guayan;tarcher"/>

# 增强您的 API 应用程序的逻辑应用程序 #

在本文中，您将学习如何定义 API 的应用程序的 API 定义，以便它能很好地与逻辑应用程序。 逻辑应用程序设计器中使用时，这将增强您 API 的应用程序的最终用户体验。

## 先决条件

如果您是新手在[Azure 应用程序服务](../app-service/app-service-value-prop-what-is.md)的[API 的应用程序](app-service-api-apps-why-best-platform.md)，建议阅读多篇系列文章创建[API 的应用程序](app-service-dotnet-create-api-app.md)


## 添加显示名称 ##
逻辑应用程序设计器会显示这有时可能会麻烦阅读，因为它们以编程方式生成操作、 字段和参数的名称。 为了提高可读性，逻辑应用程序设计器可以是可用的显示可读性更好的文本值-称为*显示名称*-而不是操作、 字段和参数的默认名称。 为此，逻辑应用程序设计器扫描 swagger 元数据提供的 API 应用程序中的某些属性存在。  显示名称用作以下属性︰

* 操作 （操作和触发器）  
  如果存在;**摘要**属性的值否则为**operationId**属性的值。 请注意，Swagger 2.0 规范允许最多 120 个字符的**摘要**属性。

* 参数 （输入）  
  如果存在; **x-ms-摘要**扩展属性的值否则为**名称**属性的值。 必须在代码中动态地设置**x-ms-摘要**的扩展属性。 本主题中的"使用自定义属性来扩展属性添加批注"一节描述了该过程。 可以使用 / / 注释设置**name**属性。 本主题的"使用 API 定义生成 XML 注释"部分描述了该过程。

* 方案字段 （输出响应）  
  如果存在; **x-ms-摘要**扩展属性的值否则为**名称**属性的值。 必须在代码中动态地设置**x-ms-摘要**的扩展属性。 本主题中的"使用自定义属性来扩展属性添加批注"一节描述了该过程。 可以使用 / / 注释设置**name**属性。 本主题的"使用 API 定义生成 XML 注释"部分描述了该过程。

**注意︰**它是建议保留您的显示名称的长度为 30 个字符或更少。

### 在 API 定义生成使用 XML 注释

对于使用 Visual Studio 开发，是很常见，要添加批注，您使用[XML 注释](https://msdn.microsoft.com/library/b2s063f7.aspx)的 API 控制器。  当使用[/doc](https://msdn.microsoft.com/library/3260k4x7.aspx)进行编译，编译器将创建一个 XML 文档文件。  API 的应用程序 SDK 中包含的 Swashbuckle 工具集可以同时生成 API 元数据，将这些意见并可以配置 API 项目若要做到这一点通过执行以下步骤︰

1. 在 Visual Studio 中打开您的项目。

2. 从**解决方案资源管理器**中，右击该项目，然后选择**属性**。

    ![项目属性](./media/app-service-api-optimize-for-logic-apps/project-properties.png)

3. 当出现在项目的属性页时，请执行以下步骤︰

    - 选择的设置将为其应用的**配置**。 通常情况下，将选择所有配置，以便您指定的设置将应用于这两种调试和发布版本。
    
    - 选择左边的**生成**选项卡
    
    - 请确认**XML 文档文件**选项被选中。 Visual Studio 将提供默认的文件名基于项目的名称。 可以将其值设置为您的命名约定所需要的任何内容或将其作为-是。

    ![将 XML 文档属性设置](./media/app-service-api-optimize-for-logic-apps/xml-documentation-file-property.png)

4. 打开*SwaggerConfig.cs*文件 （位于项目的**App_Start**文件夹中）。

5. 将**using**指令添加到*SwaggerConfig.cs*文件中的**系统**和**System.Globalization**命名空间的顶部。

        using System;
        using System.Globalization;
 
6. 调用**GetXmlCommentsPath**， *SwaggerConfig.cs*文件中搜索并取消注释行，以便执行。 尚未实现此方法，因为 Visual Studio 将下划线对**GetXmlCommentsPath**的调用，并指示当前上下文中未定义。 没关系。 您将在下一步实现它。

7. （在*SwaggerConfig.cs*文件中定义） 的**SwaggerConfig**类中添加下面的**GetXmlCommentsPath**方法实现。 此方法只是返回早在项目的设置中指定的 XML 文档文件。

        public static string GetXmlCommentsPath()
        {
            return String.Format(CultureInfo.InvariantCulture, 
                                 @"{0}\bin\ContactsList.xml", 
                                 AppDomain.CurrentDomain.BaseDirectory);
        }

8. 最后，指定控制器方法的 XML 注释。 为此，打开 API 应用程序控制器文件之一输 / / 前面控制器方法需要记录某个空行上。 Visual Studio 将自动插入的注释的部分，在其中可以指定摘要以及参数的方法，并返回值的信息。 

现在，当您生成和发布 API 的应用程序，您将看到文档文件的有效负载部分也是和上载您 API 的应用程序的其余部分。

## 高级的操作和属性进行分类

逻辑应用程序设计器具有有限的屏幕显示的操作、 参数和属性的房地产。 此外，API 的应用程序可以定义一套全面的操作和属性。 太多的信息会显示在一个小区域的结果可以使最终用户使用设计器很难。 

为了减轻此混乱，逻辑应用程序设计器允许您以分组为用户定义的类别的 API 的应用程序的操作和属性。 通过使用正确的操作和属性的分类，API 的应用程序可以改善用户体验，第一次提供了最基本和最有用的操作和属性。  

为了提供这种能力，逻辑应用程序设计器查找 swagger API 定义 API 应用程序中的特定自定义供应商扩展属性存在。 此属性命名为**x-ms 的可见性**，可以采用下列值︰

* 空或者"无"  
  这些操作和属性是由用户可随时查看。

* "高级"  
  随着这些操作和属性先进的他们会默认隐藏状态。 但是，用户可以轻松地访问他们如果需要。

* "内部"  
  这些操作和属性视为系统内部属性并不应该由用户直接使用。  因此，它们是由设计器隐藏和仅在代码视图中可用。  对于此类属性，还可以指定**x ms 计划建议**扩展属性设置的值通过逻辑应用程序设计器。  有关示例，请[添加触发器为 API 应用程序](app-service-api-dotnet-triggers.md)上的文章。


## 使用自定义属性来扩展属性添加批注

如上所述，自定义的供应商扩展属性用于批注的 API 元数据提供了更丰富的逻辑应用程序设计器可以使用的信息。  如果您使用静态元数据来描述您的 API 应用程序，您可以直接编辑项目手动添加所需的扩展属性中的*/metadata/apiDefinition.swagger.json* 。

用于 API 使用动态元数据的应用程序，您可以使用自定义属性来批注代码。  然后，您可以*SwaggerConfig.cs*文件来查找自定义属性并添加必要的供应商扩展中定义的操作筛选器。  用于动态地生成**x-ms-摘要**扩展属性下面详细介绍这种方法。

1. 定义一个特性类，称为**CustomSummaryAttribute** ，可用于批注代码。

        [AttributeUsage(AttributeTargets.All)]
        public class CustomSummaryAttribute : Attribute
        {
            public string SummaryText { get; internal set; }

            public CustomSummaryAttribute(string summaryText)
            {
                this.SummaryText = summaryText;
            }
        }

2. 定义名为**AddCustomSummaryFilter** ，看起来此操作参数中的自定义特性的操作筛选器。


        using Swashbuckle.Swagger;

        ...

        public class AddCustomSummaryFilter : IOperationFilter
        {
            public void Apply(Operation operation, SchemaRegistry schemaRegistry, System.Web.Http.Description.ApiDescription apiDescription)
            {
                if (operation.parameters == null)
                {
                    // no parameter
                    return;
                }

                foreach (var param in operation.parameters)
                {
                    var summaryAttributes = apiDescription.ParameterDescriptions.First(x => x.Name.Equals(param.name))
                                            .ParameterDescriptor.GetCustomAttributes<CustomSummaryAttribute>();

                    if (summaryAttributes != null && summaryAttributes.Count > 0)
                    {
                        // add x-ms-summary extension
                        if (param.vendorExtensions == null)
                        {
                            param.vendorExtensions = new Dictionary<string, object>();
                        }
                        param.vendorExtensions.Add("x-ms-summary", summaryAttributes[0].SummaryText);
                    }
                }
            }
        }

3. 编辑*SwaggerConfig.cs*文件并添加上面定义的筛选器类。


            GlobalConfiguration.Configuration
                .EnableSwagger(c =>
                    {
                        ...
                        c.OperationFilter<AddCustomSummaryFilter>();
                        ...
                    }

4. 使用**CustomSummaryAttribute**类来批注代码，如下面的代码段所示。

        /// <summary>
        /// Send Message
        /// </summary>
        /// <param name="queueName">The name of the Storage Queue</param>
        /// <param name="messageText">The message text to be sent</param>
        public void SendMessage(
            [CustomSummary("Queue Name")] string queueName,
            [CustomSummary("Message Text")] string messageText)
        {
             ...
        }

    在生成上面的 API 应用程序时，它将生成下面的 API 元数据︰


            ...
            "post": {
                ...
                "parameters": [
                    {
                        "name": "queueName",
                        "in": "query",
                        "description": "The name of the Storage Queue",
                        "required": true,
                        "x-ms-summary": "Queue Name",
                        "type": "string"
                    },
                    {
                        "name": "messageText",
                        "in": "query",
                        "description": "The message text to be sent",
                        "required": true,
                        "x-ms-summary": "Message Text",
                        "type": "string"
                    }
                ],
                ...

5. 同样，您可以定义架构筛选**AddCustomSummarySchemaFilter**自动批注的**x-ms-摘要**扩展属性的架构模型，如以下示例所示。

        public class AddCustomSummarySchemaFilter: ISchemaFilter
        {
            public void Apply(Schema schema, SchemaRegistry schemaRegistry, Type type)
            {
                SetCustomSummary(schema, type.GetCustomAttribute<CustomSummaryAttribute>());

                if (schema.properties != null)
                {
                    foreach (var property in schema.properties)
                    {
                        var summaryAttribute = type.GetProperty(property.Key).GetCustomAttribute<CustomSummaryAttribute>();
                        SetCustomSummary(property.Value, summaryAttribute);
                    }
                }
            }

            private static void SetCustomSummary(Schema schema, CustomSummaryAttribute summaryAttribute)
            {
                if (summaryAttribute != null)
                {
                    if (schema.vendorExtensions == null)
                    {
                        schema.vendorExtensions = new Dictionary<string, object>();
                    }
                    schema.vendorExtensions.Add("x-ms-summary", summaryAttribute.SummaryText);
                }
            }
        }

## 摘要

在本文中，您已经看到如何在逻辑应用程序设计器中使用时增强您 API 的应用程序的用户体验。  作为一种最佳做法，建议您提供正确的所有操作 （操作和触发器），参数和属性的友好名称。  此外建议您提供不超过 5 个基本操作。  输入参数的推荐方法是将基本属性的数目限制为不超过 4，而属性，建议为 5 或更少。 您的操作和属性的其余部分应标记为高级。
 
测试
