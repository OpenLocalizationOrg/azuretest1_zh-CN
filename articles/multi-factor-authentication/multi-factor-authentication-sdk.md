---
ms.openlocfilehash: 270c457e54e438c9b45149f2e26f53decc09b756
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将与 Azure Active Directory 集成内部标识。" 
    description="这是 Azure 的 AD 连接的描述它是什么以及为什么会使用它。" 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtand"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2015" 
    ms.author="billmath"/>

# 构建多元身份验证到自定义应用程序 (SDK)

Azure 多因素身份验证软件开发工具包 (SDK)，可以生成登录或事务进程中直接电话联络和文本消息验证 Azure AD 租户中应用程序。

多因素身份验证 SDK 是可用的 C#、 Visual Basic (.NET)、 Java、 Perl、 PHP 和拼音。 该 SDK 提供了解决多因素身份验证的瘦包装。 它包括一切您需要编写代码，包括带有注释的源代码文件、 示例文件和详细的自述文件。 每个 SDK 还包含证书和私钥进行加密事务处理所独有的多因素身份验证提供程序。 只要有一个提供程序，您可以下载 SDK 中尽可能多的语言和格式，您可以根据需要。

结构的多因素身份验证 SDK 中的 Api 是非常简单的。 请到与多因子选项参数，如验证模式和用户数据，如要呼叫的电话号码或 PIN 号，以验证 API 调用一个函数。 Api 将转换函数调用为 web 服务请求的基于云的 Azure 多因素身份验证服务。 所有的调用必须包括对每个 SDK 中包括的私有证书的引用。

由于 Api 没有 Azure Active Directory 中注册的用户访问，您必须提供用户信息，如电话号码和 PIN 代码，文件或数据库中。 此外，Api 不提供注册或用户管理功能，因此您需要在您的应用程序中构建这些进程。






## Azure 多因素身份验证 SDK 下载 

有两种不同方法，您可以下载 Azure 的多因素身份验证 SDK。 两者都是通过 Azure 的门户来完成的。  首先是通过直接管理多因素身份验证提供程序。  第二个是通过服务设置。  第二个选项需要多因素身份验证提供程序或 Azure 广告增值许可证。


### 从 Azure 的门户网站下载 Azure 的多因素身份验证 SDK


1. 在 Azure 门户以管理员身份登录。
2. 在左边，选择活动目录。
3. 在活动的目录页上，单击顶部**多因素身份验证提供程序**
4. 在底部单击**管理**
5. 这将打开一个新页面。  在左边，在底部，单击 SDK。
<center>![下载](./media/multi-factor-authentication-sdk/download.png)</center>
6. 选择的语言，单击其中一个相关的下载链接。
7. 将下载文件保存。



### 若要下载 Azure 多因素身份验证 SDK 通过服务设置


1. 在 Azure 门户以管理员身份登录。
2. 在左边，选择活动目录。
3. 双击您的实例的 Azure 的广告。
4. 在顶部单击**配置**
5. 在多因素身份验证，选择**管理服务设置**
![下载](./media/multi-factor-authentication-sdk/download2.png)
6. 在服务设置页中，在屏幕的底部单击**转至门户**。
![下载](./media/multi-factor-authentication-sdk/download3.png)
7. 这将打开一个新页面。  在左边，在底部，单击 SDK。
8. 选择的语言，单击其中一个相关的下载链接。
9. 将下载文件保存。

## Azure 多因素身份验证 SDK 中的内容
在 SDK 中，您将了解以下各项︰

- **自述文件**。 解释如何在新的或现有的应用程序中使用多因素身份验证 Api。
- **源文件**的多因素身份验证
- 使用与多因素身份验证服务进行通信的**客户端证书**
- 证书的**专用密钥**
- **调用的结果。** 调用的结果代码的列表。 若要打开此文件，请使用文本格式，如写字板应用程序。 使用调用结果代码进行测试并解决多因素身份验证的应用程序中的实现。 它们不是身份验证状态代码。
- **示例。** 多因素身份验证的基本工作实现的示例代码。


>[AZURE.WARNING]客户端证书是特别为您生成一个唯一的私有证书。 不要共享或丢失此文件。 它是确保安全的多因素身份验证服务与您通信的关键。

## 代码示例︰ 标准模式电话验证

此代码示例演示如何使用 Azure 的多因素身份验证 SDK 中的 Api 将标准模式语音呼叫验证添加到您的应用程序。 标准模式是通过按 # 键来响应用户的电话。

