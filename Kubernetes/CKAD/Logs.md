
kubectl logs -f event-simulator-pod event-simulator

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: event-simulator-pod
spec:
  containers:
  - name: event-simulator
    image: kodekloud/event-simulator
  - name: image-processor
    image: some-image-processor
```

- Metrics Server : 쿠버네티스 클러스터마다 하나 
-> 과거 데이터 보기 위해서 고급 모니터링 솔루션을 사용해야 함

```yaml
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml

minikube addons enable metrics-server

kubectl top node # cpu, memory
kubectl top pod
```
