---
title: 'ip-masq-agent'
---

ip-masq-agent
===

配置 iptables 規則用於隱藏 Pod 的 IP。 
通常使用在將溜量發送至Cluster CIDR 範圍外使用。

IP masquerading : It is a form of source network address translation (SNAT) that performs many-to-one IP address translations. 

GKE can use IP masquerading to change the source IP addresses of packets sent from Pods.

## ip-masq-agent
---

## key word:

NAT(Network Address Translation):

網路地址轉換，是一種透過修改 IP 的源和/或目標地址信息將一個 IP 地址重新映射 到另一 IP 地址的方法。

通常由執行 IP 路由的設備执行。

masquerading(偽裝):

NAT的一種形式，常被用於多對一的IP轉換，source IP會被隱藏在某IP後，該IP是執行IP路由的設備。(K8S即Node IP)

CIDR（无类别域间路由）： 

Classless Inter-Domain Routing , 一種用於IP和地止分配的方法，目的是更有效地利用 IP 地址和及分配IP

目前已經IP網路的標準。

Link Local :

是一種僅在主機連接到的網絡段或廣播域內有效的網絡地址。

IPv4 鏈路本地地址在 CIDR 表示法中定義為 169.254.0.0/16 地址塊。


- 當主機沒有獲取到合法的 IP 地址時，它會自動分配一個鏈路本地地址。

- 主機可以使用鏈路本地地址來發現和連接到同一網絡段上的其他主機。

- 一些服務（例如 DHCPv6）使用鏈路本地地址來進行通信。