此示例使用 C#.NET 2.0 多因素身份验证 SDK 在基本 ASP.NET 应用程序中使用 C# 服务器端逻辑，但过程很相似，在其他语言中的简单实现。 由于 SDK 包含源代码文件，不包含可执行文件，可以生成的文件和引用它们或将它们包含在应用程序中直接。

>[AZURE.NOTE]在实现多因素身份验证，使用其他因素作为二级或三级验证补充首要身份验证方法。 这些方法都不用作主要的身份验证方法。

### 代码示例概述
一个非常简单的 web 演示应用程序的示例代码使用电话 # 键响应完成用户身份验证。 此电话的系数被称为在多因素身份验证标准模式。

客户端代码不包含任何特定的多因素身份验证元素。 由于额外的身份验证因素是独立于主的身份验证，您可以添加它们而无需更改现有的登录界面。 多因素 SDK 中的 Api 允许您自定义的用户体验，但是您可能不需要更改任何内容根本。

服务器端代码添加步骤 2 中的标准模式身份验证。 具有所需的标准模式验证的参数创建一个 PfAuthParams 对象︰ 用户名，电话号码，和模式，以及需要在每次调用的客户端证书 (CertFilePath) 的路径。 PfAuthParams 中的所有参数的演示，请参见 SDK 中的示例文件。

接下来，该代码将 PfAuthParams 对象传递给 pf_authenticate() 函数。 返回值指示成功或失败的身份验证。 Out 参数，callStatus 和 errorID，包含附加的调用结果的信息。 在 SDK 中的调用结果文件中记录呼叫结果代码。

只需几行中，可以编写此最小实现。 但是，在生产代码中，将包含更复杂的错误处理、 附加的数据库代码和增强的用户体验。

### Web 客户端代码

下面是演示页的 web 客户端代码。

    
    <%@ Page Language="C#" AutoEventWireup="true" CodeFile="Default.aspx.cs" Inherits="_Default" %>
    
    <!DOCTYPE html>
    
    <html xmlns="http://www.w3.org/1999/xhtml">
    <head runat="server">
    <title>Multi-Factor Authentication Demo</title>
    </head>
    <body>
    <h1>Azure Multi-Factor Authentication Demo</h1>
    <form id="form1" runat="server">
    
    <div style="width:auto; float:left">
    Username:&nbsp;<br />
    Password:&nbsp;<br />
    </div>
    
    <div">
    <asp:TextBox id="username" runat="server" width="100px"/><br />
    <asp:Textbox id="password" runat="server" width="100px" TextMode="password" /><br />
    </div>
    
    <asp:Button id="btnSubmit" runat="server" Text="Log in" onClick="btnSubmit_Click"/>
    
    <p><asp:Label ID="lblResult" runat="server"></asp:Label></p>
    
    </form>
    </body>
    </html>


### 服务器端代码

在下面的服务器端代码，多因素身份验证配置并运行在步骤 2 中。 标准模式 (MODE_STANDARD) 是电话用户响应通过按 # 键。 

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;
    
    public partial class _Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
        }
    
        protected void btnSubmit_Click(object sender, EventArgs e)
        {
            // Step 1: Validate the username and password
            if (username.Text != "Contoso" || password.Text != "password")
            {
                lblResult.ForeColor = System.Drawing.Color.Red;
                lblResult.Text = "Username or password incorrect.";
            }
            else
            {
                // Step 2: Perform multi-factor authentication
    
                // Add call details from the user database.
                PfAuthParams pfAuthParams = new PfAuthParams();
                pfAuthParams.Username = username.Text;
                pfAuthParams.Phone = "9134884271";
                pfAuthParams.Mode = pf_auth.MODE_STANDARD;
                
                // Specify a client certificate 
                // NOTE: This file contains the private key for the client
                // certificate. It must be stored with appropriate file 
                // permissions.
                pfAuthParams.CertFilePath = "c:\\cert_key.p12";
    
                // Perform phone-based authentication
                int callStatus;
                int errorId;
    
                if(pf_auth.pf_authenticate(pfAuthParams, out callStatus, out errorId))
                {
                    lblResult.ForeColor = System.Drawing.Color.Green;
                    lblResult.Text = "Multi-Factor Authentication succeeded.";
                }
                else
                {
                    lblResult.ForeColor = System.Drawing.Color.Red;
                    lblResult.Text = " Multi-Factor Authentication failed.";
                }
            }
    
        }
    }

