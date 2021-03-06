---
title: 教程：Azure Active Directory 与 Panopto 集成 | Microsoft 文档
description: 了解如何在 Azure Active Directory 和 Panopto 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 89c88e23-93ce-4970-9baa-1104c4e8fe4a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 6ca6a99687079beaef25c72d4cea8de5984e6c50
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2018
ms.locfileid: "39051442"
---
# <a name="tutorial-azure-active-directory-integration-with-panopto"></a>教程：Azure Active Directory 与 Panopto 的集成

本教程介绍如何将 Panopto 与 Azure Active Directory (Azure AD) 集成。

将 Panopto 与 Azure AD 集成可提供以下优势：

- 可在 Azure AD 中控制谁有权访问 Panopto
- 可以让用户使用其 Azure AD 帐户自动登录 Panopto（单一登录）
- 可以在一个中心位置（即 Azure 门户）管理帐户

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 Panopto 的集成，需要以下项：

- Azure AD 订阅
- 启用了 Panopto 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 Panopto
2. 配置和测试 Azure AD 单一登录

## <a name="adding-panopto-from-the-gallery"></a>从库中添加 Panopto
若要配置 Panopto 与 Azure AD 的集成，需要从库中将 Panopto 添加到托管 SaaS 应用列表。

**若要从库中添加 Panopto，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![Active Directory][1]

2. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![应用程序][2]
    
3. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![应用程序][3]

4. 在搜索框中，键入“Panopto”。

    ![创建 Azure AD 测试用户](./media/panopto-tutorial/tutorial_panopto_search.png)

