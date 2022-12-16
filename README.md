# helm-wordpress-example
The following project takes you through the steps requires to deploy [Wordpress](https://wordpress.org/) via [Helm](https://helm.sh/).

## How to Apply/Destroy
This section details the deployment and teardown of the kubernetes-simple-webapp architecture. 

## Prerequisites
* Minikube installation - [steps](https://minikube.sigs.k8s.io/docs/start/)
* Virtualbox installation - [steps](https://www.virtualbox.org/wiki/Downloads)

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

### Deployment steps

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
* [Helm](https://helm.sh/)
* [K8s Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
* [Services- LoadBalancer]Update me
* [Secrets] Update me
* [PVC] Update me 
