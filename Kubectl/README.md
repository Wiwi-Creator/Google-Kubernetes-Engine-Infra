## kubectl 
```
kubectl exec -it ${Pod_name} -c ${container_name} -- ${command}
```
原理同Docker exec，需要進入一個已經運行中(running)的Pod，並執行指定指令。

故需要先做kubectl apply 啟動一個 Pod

但因沒有 --env 指令指定環境變數，故暫使用 export 方式

### 比較

- 比較多的做法是 kubectl exec，run比較多適用在測試Pod
- run預設只會有1個Container，但我們使用的Pod中有使用到 GCSfuse CSI Driver
  會有兩個container，因run沒有 - c 指定 container的command，故使用上會有問題(除非捨去GCSFuse CSI Driver)。
- run在執行上比較難Debug。
- Run執行時，1個table的Job傳導便會有1個Pod的產生! Exec則是可以將所有Job都在同一個Pod(Container)中執行。

### CronJob , Job 

由於Job是完全異步的處理，所以要檢視Log並配合Digdag的警示是困難的。
```
kubectl apply -f my-cronjob.yaml
sleep 10  # 等待 Pod 啟動
POD_NAME=$(kubectl get pods --selector=job-name=[YOUR_JOB_NAME_PREFIX] -o jsonpath='{.items[-1:].metadata.name}')
sleep ??  #等待 job 完成
kubectl logs $POD_NAME > output.log
```