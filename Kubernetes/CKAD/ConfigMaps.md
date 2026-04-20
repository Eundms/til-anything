# ConfigMap
```yaml
kubectl create configmap <컨피그이름> 

--from-literal=<key>=<value>. # 문자열
--from-file=<path-to-file>  # config파일에서 설정하기
```

- kubectl get configmap
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp-color
spec:
  containers:
  - name: simple-webapp-color
    image: simple-webapp-color
    ports:
      - containerPort: 8080
    envFrom:
      - configMapRef:
          name: app-config
```

- Single ENV
```yaml
env:
- name: APP_COLOR
  valueFrom:
    configMapKeyRef:
      name: app-config
      key: APP_COLOR
```

- ConfigMap
```yaml
env:
- name: APP_COLOR
  valueFrom:
    configMap:
      name: app-config
```

kubectl get pod webapp-color -o yaml > pod.yaml

kubectl create configmap webapp-config-map \
  --from-literal=APP_COLOR=darkblue \
  --from-literal=APP_OTHER=disregard


```yaml
env:
  - name: APP_COLOR
    valueFrom:
      configMapKeyRef:
        name: webapp-config-map
        key: APP_COLOR
```

