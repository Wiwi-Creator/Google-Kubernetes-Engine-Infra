---
title: 'EgressNATPolicy'
---

EgressNATPolicy
===

## EgressNATPolicy
---
EgressNATPolicy 是GKE Autopilot中特定的資源，用與控制Cluster內部Pod與外部的連線。

當 Pod 嘗試連接到Cluster外部的資源時，通常會使用Node IP 地址作為源地址（SNAT，或源網絡地址轉換）。

然而，有時您可能希望更改這種行為，EX:當目的地實際上仍然在您的 VPC 中時。

這正是 EgressNATPolicy 的用途。使用此資源，您可以定義特定的目的地 CIDR，

並指示 GKE 不要對這些目的地的出口流量進行 SNAT。

```YAML
Name:         gke-733224c7-1
Namespace:    
Labels:       <none>
Annotations:  <none>
API Version:  networking.gke.io/v1
Kind:         EgressNATPolicy
Metadata:
  Creation Timestamp:  2023-08-22T06:37:50Z
  Generation:          1
  Managed Fields:
    API Version:  networking.gke.io/v1
    Fields Type:  FieldsV1
    fieldsV1:
      f:spec:
        .:
        f:action:
        f:destinations:
      f:status:
    Manager:         egress-nat-controller
    Operation:       Update
    Time:            2023-08-22T06:37:50Z
  Resource Version:  3191
  UID:               a91c0768-f2df-483c-b0c5-d2ce036ae634
Spec:
  Action:  NoSNAT
  Destinations:
    Cidr:  Pod CIDR
    Cidr:  Service CIDR
    Cidr:  Subnet CIDR
Status:
Events:  <none>
```
參數說明：

- Kind : 值是 EgressNATPolicy，這表示該資源的種類。
- Metadata : 
  - Name : 資源名稱
  - Namespace : 資源所在的空間
  - Labels , Annotations : 用預付假數據的鍵值對。
  - 其他由K8S系統自動生成及管理。
- Spec :
  - Action : 定義出口流量如何處理(EX:NoSNAT)
  - Destinations : 包含目的地CIDR的列表，這些目的地將透過Action的指示做處理。
- Status : 由系統自動更新，反映當前狀態。


允許我們決定哪些出口流量需要被 SNAT，哪些不需要。

在需要保留 Pod 的原始 IP 地址以進行某些 VPC 內部通信時非常有用。
