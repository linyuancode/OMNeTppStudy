INET框架中IEEE802.11模型的NIC（网卡）分为了4种：
-  Ieee80211Interface：通用（可配置）网卡
- Ieee80211NicAdhoc：用于Ad Hoc模式
- Ieee80211NicAP、Ieee80211NicAPSimplified：用于接入点（AP）
- Ieee80211NicSTA、Ieee80211NicSTASimplified：用于基础设施模式站

NIC由四层组成，自上而下分别为：
- 代理
- 管理
- MAC
- 物理层（无线）

INET中WirelessHost与AccessPoint的管理组件分别为Ieee80211MgmtSta与Ieee80211MgmtAp
INET中WirelessHost的代理组件为Ieee80211AgentSTA（AP无代理组件）
为了实现WirelessHost与指定AP的连接，在AP的管理层更改默认SSID，在Host的代理层更改默认连接SSID即可设定Host在多AP环境中连接特定AP（AP信号在该Host处可用，若不可用设备默认配置下不会连接其他可用AP）
```
*.gateway[0].wlan[*].mgmt.ssid = "gateway0"					#设定SSID，默认值为SSID
*.gateway[1].wlan[*].mgmt.ssid = "gateway1"

*.host[0..5].wlan[*].agent.defaultSsid = "gateway0"			#更改默认连接SSID，默认值为SSID
```
此外，IEEE802.11设备的MAC地址可以在MAC层手动配置，相关代码为：
```
*.gateway[0].wlan[0].Address = "10:00:00:00:00:00"
```