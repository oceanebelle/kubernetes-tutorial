

Pre-requisite option using CRC
 - install a local CRC (CodeReady Container)
 - Kubernetes example https://kubernetes.io/docs/concepts/workloads/controllers/deployment/

 Pre-requiriste option using minikube to follow Kubernetes tutorials

# Working with CodeReady Containers 

OC cheatsheet: https://docs.openshift.com/container-platform/4.7/cli_reference/openshift_cli/developer-cli-commands.html

```
crc start

# login to crc
oc login -u developer -p developer https://api.crc.testing:6443

# navigate to web console
crc console
```

### Create project using CLI

```
# creates the project
oc new-project hello-openshift \
  --description="This is a sample project" \
  --display-name="Hello Openshift"

# lists projects
oc get projects 

# deletes the project
# oc delete project hello-openshift
```

### Install an existing helm chart from a helm repo


Install an existing chart from repo
```
# Download latest version of helm executable
curl -L https://mirror.openshift.com/pub/openshift-v4/clients/helm/latest -o /usr/local/bin/helm

# Add repository of helm chat to local helm client
helm repo add stable https://charts.helm.sh/stable

# Add repository from redhat
helm repo add redhat-charts https://redhat-developer.github.io/redhat-helm-charts

# update repo
helm repo update

# install an example mysql  chart
helm install example-mysql stable/mysql

# lists all installed charts
helm list

# uninstalls the helm chart
helm uninstall example-mysql
```

### Creating a custom helm chart from existing sample charts

```
# Sample helm charts for Openshift
git clone https://github.com/redhat-developer/redhat-helm-charts

# modify the Chart.yaml in 
cd redhat-helm-charts/alpha/nodejs-ex-k
..
helm lint

cd ../

# install the modified chart
helm install nodejs-chart nodejs-ex-k

```

### Building helm chart from scratch

```

# see https://helm.sh/docs/chart_template_guide/getting_started/

cd helm
helm create testchart
cd ../
# creates a directory structure of the chart, follow tutorial

helm install testchart1 ./helm/testchart

# retrieve actual manifest generated
helm get manifest testchart1

# uninstall chart
helm uninstall testchart1

# test the template rendering but not install
helm install --debug --dry-run testchart1 ./helm/testchart
```

# Working with Minikube

kubectl cheatsheet  https://kubernetes.io/docs/reference/kubectl/cheatsheet/

See getting started guide in Minikube https://minikube.sigs.k8s.io/docs/start/
See getting started guide in Kubernetes https://kubernetes.io/docs/setup/

Tutorials https://kubernetes.io/docs/tutorials/

## Helpful commands to know 
```bash
# starts minikube
minikube start

# checks the status of the kubernetes cluster
kubectl cluster-info

# shows kubernetes cluster on a brower
minikube dashboard

# shows available addons that can be installed
minikube addons list

# show all the workers in the cluster
kubectl get nodes

# list the current services in the cluster
kubectl get services
```

## Deploy sample service

```bash
# deploy an image
kubectl create deployment echo-service --image=mendhak/http-https-echo:19

# expose the service to external traffic
kubectl expose deployment echo-service --type=NodePort --port=8080

# open in web browser
minikube service echo-service

# alternatively port forward the service to access in http://localhost:7080
kubectl port-forward service/echo-service 7080:8080

# list deployments
kubectl get deployments

# get all pods
kubectl get pods

# describe the pods
kubectl describe pods

# show the logs of the service
kubectl logs $POD_NAME

# execute commands (env) directly on the pod
kubectl exec $POD_NAME -- env
kubectl exec $POD_NAME -- bash
```

## Using labels
```bash
# use label to query list of pods based on labels
kubectl get pods -l app=echo-service

# add a label to a pod
kubectl label pod $POD_NAME mylabel=awesome

# delete a service using label
kubectl delete service -l app=echo-service
```

## Scaling
```bash
kubectl get deployments

# list the available replica sets
kubectl get rs

# scale up the deployment
kubectl scale deployments/echo-service --replicas=2

# scale down
kubectl scale deployments/echo-service --replicas=1

kubectl get pods -o wide
```

## Kubernetes manifests
```bash
# apply yaml manifest
kubectl apply -f kubernetes.yaml

# watch the pod deployments
kubectl get --watch pods
```