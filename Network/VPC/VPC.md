---
title: 'Network'
---

Network
===

## Networking
---
在GKE中建立cluster，通常透過以下幾種網路方式做內部Pod的連接

**建立一個cluster**
```
gcloud container clusters create-auto ${cluster_name} --region=${REGION}

gcloud container clusters create [CLUSTER_NAME] --enable-stackdriver-kubernetes

```
**檢視cluster list**
```
gcloud container clusters list
```
**獲取身份驗證** (須確認有無gke-gcloud-auth-plugin)
```
gcloud container clusters get-credentials hello-cluster --region ${REGION}
```
**創建Deployment**
因為我們需要在cluster中執行embulk
```
kubectl create deployment hello-server --image=image_name
```
刪除
---
**刪除cluster**
```
gcloud container clusters delete hello-cluster --region ${REGION}
```
###### tags: `Templates` `Documentation`


Demo
---
建立image，並上傳至GCR
接下來可以在本機，或是在console上建立cluster
```
gcloud container clusters create-auto ${cluster_name} --region=${region}
```
接下來我們希望在該cluster中建立Job，有兩種方式:
1.在console中，使用cloudshell
2.在本地連接指定cluster，並建立Job:
```
kubectl apply -f job.yaml
```
![](https://hackmd.io/_uploads/r1KUr4jr2.png)

