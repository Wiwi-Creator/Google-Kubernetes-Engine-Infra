Workload-Identity
===
--- 
應用程式通常一些認證才得以連接其他服務。
而 fleet Workload Identity 則是最推薦提供App工作負載 去和GCP API / 服務作認證的方式。
### What is Fleet Workload-Identity?

它擴展了GKE中工作負載的功能。使cluster中的工作負載 可以不需透過下載,管理GSA。
GKE可以直接使用 workload identity pool (允許 Access Management 信任你的 KSA)

### How it works?
使用了工作負載 使你的應用程式可以特別使用一個認證去連接GCP

```
serviceAccount:FLEET_PROJECT_ID.svc.id.goog[K8S_NAMESPACE/KSA_NAME]
```

### step1. Setting Cluster

如果使用的是Autopilot-cluster，不用再手動建立或修改 Workload-pool，因為會自動Default分配

### step2. Create KSA
Create a Kubernetes service account for your application to use.

```
kubectl create serviceaccount KSA_NAME --namespace NAMESPACE
```

### step3. Configure applications to use Workload Identity

使得 KSA 可以使用 GSA 的相等權限

```
gcloud iam service-accounts add-iam-policy-binding GSA_NAME@GSA_PROJECT.iam.gserviceaccount.com \
    --role roles/iam.workloadIdentityUser \
    --member "serviceAccount:PROJECT_ID.svc.id.goog[NAMESPACE/KSA_NAME]"
```
### step4. Annotate KSA

```
kubectl annotate serviceaccount KSA_NAME \
    --namespace NAMESPACE \
    iam.gke.io/gcp-service-account=GSA_NAME@GSA_PROJECT.iam.gserviceaccount.com
```

### step5. Update Spec on Pod .

修改 Pod spec 並指定 serviceaccount

```
spec:
  serviceAccountName: KSA_NAME
  nodeSelector:
    iam.gke.io/gke-metadata-server-enabled: "true"
```

這樣Pod便可以使用建立的KSA，且該KSA已經有了和 GSA 相同的權限。

### Reference:
- [Kubernetes](https://docs.docker.com/get-started/overview/)
- [Google Kubernetes Engine](https://docs.docker.com/engine/reference/commandline/cli/)
- [GKE-ip-masq_agent](https://cloud.google.com/kubernetes-engine/docs/how-to/ip-masquerade-agent?hl=zh-cn)
- [GKE-Cloud_NAT_policy](https://cloud.google.com/kubernetes-engine/docs/how-to/egress-nat-policy-ip-masq-autopilot)
- [GKE-Workload Identity](https://cloud.google.com/kubernetes-engine/docs/how-to/workload-identity)
- [Container Registry](https://cloud.google.com/container-registry/docs)