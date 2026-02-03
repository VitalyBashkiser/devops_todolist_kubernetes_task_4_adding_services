# Inspection Instructions

## Applying Manifests

```bash
kubectl apply -f k8s/pods.yaml -f k8s/service-clusterip.yaml -f k8s/service-nodeport.yaml
```

## Check via ClusterIP DNS from busybox

1. Run a temporary busybox container:

```bash
kubectl -n mateapp run -it --rm busybox --image=busybox:1.36 --restart=Never -- sh
```

2. Inside busybox, query the service via DNS:

```sh
wget -qO- http://todolist-clusterip
```

If you need full DNS:

```sh
wget -qO- http://todolist-clusterip.mateapp.svc.cluster.local
```

3. Log out of busybox:

```sh
exit
```

## Validation via port-forward

1. Forward the service port to the local machine:

```bash
kubectl -n mateapp port-forward service/todolist-clusterip 8080:80
```

2. In another terminal, open the app:

```bash
curl http://localhost:8080/
```

## NodePort access

1. Find out the IP of the cluster node:

```bash
kubectl get nodes -o wide
```

2. Open the application using NodePort:

```bash
NODE_PORT=$(kubectl -n mateapp get svc todolist-nodeport -o jsonpath='{.spec.ports[0].nodePort}')
curl http://<NODE_IP>:$NODE_PORT/
```

If you're using minikube, instead of step 1, you can take the IP as follows:

```bash
minikube ip
```
