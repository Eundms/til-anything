# kubectl 명령어

kubectl api-resources

kubectl explain pods 

kubectl explain pods.spec

kubectl explain pods --recursive

kubectl run <파드명> --image=<이미지명> --port=<포트>

kubectl run <파드명> --image=<이미지명> --labels="<라벨명>=<값>"


1. Deployment 생성
kubectl create deployment webapp --image=nginx 

2. Replica 3으로 스케일
kubectl scale deployment webapp --replicas=3


kubectl create deployment redis-deploy --namespace=dev-ns --image=redis --replicas=2


```markdown
Create a pod named httpd using the image httpd:alpine 
in the default namespace. Then, create a service of type 
ClusterIP with the same name(httpd) that exposes the pod
on port 80.
```

```yaml
kubectl expose pod redis \
--name=redis-service \ 
--port=6379 \
--target-port=6379 \
--selector=app=redis
```

## Commands and Arguments in Kubernetes

```yaml
kubectl create -f ubuntu-sleeper-2.yaml
```