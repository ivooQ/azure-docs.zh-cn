---
title: 滚动应用程序升级 - Azure SQL 数据库 | Microsoft Docs
description: 了解如何使用 Azure SQL 数据库的异地复制来支持云应用程序的在线升级。
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: sashan
ms.openlocfilehash: a73284d679b4be1fbae6d5e1688915c98cbf2392
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/01/2018
ms.locfileid: "34649493"
---
# <a name="managing-rolling-upgrades-of-cloud-applications-using-sql-database-active-geo-replication"></a>使用 SQL 数据库活动异地复制管理云应用程序的滚动升级
> [!NOTE]
> [活动异地复制](sql-database-geo-replication-overview.md)现在可供所有层中的所有数据库使用。
> 

了解如何使用 SQL 数据库中的[异地复制](sql-database-geo-replication-overview.md)来启用云应用程序的滚动升级。 由于升级是中断性操作，所以它应成为业务连续性规划和设计的一部分。 本文我们介绍了编排升级过程的两种不同方法，并讨论了每种方法的优点和缺点。 针对本文的目的，我们将使用一个简单的应用程序，该应用程序包含一个连接到作为其数据层的单一数据库的网站。 我们的目标是在不对最终用户体验产生任何显著影响的情况下将版本 1 的应用程序升级到版本 2。 

在评估升级选项时应考虑以下因素：

* 升级过程中对应用程序可用性的影响。 应用程序的功能可能被限制或降级多长时间。
* 升级失败时的回滚能力。
* 升级过程中发生灾难性错误时的应用程序的漏洞问题。
* 总的费用成本。  该成本包括升级过程使用的额外的冗余成本和临时组件的增量成本。 

## <a name="upgrading-applications-that-rely-on-database-backups-for-disaster-recovery"></a>升级依赖于数据库备份进行灾难恢复的应用程序
如果应用程序依赖于自动的数据库备份，并且使用异地还原来实现灾难恢复，那么通常将它部署到单个 Azure 区域。 在此事例中升级过程包括创建升级所涉及的所有应用程序组件的备份部署。 要最小化对最终用户的干扰，可以使用具有故障转移配置文件的 Azure 流量管理器 (WATM)。  下图说明了在升级过程开始前的操作环境。 终结点 <i>contoso-1.azurewebsites.net</i> 表示需要升级的应用程序的一个生产槽。 若要启用对升级进行回滚的功能，需要使用应用程序的完全同步副本创建过渡槽。 准备应用程序升级需要执行以下步骤：

1. 为升级创建过渡槽。 要执行此操作需要在同一 Azure 区域中创建一个辅助数据库 (1)，并部署相同的网站。 监视此辅助数据库以查看种子设定过程是否已完成。
2. 使用作为联机终结点的 <i>contoso-1.azurewebsites.net</i> 和作为离线终结点的 <i>contoso-2.azurewebsites.net</i> 在 WATM 中创建故障转移配置文件。 

> [!NOTE]
> 请注意上述准备步骤不会影响生产槽中的应用程序，应用程序可以在完全访问模式下运行。
>  

![SQL 数据库异地复制配置。 云灾难恢复。](media/sql-database-manage-application-rolling-upgrade/Option1-1.png)

完成上述准备步骤后，应用程序就可以进行真正的升级了。 下图说明了升级过程所涉及的步骤。 

1. 将生产槽中的主数据库设置为只读模式 (3)。 这可以保证在升级过程中应用程序 (V1) 的生产实例将保持只读模式，从而避免 V1 和 V2 数据库实例之间出现数据分歧。  
2. 使用计划的终止模式断开辅助数据库的连接 (4)。 这会创建主数据库的完全同步独立副本。 将升级该数据库。
3. 将主数据库切换为读写模式，并在过渡槽中运行升级脚本 (5)。     

![SQL 数据库异地复制配置。 云灾难恢复。](media/sql-database-manage-application-rolling-upgrade/Option1-2.png)

