
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
----------------------------------------------------------
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
~                                  
