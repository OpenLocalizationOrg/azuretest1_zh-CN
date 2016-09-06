---
ms.openlocfilehash: 984cfe0738dfabea131e5bd1eb7c5f3fe4a9ce26
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="生成服务与移动服务.NET 后端使用现有 SQL 数据库 |Microsoft Azure"
    description="了解如何使用现有的云或使用您的.NET 内部 SQL 数据库基于移动服务"
    services="mobile-services"
    documentationCenter=""
    authors="ggailey777"
    manager="dwrede"
    editor="mollybos"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article" 
    ms.date="06/16/2015"
    ms.author="glenga"/>


# 建立现有 SQL 数据库使用移动服务.NET 后端服务

移动服务.NET 后端便于利用现有的资源，建立移动服务。 一个特别有意思的方案使用现有 SQL 数据库 (或者内部部署或云)，，可能已用于其他应用程序，使现有数据可用于移动客户端。 在这种情况下它是数据库模型 （或*架构*） 保持不变，以便继续使用的现有解决方案的要求。

<a name="ExistingModel"></a>
## 了解现有的数据库模型

在本教程中，我们将使用的数据库的创建与您的移动服务，但是我们不会使用创建的默认模型。 相反，我们将手动创建的任意模型表示现有的应用程序可能会遇到。 有关如何而是连接到本地数据库的完整详细信息，查看[连接到内部部署 SQL Server 从 Azure 移动服务使用混合连接](mobile-services-dotnet-backend-hybrid-connections-get-started.md)。

