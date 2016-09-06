---
ms.openlocfilehash: d1b0b5e0527c36c427a9aebe3e173783416ca447
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
## 在复制期间可再现

数据复制和到关系存储，需要记住一点，以避免意外的成果可再现。 

**注意︰**一块可以重新自动运行在 Azure 数据工厂按照指定的重试策略。 建议以防止瞬间失败重试策略设置。 因此可再现是来处理在数据移动过程中的一个重要方面。 

**作为源︰**

在大多数情况下从关系存储区读取数据时，您想要阅读只对应于该扇区的数据。 要做到这一点的方法就是通过在 Azure 数据工厂中使用的 WindowStart 和 WindowEnd 变量。 阅读有关的变量和函数在 Azure 数据工厂在[计划和执行](data-factory-scheduling-and-execution.md)项目。 示例： 
    
      "source": {
        "type": "SqlSource",
        "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
      },

上面的查询将从 MyTable 切片工期范围内读取数据。 重新运行此片的也始终将确保这种行为。 

在其他情况下，您可能希望阅读整个表 （假设一次仅移动） 可以定义 sqlReaderQuery，如下所示︰

    
    "source": {
                "type": "SqlSource",
                "sqlReaderQuery": "select * from MyTable"
              },
    
