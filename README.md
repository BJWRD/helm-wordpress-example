# helm-wordpress-example
The following project takes you through the steps required to deploy a [Wordpress](https://wordpress.org/) system via the [Helm](https://helm.sh/) [Kubernetes](https://kubernetes.io/) Deployment tool.

## Architecture
![image](https://user-images.githubusercontent.com/83971386/208942374-d4b84b9a-af40-4ca0-a197-33e2c2a54199.png)

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

## Deployment steps - Manual Installation/Setup

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

### 5. Creating the Kubernetes Secret
MySQL DB password values -

     kubectl create secret generic db-secret --from-literal=DB_Host=sql01 --from-literal=DB_User=root --from-literal=DB_Password=password123
     
To view the secret values, enter the following -

     kubectl get secret db-secret -o yaml

Enter Image 

Encode the plain-text passwords to an encoded format  -

     echo -n 'mysql' | base64
     echo -n 'root' | base64
     echo -n 'passwd' | base64

### 6. Secret Pod Injection
Now that we have generated a secret for the MySQL DB credentials. It's now time to inject the Secret definitions within the wordpress.yml file, by appending the following text at the bottom of the yaml file -

    envFrom:
        - secretRef:
            name: db-secret

### 7. Helm Chart Installation
     helm install wordpress wordpress/
     
***NOTE:*** 
* My Application name is wordpress 
* wordpress/ is the directory folder where the templates folder and the Chart.yaml file exists.
* This helm command will install the wordpress app (i.e. it will launch the wordpress/database pods and expose wordpress pod on port 80)
     
### 8. Helm verification
     helm list

Enter image

The status `deployed` indicates that the pods have been successfully deployed.

### 9. Kubectl pod verification
     kubectl get all
     
Enter Image

## Deployment steps - Automated Installation/Setup

###	1. Clone the helm-wordpress-example repo
     git clone https://github.com/BJWRD/helm-wordpress-example

### 2. Start the minikube instance within Virtualbox
     minikube start --driver=virtualbox

Enter Image

### 3. Helm Chart Installation
     helm install wordpress wordpress/
     
***NOTE:*** 
* My Application name is wordpress 
* wordpress/ is the directory folder where the templates folder and the Chart.yaml file exists.
* This helm command will install the wordpress app (i.e. it will launch the wordpress/database pods and expose wordpress pod on port 80)
     
### 4. Helm verification
     helm list

## Test Wordpress site accessibility
Now that the Kubernetes Wordpress Cluster has been deployed, it's time to test to see whether we can now access Wordpress -

Enter the Host IP address followed by port 80 within a browser i.e. http://<Host IP>:8080

   Enter Image

### Teardown steps

### 1. Delete the deployed K8's infrastructure
    Enter Helm command
    
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
