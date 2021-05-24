# openshift-tutorial

Pre-requisite
 - install a local CRC (CodeReady Container)
 - Kubernetes example https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
## Working with CRC

```
# login to crc
oc login -u developer -p developer https://api.crc.testing:6443

# navigate to web console
crc console
```

## Create project using CLI

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

## Install an existing helm chart from a helm repo


Install an existing chart from repo
```
# Download latest version of helm executable
curl -L https://mirror.openshift.com/pub/openshift-v4/clients/helm/latest/helm-darwin-amd64 -o /usr/local/bin/helm

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

## Creating a custom helm chart from existing sample charts

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

## Building helm chart from scratch

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