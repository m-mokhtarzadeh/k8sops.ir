---
date: 2023-01-14T16:35:00Z
lastmod: 2023-01-14T16:35:00Z
publishdate: 2023-01-14T16:35:00Z

title: Explaining Pod manifest structure
#description: Create simple Pod in kubernetes
images:
- images/kops.jpg
---
---
Every Kubernetes resource manifest inlude following sections
* apiVersion
* kind
* metadata
* spec

You can see some example bellow.  
Pod example:
```yml
apiVersion: v1
kind: Pod
metadata:
  name: app
spec:
  containers:
  - name: app
    image: nginx
```
Replicaset example:
```yml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```