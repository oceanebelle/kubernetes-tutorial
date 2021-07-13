
# install minikube and kubectl and statt kubernetes cluster

    # Start minikube
    minikube start --addons=dashboard --addons=metrics-server --addons="ingress" --addons="ingress-dns"

    # Check version
    kubectl version

    # Check the nodes. 
    # master node - run software that control the cluster. the kubernetes software aka controller plane.# worker nodes - where containers run
    kubectl get nodes

    # Stop the minikube cluster
    minikube stop

    # Start minikube again
    minikube start

# run a container to the cluster (the imperative way) (synonymous to docker run)

    # Use the kubectl command to run an nginx container. Pod is a container with more features.
    # imperative commands - one off request. fragile
    kubectl run --image=nginx web

    # check if the pod is running
    kubectl get pods

    # get details of a pod
    kubectl describe pod web

# run a container to the cluster using the declarative way (synonymous to docker-compose)

    # deploy the newly defined pod
    kubectl apply -f web-declarative.yaml

    kubectl get pods

# Lesson1 Blue Green deployment (exposing pods to other pods in the same cluster using ClusterIP service)

    cd lesson1
    
    # deploy the green pod and expose the port and give it a name (create a service)
    kubectl apply -f lesson1/green.yaml

    # imperative command of creating a service *(manual)
    kubectl expose pod green --port 8080 --name blue-green
    kubectl delete service blue-green

    # use declarative definition of the service to expose a pod to other pods running in same cluster
    kubectl apply -f lesson1/blue-green.yaml

    # same app labels but different images so a blue green test. each pod behaves differently.     
    kubectl apply -f lesson1/blue.yaml

# Lesson2 Exposing pods outside of the cluster (using NodePort service)

    cd lesson2

    # deploy all yamls in current directory
    kubectl apply -f .

    # Change the type of the service from ClusterIP to NodePort (random)
    kubectl apply -f .

    # find the ip of the cluster using minikube and use get services to find out the port
    minikube ip
    kubectl get services

    # Using the ip and the port ie 80:30132 access the service
    curl http://192.168.49.2:30132

    # Problem with NodePort - random high port.
    # Resolve by putting a LoadBalancer on top of the cluster so that public clients have public dns.

# Lesson3 Reverse proxy like behavior via ingress

    # In Kubernetes, the "Ingress Controller" is synonymous to a reverse proxy which routes traffic via path and does additional services like authentication
    # all requests going to the pod goes via the Ingress Controller
    # Ingress Controller is the realm of the cluster administrator and will not be exposed to users

    cd lesson3
    kubectl apply -f .

    # TODO: minikube setup to allow dns to resolve to the minikube cluster ip
    # Alternatively fetch the minikube ip and add the dns entry to /etc/hosts

    curl http://nginx.example.com
    curl http://blue-green.example.com/blue
    curl http://blue-green.example.com/green

    
