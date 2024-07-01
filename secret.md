### Create a Secret
```
kubectl create secret generic secret-1 --from-literal=db_user=admin --from-literal=db_pwd=123
```
```
kubectl get secret
```
```
kubectl describe secret secret-1
```
### Inject PARTICULAR variables from Secret(FromLiteral) into POD
Create secret as shown below
```
vi sc-pod-2
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: web
  name: sc-pod-2
spec:
  containers:
  - image: httpd
    name: ctr-1
    ports:
    - containerPort: 80
    env:
    - name: db_password   #new-key
      valueFrom:
        secretKeyRef:
          name: secret-1
          key: db_pwd     #old=key
```
Apply the Pod configuration to your 

```
kubectl apply -f sc-pod-2.yaml
```
check the status of pod
```
kubectl get pod
```
Enter into the pod and check if the variable has been passed correctly or not
```
kubectl exec -it env-pod -- bash
```
```
env | grep db_
```
```
exit
```
### Injecting ConfigMap as volume mount
Create a file
```
vi token
```
```
This is CKAD Training. We are practicing Injecting variables from ConfigMaps(FromFile) into POD.
Create a ConfigMap
```
save the file

```
kubectl create secret generic file-secret --from-file=token
```
```
kubectl get secret
```
```
kubectl describe secret file-secret
```
```
kubectl get secret file-secret -o yaml
```

```
vi sc-pod-3.yaml
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: web
  name: sc-pod-3
spec:
  volumes:
  - name: secret-volume
    secret:
      secretName: file-secret
  containers:
  - image: httpd
    name: ctr-1
    ports:
    - containerPort: 80
    volumeMounts:
    - name: secret-volume
      mountPath: /tmp/credential/
```                               
Apply the Pod configuration to your 

```
kubectl apply -f sc-pod-3.yaml
```
check the status of pod
```
kubectl get pod
```
Enter into the pod and check if the variable has been passed correctly or not
```
kubectl exec -it sc-pod-3 -- bash
```
```
cd /tmp/credential/
```
```
ls
cat token
```
```
exit
```
