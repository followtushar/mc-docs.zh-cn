通过在子网或 VM 网络接口上创建网络筛选器可为 Azure 中的虚拟机 (VM) 打开端口或创建终结点。将这些筛选器（控制入站和出站流量）放在网络安全组中，并附加到将接收流量的资源。

让我们在端口 80 上使用 Web 流量的常见示例。将 VM 配置为处理标准 TCP 端口 80 上的 Web 请求后（请记住还要在 VM 上启动相应的服务，并打开所有 OS 防火墙规则），可以：

1. 创建网络安全组。
2. 使用以下项创建允许流量的入站规则：
  - 目标端口范围为“80”
  - 源端口范围为“*”（允许任何源端口）
  - 优先级值小于 65,500（优先级应比默认的“捕获全部”拒绝入站规则要高）
3. 将网络安全组与 VM 网络接口或子网相关联。

可以创建复杂的网络配置，以使用网络安全组和规则来保护你的环境。本示例仅使用一个或两个允许 HTTP 流量或远程管理的规则。有关详细信息，请参阅下面的[更多信息](#more-information-on-network-security-groups)部分或[什么是网络安全组？](../articles/virtual-network/virtual-networks-nsg.md)

<!---HONumber=Mooncake_1010_2016-->