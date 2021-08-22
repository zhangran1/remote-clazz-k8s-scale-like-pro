### GCP Sample commands

Before you execute the command, please update image name in flask-demo.yaml to point to the valid address.

Set zone
```shell
gcloud config set compute/zone asia-southeast1-a
```

Launch 1 node GKE cluster with vertical pod autoscaling enabled
```shell
gcloud container clusters create remote-clazz --num-nodes=1 --enable-vertical-pod-autoscaling
```

Create deployment for flask-demo app
```shell
kubectl apply -f flask-demo.yaml
```

Get Pods provision status
```shell
kubectl get pods
```

Get Pods provision status
```shell
kubectl get deployment
```

Enable AutoScaling for Node
```shell
gcloud container clusters update remote-clazz --enable-autoscaling --min-nodes 1 --max-nodes 5
gcloud  container clusters update remote-clazz --autoscaling-profile optimize-utilization
```

Enable Horizontal AutoScaling for Pods
```shell
kubectl autoscale deployment flask-demo --cpu-percent=10 --min=1 --max=10
```

Get HPA status
```shell
kubectl get hpa
```

Expose Deployment for external access
```shell
kubectl expose deployment flask-demo --type=LoadBalancer --name=flask-demo-external
```

```shell
kubectl get services flask-demo-external
```

Check VPA status
```shell
gcloud container clusters describe remote-clazz | grep ^verticalPodAutoscaling -A 1
```
If VPA is not enabled use the following command, else skip the following command
```shell
gcloud container clusters update remote-clazz --enable-vertical-pod-autoscaling
```

Deploy hello server to test VPA
```shell
kubectl apply -f hello-server.yaml
```

Deploy VPA to monitor hello server
```shell
kubectl apply -f hello-server-vpa.yaml
```

Check recommended resources
```shell
kubectl describe vpa hello-server-vpa
```
Create new replicas to allow VPA take effect
```shell
kubectl scale deployment hello-server --replicas=2
```

Remove cluster
```shell
gcloud container clusters delete remote-clazz
```