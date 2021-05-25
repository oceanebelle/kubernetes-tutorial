# Tutorial using Stateless applications
https://kubernetes.io/docs/tutorials/stateless-application/guestbook/

```bash
# Create a deployment
kubectl apply -f mongo-deployment.yaml
kubectl get deployments
kubectl logs -f deployment/mongo

# Create a service for the deployment. the service is attached to deployment via tags
kubectl apply -f mongo-service.yaml
kubectl get services

# deploy the front end
kubectl apply -f frontend-deployment.yaml
kubectl get pods -l app.kubernetes.io/name=guestbook

# deploy the front end service
kubectl apply -f frontend-service.yaml
kubectl get services

# port forward the port to be accessible outside of the cluster
kubectl port-forward service/frontend 8080:80

curl http://localhost:8080

# to use load balancer, find the external ip 
kubectl get service frontend

# scale up the frontend
kubectl scale deployment frontend --replicas=2
kubectl get pods --watch

# Cleanup using labels
kubectl delete deployment -l app.kubernetes.io/name=mongo
kubectl delete service -l app.kubernetes.io/name=mongo
kubectl delete deployment -l app.kubernetes.io/name=guestbook
kubectl delete service -l app.kubernetes.io/name=guestbook
```

