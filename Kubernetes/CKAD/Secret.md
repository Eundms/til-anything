# Secret

## --from-literal
```shell
kubectl create secret generic <secret-name> \ 
  --from-literal=<key>=<value>

kubectl create secret generic app-secret \ 
  --from-literal=DB_HOST=mysql \
  --from-literal=DB_Passwowrd=passwrd
```

## --from-file
```shell
# app_secret.properties
DB_HOST: mysql
DB_User: root
DB_Password: paswrd
```

```shell
kubectl create secret generic app-secret \ 
  --from-file=app_secret.properties
```

## 명령어 확인하기 
```shell
kubectl get secrets -o yaml #해시값
kubectl describe secrets
base64 --decode
```

- env
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp-color
labels:
  name: simple-webapp-color
spec:
  containers:
  - name: simple-webapp-color
    image: simple-webapp-color
    ports:
      - containerPort: 8080
    envFrom:
      - secretRef:
          name: app-secret
```

- simple env
```yaml
env:
  - name: DB_Password
    valueFrom:
      secretKeyRef:
        name: app-secret
        key: DB_Password
```

- volume
```yaml
volumes:
- name: app-secret-volume
  secret:
    secretName: app-secret
```

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
data:
  DB_Host: bdfdfdf
  DB_User: cm9vdA==
  DB_PASSWORD: cGFzd3JK
```

## etcd Encryption at Rest 활성화
- https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/

kubectl create secret --help

kubectl create secret generic my-secret --from-literal=key1=supersecret
kubectl describe secret my-secret -o yaml
echo "" | base64 --decode

kubectl run nginx --image=nginx

kubectl get pods

