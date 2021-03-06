---
title: 查看虚拟中心的有效路由：Azure 虚拟 WAN | Azure
description: 查看 Azure 虚拟 WAN 中的虚拟中心的有效路由
services: virtual-wan
author: rockboyfor
ms.service: virtual-wan
ms.topic: conceptual
origin.date: 03/02/2020
ms.date: 03/02/2020
ms.author: v-yeche
ms.openlocfilehash: 3d0cdc6f5d917e5d74690347d7a238898f4c2023
ms.sourcegitcommit: 69cadf1fa0ed81751c48fbce919a6bb44b1053ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2020
ms.locfileid: "78209182"
---
# <a name="view-effective-routes-of-a-virtual-hub"></a>查看虚拟中心的有效路由

可以在 Azure 门户中查看虚拟 WAN 中心的所有路由。 若要查看路由，请导航到虚拟中心，然后选择“路由”>“查看有效路由”  。

<a name="understand"></a>
## <a name="understanding-routes"></a>了解路由

以下示例可帮助你更好地了解虚拟 WAN 路由的显示方式。

<!-- West Europe Mapping China North and West US mapping China North 2-->

在此示例中，我们有一个具有三个中心的虚拟 WAN。 第一个中心在“中国东部”区域，第二个中心在“中国北部”区域，第三个中心在“中国北部 2”区域。 在虚拟 WAN 中，所有中心都是互联的。 在此示例中，我们假设“中国东部”和“中国北部”中心与本地分支（辐射）和 Azure 虚拟网络（辐射）之间都有连接。

包含网络虚拟设备 (10.4.0.6) 的 Azure VNet 辐射 (10.4.0.0/16) 进一步对等互连到一个 VNet (10.5.0.0/16)。 有关中心路由表的详细信息，请参阅本文下文中的[其他信息](#abouthubroute)。

在此示例中，我们还假设“中国北部分支 1”连接到“中国东部”中心以及“中国北部”中心。 “中国东部”中的一个 ExpressRoute 线路将分支 2 连接到“中国东部”中心。

![示意图](./media/effective-routes-virtual-hub/diagram.png)

<a name="view"></a>
## <a name="view-effective-routes"></a>查看有效路由

当你在门户中选择“查看有效路由”时，它将为“中国东部”中心生成[中心路由表](#routetable)中显示的输出。

如下所示，第一行表示，由于 VPN“下一跃点类型”  连接（“下一跃点”VPN 网关实例 0 的 IP 为 10.1.0.6、实例 1 的 IP 为 10.1.0.7），“中国东部”中心已获知了 10.20.1.0/24（分支 1）的路由。 “路由原点”  指向资源 ID。 “AS 路径”  表示分支 1 的 AS 路径。

<a name="routetable"></a>
### <a name="hub-route-table"></a>中心路由表

可以使用表底部的滚动条查看“AS 路径”。

| **前缀** |  **下一跃点类型** | **下一跃点** |  **路由原点** |**AS 路径** |
| --- | --- | --- | --- | --- |
| 10.20.1.0/24|VPN |10.1.0.6、10.1.0.7| /subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/vpnGateways/343a19aa6ac74e4d81f05ccccf1536cf-chinaeast-gw| 20000|
|10.21.1.0/24 |ExpressRoute|10.1.0.10、10.1.0.11|/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/expressRouteGateways/4444a6ac74e4d85555-chinaeast-gw|21000|
|10.23.1.0/24| VPN |10.1.0.6、10.1.0.7|/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/vpnGateways/343a19aa6ac74e4d81f05ccccf1536cf-chinaeast-gw|23000|
|10.4.0.0/16|虚拟网络连接| 在链路上 |  |  |
|10.5.0.0/16| IP 地址| 10.4.0.6|/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/virtualHubs/easthub_1/routeTables/table_1| |
|0.0.0.0/0| IP 地址| `<Azure Firewall IP>` |/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/virtualHubs/easthub_1/routeTables/table_1| |
|10.22.1.0/16| 远程中心|10.8.0.6、10.8.0.7|/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/virtualHubs/northhub_| 4848-22000 |
|10.9.0.0/16| 远程中心|  在链路上 |/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/virtualHubs/northhub_1| |

<!--CORRECT ON westhub MAPPING northnub-->

>[!NOTE]
> 在示例拓扑中，如果“中国东部”和“中国北部”中心没有相互通信，则所获知的路由 (10.9.0.0/16) 将不存在。 中心仅播发直接连接到它们的网络。
>

<a name="additional"></a>
## <a name="additional-information"></a>其他信息

<a name="abouthubroute"></a>
### <a name="about-the-hub-route-table"></a>关于中心路由表

可以创建一个虚拟中心路由，并将该路由应用于虚拟中心路由表。 可以将多个路由应用于虚拟中心路由表。 这允许你通过 IP 地址（通常是辐射 VNet 中的网络虚拟设备 (NVA)）设置目标 VNet 的路由。 有关 NVA 的详细信息，请参阅[将流量从虚拟中心路由到 NVA](virtual-wan-route-table-portal.md)。

<a name="aboutdefaultroute"></a>
### <a name="about-default-route-00000"></a>关于默认路由 (0.0.0.0/0)

虚拟中心能够将获知的默认路由传播到虚拟网络、站点到站点 VPN 和 ExpressRoute 连接，前提是连接上的此标志设置为“已启用”。 当你编辑虚拟网络连接、VPN 连接或 ExpressRoute 连接时，将显示此标志。 默认情况下，“EnableInternetSecurity”在中心 VNet、ExpressRoute 和 VPN 连接上始终为 false。

默认路由并非源自虚拟 WAN 中心。 当虚拟 WAN 中心由于在中心部署防火墙而获知了默认路由或另一个已连接的站点已启用强制隧道时，将会传播默认路由。

## <a name="next-steps"></a>后续步骤

有关虚拟 WAN 的详细信息，请参阅[虚拟 WAN 概述](virtual-wan-about.md)。

<!-- Update_Description: update meta properties, wording update, update link -->