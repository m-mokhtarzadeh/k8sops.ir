---
date: 2023-01-14T16:34:00Z
lastmod: 2023-01-14T16:34:00Z
publishdate: 2023-01-14T16:34:00Z

title: Checking Pod with liveness probe in Kubernetes
#description: Create simple Pod in kubernetes
images:
- images/kops.jpg
---

---
With liveness probe Kubernetes can check a container is still alive.  
Following example is a HTTP Get liveness probe.
```yml
apiVersion: v1
kind: Pod
metadata:
  name: app
spec:
  containers:
  - name: app
	  image: k8sops-pod-example
    ports:
    - containerPort: 80
		# Following lines were added
    livenessProbe:
	  httpGet:
	    path: /
		port: 80
    initialDelaySeconds: 15
```
Kubernetes can probe a container using one of the three mechanisms:
* HTTP Get
* TCP Socket
* Exec  

`initialDelaySeconds` Kubernetes with this option will wait 15 seconds before executing the first probe.  
You can create a file with .yml extension and paste the above yml manifest and use kubectl to create a simple Nginx Pod resource with liveness probe support.  

```bash
kubectl apply -f liveness-example.yml
```

You can also create the above resource directly from the site content.  
```bash
kubectl apply -f https://k8sops.ir/session6/liveness-example.yml
```