---
title: Azure Stack Hub 的网络集成规划
description: 了解如何规划数据中心网络与 Azure Stack Hub 集成式系统的集成。
author: WenJason
ms.topic: article
origin.date: 10/23/2019
ms.date: 02/24/2020
ms.author: v-jay
ms.reviewer: wamota
ms.lastreviewed: 06/04/2019
ms.openlocfilehash: 23ad4936109ba5dacec1015ab34fb1d8eb25553e
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79291157"
---
# <a name="network-integration-planning-for-azure-stack-hub"></a>Azure Stack Hub 的网络集成规划

本文提供 Azure Stack Hub 网络基础架构信息，可帮助你确定如何以最佳方式将 Azure Stack Hub 集成到现有的网络环境。

> [!NOTE]
> 若要从 Azure Stack Hub 解析外部 DNS 名称（例如 www\.bing.com），必须提供 DNS 服务器来转发 DNS 请求。 有关 Azure Stack Hub DNS 要求的详细信息，请参阅 [Azure Stack Hub 数据中心集成 - DNS](azure-stack-integrate-dns.md)。

## <a name="physical-network-design"></a>物理网络设计

Azure Stack Hub 解决方案需有弹性且高度可用的物理基础结构才能支持其操作和服务。 从 ToR 到边界交换机的上行链路限制为 SFP+ 或 SFP28 介质以及 1 GB、10 GB 或 25 GB 的速度。 请咨询原始设备制造商 (OEM) 硬件供应商以了解可用性。 下图显示了建议的设计：

![建议的 Azure Stack Hub 网络设计](media/azure-stack-network/recommended-design.png)


## <a name="logical-networks"></a>逻辑网络

逻辑网络表示底层物理网络基础结构的抽象。 它们用于组织和简化主机、虚拟机 (VM) 和服务的网络分配。 网络站点在创建逻辑网络时创建，定义虚拟局域网 (VLAN)、IP 子网，以及与每个物理位置中逻辑网络关联的 IP 子网/VLAN 对。

下表显示了逻辑网络以及必须规划的关联 IPv4 子网范围：

