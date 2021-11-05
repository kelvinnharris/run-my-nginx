# Kubernetes Service

In this demo, we would like to deploy an image using Kubernetes and configure a service to access your application image.

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

After a few moments, the Deployments should be ready. You can check running Deployments by executing the command:

```
$ kubectl get deployments
NAME       READY   UP-TO-DATE   AVAILABLE   AGE
my-nginx   2/2     2            2           23m
```

In this Deployment, we have specified 2 replicas in `run-my-nginx.yaml`. You can check running Pods too by running:

```
$ kubectl get pods
NAME                        READY   STATUS    RESTARTS   AGE
my-nginx-5b56ccd65f-4sv4j   1/1     Running   0          9m26s
my-nginx-5b56ccd65f-nhrvb   1/1     Running   0          9m26s
```

### 3. Create a Service (and expose it to external IP address)

As mentioned in the documentation, Kubernetes supports two ways of exposing a Service onto an external IP address: NodePorts and LoadBalancers. In this case, we use LoadBalancers.  Run the following command to expose it:

```
$ kubectl expose deployment/my-nginx --port=80 --target-port=80 --type=LoadBalancer
service/my-nginx exposed
```

This specification will create a Service which targets TCP port 80 (`target-port`) on any Pod with the `run: my-nginx` label, and expose it on an abstracted Service port (`port`).

The Service created and configured can be checked with the following command:

```
$ kubectl get svc
NAME         TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
kubernetes   ClusterIP      10.96.0.1      <none>        443/TCP        50m
my-nginx     LoadBalancer   10.97.81.204   localhost     80:30906/TCP   3m47s
```

After having connected the configured service, the nginx server can now be reached from any node by doing `curl`, which cannot be done without a configured service.

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


# Task A3

## 1. Apply ingress

`ingress-nginx.yaml` file is configured with ingress rules, where the backend Service and port is configured to the Service created in Task A2.

```
$ kubectl apply -f ./ingress-nginx.yaml
ingress.networking.k8s.io/ingress-myservicea created
```

```
$ kubectl get ingress
NAME                 CLASS   HOSTS       ADDRESS   PORTS   AGE
ingress-myservicea   nginx   localhost             80      55s
```

## 2. 