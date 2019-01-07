# 概述
## [关于 Azure 网络](networking-overview.md)
## 体系结构
### [虚拟数据中心](https://docs.microsoft.com/zh-cn/azure/architecture/vdc/networking-virtual-datacenter)


### [中心辐射型拓扑](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)
### [网络安全最佳做法](../security/azure-security-network-security-best-practices.md?toc=%2fnetworking%2ftoc.json)
### [高度可用的网络虚拟设备](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha )
### [结合使用负载均衡方法](../traffic-manager/traffic-manager-load-balancing-azure.md?toc=%2fnetworking%2ftoc.json)
### [使用 Azure DNS 和流量管理器进行灾难恢复](disaster-recovery-dns-traffic-manager.md)
## 规划和设计
### [虚拟网络](../virtual-network/virtual-network-vnet-plan-design-arm.md?toc=%2fnetworking%2ftoc.json)
### [跨界连接 - VPN](../vpn-gateway/vpn-gateway-plan-design.md?toc=%2fnetworking%2ftoc.json)
### [跨界连接 - 专用私有连接](../expressroute/expressroute-workflows.md?toc=%2fnetworking%2ftoc.json)
### 后端连接互操作性
#### [前言和测试设置](connectivty-interoperability-preface.md?toc=%2fnetworking%2ftoc.json)
#### [测试设置配置](connectivty-interoperability-configuration.md?toc=%2fnetworking%2ftoc.json)
#### [控制平面分析](connectivty-interoperability-control-plane.md?toc=%2fnetworking%2ftoc.json)
#### [数据平面分析](connectivty-interoperability-data-plane.md?toc=%2fnetworking%2ftoc.json)

##  概念
### [虚拟网络](../virtual-network/virtual-networks-overview.md?toc=%2fnetworking%2ftoc.json)
### [网络负载均衡](../load-balancer/load-balancer-overview.md?toc=%2fnetworking%2ftoc.json)
### [应用程序负载均衡](../application-gateway/overview.md?toc=%2fnetworking%2ftoc.json)
### [DNS](../dns/dns-overview.md?toc=%2fnetworking%2ftoc.json)
### [基于 DNS 的流量分配](../traffic-manager/traffic-manager-overview.md?toc=%2fnetworking%2ftoc.json)
### [本地连接 - VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fnetworking%2ftoc.json)
### [本地连接 - 专用](../expressroute/expressroute-introduction.md?toc=%2fnetworking%2ftoc.json)


# 入门
## [创建自己的首个虚拟网络](../virtual-network/quick-create-portal.md?toc=%2fnetworking%2ftoc.json)

# 如何
## Internet 连接
### [网络负载均衡公共服务器](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fnetworking%2ftoc.json)
### [应用程序负载均衡公共服务器](../application-gateway/quick-create-portal.md?toc=%2fnetworking%2ftoc.json)
### [保护 Web 应用程序](../application-gateway/application-gateway-web-application-firewall-portal.md?toc=%2fnetworking%2ftoc.json)
### [跨位置分配流量](../traffic-manager/traffic-manager-manage-profiles.md)
## 内部连接
### [网络负载均衡专用服务器](../load-balancer/load-balancer-get-started-ilb-arm-portal.md?toc=%2fnetworking%2ftoc.json)
### [应用程序负载均衡专用服务器](../application-gateway/application-gateway-ilb-arm.md?toc=%2fnetworking%2ftoc.json)
### [连接虚拟网络（在同一位置）](../virtual-network/virtual-networks-create-vnetpeering-arm-portal.md?toc=%2fnetworking%2ftoc.json)
### [连接虚拟网络（在不同位置）](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fnetworking%2ftoc.json)
## 跨界连接
### [创建 S2S VPN 连接 (IPsec/IKE)](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fnetworking%2ftoc.json)
### [创建 P2S VPN 连接（使用证书的 SSTP）](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fnetworking%2ftoc.json)
### [创建专用私有连接 (ExpressRoute)](../expressroute/expressroute-howto-circuit-portal-resource-manager.md?toc=%2fnetworking%2ftoc.json)

## 管理

### [根据 Azure 限制查看资源使用情况](check-usage-against-limits.md)
### [查看网络拓扑](../network-watcher/network-watcher-topology-powershell.md?toc=%2fnetworking%2ftoc.json)

## 示例脚本
### [Azure CLI](cli-samples.md)
### [Azure PowerShell](powershell-samples.md)

## 教程
### [对 VM 进行负载均衡](../virtual-machines/linux/tutorial-load-balancer.md?toc=%2fnetworking%2ftoc.json)
### [将计算机连接到虚拟网络](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fnetworking%2ftoc.json)

# 参考
## [Azure CLI](https://docs.azure.cn/zh-cn/cli/network)
## [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.network/?view=azurermps-3.8.0)
## [.Net](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.management.network?view=azuremgmtnetwork-9.1.0-preview)
## [Node.js](https://azure.microsoft.com/develop/nodejs/#azure-sdk)
## [REST](https://msdn.microsoft.com/library/mt163658.aspx)

# 资源
## [创作模板](/azure-resource-manager/resource-group-authoring-templates?toc=%2fnetworking%2ftoc.json)


## [网络博客](https://azure.microsoft.com/blog/topics/networking)
## [价格](https://www.azure.cn/zh-cn/pricing/overview/)
## [定价计算器](https://www.azure.cn/zh-cn/pricing/calculator/)
## [区域可用性](https://www.azure.cn/zh-cn/home/features/products-by-region)
## [MSDN](https://social.msdn.microsoft.com/Forums/zh-CN/home?forum=windowsazurezhchs)

