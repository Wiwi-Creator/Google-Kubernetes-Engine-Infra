---
title: 'Network'
---

GKE Network
===

## GKE 內部的Network
---
在GCP中，Virtual Private Cloud（簡稱 VPC）為使用者提供了全球性、可擴展且靈活的虛擬私有網絡，在 GCP 中是一項非常重要的基礎服務，它能夠讓使用者部署像是 Google Compute Engine、Google Kubernetes Engine 和 Google App Engine 等 GCP 服務到雲端。
越複雜的架構到後期更難去變動，因此會建議使用者應在一開始時就對 VPC Network 的設計進行規劃。

**Pod及Node間的網路**

- 每個Pod皆會被Node分配一個IP，其中的container可以自由通信。
![Alt text](image-3.png)
- 在同一個Node中，Pod1和Pod2雖有不同的IP，但皆在同一網段(Docker0brudge取得)中，故也可以直接通信。
![Alt text](image-4.png)

- 不同node中，docker0與Node主機是兩個不同的網段，透過不同Node間的溝通只能依賴Node主機上的網路才行。主要是結合Node IP與Pod IP，讓Pod可以找到目的IP，而GKE中的Google網路會自動幫您處理。
![Alt text](image-5.png)

而虛擬機器(node)包含了兩項重要的參數：NETWORK_IP, NAT_IP。
- NETWORK_IP: 此為GCE被GCP內網所分配的IP位址，可作為內部IP
- NAT_IP: 可解讀為GCE的外部IP，是連上Internet的IP位址

這裡面有三項參數：EXTERNAL-IP, INTERNAL-IP, POD-CIDR
- EXTERNAL-IP: 104.155.226.39為External IP，與GCE的NAT_IP一致
- INTERNAL-IP: 10.140.0.2為Intrenal IP，與GCE的NETWORK_IP一致
- POD-CIDR: 10.76.0.0/24，與該台GCE的routing相同範圍(DEST_RANGE)，從網域來看即為該節點所支持的Pod連結數

以上無論是從GCE看或者從GKE看，各項參數其實背後的原理都是串聯在一起的。

---

VPC Network 設定
===

## Networking
---
在GCP中，Virtual Private Cloud（簡稱 VPC）為使用者提供了全球性、可擴展且靈活的虛擬私有網絡，在 GCP 中是一項非常重要的基礎服務，它能夠讓使用者部署像是 Google Compute Engine、Google Kubernetes Engine 和 Google App Engine 等 GCP 服務到雲端。
越複雜的架構到後期更難去變動，因此會建議使用者應在一開始時就對 VPC Network 的設計進行規劃。

![Alt text](image-2.png)

- 網路(network)選擇已經新增的VPC network (EX:"vpn")
- 節點子網路(subnet)選擇所選的network下的子網路(vpn-asia-east1)

## Ip-Masq-Agent

## NAT Policy
