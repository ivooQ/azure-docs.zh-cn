---
title: 在 Azure Stack 中监视运行状况和警报 | Microsoft Docs
description: 了解如何在 Azure Stack 中监视运行状况和警报。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 69901c7b-4673-4bd8-acf2-8c6bdd9d1546
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/10/2017
ms.author: mabrigg
ms.openlocfilehash: e20670476f4549a06d80d5f85e2ebd8ab1f50b05
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2018
ms.locfileid: "37047235"
---
# <a name="monitor-health-and-alerts-in-azure-stack"></a>在 Azure Stack 中监视运行状况和警报

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

Azure Stack 包含基础结构监视功能，可用于查看 Azure Stack 区域的运行状况和警报。 “区域管理”磁贴默认固定在默认提供商订阅的管理员门户上，其中列出了 Azure Stack 的所有已部署区域。 该磁贴除了显示每个区域的活动严重警报和警告警报数目以外，也是 Azure Stack 运行状况和警报功能的入口点。

 ![“区域管理”磁贴](media/azure-stack-monitor-health/image1.png)

 ## <a name="understand-health-in-azure-stack"></a>了解 Azure Stack 中的运行状况

 运行状况和警报由运行状况资源提供程序管理。 在 Azure Stack 部署和配置期间，Azure Stack 基础结构组件将注册到运行状况资源提供程序。 注册后，可以显示每个组件的运行状况和警报。 Azure Stack 中的“运行状况”是个简单的概念。 如果组件的已注册实例存在警报，该组件的运行状况会反映最不利的活动警报的严重性：警告或严重。

## <a name="alert-severity-definition"></a>警报严重性定义

在 Azure 堆栈警报只有两个严重级别的：**警告**和**关键**。

**警告**  
运算符可以计划的方式来解决警告性警报。 另外，此警报通常不会影响用户的工作负荷。

**严重**  
运算符应与紧急性解决严重警报。 这些是当前影响或很快会影响 Azure 堆栈用户的问题。 

 
 ## <a name="view-and-manage-component-health-state"></a>查看和管理组件运行状况
 
 Azure Stack 操作员可以通过管理员门户以及 REST API 和 PowerShell 查看组件的运行状况。
 
若要在门户中查看运行状况，请在“区域管理”磁贴中单击想要查看的区域。 可以查看基础结构角色和资源提供程序的运行状况。

![基础结构角色列表](media/azure-stack-monitor-health/image2.png)

可以单击资源提供程序或基础结构角色来查看更详细的信息。

> [!WARNING]
>如果单击基础结构角色，然后单击角色实例，则会看到“启动”、“重启”或“关机”选项。 对集成系统应用更新时，请勿使用这些操作。 此外，**请勿**在 Azure Stack 开发工具包环境中使用这些选项。 这些选项是针对每个基础结构角色具有多个角色实例的集成系统环境设计的。 在开发工具包中重启角色实例（特别是 AzS-Xrp01）会导致系统不稳定。 如需故障排除方面的帮助，请在 [Azure Stack 论坛](https://aka.ms/azurestackforum)中提问。
>
 
## <a name="view-alerts"></a>查看警报

可直接从“区域管理”边栏选项卡查看每个 Azure Stack 区域的活动警报列表。 默认配置中的第一个磁贴是“警报”磁贴，其中显示区域的严重警报和警告警报摘要。 如同此边栏选项卡中的其他磁贴一样，可将“警报”磁贴固定到仪表板，以便快速访问。   

![显示警告的“警报”磁贴](media/azure-stack-monitor-health/image3.png)

选择“警报”磁贴的上半部分可以导航到区域的所有活动警报列表。 如果选择磁贴中的“严重”或“警告”行项，则会导航到警报的筛选列表（“严重”或“警告”）。 

![筛选的警告警报](media/azure-stack-monitor-health/image4.png)
  
“警报”边栏选项卡支持按状态（“活动”或“已关闭”）和严重性（“严重”或“警告”）进行筛选。 默认视图显示所有活动警报。 所有已关闭的警报在七天后将从系统中删除。

![在“筛选器”窗格中按严重或警告状态进行筛选](media/azure-stack-monitor-health/image5.png)

“视图 API”操作显示用于生成列表视图的 REST API。 借助此操作可以快速熟悉可用于查询警报的 REST API 语法。 可在自动化中使用此 API，或者将它与现有的数据中心监视、报告和票证解决方案相集成。 

![显示 REST API 的“视图 API”选项](media/azure-stack-monitor-health/image6.png)

可以单击特定的警报来查看警报详细信息。 警报详细信息显示与警报关联的所有字段，并可让用户快速导航到受影响的组件和警报源。 例如，如果某个基础结构角色实例脱机或不可访问，则会发生以下警报。  

![“警报详细信息”边栏选项卡](media/azure-stack-monitor-health/image7.png)

基础结构角色实例重新联机后，会自动关闭此警报。 在根本问题得到解决后，许多（但并非所有）警报会自动关闭。 我们建议在执行补救步骤之后选择“关闭警报”。 如果问题仍然存在，Azure Stack 会生成新警报。 如果解决了问题，警报将保持关闭，且无需采取其他措施。

## <a name="next-steps"></a>后续步骤

[在 Azure Stack 中管理更新](azure-stack-updates.md)

[Azure Stack 中的区域管理](azure-stack-region-management.md)
