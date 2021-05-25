See tutorial https://kubernetes.io/docs/tutorials/configuration/configure-redis-using-configmap/

```bash
kubectl apply -f example-redis-config.yaml
#kubectl apply -f https://raw.githubusercontent.com/kubernetes/website/master/content/en/examples/pods/config/redis-pod.yaml
# or
kubectl apply -f redis-pod.yaml

# examine created objects
kubectl get pod/redis configmap/example-redis-config

# execute a command on the redis pod
kubectl exec -it redis -- redis-cli

# run commands: CONFIG GET maxmemory

# when updating and applying config maps, pods needs to be restarted to pick updated values
kubectl delete pods redis
kubectl apply -f redis-pod.yaml

# delete all resources
kubectl delete pod/redis configmap/example-redis-config

```