1. 通过在**Visual Studio 2013年更新 2**中创建的移动服务服务器项目或通过[Azure 管理门户](http://manage.windowsazure.com)中服务使用移动服务选项卡中可以下载该快速入门项目启动。 对于本教程，我们将假定您服务器的项目名称是**ShoppingService**。

2. 创建**模型**文件夹中的**Customer.cs**文件并使用下面的实现。 您将需要向项目中添加程序集引用指向**System.ComponentModel.DataAnnotations** 。

        using System.Collections.Generic;
        using System.ComponentModel.DataAnnotations;

        namespace ShoppingService.Models
        {
            public class Customer
            {
                [Key]
                public int CustomerId { get; set; }

                public string Name { get; set; }

                public virtual ICollection<Order> Orders { get; set; }

            }
        }

3. 创建**模型**文件夹中的**Order.cs**文件并使用下面的实现︰

        using System.ComponentModel.DataAnnotations;

        namespace ShoppingService.Models
        {
            public class Order
            {
                [Key]
                public int OrderId { get; set; }

                public string Item { get; set; }

                public int Quantity { get; set; }

                public bool Completed { get; set; }

                public int CustomerId { get; set; }

                public virtual Customer Customer { get; set; }

            }
        }

    您将注意这两个类有*关系*︰ 每个**订单**是与单个**客户**关联，**客户**可以与多个**订单**相关联。 关系是在现有的数据模型中常见。

4. 创建**模型**文件夹中的**ExistingContext.cs**文件，因此为实现它︰

        using System.Data.Entity;

        namespace ShoppingService.Models
        {
            public class ExistingContext : DbContext
            {
                public ExistingContext()
                    : base("Name=MS_TableConnectionString")
                {
                }

                public DbSet<Customer> Customers { get; set; }

                public DbSet<Order> Orders { get; set; }

            }
        }

上述结构近似于您可能正在使用现有应用程序的现有实体框架模型。 请注意在这一阶段尚未以任何方式移动服务发现模型。

<a name="DTOs"></a>
## 创建数据传输对象 (Dto) 为您的移动服务

您想要与您的移动服务使用的数据模型可能是任意复杂;它可能包含数百个实体具有不同的相互关系。 当构建移动应用程序，则通常需要简化的数据模型和消除关系 （或手动处理它们） 为了最小化应用程序和服务之间来回发送的有效负载。 在本节中，我们将创建一组简化的对象 (称为"数据传输对象"或"Dto")，必须在数据库中的数据映射，还包含只最小的移动应用程序所需的属性集。

1. 服务项目的**DataObjects**文件夹中创建的**MobileCustomer.cs**文件，并使用下面的实现︰

        using Microsoft.WindowsAzure.Mobile.Service;

        namespace ShoppingService.DataObjects
        {
            public class MobileCustomer : EntityData
            {
                public string Name { get; set; }
            }
        }

    请注意，此类类似于在模型中，**客户**类除**顺序**关系属性被删除。 正确使用移动服务脱机同步对象，它需要一组*系统属性*的开放式并发，这样您会注意到 DTO 继承[**EntityData**](http://msdn.microsoft.com/library/microsoft.windowsazure.mobile.service.entitydata.aspx)，其中包含这些属性。 从原始模型基于 int 的**客户 id**属性将被替换基于字符串的**Id**属性**EntityData**，后者将是移动服务将使用**Id** 。

2. 在服务项目的**DataObjects**文件夹中创建的**MobileOrder.cs**文件。

        using Microsoft.WindowsAzure.Mobile.Service;
        using Newtonsoft.Json;
        using System.ComponentModel.DataAnnotations;
        using System.ComponentModel.DataAnnotations.Schema;

        namespace ShoppingService.DataObjects
        {
            public class MobileOrder : EntityData
            {
                public string Item { get; set; }

                public int Quantity { get; set; }

                public bool Completed { get; set; }

                [NotMapped]
                public int CustomerId { get; set; }

                [Required]
                public string MobileCustomerId { get; set; }

                public string MobileCustomerName { get; set; }
            }
        }

    **客户**关系属性已替换为**客户**名称和一个**MobileCustomerId**属性，可用于手动模仿客户端上的关系。 对于现在可以忽略的**客户 id**属性，它仅在以后使用。

3. 您可能会注意到，新增的**EntityData**基类上的系统属性，与我们 Dto 现在有更多比模型类型的属性。 显然，我们需要一个位置来存储这些属性，因此我们将添加一些额外的列到原始数据库。 而这发生了更改数据库，它不会中断现有应用程序，因为所做的更改是纯添加 （将新列添加到架构）。 要做到这一点，添加到顶部的**Customer.cs**和**Order.cs**下面的语句︰

        using System.ComponentModel.DataAnnotations.Schema;
        using Microsoft.WindowsAzure.Mobile.Service.Tables;
        using System.ComponentModel.DataAnnotations;
        using System;

4. 接下来，将这些额外的属性添加到每个类︰

        [DatabaseGenerated(DatabaseGeneratedOption.Identity)]
        [Index]
        [TableColumn(TableColumnType.CreatedAt)]
        public DateTimeOffset? CreatedAt { get; set; }

        [TableColumn(TableColumnType.Deleted)]
        public bool Deleted { get; set; }

        [Index]
        [TableColumn(TableColumnType.Id)]
        [MaxLength(36)]
        public string Id { get; set; }

        [DatabaseGenerated(DatabaseGeneratedOption.Computed)]
        [TableColumn(TableColumnType.UpdatedAt)]
        public DateTimeOffset? UpdatedAt { get; set; }

        [TableColumn(TableColumnType.Version)]
        [Timestamp]
        public byte[] Version { get; set; }

4. 刚添加的系统属性具有与数据库操作透明地发生一些内置行为 （例如自动更新在创建/更新）。 若要启用这些行为，我们需要对**ExistingContext.cs**进行了更改。 在文件的顶部，添加以下代码︰

        using System.Data.Entity.ModelConfiguration.Conventions;
        using Microsoft.WindowsAzure.Mobile.Service.Tables;
        using System.Linq;

5. 在**ExistingContext**的正文中，重写[**OnModelCreating**](http://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx):

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Conventions.Add(
                new AttributeToColumnAnnotationConvention<TableColumnAttribute, string>(
                    "ServiceTableColumn", (property, attributes) => attributes.Single().ColumnType.ToString()));

            base.OnModelCreating(modelBuilder);
        }

5. 让我们来填充包含一些示例数据的数据库。 打开的文件**WebApiConfig.cs**。 创建新的[**IDatabaseInitializer**](http://msdn.microsoft.com/library/gg696323.aspx) ，并在**注册**方法中进行配置，如下所示。

        using Microsoft.WindowsAzure.Mobile.Service;
        using ShoppingService.Models;
        using System;
        using System.Collections.Generic;
        using System.Collections.ObjectModel;
        using System.Data.Entity;
        using System.Web.Http;

        namespace ShoppingService
        {
            public static class WebApiConfig
            {
                public static void Register()
                {
                    ConfigOptions options = new ConfigOptions();

                    HttpConfiguration config = ServiceConfig.Initialize(new ConfigBuilder(options));

                    Database.SetInitializer(new ExistingInitializer());
                }
            }

            public class ExistingInitializer : ClearDatabaseSchemaIfModelChanges<ExistingContext>
            {
                protected override void Seed(ExistingContext context)
                {
                    List<Order> orders = new List<Order>
                    {
                        new Order { OrderId = 10, Item = "Guitars", Quantity = 2, Id = Guid.NewGuid().ToString()},
                        new Order { OrderId = 20, Item = "Drums", Quantity = 10, Id = Guid.NewGuid().ToString()},
                        new Order { OrderId = 30, Item = "Tambourines", Quantity = 20, Id = Guid.NewGuid().ToString() }
                    };

                    List<Customer> customers = new List<Customer>
                    {
                        new Customer { CustomerId = 1, Name = "John", Orders = new Collection<Order> {
                            orders[0]}, Id = Guid.NewGuid().ToString()},
                        new Customer { CustomerId = 2, Name = "Paul", Orders = new Collection<Order> {
                            orders[1]}, Id = Guid.NewGuid().ToString()},
                        new Customer { CustomerId = 3, Name = "Ringo", Orders = new Collection<Order> {
                            orders[2]}, Id = Guid.NewGuid().ToString()},
                    };

                    foreach (Customer c in customers)
                    {
                        context.Customers.Add(c);
                    }

                    base.Seed(context);
                }
            }
        }

<a name="Mapping"></a>
## 建立 Dto 和模型之间的映射

现在，我们有模型类型**客户**和**订单**和 Dto **MobileCustomer**和**MobileOrder**，但我们需要指示后端，两者之间的自动转换。 这里移动服务依赖于[**AutoMapper**](http://automapper.org/)，对象关系映射器，它已被引用项目中。

1. **WebApiConfig.cs**的顶部添加以下项︰

        using AutoMapper;
        using ShoppingService.DataObjects;

2. 若要定义映射，添加以下内容到**WebApiConfig**类的**注册**方法。

        Mapper.Initialize(cfg =>
        {
            cfg.CreateMap<MobileOrder, Order>();
            cfg.CreateMap<MobileCustomer, Customer>();
            cfg.CreateMap<Order, MobileOrder>()
                .ForMember(dst => dst.MobileCustomerId, map => map.MapFrom(x => x.Customer.Id))
                .ForMember(dst => dst.MobileCustomerName, map => map.MapFrom(x => x.Customer.Name));
            cfg.CreateMap<Customer, MobileCustomer>();

        });

现在，AutoMapper 会将对象映射到另一个。 将匹配具有相应名称的所有属性，例如**MobileOrder.CustomerId**将自动映射到**Order.CustomerId**。 如上所示，在其中我们将**MobileCustomerName**属性映射到**客户**关系属性的**Name**属性，则可以配置自定义映射。

<a name="DomainManager"></a>
## 实现特定于域的逻辑

下一步是实现[**MappedEntityDomainManager**](http://msdn.microsoft.com/library/dn643300.aspx)，作为我们映射的数据存储和从我们的客户提供 HTTP 通信的控制器之间一个抽象层。 我们将能够在下一节单纯的 Dto 中编写我们的控制器，我们在此处添加**MappedEntityDomainManager**将处理与原始数据存储区，同时也给予我们一个位置，以便实现特定于它的任何逻辑的通信。

1. 将**MobileCustomerDomainManager.cs**添加到**模型**文件夹中的项目。 将粘贴下面的实现︰

        using AutoMapper;
        using Microsoft.WindowsAzure.Mobile.Service;
        using ShoppingService.DataObjects;
        using System.Data.Entity;
        using System.Linq;
        using System.Net.Http;
        using System.Threading.Tasks;
        using System.Web.Http;
        using System.Web.Http.OData;

        namespace ShoppingService.Models
        {
            public class MobileCustomerDomainManager : MappedEntityDomainManager<MobileCustomer, Customer>
            {
                private ExistingContext context;

                public MobileCustomerDomainManager(ExistingContext context, HttpRequestMessage request, ApiServices services)
                    : base(context, request, services)
                {
                    Request = request;
                    this.context = context;
                }

                public static int GetKey(string mobileCustomerId, DbSet<Customer> customers, HttpRequestMessage request)
                {
                    int customerId = customers
                       .Where(c => c.Id == mobileCustomerId)
                       .Select(c => c.CustomerId)
                       .FirstOrDefault();

                    if (customerId == 0)
                    {
                        throw new HttpResponseException(request.CreateNotFoundResponse());
                    }
                    return customerId;
                }

                protected override T GetKey<T>(string mobileCustomerId)
                {
                    return (T)(object)GetKey(mobileCustomerId, this.context.Customers, this.Request);
                }

                public override SingleResult<MobileCustomer> Lookup(string mobileCustomerId)
                {
                    int customerId = GetKey<int>(mobileCustomerId);
                    return LookupEntity(c => c.CustomerId == customerId);
                }

                public override async Task<MobileCustomer> InsertAsync(MobileCustomer mobileCustomer)
                {
                    return await base.InsertAsync(mobileCustomer);
                }

                public override async Task<MobileCustomer> UpdateAsync(string mobileCustomerId, Delta<MobileCustomer> patch)
                {
                    int customerId = GetKey<int>(mobileCustomerId);

                    Customer existingCustomer = await this.Context.Set<Customer>().FindAsync(customerId);
                    if (existingCustomer == null)
                    {
                        throw new HttpResponseException(this.Request.CreateNotFoundResponse());
                    }

                    MobileCustomer existingCustomerDTO = Mapper.Map<Customer, MobileCustomer>(existingCustomer);
                    patch.Patch(existingCustomerDTO);
                    Mapper.Map<MobileCustomer, Customer>(existingCustomerDTO, existingCustomer);

                    await this.SubmitChangesAsync();

                    MobileCustomer updatedCustomerDTO = Mapper.Map<Customer, MobileCustomer>(existingCustomer);

                    return updatedCustomerDTO;
                }

                public override async Task<MobileCustomer> ReplaceAsync(string mobileCustomerId, MobileCustomer mobileCustomer)
                {
                    return await base.ReplaceAsync(mobileCustomerId, mobileCustomer);
                }

                public override async Task<bool> DeleteAsync(string mobileCustomerId)
                {
                    int customerId = GetKey<int>(mobileCustomerId);
                    return await DeleteItemAsync(customerId);
                }
            }
        }

    此类的一个重要部分是**GetKey**方法，指出如何在原始数据模型中找到的对象的 ID 属性。

2. 将**MobileOrderDomainManager.cs**添加到**模型**文件夹中的项目︰

        using AutoMapper;
        using Microsoft.WindowsAzure.Mobile.Service;
        using ShoppingService.DataObjects;
        using System.Linq;
        using System.Net.Http;
        using System.Threading.Tasks;
        using System.Web.Http;
        using System.Web.Http.OData;

        namespace ShoppingService.Models
        {
            public class MobileOrderDomainManager : MappedEntityDomainManager<MobileOrder, Order>
            {
                private ExistingContext context;

                public MobileOrderDomainManager(ExistingContext context, HttpRequestMessage request, ApiServices services)
                    : base(context, request, services)
                {
                    Request = request;
                    this.context = context;
                }

                protected override T GetKey<T>(string mobileOrderId)
                {
                    int orderId = this.context.Orders
                        .Where(o => o.Id == mobileOrderId)
                        .Select(o => o.OrderId)
                        .FirstOrDefault();

                    if (orderId == 0)
                    {
                        throw new HttpResponseException(this.Request.CreateNotFoundResponse());
                    }
                    return (T)(object)orderId;
                }

                public override SingleResult<MobileOrder> Lookup(string mobileOrderId)
                {
                    int orderId = GetKey<int>(mobileOrderId);
                    return LookupEntity(o => o.OrderId == orderId);
                }

                private async Task<Customer> VerifyMobileCustomer(string mobileCustomerId, string mobileCustomerName)
                {
                    int customerId = MobileCustomerDomainManager.GetKey(mobileCustomerId, this.context.Customers, this.Request);
                    Customer customer = await this.context.Customers.FindAsync(customerId);
                    if (customer == null)
                    {
                        throw new HttpResponseException(Request.CreateBadRequestResponse("Customer with name '{0}' was not found", mobileCustomerName));
                    }
                    return customer;
                }

                public override async Task<MobileOrder> InsertAsync(MobileOrder mobileOrder)
                {
                    Customer customer = await VerifyMobileCustomer(mobileOrder.MobileCustomerId, mobileOrder.MobileCustomerName);
                    mobileOrder.CustomerId = customer.CustomerId;
                    return await base.InsertAsync(mobileOrder);
                }

                public override async Task<MobileOrder> UpdateAsync(string mobileOrderId, Delta<MobileOrder> patch)
                {
                    Customer customer = await VerifyMobileCustomer(patch.GetEntity().MobileCustomerId, patch.GetEntity().MobileCustomerName);

                    int orderId = GetKey<int>(mobileOrderId);

                    Order existingOrder = await this.Context.Set<Order>().FindAsync(orderId);
                    if (existingOrder == null)
                    {
                        throw new HttpResponseException(this.Request.CreateNotFoundResponse());
                    }

                    MobileOrder existingOrderDTO = Mapper.Map<Order, MobileOrder>(existingOrder);
                    patch.Patch(existingOrderDTO);
                    Mapper.Map<MobileOrder, Order>(existingOrderDTO, existingOrder);

                    // This is required to map the right Id for the customer
                    existingOrder.CustomerId = customer.CustomerId;

                    await this.SubmitChangesAsync();

                    MobileOrder updatedOrderDTO = Mapper.Map<Order, MobileOrder>(existingOrder);

                    return updatedOrderDTO;
                }

                public override async Task<MobileOrder> ReplaceAsync(string mobileOrderId, MobileOrder mobileOrder)
                {
                    await VerifyMobileCustomer(mobileOrder.MobileCustomerId, mobileOrder.MobileCustomerName);

                    return await base.ReplaceAsync(mobileOrderId, mobileOrder);
                }

                public override Task<bool> DeleteAsync(string mobileOrderId)
                {
                    int orderId = GetKey<int>(mobileOrderId);
                    return DeleteItemAsync(orderId);
                }
            }
        }

    在这种情况下的**InsertAsync**和**UpdateAsync**方法是有趣;这就是我们，强制每个**订单**，必须具有有效关联的**客户**的关系。 在**InsertAsync**中，您会发现我们填充的**MobileOrder.CustomerId**属性，它映射到**Order.CustomerId**属性。 我们可以通过基于查找匹配的**MobileOrder.MobileCustomerId****客户**获取该值。 这是因为默认情况下客户端是只感知的移动服务 id (**MobileOrder.MobileCustomerId**) 的**客户**，这是不同于实际主键外键将设置 (**MobileOrder.CustomerId**) 从**订单**到**客户**所需。 这是仅用于内部服务中促进执行插入操作。

现在我们就可以创建公开向我们的客户我们 Dto 的控制器。

<a name="Controller"></a>
## 实现使用 Dto TableController

1. 在**控制器**文件夹中添加文件**MobileCustomerController.cs**:

        using Microsoft.WindowsAzure.Mobile.Service;
        using Microsoft.WindowsAzure.Mobile.Service.Security;
        using ShoppingService.DataObjects;
        using ShoppingService.Models;
        using System.Linq;
        using System.Threading.Tasks;
        using System.Web.Http;
        using System.Web.Http.Controllers;
        using System.Web.Http.OData;

        namespace ShoppingService.Controllers
        {
            public class MobileCustomerController : TableController<MobileCustomer>
            {
                protected override void Initialize(HttpControllerContext controllerContext)
                {
                    base.Initialize(controllerContext);
                    var context = new ExistingContext();
                    DomainManager = new MobileCustomerDomainManager(context, Request, Services);
                }

                public IQueryable<MobileCustomer> GetAllMobileCustomers()
                {
                    return Query();
                }

                public SingleResult<MobileCustomer> GetMobileCustomer(string id)
                {
                    return Lookup(id);
                }

                [AuthorizeLevel(AuthorizationLevel.Admin)]
                protected override Task<MobileCustomer> PatchAsync(string id, Delta<MobileCustomer> patch)
                {
                    return base.UpdateAsync(id, patch);
                }

                [AuthorizeLevel(AuthorizationLevel.Admin)]
                protected override Task<MobileCustomer> PostAsync(MobileCustomer item)
                {
                    return base.InsertAsync(item);
                }

                [AuthorizeLevel(AuthorizationLevel.Admin)]
                protected override Task DeleteAsync(string id)
                {
                    return base.DeleteAsync(id);
                }
            }
        }

    您将注意使用 AuthorizeLevel 特性来限制公共访问控制器上的插入/更新/删除操作。 对于这种情况下，客户列表是只读的但我们将允许创建新的订单并将它们与现有客户关联起来。

2. 在**控制器**文件夹中添加文件**MobileOrderController.cs**:

        using Microsoft.WindowsAzure.Mobile.Service;
        using ShoppingService.DataObjects;
        using ShoppingService.Models;
        using System.Linq;
        using System.Threading.Tasks;
        using System.Web.Http;
        using System.Web.Http.Controllers;
        using System.Web.Http.Description;
        using System.Web.Http.OData;

        namespace ShoppingService.Controllers
        {
            public class MobileOrderController : TableController<MobileOrder>
            {
                protected override void Initialize(HttpControllerContext controllerContext)
                {
                    base.Initialize(controllerContext);
                    ExistingContext context = new ExistingContext();
                    DomainManager = new MobileOrderDomainManager(context, Request, Services);
                }

                public IQueryable<MobileOrder> GetAllMobileOrders()
                {
                    return Query();
                }

                public SingleResult<MobileOrder> GetMobileOrder(string id)
                {
                    return Lookup(id);
                }

                public Task<MobileOrder> PatchMobileOrder(string id, Delta<MobileOrder> patch)
                {
                    return UpdateAsync(id, patch);
                }

                [ResponseType(typeof(MobileOrder))]
                public async Task<IHttpActionResult> PostMobileOrder(MobileOrder item)
                {
                    MobileOrder current = await InsertAsync(item);
                    return CreatedAtRoute("Tables", new { id = current.Id }, current);
                }

                public Task DeleteMobileOrder(string id)
                {
                    return DeleteAsync(id);
                }
            }
        }

3. 现在您可以运行您的服务。 按**f5 键**并使用测试客户端内置帮助页修改的数据。

请注意两个控制器实现使独占使用 Dto 的**MobileCustomer**和**MobileOrder**和不可知的基础模型。 这些 Dto 随时到 JSON 序列化，并可用于与移动服务客户端 SDK 在所有平台上交换数据。 例如，如果构建一个 Windows 应用商店应用程序，如下所示外观相应的客户端类型。 该类型是其他客户端平台上类似。

    using Microsoft.WindowsAzure.MobileServices;
    using System;

    namespace ShoppingClient
    {
        public class MobileCustomer
        {
            public string Id { get; set; }

            public string Name { get; set; }

            [CreatedAt]
            public DateTimeOffset? CreatedAt { get; set; }

            [UpdatedAt]
            public DateTimeOffset? UpdatedAt { get; set; }

            public bool Deleted { get; set; }

            [Version]
            public string Version { get; set; }

        }

    }

作为下一步，您可以现在构建客户端应用程序访问该服务。 有关详细信息，请参阅[添加到现有应用程序的移动服务](mobile-services-dotnet-backend-windows-universal-dotnet-get-started-data.md#update-the-app-to-use-the-mobile-service)。

测试