如果升级成功完成，那么你现在可以将最终用户切换到应用程序的暂存副本。 该副本将成为应用程序的生产槽。  如下图所示，该操作又包含以下几个步骤。

1. 将 WATM 配置文件中的联机终结点切换为 <i>contoso-2.azurewebsites.net</i>，该终结点指向网站的 V2 版本 (6)。 现在该网站成为 V2 应用程序的生产槽，最终用户流量将定向到该网站。  
2. 如果不再需要 V1 应用程序组件，可以放心删除它们 (7)。   

![SQL 数据库异地复制配置。 云灾难恢复。](media/sql-database-manage-application-rolling-upgrade/Option1-3.png)

如果升级过程不成功，例如由于升级的脚本中的一个错误，那么过渡槽应被视为已损坏。 要将应用程序回滚到升级前的状态，只需将生产槽中的应用程序还原为完全访问模式。 所涉及的步骤如下图所示。    

1. 将数据库副本设置为读写模式 (8)。 这可以在功能上还原生产槽中的完整 V1。
2. 执行根本原因分析，并删除过渡槽中已损坏的组件 (9)。 

此时应用程序可完全正常运行，并且可以重复上述升级步骤。

> [!NOTE]
> 回滚操作不需要更改 WATM 配置文件，因为它已指向作为活动终结点的 <i>contoso-1.azurewebsites.net</i>。
> 
> 

![SQL 数据库异地复制配置。 云灾难恢复。](media/sql-database-manage-application-rolling-upgrade/Option1-4.png)

此选项的主要**优点**是可以使用一系列简单步骤升级单个区域中的应用程序。 此升级的费用成本相对较低。 此方法的主要**缺点**在于，如果在升级过程中发生灾难性故障，那么恢复到升级前的状态将涉及在不同的区域重新部署应用程序，并且使用异地还原从备份中还原数据库。 此过程将导致大量的停机时间。   

## <a name="upgrading-applications-that-rely-on-database-geo-replication-for-disaster-recovery"></a>升级依赖于数据库异地复制进行灾难恢复的应用程序
如果应用程序利用异地复制来实现业务连续性，那么该应用程序至少部署到两个不同的区域，主要区域为活动部署，备份区域为备用部署。 除了前面提到的各个因素，此升级过程还必须保证：

* 在升级过程的任何时候都保护应用程序免受灾难性故障
* 应用程序的地理冗余组件与活动组件一同升级

为了实现这些目标，将通过包含一个活动终结点和三个备份终结点的故障转移配置文件来使用 Azure 流量管理器 (WATM)。  下图说明了在升级过程开始前的操作环境。 网站 <i>contoso-1.azurewebsites.net</i> 和 <i>contoso-dr.azurewebsites.net</i> 代表具有完全地理冗余的应用程序的生产槽。 若要启用对升级进行回滚的功能，需要使用应用程序的完全同步副本创建过渡槽。 你需要确保在升级过程中发生灾难性故障时应用程序可以快速恢复，另外过渡槽必须也是地理冗余。 准备应用程序升级需要执行以下步骤：

1. 为升级创建过渡槽。 要执行此操作需要在同一 Azure 区域中创建一个辅助数据库 (1)，并部署相同的网站副本。 监视此辅助数据库以查看种子设定过程是否已完成。
2. 通过将辅助数据库异地复制到备份区域（称为“链接异地复制”）在过渡槽中创建地理冗余的辅助数据库。 监视此备份的辅助数据库以查看种子设定过程是否已完成 (3)。
3. 在备份区域中创建网站的备用副本，并将其链接到地理冗余的辅助数据库 (4)。  
4. 将额外的终结点 <i>contoso-2.azurewebsites.net</i> 和 <i>contoso-3.azurewebsites.net</i> 作为离线终结点添加到 WATM 中的故障转移配置文件 (5)。 

