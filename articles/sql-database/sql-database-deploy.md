---
ms.openlocfilehash: 0de61bd4ec95c8986e31a59f2e848f78841f2204
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何将 SQL 数据库部署到 SQL Azure" 
    description="将 SQL Server 数据库部署到 Azure SQL 数据库使用 SQL Server 2016年管理 Studio 中的向导。" 
    services="sql-database" 
    documentationCenter="" 
    authors="sidneyh" 
    manager="jeffreyg" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/17/2015" 
    ms.author="sidneyh"/>


# 如何将一个 SQL Server 数据库部署到 SQL Azure 数据库

在本文中，您将使用**到 SQL Azure 数据库向导部署数据库**上载到 SQL Azure 数据库的示例数据库。 对于本教程，您必须下载**SQL Server 2016年管理 Studio (CTP 2.1)** 。

估计完成时间︰ 15 分钟 （包括下载时间）

> [AZURE.NOTE] 本教程使用的"学校"示例数据库，则有目的地简单;所有的对象都与 SQL Azure 数据库，无需修改或准备数据库以进行迁移。 如果您正在迁移更复杂的现有数据库，也可以考虑使用[SQL 数据库迁移向导](http://sqlazuremw.codeplex.com/)，请参见本[概述](sql-database-cloud-migrate.md)。

## 先决条件

一种**Microsoft Azure 帐户**。 免费试用版，请参阅此[提供](http://azure.microsoft.com/pricing/free-trial/)。

下载[**SQL Server 管理 Studio**](https://msdn.microsoft.com/library/mt238290.aspx)。  （有关此工具的详细信息，请参见[SQL Server 管理 Studio 6 月 2015年发行说明](https://msdn.microsoft.com/library/mt238486.aspx)）。

现有的**SQL Azure 数据库服务器**。 为了创建一个服务器，您必须先创建至少一个数据库。 在创建数据库时，必须选择新服务器或现有服务器上创建它。 有关创建新数据库 （在新服务器上） 的说明，请参阅[创建第一个 SQL Azure 数据库](sql-database-get-started.md)。

## 在内部服务器上创建 school 数据库

在 SQL Server 管理 Studio (SSMS) 创建的"学校"数据库的内部部署版本中运行这些脚本。

1. 在 SSMS，连接到本地服务器。 用鼠标右键单击**数据库**，单击**新建数据库**并输入*学校*。

2. 在*学校*上右击，然后单击**新建查询**。 

3. 复制并执行此脚本︰ 

<div style="width:auto; height:300px; overflow:auto"><pre>
--创建部门表。
如果不存在 (选择 * sys.objects 从位置省略 object_id = 省略 OBJECT_ID (N'[dbo]。 [部门])，然后键入在 (N'U')) 开始创建表 [dbo]。[部门]([DepartmentID] [int] 不为 NULL，[名称] [nvarchar](50)不空、 [预算] [钱] 不空，[开始日期] [日期] 不空、 管理员 [int] 空，约束 [PK_Department] 主要密钥聚集 ([DepartmentID] ASC) 替换 (失败 = 关闭))结束;转到

    -- Create the Person table.
    IF NOT EXISTS (SELECT * FROM sys.objects 
        WHERE object_id = OBJECT_ID(N'[dbo].[Person]') 
        AND type in (N'U'))
    BEGIN
    CREATE TABLE [dbo].[Person](
        [PersonID] [int] IDENTITY(1,1) NOT NULL,
        [LastName] [nvarchar](50) NOT NULL,
        [FirstName] [nvarchar](50) NOT NULL,
        [HireDate] [datetime] NULL,
        [EnrollmentDate] [datetime] NULL,
     CONSTRAINT [PK_School.Student] PRIMARY KEY CLUSTERED   
    (
    [PersonID] ASC
    )WITH (IGNORE_DUP_KEY = OFF)
    ) 
    END;
    GO

    -- Create the OnsiteCourse table.
    IF NOT EXISTS (SELECT * FROM sys.objects 
        WHERE object_id = OBJECT_ID(N'[dbo].[OnsiteCourse]') 
        AND type in (N'U'))
    BEGIN
    CREATE TABLE [dbo].[OnsiteCourse](
        [CourseID] [int] NOT NULL,
        [Location] [nvarchar](50) NOT NULL,
        [Days] [nvarchar](50) NOT NULL,
        [Time] [smalldatetime] NOT NULL,
     CONSTRAINT [PK_OnsiteCourse] PRIMARY KEY CLUSTERED 
    (
        [CourseID] ASC
    )WITH (IGNORE_DUP_KEY = OFF)
    ) 
    END;
    GO

    -- Create the OnlineCourse table.
    IF NOT EXISTS (SELECT * FROM sys.objects 
        WHERE object_id = OBJECT_ID(N'[dbo].[OnlineCourse]') 
        AND type in (N'U'))
    BEGIN
    CREATE TABLE [dbo].[OnlineCourse](
        [CourseID] [int] NOT NULL,
        [URL] [nvarchar](100) NOT NULL,
     CONSTRAINT [PK_OnlineCourse] PRIMARY KEY CLUSTERED 
    (
        [CourseID] ASC
    )WITH (IGNORE_DUP_KEY = OFF)
    ) 
    END;
    GO

    --Create the StudentGrade table.
    IF NOT EXISTS (SELECT * FROM sys.objects 
        WHERE object_id = OBJECT_ID(N'[dbo].[StudentGrade]') 
        AND type in (N'U'))
    BEGIN
    CREATE TABLE [dbo].[StudentGrade](
        [EnrollmentID] [int] IDENTITY(1,1) NOT NULL,
        [CourseID] [int] NOT NULL,
        [StudentID] [int] NOT NULL,
        [Grade] [decimal](3, 2) NULL,
     CONSTRAINT [PK_StudentGrade] PRIMARY KEY CLUSTERED 
    (
        [EnrollmentID] ASC
    )WITH (IGNORE_DUP_KEY = OFF)
    ) 
    END;
    GO

    -- Create the CourseInstructor table.
    IF NOT EXISTS (SELECT * FROM sys.objects 
        WHERE object_id = OBJECT_ID(N'[dbo].[CourseInstructor]') 
        AND type in (N'U'))
    BEGIN
    CREATE TABLE [dbo].[CourseInstructor](
        [CourseID] [int] NOT NULL,
        [PersonID] [int] NOT NULL,
     CONSTRAINT [PK_CourseInstructor] PRIMARY KEY CLUSTERED 
    (
        [CourseID] ASC,
        [PersonID] ASC
    )WITH (IGNORE_DUP_KEY = OFF)
    ) 
    END;
    GO

    -- Create the Course table.
    IF NOT EXISTS (SELECT * FROM sys.objects 
        WHERE object_id = OBJECT_ID(N'[dbo].[Course]') 
        AND type in (N'U'))
    BEGIN
    CREATE TABLE [dbo].[Course](
        [CourseID] [int] NOT NULL,
        [Title] [nvarchar](100) NOT NULL,
        [Credits] [int] NOT NULL,
        [DepartmentID] [int] NOT NULL,
     CONSTRAINT [PK_School.Course] PRIMARY KEY CLUSTERED 
    (
        [CourseID] ASC
    )WITH (IGNORE_DUP_KEY = OFF)
    )
    END;
    GO

    -- Create the OfficeAssignment table.
    IF NOT EXISTS (SELECT * FROM sys.objects 
        WHERE object_id = OBJECT_ID(N'[dbo].[OfficeAssignment]')
        AND type in (N'U'))
    BEGIN
    CREATE TABLE [dbo].[OfficeAssignment](
        [InstructorID] [int] NOT NULL,
        [Location] [nvarchar](50) NOT NULL,
        [Timestamp] [timestamp] NOT NULL,
     CONSTRAINT [PK_OfficeAssignment] PRIMARY KEY CLUSTERED 
    (
        [InstructorID] ASC
    )WITH (IGNORE_DUP_KEY = OFF)
    )
    END;
    GO

    -- Define the relationship between OnsiteCourse and Course.
    IF NOT EXISTS (SELECT * FROM sys.foreign_keys 
       WHERE object_id = OBJECT_ID(N'[dbo].[FK_OnsiteCourse_Course]')
       AND parent_object_id = OBJECT_ID(N'[dbo].[OnsiteCourse]'))
    ALTER TABLE [dbo].[OnsiteCourse]  WITH CHECK ADD  
       CONSTRAINT [FK_OnsiteCourse_Course] FOREIGN KEY([CourseID])
    REFERENCES [dbo].[Course] ([CourseID]);
    GO
    ALTER TABLE [dbo].[OnsiteCourse] CHECK 
       CONSTRAINT [FK_OnsiteCourse_Course];
    GO

    -- Define the relationship between OnlineCourse and Course.
    IF NOT EXISTS (SELECT * FROM sys.foreign_keys 
       WHERE object_id = OBJECT_ID(N'[dbo].[FK_OnlineCourse_Course]')
       AND parent_object_id = OBJECT_ID(N'[dbo].[OnlineCourse]'))
    ALTER TABLE [dbo].[OnlineCourse]  WITH CHECK ADD  
       CONSTRAINT [FK_OnlineCourse_Course] FOREIGN KEY([CourseID])
    REFERENCES [dbo].[Course] ([CourseID]);
    GO
    ALTER TABLE [dbo].[OnlineCourse] CHECK 
       CONSTRAINT [FK_OnlineCourse_Course];
    GO
    -- Define the relationship between StudentGrade and Course.
    IF NOT EXISTS (SELECT * FROM sys.foreign_keys 
       WHERE object_id = OBJECT_ID(N'[dbo].[FK_StudentGrade_Course]')
       AND parent_object_id = OBJECT_ID(N'[dbo].[StudentGrade]'))
    ALTER TABLE [dbo].[StudentGrade]  WITH CHECK ADD  
       CONSTRAINT [FK_StudentGrade_Course] FOREIGN KEY([CourseID])
    REFERENCES [dbo].[Course] ([CourseID]);
    GO
    ALTER TABLE [dbo].[StudentGrade] CHECK 
       CONSTRAINT [FK_StudentGrade_Course];
    GO

    --Define the relationship between StudentGrade and Student.
    IF NOT EXISTS (SELECT * FROM sys.foreign_keys 
       WHERE object_id = OBJECT_ID(N'[dbo].[FK_StudentGrade_Student]')
       AND parent_object_id = OBJECT_ID(N'[dbo].[StudentGrade]'))   
    ALTER TABLE [dbo].[StudentGrade]  WITH CHECK ADD  
       CONSTRAINT [FK_StudentGrade_Student] FOREIGN KEY([StudentID])
    REFERENCES [dbo].[Person] ([PersonID]);
    GO
    ALTER TABLE [dbo].[StudentGrade] CHECK 
       CONSTRAINT [FK_StudentGrade_Student];
    GO

    -- Define the relationship between CourseInstructor and Course.
    IF NOT EXISTS (SELECT * FROM sys.foreign_keys 
     WHERE object_id = OBJECT_ID(N'[dbo].[FK_CourseInstructor_Course]')
     AND parent_object_id = OBJECT_ID(N'[dbo].[CourseInstructor]'))
    ALTER TABLE [dbo].[CourseInstructor]  WITH CHECK ADD  
     CONSTRAINT [FK_CourseInstructor_Course] FOREIGN KEY([CourseID])
    REFERENCES [dbo].[Course] ([CourseID]);
    GO
    ALTER TABLE [dbo].[CourseInstructor] CHECK 
      CONSTRAINT [FK_CourseInstructor_Course];
    GO

    -- Define the relationship between CourseInstructor and Person.
    IF NOT EXISTS (SELECT * FROM sys.foreign_keys 
      WHERE object_id = OBJECT_ID(N'[dbo].[FK_CourseInstructor_Person]')
       AND parent_object_id = OBJECT_ID(N'[dbo].[CourseInstructor]'))
    ALTER TABLE [dbo].[CourseInstructor]  WITH CHECK ADD  
      CONSTRAINT [FK_CourseInstructor_Person] FOREIGN KEY([PersonID])
    REFERENCES [dbo].[Person] ([PersonID]);
    GO
    ALTER TABLE [dbo].[CourseInstructor] CHECK 
     CONSTRAINT [FK_CourseInstructor_Person];
    GO

    -- Define the relationship between Course and Department.
    IF NOT EXISTS (SELECT * FROM sys.foreign_keys 
       WHERE object_id = OBJECT_ID(N'[dbo].[FK_Course_Department]')
       AND parent_object_id = OBJECT_ID(N'[dbo].[Course]'))
    ALTER TABLE [dbo].[Course]  WITH CHECK ADD  
       CONSTRAINT [FK_Course_Department] FOREIGN KEY([DepartmentID])
    REFERENCES [dbo].[Department] ([DepartmentID]);
    GO
    ALTER TABLE [dbo].[Course] CHECK CONSTRAINT [FK_Course_Department];
    GO

    --Define the relationship between OfficeAssignment and Person.
    IF NOT EXISTS (SELECT * FROM sys.foreign_keys 
      WHERE object_id = OBJECT_ID(N'[dbo].[FK_OfficeAssignment_Person]')
      AND parent_object_id = OBJECT_ID(N'[dbo].[OfficeAssignment]'))
    ALTER TABLE [dbo].[OfficeAssignment]  WITH CHECK ADD  
      CONSTRAINT [FK_OfficeAssignment_Person] FOREIGN KEY([InstructorID])
    REFERENCES [dbo].[Person] ([PersonID]);
    GO
    ALTER TABLE [dbo].[OfficeAssignment] CHECK 
     CONSTRAINT [FK_OfficeAssignment_Person];
    GO
</pre></div>

下一步，复制并执行数据插入脚本。

<div style="width:auto; height:300px; overflow:auto"><pre>
-向人表中插入数据。
设置 IDENTITY_INSERT dbo。人开启;请插入到 dbo。人的过程，姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (1，Abercrombie，Kim，"1995年-03-11，空);插入到 dbo。人的过程，姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (2，Barzdukas、 Gytis，则为空，2005年-09-01);插入到 dbo。人过程、 姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (3，司法，Peggy'，而空，2001年-09-01);插入到 dbo。人的过程，姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (4 日 'Fakhouri'，'Fadi' ' 2002年-08-06 年，空);插入到 dbo。人过程、 姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (5 日，Harui，李文"1998年-07-01，空);插入到 dbo。人的过程，姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (6，Li、 Yan 为 null，' 2002年-09-01);插入到 dbo。人的过程，姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (7，诺曼、 刘娜为 null，2003年-09-01);插入到 dbo。人的过程，姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (8，Olivotto、 Nino 为 null，2005年-09-01);插入到 dbo。人的过程，姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (9，Tang'、 '于韦恩，则为 null，2005年-09-01);插入到 dbo。人的过程，姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (10，'Alonso'、 'Meredith 为 null，' 2002年-09-01);插入到 dbo。过程、 姓氏、 名字、 雇佣日期 （EnrollmentDate） 的人值 (11，Lopez' 'Sophia'，而空，2004年-09-01);插入到 dbo。人的过程，姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (12 日 'Browning'，'Meredith 为 null，' 2000年-09-01);插入到 dbo。过程、 姓氏、 名字、 雇佣日期 （EnrollmentDate） 的人值 (13，Anand' 'Arturo'，而空，2003年-09-01);插入到 dbo。人的过程，姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (14，查看器、 Alexandra 为 null，' 2000年-09-01);插入到 dbo。人的过程，姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (15，Powell'、 '没有意义，为空，2004年-09-01);插入到 dbo。人过程、 姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (16，Jai 'Damien'，而空，2001年-09-01);插入到 dbo。人的过程，姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (17，Carlson'、 'Robyn 为 null，2005年-09-01);插入到 dbo。人的过程，姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (18 日，Zheng'，李文，"2004年-02-12，空);插入到 dbo。人的过程，姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (19，Bryant'、 '没有意义，为空，2001年-09-01);插入到 dbo。过程、 姓氏、 名字、 雇佣日期 （EnrollmentDate） 的人值 (20，Suarez' 'Robyn'，而空，2004年-09-01);插入到 dbo。人的过程，姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (21，兵威、 李文为 null，2004年-09-01);插入到 dbo。人的过程，姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (22，Alexander'、 '没有意义，为空，2005年-09-01);插入到 dbo。人的过程，姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (23，Morgan'、 'Isaiah 为 null，' 2001年-09-01);插入到 dbo。人过程、 姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (null 24，马丁、 美丽，2005年-09-01);插入到 dbo。人过程、 姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (25 日，Kapoor'，'Candace' ' 2001年-01-15，空);插入到 dbo。人的过程，姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (26，Rogers'、 'Cody 为 null，' 2002年-09-01);插入到 dbo。人的过程，姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (27 日 'Serrano'，'Stacy' ' 1999年-06-01，空);插入到 dbo。人的过程，姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (28 日，白，安为 null，' 2001年-09-01);插入到 dbo。过程、 姓氏、 名字、 雇佣日期 （EnrollmentDate） 的人值 (29，怪兽，Rachel'，空的 ' 2004年-09-01);插入到 dbo。人的过程，姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (30 日，山，艾丽西亚，则为 null，2003年-09-01);插入到 dbo。人的过程，姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (31，Stewart'、 'Jasmine，"1997年-10-12，空);插入到 dbo。人的过程，姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (32，Xu'，'Kristen' ' 2001年-7-23，空);插入到 dbo。人过程、 姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (33，Gao 'Erica'，而空，2003年-01-30);插入到 dbo。人的过程，姓氏、 名字、 雇佣日期 （EnrollmentDate） 值 (34，Van Houten、 李文 ' 2000年-12-07，空);转到设置 IDENTITY_INSERT dbo。人从;转-将数据插入到部门表。
插入到 dbo。部门 (DepartmentID、 [名称] 预算，开始日期，管理员) 值 (1，工程 350000.00，2007 年-09-01，2);插入到 dbo。部门 (DepartmentID、 [名称] 预算，开始日期，管理员) 值 (2，英语，120000.00，"2007年-09-01，6);插入到 dbo。部门 (DepartmentID、 [名称] 预算，开始日期，管理员) 值 (4，经济效益，200000.00，2007 年-09-01，4);插入到 dbo。部门 (DepartmentID、 [名称] 预算，开始日期，管理员) 值 (7、 数学，250000.00，"2007年-09-01，3);转-将数据插入到课程目录。
插入到 dbo。课程 （CourseID、 标题、 信用、 DepartmentID） 值 (1050，'化学'，4，1);插入到 dbo。课程 （CourseID、 标题、 信用、 DepartmentID） 值 (1061，'物理'，4，1);插入到 dbo。课程 （CourseID、 标题、 信用、 DepartmentID） 值 (1045，微积分 4、 7);插入到 dbo。课程 （CourseID、 标题、 信用、 DepartmentID） 值 (2030，诗歌，2，2);插入到 dbo。课程 （CourseID、 标题、 信用、 DepartmentID） 值 (2021，组成 3，2);插入到 dbo。课程 （CourseID、 标题、 信用、 DepartmentID） 值 (2042，宣传资料'，4，2);插入到 dbo。课程 （CourseID、 标题、 信用、 DepartmentID） 值 （4022，微观经济学，3，4）;插入到 dbo。课程 （CourseID、 标题、 信用、 DepartmentID） 值 (4041，' Macroeconomics'，3，4);插入到 dbo。课程 （CourseID、 标题、 信用、 DepartmentID） 值 （4061，定量，2，4）;插入到 dbo。课程 （CourseID、 标题、 信用、 DepartmentID） 值 （3141，三角学'，4，7）;转-将数据插入到 OnlineCourse 表。
插入到 dbo。OnlineCourse （CourseID，URL） 值 (2030，http://www.fineartschool.net/Poetry);插入到 dbo。OnlineCourse （CourseID，URL） 值 (2021，http://www.fineartschool.net/Composition);插入到 dbo。OnlineCourse （CourseID，URL） 值 (4041，http://www.fineartschool.net/Macroeconomics);插入到 dbo。OnlineCourse （CourseID，URL） 值 (3141，http://www.fineartschool.net/Trigonometry);-用 OnsiteCourse 表插入数据。
插入到 dbo。OnsiteCourse （CourseID，位置，天，[时间]） 值 (1050，'123 Smith'，'MTWH'，' 11:30);插入到 dbo。OnsiteCourse （CourseID，位置，天，[时间]） 值 (1061，'234 Smith'，'TWHF'，' 13:15 ');插入到 dbo。OnsiteCourse （CourseID，位置，天，[时间]） 值 (1045，'121 Smith'，'MWHF'，' 15:30);插入到 dbo。OnsiteCourse （CourseID，位置，天，[时间]） 值 (4061，' 22 Williams，'日'，' 11:15 ');插入到 dbo。OnsiteCourse （CourseID，位置，天，[时间]） 值 (2042，225 Adams'，'MTWH'，' 11:00 ');插入到 dbo。OnsiteCourse （CourseID，位置，天，[时间]） 值 (4022，Williams ' 23'、 'MWF'，' 9:00 ');-向 CourseInstructor 表中插入数据。
插入到 dbo。CourseInstructor （CourseID、 过程） 值 (1050，1);插入到 dbo。CourseInstructor （CourseID、 过程） 值 (1061，31);插入到 dbo。CourseInstructor （CourseID、 过程） 值 (1045，5);插入到 dbo。CourseInstructor （CourseID、 过程） 值 (2030，4);插入到 dbo。CourseInstructor （CourseID、 过程） 值 (2021，27);插入到 dbo。CourseInstructor （CourseID、 过程） 值 (2042，25);插入到 dbo。CourseInstructor （CourseID、 过程） 值 4022 (18）;插入到 dbo。CourseInstructor （CourseID、 过程） 值 4041 (32）;插入到 dbo。CourseInstructor （CourseID、 过程） 值 4061 (34）;转到-向 OfficeAssignment 表中插入数据。
插入到 dbo。OfficeAssignment （InstructorID，位置） 值 (1，'17 Smith');插入到 dbo。OfficeAssignment （InstructorID，位置） 值 (4，"29 Adams);插入到 dbo。OfficeAssignment （InstructorID，位置） 值 (5，"37 Williams);插入到 dbo。OfficeAssignment （InstructorID，位置） 值 (18 '143 Smith');插入到 dbo。OfficeAssignment （InstructorID，位置） 值 (25，"57 Adams);插入到 dbo。OfficeAssignment （InstructorID，位置） 值 (27、"271 Williams 的);插入到 dbo。OfficeAssignment （InstructorID，位置） 值 (31，'131 Smith');插入到 dbo。OfficeAssignment （InstructorID，位置） 值 (32、"203 Williams 的);插入到 dbo。OfficeAssignment （InstructorID，位置） 值 (34，'213 Smith');-向 StudentGrade 表中插入数据。
插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (2021，2，4);插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (2030，2、 3.5 英寸);插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (2021，3，3);插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (2030，3，4);插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (2021，6、 2.5);插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (2042，6、 3.5 英寸);插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (2021，7、 3.5 英寸);插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (2042，7，4);插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (2021，8，3);插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (2042，8，3);插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (4041、 9，3.5 英寸);插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (4041，10，则为 null)。插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (4041、 11 日 2.5);插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (4041，12，则为 null)。插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (4061，12，则为 null)。插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 4022、 14 (3）;插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 4022、 13 (4）;插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 4061、 13 (4）;插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 4041、 14 (3）;插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (4022、 15 日 2.5);插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 4022、 16 (2）;插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (4022，17 日，则为 null)。插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (4022、 19 日，3.5 英寸);插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 4061、 20 (4）;插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 4061、 21 (2）;插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 4022，22 (3）;插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (4041、 22 日，3.5 英寸);插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (4061、 22 日 2.5);插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 4022，23 (3）;插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (1045，23、 1.5);插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (1061，24，4);插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (1061，25 日，3);插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (1050，26、 3.5 英寸);插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (1061，26、 3);插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (1061，27、 3);插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (1045，28、 2.5);插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (1050，28、 3.5 英寸);插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (1061，29，4);插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (1050，30、 3.5 英寸);插入到 dbo。StudentGrade （CourseID，StudentID，年级） 的值 (1061，30，4);转到
</pre></div>
    
现在，您有一个内部数据库，您可以将它们导出到 Azure。 接下来，您将运行向导将创建一个.bacpac 文件，将软件加载到 Azure，并将其导入到 SQL 数据库。

    
## 将数据库部署到 SQL Azure 
    
1. 在管理 Studio 中，右击刚刚创建的学校数据库，指向**任务**，单击**部署到 Microsoft Azure SQL 数据库的数据库**。
2. 中的**部署设置**，输入数据库，如*学校*的名称。
5. 单击**连接**。 要解决连接问题，请尝试使用此[故障诊断程序](https://support2.microsoft.com/common/survey.aspx?scid=sw;en;3844&showpage=1)。
6. 在**服务器名称**，输入 10 个字符的服务器名称后, 跟**。 database.windows.net**
7. 在**身份验证**，选择**SQL Server 身份验证**。
8. 输入管理员登录名和资源调配的 SQL 数据库逻辑服务器时创建的密码。
9. 单击**选项**。
10. 在连接属性在**连接到数据库**中，键入**主**。

    **请注意**要在 SQL Azure 数据库服务器上创建数据库时，您必须连接到**主**数据库。 
11. 单击**连接**。 此步骤结束连接规范，并将向导返回。
12. 单击**下一步**，然后单击**完成**以运行此向导。

    
## 如何︰ 验证数据库部署
    
1. 在管理 Studio 中，在**对象资源管理器中**，单击**连接**图标。
2. 在**服务器**名称框中，键入 SQL Azure 服务器，跟**database.windows.net**的名称
3. 在**身份验证**，选择**SQL Server 身份验证**。
4. 输入管理员登录名和资源调配服务器时创建的密码。 
5. 单击**选项**按钮。
6. 单击**连接到数据库**下拉列表，然后单击**浏览服务器**。 在下面的对话框中，单击**是**以允许浏览服务器。
7. 单击**school**数据库以选择它，然后单击**确定**。 
8. 然后单击**连接**。 要解决连接问题，请尝试使用此[故障诊断程序](https://support2.microsoft.com/common/survey.aspx?scid=sw;en;3844&showpage=1)。
2. 展开**数据库**文件夹。 您应该看到**学校**数据库列表中。

    **请注意**您必须连接到您要查询的数据库。 
3. **学校**用鼠标右键单击，并单击**新建查询**。
4. 执行以下查询以验证数据可以访问。

        SELECT
            Course.Title as "Course Title"
                ,Department.Name as "Department"
                ,Person.LastName as "Instructor"
                ,OnsiteCourse.Location as "Location"
                ,OnsiteCourse.Days as "Days"
                ,OnsiteCourse.Time as "Time"
        FROM
             Course
             INNER JOIN Department
              ON Course.DepartmentID = Department.DepartmentID
             INNER JOIN CourseInstructor
               ON Course.CourseID = CourseInstructor.CourseID
             INNER JOIN Person
               ON CourseInstructor.PersonID = Person.PersonID
             INNER JOIN OnsiteCourse
        ON OnsiteCourse.CourseID = CourseInstructor.CourseID;
        
## 下一步行动

创建新的 SQL Azure 数据库的教程，请参阅[SQL 数据库管理入门知识](sql-database-get-started.md)。 从 C# 应用程序连接到 SQL Azure 数据库的基本知识，请参阅[连接到并查询您的 SQL 数据库与 C#](sql-database-connect-query.md)。 有关在从不同的平台 （如 PHP) 的连接上的更多教程，请参见[Azure SQL 数据库开发︰ 帮助主题](https://msdn.microsoft.com/library/azure/ee621787.aspx)。

 