5. 在结果窗格中，选择“Panopto”，然后单击“添加”按钮添加该应用程序。

    ![创建 Azure AD 测试用户](./media/panopto-tutorial/tutorial_panopto_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

在本部分中，将基于名为“Britta Simon”的测试用户配置并测试 Panopto 的 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 Panopto 用户。 换句话说，需要在 Azure AD 用户与 Panopto 中的相关用户之间建立链接关系。

可通过将 Azure AD 中“用户名”的值指定为 Panopto 中“用户名”的值来建立此链接关系。

若要配置并测试 Panopto 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-sign-on)** - 让用户使用此功能。
2. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
3. [创建 Panopto 测试用户](#creating-a-panopto-test-user) - 在 Panopto 中创建 Britta Simon 的对应用户，将其链接到该用户的 Azure AD 表示形式。
4. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 使用 Azure AD 单一登录。
5. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将在 Azure 门户中启用 Azure AD 单一登录并在 Panopto 应用程序中配置单一登录。

**若要配置 Panopto 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中的“Panopto”应用程序集成页上，单击“单一登录”。

    ![配置单一登录][4]

2. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![配置单一登录](./media/panopto-tutorial/tutorial_panopto_samlbase.png)

3. 在“Panopto 域和 URL”部分中，请执行以下步骤：

    ![配置单一登录](./media/panopto-tutorial/tutorial_panopto_url.png)

    在“登录 URL”文本框中，使用以下模式键入 URL： `https://<tenant-name>.panopto.com`

    > [!NOTE] 
    > 此值不是真实值。 请使用实际登录 URL 更新此值。 请联系 [Panopto 客户端支持团队](mailto:support@panopto.com‎)获取此值。 
 
4. 在“SAML 签名证书”部分中，单击“元数据 XML”，并在计算机上保存元数据文件。

    ![配置单一登录](./media/panopto-tutorial/tutorial_panopto_certificate.png) 

5. 单击“保存”按钮。

    ![配置单一登录](./media/panopto-tutorial/tutorial_general_400.png)

6. 在“Panopto 配置”部分，单击“配置 Panopto”打开“配置登录”窗口。 从“快速参考”部分中复制“SAML 实体 ID 和 SAML 单一登录服务 URL”。

    ![配置单一登录](./media/panopto-tutorial/tutorial_panopto_configure.png) 

7. 在另一 Web 浏览器窗口中，以管理员身份登录到 Panopto 公司站点。

8. 在左侧工具栏中，单击“系统”，并单击“标识提供者”。
   
   ![系统](./media/panopto-tutorial/ic777670.png "系统")
9. 单击“添加提供者”。
   
   ![标识提供者](./media/panopto-tutorial/ic777671.png "标识提供者")
   
10. 在“SAML 提供者”部分执行以下步骤：
   
    ![SaaS 配置](./media/panopto-tutorial/ic777672.png "SaaS 配置")
    
    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 在“提供者类型”列表中，选择“SAML20”。    
    
    b. 在“实例名称”文本框中，键入实例的名称。

    c. 在“易懂描述”文本框中，键入通俗易懂的描述。
    
    d. 在“弹出页 URL”文本框中，粘贴从 Azure 门户复制的“SAML 单一登录服务 URL”值。

    e. 将从 Azure 门户复制的“SAML 实体 ID”的值粘贴到“颁发者”文本框中。

    f. 打开从 Azure 门户中下载的 base-64 编码证书，将其内容复制到剪贴板，然后再粘贴到“公钥”文本框中。

11. 单击“ **保存**”。

> [!TIP]
> 之后在设置应用时，就可以在 [Azure 门户](https://portal.azure.com)中阅读这些说明的简明版本了！  从“Active Directory”>“企业应用程序”部分添加此应用后，只需单击“单一登录”选项卡，即可通过底部的“配置”部分访问嵌入式文档。 可在此处阅读有关嵌入式文档功能的详细信息：[ Azure AD 嵌入式文档]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

![创建 Azure AD 用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 **Azure 门户**的左侧导航窗格中，单击“Azure Active Directory”图标。

    ![创建 Azure AD 测试用户](./media/panopto-tutorial/create_aaduser_01.png) 

2. 若要显示用户列表，请转到“用户和组”，单击“所有用户”。
    
    ![创建 Azure AD 测试用户](./media/panopto-tutorial/create_aaduser_02.png) 

3. 若要打开“用户”对话框，请在对话框顶部单击“添加”。
 
    ![创建 Azure AD 测试用户](./media/panopto-tutorial/create_aaduser_03.png) 

4. 在“用户”对话框页上，执行以下步骤：
 
    ![创建 Azure AD 测试用户](./media/panopto-tutorial/create_aaduser_04.png) 

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 在“名称”文本框中，键入 **BrittaSimon**。

    b. 在“用户名”文本框中，键入 BrittaSimon 的“电子邮件地址”。

    c. 选择“显示密码”并记下“密码”的值。

    d. 单击“创建”。
 
### <a name="creating-a-panopto-test-user"></a>创建 Panopto 测试用户

没有相关操作项，因此无法通过配置将用户预配到 Panopto。  
当已分配的用户尝试使用访问面板登录到 Panopto 时，Panopto 会检查该用户是否存在。  

如果尚无用户帐户可用，Panopto 会自动创建该帐户。

>[!NOTE]
>可以使用任何其他 Panopto 用户帐户创建工具或 Panopto 提供的 API 来预配 AAD 用户帐户。
>
>

### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 Panopto 的权限，允许使用 Azure 单一登录。

![分配用户][200] 

**若要将 Britta Simon 分配到 Panopto，请执行以下步骤：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

2. 在应用程序列表中，选择“Panopto”。

    ![配置单一登录](./media/panopto-tutorial/tutorial_panopto_app.png) 

3. 在左侧菜单中，单击“用户和组”。

    ![分配用户][202] 

4. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![分配用户][203]

5. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

6. 在“用户和组”对话框中单击“选择”按钮。

7. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="testing-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

在访问面板中单击 Panopto 磁贴时，会自动转到 Panopto 应用程序的登录页面。
有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [什么是使用 Azure Active Directory 的应用程序访问和单一登录？](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/panopto-tutorial/tutorial_general_01.png
[2]: ./media/panopto-tutorial/tutorial_general_02.png
[3]: ./media/panopto-tutorial/tutorial_general_03.png
[4]: ./media/panopto-tutorial/tutorial_general_04.png

[100]: ./media/panopto-tutorial/tutorial_general_100.png

[200]: ./media/panopto-tutorial/tutorial_general_200.png
[201]: ./media/panopto-tutorial/tutorial_general_201.png
[202]: ./media/panopto-tutorial/tutorial_general_202.png
[203]: ./media/panopto-tutorial/tutorial_general_203.png

