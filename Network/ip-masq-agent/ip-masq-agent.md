---
title: 'ip-masq-agent'
---
ip-masq-agent
===

### 背景
配置 iptables 規則用於隱藏 Pod 的 IP。 Pod通常會有一個不同於外部網絡的私有 IP 地址。當這些 Pod 嘗試與Cluster外部的網絡資源通信時，需要某種機制來讓這些資源知道如何響應。

### 目的

IP Masquerade Agent 主要用於設定 `IP Masquerading` (或 Source NAT) 規則，使得 Pod 內的container在訪問Cluster 外部的 IP 地址時，可以使用Node的 IP 地址作為源地址。

### 作用
確保從 Pod 發出的流量在離開Node時會被 NAT成該 Node 的 IP 地址，這樣外部資源就可以對其進行響應。

### 參數
- nonMasqueradeCIDRs: 一個 CIDR 地址列表。來自這些地址的流量不會被偽裝。

    通常包括 **Pod 和 Service 的 CIDR 範圍**，以確保Cluster內部的流量不會進行不必要的 NAT。(EX:Autopilot-clsuter預設)

- masqLinkLocal: 一個Boolean，用於決定是否偽裝 link-local 地址 (169.254.0.0/16)。通常被設置 false。
    因為 link-local 地址在本地網路上是自動配置的，並且不應該在網路之外被路由。

- resyncInterval: 這定義了 ip-masq-agent 應該多久重新同步一次 IP masquerade 規則。EX:"60s" 每60秒同步一次。

### 不希望偽裝Pod IP的原因

K8S中，Pod有自己的IP，其不會是VPC或是外部網路可路由到的地址。

而這些Pod在cluser內部是可以通信的。

當訪問 **VPC外服務和外部服務**時，可以不需要設定偽裝Pod IP，因為Pod IP會被NAT成Node IP。

有以下原因會不希望Pod IP被偽裝

- 當Pod需要和cluser內部Pod通信時
    因為該狀況下，源地址和目的地址都在同Pod CIDR。所以不希望之間流量被 NAT。

- 訪問VPC內部其他資源時
    因為VPC內部資源可以互相識別通信。所以不需要設定偽裝。


即使Pod需要訪問Cluster外部的資源，而這外部資源位於同一VPC中。

這些資源知道如何路由回 Pod IP。因此，保持原始 Pod IP 可能更有利於網路策略和路由。

EX:許多公司的內部資源可能已經設定了基於源 IP 的訪問控制。

在這種情況下，保持原始 Pod IP 有助於維護這些訪問控制策略的正確性。
---

## ip-masq-agent DaemonSet

### DaemonSet Vs Configmap
DaemonSet
- 功能:
    `DaemonSet`確保每個Node上都運行指定的Pod。

    當新增Node到Cluster時，`DaemonSet`會確保這個Node上也啟動指定的Pod。
    
    反之，當Node移除時，該Node上由`DaemonSet`管理的Pod也會被移除。

- 使用情境:
    對於需要在每個Node上運行的系統級應用程序(EX:Monitor,Log監控..等)非常有用

Configmap
- 功能:
    允許我們將配置資料和Container的Image分開。
    這樣即可在`ConfigMap`中儲存設定資料，然後在Pod中引用，而不必每次修改配置時都重新建立image。
- 使用情境:
    你的應用程序需要配置。你可以將配置存放在`ConfigMap`中，然後在 Pod 的定義中參照它，即使配置變更時，只需要更新`ConfigMap`，不需要重新build和push image。

### ip-masq-agent DaemonSet
是一個特殊的 DaemonSet，其目的是在 Kubernetes 節點上設置 IP Masquerading (或稱為 Source NAT) 規則。

## key word:

NAT(Network Address Translation):

網路地址轉換，是一種透過修改 IP 的源和/或目標地址信息將一個 IP 地址重新映射 到另一 IP 地址的方法。

通常由執行 IP 路由的設備执行。

masquerading(偽裝):

NAT的一種形式，常被用於多對一的IP轉換，source IP會被隱藏在某IP後，該IP是執行IP路由的設備。(K8S 即 Node IP)

CIDR（无类别域间路由）： 

Classless Inter-Domain Routing , 一種用於IP和地止分配的方法，目的是更有效地利用 IP 地址和及分配IP，目前已經是IP網路的標準。