> [!NOTE]
> 请注意上述准备步骤不会影响生产槽中的应用程序，应用程序可以在完全访问模式下运行。
> 
> 

![SQL 数据库异地复制配置。 云灾难恢复。](media/sql-database-manage-application-rolling-upgrade/Option2-1.png)

完成上述准备步骤后，就可以将过渡槽用于升级了。 下图显示了升级的步骤。

1. 将生产槽中的主数据库设置为只读模式 (6)。 这可以保证在升级过程中应用程序 (V1) 的生产实例将保持只读模式，从而避免 V1 和 V2 数据库实例之间出现数据分歧。  
2. 使用计划的终止模式断开同一区域中的辅助数据库的连接 (7)。 该操作将创建主数据库的完全同步独立副本，该副本会在终止后自动成为主数据库。 将升级该数据库。
3. 将过渡槽中的主数据库切换为读写模式，并运行升级脚本 (8)。    

![SQL 数据库异地复制配置。 云灾难恢复。](media/sql-database-manage-application-rolling-upgrade/Option2-2.png)

如果升级成功完成，那么你现在可以将最终用户切换到应用程序的 V2 版本。 下图说明了所涉及的步骤。

1. 将 WATM 配置文件中的活动终结点切换为 <i>contoso-2.azurewebsites.net</i>，该终结点指向网站的 V2 版本 (9)。 现在该网站成为 V2 应用程序的生产槽，最终用户流量将定向到该网站。 
2. 如果不再需要 V1 应用程序，可以放心删除它（10 和 11）。  

![SQL 数据库异地复制配置。 云灾难恢复。](media/sql-database-manage-application-rolling-upgrade/Option2-3.png)

如果升级过程不成功，例如由于升级的脚本中的一个错误，那么过渡槽应被视为已损坏。 要将应用程序回滚到升级前的状态，只需将生产槽中的应用程序还原为完全访问模式。 所涉及的步骤如下图所示。    

1. 将生产槽中的主数据库设置为读写模式 (12)。 这可以在功能上还原生产槽中的完整 V1。
2. 执行根本原因分析，并删除过渡槽中已损坏的组件（13 和 14）。 

此时应用程序可完全正常运行，并且可以重复上述升级步骤。

> [!NOTE]
> 回滚操作不需要更改 WATM 配置文件，因为它已指向作为活动终结点的 <i>contoso-1.azurewebsites.net</i>。
> 
> 

![SQL 数据库异地复制配置。 云灾难恢复。](media/sql-database-manage-application-rolling-upgrade/Option2-4.png)

此升级方法的主要**优点**是可以同时升级应用程序及其地理冗余副本，并且不会在升级过程中破坏业务连续性。 此方法的主要**缺点**是它需要每个应用程序组件的双倍冗余，因此会导致更高的费用成本。 它还涉及更复杂的工作流。 

## <a name="summary"></a>摘要
本文中所述的两种升级方法具有不同的复杂性和费用成本，但它们都关注于最小化最终用户仅限于执行只读操作的时间。 该时间由升级脚本的持续时间直接定义。 该时间不依赖于数据库大小、所选的服务层、网站配置和你无法轻松控制的其他因素。 这是因为所有准备步骤都从升级步骤中分离出来，可以在不影响生产应用程序的情况下完成。 升级脚本的效率是决定升级期间的最终用户体验的关键因素。 因此改进升级的最佳做法是致力于尽可能地提高升级脚本的效率。  

## <a name="next-steps"></a>后续步骤
* 有关业务连续性概述和应用场景，请参阅[业务连续性概述](sql-database-business-continuity.md)。
* 若要了解 Azure SQL 数据库的自动备份，请参阅 [SQL 数据库自动备份](sql-database-automated-backups.md)。
* 若要了解如何使用自动备份进行恢复，请参阅[从自动备份中还原数据库](sql-database-recovery-using-backups.md)。
* 若要了解更快的恢复选项，请参阅[活动异地复制](sql-database-geo-replication-overview.md)。


