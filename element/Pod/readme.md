# Pod Setting

### Resources of Autopilot-Cluster
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

### Prority Class

priority_class: 預設為priority-nonpreempting，如果是優先級較高的Job，則可以定義為high-priority(若發生資源不足，但會強制搶佔其他Pod的資源)，詳情見[文件](https://kubernetes.io/docs/concepts/scheduling-eviction/pod-priority-preemption/)