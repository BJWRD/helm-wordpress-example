# helm-wordpress-example
The following project takes you through the steps requires to deploy [Wordpress](https://wordpress.org/) via [Helm](https://helm.sh/).

## How to Apply/Destroy
This section details the deployment and teardown of the kubernetes-simple-webapp architecture. 

## Prerequisites
* Minikube installation - [steps](https://minikube.sigs.k8s.io/docs/start/)
* Virtualbox installation - [steps](https://www.virtualbox.org/wiki/Downloads) 
* Hyperkit installation - [steps](https://minikube.sigs.k8s.io/docs/drivers/hyperkit/)

## Helm Installation

### 1. Helm Install
Enter the following commands to install Helm -

    curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
    chmod 700 get_helm.sh
    ./get_helm.sh
    
Source: [Helm Installation Documentation](https://helm.sh/docs/intro/install/)

### 2. Helm Version Verification
To confirm the version of Helm which you have installed, enter the following -

    helm version

Enter Image

## Deployment steps - Manual Installation

### 1. Start the minikube instance within Virtualbox/Hyperkit
     minikube start --driver=virtualbox/hyperkit

Enter Image

### 2. Setup your Directory structure
     mkdir -p wordpress/templates
     
### 3. Create the Chart.yaml file
     cd wordpress
     vi Chart.yaml

***NOTE:*** Ensure that the ***C*** within the Chart.yaml file is capitalised. This file consists of the metadata about the Chart. i.e., The name of the chart, version, and app version, etc.

     apiVersion: v1
     name: Wordpress
     decription: Helm Chart setup for Wordpress instance
     version: 0.1
     appVersion: "0.16.0"
     keywords:
        - wordpress
        - database

### 4. Create the yaml files
     cd wordpress/templates
     kubectl run wordpress --image=wordpress:latest --dry-run=client -o yaml > wordpress.yml
     kubectl run database --image=mysql:latest --dry-run=client -o yaml > database.yml
     kubectl create -f wordpress.yml
     kubectl  expose  pod  wordpress  --type=NodePort  --port=80  --dry-run=client -o yaml  >  service.yml
     kubectl delete pod wordpress
     ls -l
     
Enter Image of .yml files

### 5. Helm Chart Installation
     helm install wordpress wordpress/
     
***NOTE:*** 
* My Application name is wordpress 
* wordpress/ is the directory folder where the templates folder and the Chart.yaml file exists.
* This helm command will install the wordpress app (i.e. it will launch the wordpress/database pods and expose wordpress pod on port 80)
     
### 6. Helm verification
     helm list

Enter image

The status `deployed` indicates that the pods have been successfully deployed.

### 7. Kubectl pod verification
     kubectl get all
     
Enter Image

## Deployment steps - Automated Installation

###	1. Clone the kubenetes-ingress-controller repo
     git clone https://github.com/BJWRD/helm-wordpress-example

### 2. Start the minikube instance within Virtualbox
     minikube start --driver=virtualbox



### 8. Test Wordpress site accessibility

   Enter Image

### Teardown steps

### 1. Delete the deployed K8's infrastructure
     kubectl delete -f 
     kubectl delete -f 
    
### 2.  Delete the running minikube instance
     minikube delete

## List of tools/services used
* [Minikube](https://minikube.sigs.k8s.io/docs/)
* [Virtualbox](https://www.virtualbox.org/wiki/Downloads)
* [Hyperkit](https://minikube.sigs.k8s.io/docs/drivers/hyperkit/)
* [Helm](https://helm.sh/)
* [K8s Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
* [Services- NodePort](https://kubernetes.io/docs/concepts/services-networking/service/)
* [Secrets](https://kubernetes.io/docs/concepts/configuration/secret/)
* [PVC] Update me 
