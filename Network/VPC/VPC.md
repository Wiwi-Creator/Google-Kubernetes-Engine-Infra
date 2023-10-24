
VPC Network
===

## Networking
---
在GCP中，Virtual Private Cloud（簡稱 VPC）為使用者提供了全球性、可擴展且靈活的虛擬私有網絡，在 GCP 中是一項非常重要的基礎服務，它能夠讓使用者部署像是 Google Compute Engine、Google Kubernetes Engine 和 Google App Engine 等 GCP 服務到雲端。
越複雜的架構到後期更難去變動，因此會建議使用者應在一開始時就對 VPC Network 的設計進行規劃。

- 網路(network)選擇已經新增的VPC network (EX:"vpn")
- 節點子網路(subnet)選擇所選的network下的子網路(vpn-asia-east1)