---
title: 'Google Kubernetes Engine (GKE)'
---

Google Kubernetes Engine (GKE)
===

## Google Kubernetes Engine 

### Why GKE?

- New Tools ,Framework <- 需要動態滿足及擴增情境提供客戶。 良好的擴展性
- AI Foundation platform. 客製化的CPU , Memory , TPU
- 大型語言模型 網路至關重要 -> Node可能會擴展
- 模型成長遠遠大於硬體成長

A Platform For All Workloads
![Alt text](img/image.png)

### AI journey on GKE
![Alt text](img/messageImage_1697614300334.jpg)


### Trainging jobs on TPU VMS today
![Alt text](img/messageImage_1697614575031.jpg)


### Trainging jobs on in GKE VMS
![Alt text](img/messageImage_1697614632188.jpg)


### Muliclice Training jobs on GKE
![Alt text](img/messageImage_1697614827212.jpg)


### Keyword 

Node Pool：很多機器 ＝ instance group

Node： 相當於 VM 一個node及對應一台GCE (使用standard cluster時可以在GCE上看到許多Node)

Cluster：整個叢集(包含node,pod,....)

Master：是這個Cluster的控制中心，裡面安裝了kubernetes controller，可以控制cluster中所有components的行為


### Cloud Shell ###
Cloud Shell預設有Google Cloud CLI,kubectl工具

設置默認項目ID(projectID):
```
gcloud config set project PROJECT_ID
```

## Cluster
在理解基礎架構後 , 需要先建立Cluster
- [Cluster](element/Cluster/cluster.md)

## Workload Identity
需要先將Kubernetes Service account做相關設定
- [Workload Identity](Workload_Identity/workload-Identity.md)

## Network
希望將Pod透過VPC對外連線 , 需要設定以下:

1. [ip-masq-agent](Network/ip-masq-agent/ip-masq-agent.md)

2. [EgressNATpolicy](Network/EgressNATpolicy/EgressNATpolicy.md)

3. [Pod](element/Pod/readme.md)
   - [Priority class]
   - [Resources request]

4. [Cloud Storage FUSE CSI driver](gcs_fuse_csi_driver/README.md)
   
5. [kubectl aplly & kubectl exec](Kubectl/README.md)