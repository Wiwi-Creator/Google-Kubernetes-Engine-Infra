## Helm


先下載 helm
``` bash
brew install helm
```

連接cluster後

- add Dagster into helm repo
```bash
helm repo add ${application} https://dagster-io.github.io/helm
```

請確定有 roles/container.admin 權限 , 有些服務需要設定 Kubernetes RBAC
stack_overflow:https://github.com/cert-manager/cert-manager/issues/256
refer:https://cloud.google.com/kubernetes-engine/docs/how-to/iam

```bash
gcloud projects add-iam-policy-binding your-project-id --member=user:${service_account} --role=roles/container.admin
```

將 values.yaml output
```bash
helm show values dagster/dagster > values.yaml
```

透過 values.yaml 客製修改你需要異動的內容
```bash
helm upgrade --install dagster dagster/dagster -f values.yaml
```

檢視當前所有的 Helm release

```bash
helm list -a
```

Uninstall
```bash
helm uninstall dagster
```

若 helm list -a 呈現 Status : uninstalling
下列可將服務移除
```bash
kubectl get secrets
kubectl delete secrets$(releasr_secret) (Ex:sh.helm.release.v1.dagster.v1)
```
