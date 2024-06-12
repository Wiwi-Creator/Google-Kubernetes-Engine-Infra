---
title: 'Cluster'
---

Cluster
===

## Cluster

### **Autopilot VS Standard Cluster**

- Standard 是根據所開的 Node 及 master 管理費做收費

    Autopilot則是根據 Pod 所起的 CPU/Memory/Storage..去做收費

- Standard 須管理Nodes(及相關 Scalling..,node,IP...等)

- Autopilot 有非常多管理上的受限，所以有些特殊的Config設定，不一定能自由修改。

註1:Autopilot default以Google經歷的Best Practice，可以使開發者更專注於開發

註2:Standard則是多數仍須自行管理，但有了更多的彈性

註3:目前兩種模式不存在切換!

### Public Cluster（公共集群）：

- GKE 的 Master 部分是公開可訪問的，它具有Public IP 地址，可以從 Internet 上的任何位置訪問。

- Worker Nodes位於 GCP 的公共 IP 網路中，它們可以直接連接到公共 IP 網路上的其他 GCP 服務和外部服務。

- 在 Public Cluster 中，Pod 和 Service 可以直接訪問外部的公共 IP 和 DNS 網址，並且也可以被外部訪問。

### Private Cluster（私有集群）：
- GKE 的 Master 部分是私有的，沒有公共 IP 地址，不允許從 Internet 直接訪問

    只有集群所在的 VPC 內的機器和服務可以訪問 Master。

- Worker Nodes 位於 GCP 的私有 IP 網路中

    不能直接連接到公共 IP 網路上的外部服務，除非通過 VPC Peering、VPN 等方式進行連接。

- 在 Private Cluster 中，Pod 和 Service 預設只能訪問 VPC 內部的資源

    除非進行相應的網路配置，否則不能直接訪問外部的公共 IP 和 DNS 網址。
    
    需要透過 Private Google Access、VPC Peering 等方式來實現連接外部資源的需求。

Public Cluster 提供了直接的公共 Internet 訪問，而 Private Cluster 則適合更高安全性的場景，它將 GKE Master 隔離在私有網路中，並且僅允許授權的Node和服務訪問。

-----------------------------------------
## 建立GKE Cluster(集群)

一個集群(cluster)包含至少一個cluster control機器和多台worker機器

節點(Nodes)是實際運行應用程式的地方。透過將應用程式部署到叢集，您可以管理和調度應用程式的執行，使其在節點上運行並提供所需的服務。

**建立一個cluster(standard)**
```
gcloud container clusters create ${cluster_name} --region=${REGION}
```
**建立一個cluster(Autopilot)**
```
gcloud container clusters create-auto ${cluster_name} --region=${REGION}
```

    
可用參數說明: 
- --enable-ip-alias 使Cluster成為VPC原生cluster(目前Autopilot模式已預設啟用)
- --enable-private-nodes 使該cluster沒有External IP
- --master-ipv4-cidr 用於指定Control Plane Internal IP範圍
- --region  該cluster區域
- --project=${project_name} 該cluster所屬project
- --network vpn 選擇該cluster的VPC network
- --subnetwork vpn-asia-east1 選擇該cluster的VPC network的subnetwork
- --enable-master-authorized-networks 用來指定授權的 IP CIDR 訪問。
- --enable-private-endpoint 表示使用控制层面 API 端点的专用 I- 地址管理集群。

**檢視cluster list**
```
gcloud container clusters list
```
**獲取身份驗證** (須確認有無gke-gcloud-auth-plugin)
```
gcloud container clusters get-credentials hello-cluster --region ${REGION}
```
刪除
---
**刪除cluster**
```
gcloud container clusters delete hello-cluster --region ${REGION}
```
###### tags: `Templates` `Documentation`


**建立cluster後**
建立reate一個cluster後 GKE會幫你做這些事情：

- 根據你選定的VPC及設定的 VM type，開啟node

- 建立一個 Master 在Google管理的VPC裡面，為了讓master管理node，因此有了VPC peering
