---
date: 2023-01-14T16:37:00Z
lastmod: 2023-01-14T16:37:00Z
publishdate: 2023-01-14T16:37:00Z

title: Creating a simple Pod in Kubernetes
#description: Create simple Pod in kubernetes
images:
- images/kops.jpg
---

---
The following example is a simple manifest to create Pod.  
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
You can create a file with .yml extension and paste the above yml manifest and use kubectl to create a simple Nginx Pod resource.  

```bash
kubectl apply -f pod-example.yml
```

You can also create the above resource directly from the site content.  
```bash
kubectl apply -f https://k8sops.ir/session6/pod-example.yml
```