| 逻辑网络 | 说明 | 大小 | 
| -------- | ------------- | ------------ | 
| 公共 VIP | Azure Stack Hub 总共使用此网络中的 31 个地址。 8 个公共 IP 地址由少量的 Azure Stack Hub 服务使用，剩余的地址由租户 VM 使用。 如果打算使用应用服务和 SQL 资源提供程序，则还要额外使用 7 个地址。 其余 15 个 IP 保留用于将来的 Azure 服务。 | /26（62 台主机）- /22（1022 台主机）<br><br>建议使用 /24（254 台主机） | 
| 交换机基础结构 | 用于路由的专用交换机管理接口的点到点 IP 地址，以及分配给交换机的环回地址。 | /26 | 
| 基础结构 | 用于通信的 Azure Stack Hub 内部组件。 | /24 |
| 专用 | 用于存储网络、专用 VIP、基础结构容器和其他内部功能。 从版本 1910 开始，此子网的大小将更改为 /20。 有关详细信息，请参阅本文中的[专用网络](#private-network)部分。 | /20 | 
| BMC | 用于与物理主机上的 BMC 通信。 | /26 | 
| | | |

> [!NOTE]
> 系统更新到版本 1910 后，门户上的警报将提醒操作员运行新的 PEP cmdlet **Set-AzsPrivateNetwork** 来添加新的 /20 专用 IP 空间。 有关运行该 cmdlet 的说明，请参阅 [1910 发行说明](release-notes.md)。 有关选择 /20 专用 IP 空间的详细信息和指导，请参阅本文中的[专用网络](#private-network)部分。

## <a name="network-infrastructure"></a>网络基础结构

Azure Stack Hub 的网络基础结构包括交换机上配置的多个逻辑网络。 下图显示了这些逻辑网络，及其如何与架顶 (TOR)、基板管理控制器 (BMC) 和边界（客户网络）交换机集成。

![逻辑网络示意图和交换机连接](media/azure-stack-network/NetworkDiagram.png)

### <a name="bmc-network"></a>BMC 网络

此网络专门用于将所有基板管理控制器（也称为 BMC 或服务处理器）连接到管理网络。 示例包括：iDRAC、iLO、iBMC 等。 仅一个 BMC 帐户用于与任何 BMC 节点通信。 如果硬件生命周期主机 (HLH) 存在，它将位于此网络，并可提供 OEM 特定的软件，用于硬件维护或监视。

HLH 也托管部署 VM (DVM)。 此 DVM 在 Azure Stack Hub 部署期间使用，在部署完成后删除。 此 DVM 在联网部署场景中需要进行 Internet 访问，以便测试、验证和访问多个组件。 这些组件可以在公司网络内，也可以在公司网络外（例如：NTP、DNS 和 Azure）。 有关连接要求的详细信息，请参阅 [Azure Stack Hub 防火墙集成中的 NAT 部分](azure-stack-firewall.md#network-address-translation)。

### <a name="private-network"></a>专用网络

此 /20（4096 个 IP）网络专用于 Azure Stack Hub 区域（不会路由到 Azure Stack Hub 系统的边界交换机设备以外），并划分为多个子网，下面是一些示例：

- **存储网络**：一个 /25（128 个 IP）网络，用于支持空间直通和服务器消息块 (SMB) 存储流量与 VM 实时迁移的使用。
- **内部虚拟 IP 网络**：一个 /25 网络，专用于软件负载均衡器的仅限内部的 VIP。
- **容器网络**：一个 /23（512 个 IP）网络，专用于在运行基础结构服务的容器之间处理仅限内部的流量。

从版本 1910 开始，专用网络的大小将更改为专用 IP 空间的 /20（4096 个 IP）。 此网络专用于 Azure Stack Hub 系统（不会路由到 Azure Stack Hub 系统的边界交换机设备以外），并且可以在数据中心内的多个 Azure Stack Hub 系统上重复使用。 这是 Azure Stack Hub 的专用网络，不能与数据中心内的其他网络重叠。 如果发生重叠，Azure Stack Hub 可能无法在外部路由企业网络流量。 有关专用 IP 空间的指导，建议遵循 [RFC 1918](https://tools.ietf.org/html/rfc1918)。

此 /20 专用 IP 空间将划分为多个网络，以便在将来版本的容器上运行 Azure Stack Hub 系统内部基础结构。 有关详细信息，请参阅 [1910 发行说明](release-notes.md)。 此外，借助此新专用 IP 空间，可以在部署之前持续减少所需的可路由 IP 空间。

对于在版本 1910 之前部署的系统，此 /20 子网将是更新到 1910 之后，要输入系统中的附加网络。 需要通过 **Set-AzsPrivateNetwork** PEP cmdlet 将此附加网络提供给系统。 有关此 cmdlet 的指导，请参阅 [1910 发行说明](release-notes.md)。

### <a name="azure-stack-hub-infrastructure-network"></a>Azure Stack Hub 基础结构网络

此 /24 网络专用于内部 Azure Stack Hub 组件，使这些组件能够相互通信和交换数据。 对于 Azure Stack Hub 解决方案，此子网可从外部路由到数据中心。 我们不建议在此子网上使用公共或可从 Internet 路由的 IP 地址。 此网络广播到边界，但其大多数 IP 受访问控制列表 (ACL) 的保护。 允许进行访问的 IP 在一个小的范围内（其大小相当于一个 /27 网络），可托管[特权终结点 (PEP)](azure-stack-privileged-endpoint.md) 和 [Azure Stack Hub 备份](azure-stack-backup-reference.md)之类的服务。

### <a name="public-vip-network"></a>公共 VIP 网络

公共 VIP 网络分配给 Azure Stack Hub 中的网络控制器。 它不是交换机上的逻辑网络。 SLB 针对租户工作负荷使用地址池并分配 /32 网络。 在交换机路由表中，这些 /32 IP 通过 BGP 播发为可用路由。 此网络包含外部可访问的 IP 地址或公共 IP 地址。 Azure Stack Hub 基础结构会保留此公共 VIP 网络中的前 31 个地址，剩余的地址由租户 VM 使用。 此子网中的网络大小范围为最小 /26（64 台主机）到最大 /22（1022 台主机）。 我们建议规划 /24 网络。

### <a name="switch-infrastructure-network"></a>交换机基础结构网络

此 /26 网络是一个子网，其中包含可路由的点到点 IP /30（两个主机 IP）子网和环回（专用于带内交换机管理和 BGP 路由器 ID 的 /32 子网）。 此 IP 地址范围必须可从 Azure Stack Hub 解决方案外部路由到数据中心， 可以是专用或公共 IP。

### <a name="switch-management-network"></a>交换机管理网络

此 /29（六个主机 IP）网络专用于连接交换机的管理端口。 其允许带外访问，以完成部署、管理和故障排除。 它是从上述交换机基础结构网络计算而来的。

## <a name="permitted-networks"></a>允许的网络

从版本 1910 开始，部署工作表将包含一个新字段，允许操作员更改某些访问控制列表 (ACL)，以允许从受信任的数据中心网络范围访问网络设备管理接口和硬件生命周期主机 (HLH)。 更改访问控制列表后，操作员可以允许特定网络范围内的管理 Jumpbox VM 访问交换机管理接口、HLH OS 和 HLH BMC。 操作员可在此列表中提供一个或多个子网；如果留空，则默认为拒绝访问。 借助此项新功能，在部署后，就不需要根据[修改 Azure Stack Hub 交换机配置中的特定设置](azure-stack-customer-defined.md#access-control-list-updates)所述进行人工干预。

## <a name="next-steps"></a>后续步骤

了解网络规划：[边界连接](azure-stack-border-connectivity.md)。
