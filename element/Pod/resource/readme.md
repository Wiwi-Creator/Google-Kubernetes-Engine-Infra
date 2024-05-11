## Resources of Autopilot-Cluster
可以先檢視官網中，Default的Container中 CPU , GPU , Memory大小 如下:

```
      cpu:                0.5v 
      ephemeral-storage:  1Gi  
      memory:             2GiB
```
-  ephemeral-storage:臨時存儲空間(執行embulk job時會產生Log,臨時csv時會佔用許多空間) 
-  cpu:預設只有 0.5 核 速度非常慢

所以需要針對 日常執行的Job做測試，檢視目前預設Pod/Node的規格是否足夠

```
      resources:
        requests:
          cpu: "8"
          ephemeral-storage: "5Gi" 
          memory: "32Gi"
        limits:
          ephemeral-storage: "10Gi" 
```