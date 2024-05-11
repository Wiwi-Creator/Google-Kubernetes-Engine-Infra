##  Cloud Storage FUSE CSI driver

- Recommanded way for GKE to mount file On GCS
- 原理是新增一個附加Container(CSI sidecar)當作Pod和GCS的接口

### Enable the Cloud Storage FUSE CSI driver

如果使用的是Autopilot-cluster，不用再手動建立或修改，因為會自動開啟該功能。

希望透過Pod(KSA) mount到 GCS上的file，所以會需要GSA的權限!

### Consume the CSI ephemeral storage volume in a Pod

```
apiVersion: v1
kind: Pod
metadata:
  name: fuse-pod
  annotations:
    gke-gcsfuse/volumes: "true"
spec:
  terminationGracePeriodSeconds: 300
  serviceAccountName: ${service-account}
  containers:
    - name: ${container_name}}
      image: ${image_name}}
      volumeMounts:
      - name: gcs-fuse-csi-ephemeral
        mountPath: /app/gcs_bucket
        readOnly: false
  volumes:
  - name: gcs-fuse-csi-ephemeral
    csi:
      driver: gcsfuse.csi.storage.gke.io
      readOnly: false
      volumeAttributes:
        bucketName: wiwi_test
        mountOptions: "implicit-dirs"
  restartPolicy: Always
```
