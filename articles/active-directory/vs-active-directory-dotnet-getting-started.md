---
ms.openlocfilehash: fa94f2a3e42d377256b582c6c03dab4b888b92e0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="要开始使用 Azure Active Directory （.NET 项目）" 
    description="如何在 Visual Studio 中使用 Azure Active Directory 开始" 
    services="active-directory" 
    documentationCenter="" 
    authors="patshea123" 
    manager="douge" 
    editor="tglee"/>
  
<tags 
    ms.service="active-directory" 
    ms.workload="web" 
    ms.tgt_pltfrm="vs-getting-started" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/22/2015" 
    ms.author="patshea"/>

# 要开始使用 Azure Active Directory （.NET 项目）

> [AZURE.SELECTOR]
> - [入门教程](vs-active-directory-dotnet-getting-started.md)
> - [发生了什么事](vs-active-directory-dotnet-what-happened.md)
 
##要求验证访问控制器 

您的项目中的所有控制器被都装饰与**授权**特性。 此特性将要求用户进行身份验证才能访问这些控制器。 若要允许匿名访问控制器，从控制器中删除此属性。 如果您想要在更精细的级别设置权限，给需要授权而不是将其应用于控制器类的每个方法应用的特性。
 
##添加登录 / 注销控件 

若要添加登录/注销控件添加到您的视图，您可以使用**_LoginPartial.cshtml**分部视图将功能添加到您的视图之一。 下面是标准的**_Layout.cshtml**视图中添加功能的示例。 （请注意使用类导航栏折叠的 div 中的最后一个元素）︰

<pre>
    &lt;!DOCTYPE html&gt; 
&lt;html&gt; 
&lt;head&gt; 
&lt;meta charset="utf-8" /&gt; 
&lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt; 
&lt;title&gt;@ViewBag.Title - My ASP.NET Application&lt;/title&gt; 
        @Styles.Render("~/Content/css") @Scripts.Render("~/bundles/modernizr") &lt;/head&gt; 
&lt;body&gt; 
&lt;div class="navbar navbar-inverse navbar-fixed-top"&gt; 
&lt;div class="container"&gt; 
&lt;div class="navbar-header"&gt; 
&lt;button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse"&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;spanclass="icon-bar"&gt;&lt;/span&gt; 
&lt;/button&gt; 
                    @Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" }) &lt;/div&gt; 
&lt;div class="navbar-collapse collapse"&gt; 
&lt;ul class="nav navbar-nav"&gt; 
&lt;li&gt;@Html.ActionLink("Home", "Index", "Home")&lt;/li&gt; 
&lt;li&gt;@Html.ActionLink("About", "About", "Home")&lt;/li&gt; 
&lt;li&gt;@Html.ActionLink("Contact", "Contact", "Home")&lt;/li&gt; 
&lt;/ul&gt; 
                    <span style="background-color:yellow">@Html.Partial("_LoginPartial")</span> 
                &lt;/div&gt; 
&lt;/div&gt; 
&lt;/div&gt; 
&lt;div class="container body-content"&gt; 
            @RenderBody() &lt;hr /&gt; 
&lt;页脚&gt; 
&lt;p&gt;&amp;复制;@DateTime.Now.Year-我的 ASP.NET 应用程序&lt;/p&gt; 
&lt;/footer&gt; 
&lt;/div&gt; 
 @Scripts.Render("~/bundles/jquery") @Scripts.Render("~/bundles/bootstrap") @RenderSection("scripts", required: false) &lt;/主体&gt; 
&lt;/html                                                                                                                                                                                                                                                                                                                                                                                                           &gt;
</pre>

[了解更多有关 Azure Active Directory](http://azure.microsoft.com/services/active-directory/) 

测试
