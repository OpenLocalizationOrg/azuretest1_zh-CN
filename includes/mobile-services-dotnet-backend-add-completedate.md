---
ms.openlocfilehash: df0966409dc1b8300f8960178a6ec43148c46954
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
在本节中我们将通过添加一个名为**CompleteDate**的新时间戳字段修改我们的数据库模型。 此字段将记录已完成的 todo 项，最后一次。 实体框架将更新数据库，根据我们使用从[DropCreateDatabaseIfModelChanges](http://go.microsoft.com/fwlink/?LinkId=394621)派生的默认数据库初始值设定项类的模型更改。 

1. 在解决方案资源管理器中的 Visual Studio，展开的**App_Start**文件夹中的任务列表服务项目。 打开 WebApiConfig.cs 文件。

2. 在 WebApiConfig.cs 文件中，请注意您默认数据库初始值设定项的类从派生`DropCreateDatabaseIfModelChanges`类。 这意味着对该模型的任何更改将导致表会删除并重新创建，以适应新的模型。 因此，表中的数据将会丢失，表格将重新植入。 修改数据库初始值设定项种子方法，使种子数据，如下所示将 WebApiConfig.cs 文件保存为是。

    >[AZURE.NOTE] 时使用的默认数据库初始值设定项，实体框架将除去并重新创建数据库，在第一个代码模型定义中检测到数据模型更改时。 若要使此数据模型更改和维护数据库中的现有数据，则必须使用代码第一个迁移。 有关详细信息，请参阅[如何使用代码第一个迁移来更新数据模型](../articles/mobile-services-dotnet-backend-how-to-use-code-first-migrations.md)。

        List<TodoItem> todoItems = new List<TodoItem>
        {
          new TodoItem { Id = "1", Text = "First seed item", Complete = false },
          new TodoItem { Id = "2", Text = "Second seed item", Complete = false },
        };
     

3. 在解决方案资源管理器中的 Visual Studio，展开任务列表服务项目中的**DataObjects**文件夹。 打开 TodoItem.cs 文件，并更新以包括 CompleteDate 字段，如下所示的 TodoItem 类。 然后将保存的 TodoItem.cs 文件。

        public class TodoItem : EntityData
        {
          public string Text { get; set; }
          public bool Complete { get; set; }
          public System.DateTime? CompleteDate { get; set; }
        }

4. 在解决方案资源管理器中的 Visual Studio，展开任务列表服务项目中的**控制器**文件夹。 打开 TodoItemController.cs 文件并更新`PatchTodoItem`因此**Complete**属性更改从假到真时，它将设置**CompleteDate**的方法。 然后将保存的 TodoItemController.cs 文件。

        public Task<TodoItem> PatchTodoItem(string id, Delta<TodoItem> patch)
        {
            // If complete is being set to true and was false, set CompleteDate
            if ((patch.GetEntity().Complete == true) &&
                (GetTodoItem(id).Queryable.Single().Complete == false))
            {
                patch.TrySetPropertyValue("CompleteDate", System.DateTime.Now);
            }
            return UpdateAsync(id, patch);
        }


5. 重新生成任务列表.NET 后端服务项目，请确保您有没有生成错误。 

接下来，您将更新以显示新的**CompleteDate**数据客户端应用程序。