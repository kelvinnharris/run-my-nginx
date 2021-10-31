# Task A2

Deploy an image using Kubernetes and configure a service to access your application image.

## Steps to reproduce solution

### 1. Ensure Docker desktop is configured with Kubernetes

- Go to settings &#8594; Kubernetes
- Check the option 'Enable Kubernetes'
- Click 'Apply & Restart'

### 2. Expose pods to the cluster

Pods are configured in the yaml file. The image used is nginx. Create Deployment by running the following command:

```
$ kubectl apply -f ./run-my-nginx.yaml
deployment.apps/my-nginx created
```

Check running Deployments by running:

```
$ kubectl get deployments
NAME       READY   UP-TO-DATE   AVAILABLE   AGE
my-nginx   2/2     2            2           23m
```

### 3. Create a Service (and expose it to external IP address)

In this case, we use LoadBalancers. 
```
$ kubectl expose deployment/my-nginx --port=80 --target-port=80 --type=LoadBalancer
service/my-nginx exposed
```

The Service created and configured can be checked with the following command:

```
$ kubectl get svc
NAME         TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
kubernetes   ClusterIP      10.96.0.1      <none>        443/TCP        50m
my-nginx     LoadBalancer   10.97.81.204   localhost     80:30906/TCP   3m47s
```

The nginx server can now be reached from any node.

```
$ curl localhost:80
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```


## References
- https://kubernetes.io/docs/concepts/services-networking/connect-applications-service/
- https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
