# helm-wordpress-example
The following project takes you through the steps required to deploy a [Wordpress](https://wordpress.org/) system via the [Helm](https://helm.sh/) [Kubernetes](https://kubernetes.io/) Deployment tool.

## Architecture
![image](https://user-images.githubusercontent.com/83971386/208942748-439a100e-d25a-4f1e-8657-006a595548c9.png)

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

## 1) Deployment steps - Manual Installation/Setup

### 1. Start the minikube instance within Virtualbox/Hyperkit
     minikube start --driver=virtualbox/hyperkit

<img width="475" alt="image" src="https://user-images.githubusercontent.com/83971386/209128627-b929484a-687f-4251-bd5b-198bd4424027.png">

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
 
<img width="335" alt="image" src="https://user-images.githubusercontent.com/83971386/209128737-eba9218d-6293-4a2e-9eeb-8bcfc34dc3f9.png">

### 5. Creating the Kubernetes Secret
MySQL DB password values -

     kubectl create secret generic db-secret --from-literal=MYSQL_ROOT_PASSWORD-sql01
     
To view the secret values, enter the following -

     kubectl get secret db-secret -o yaml

<img width="356" alt="image" src="https://user-images.githubusercontent.com/83971386/209127042-1a2f6f34-1a5c-4259-baff-d0d67c61bbb8.png">

Encode the plain-text passwords to an encoded format  -

     echo -n 'mysql01' | base64

### 6. Secret Pod Injection
Now that we have generated a secret for the MySQL DB credentials. It's now time to inject the Secret definitions within the wordpress.yml file, by appending the following text at the bottom of the yaml file -

    envFrom:
      - secretRef:
          name: db-secret

<img width="208" alt="image" src="https://user-images.githubusercontent.com/83971386/209127461-efdfb2d6-dc1e-4594-a786-1c448c7e859f.png">

### 7. Helm Chart Installation

     cd ../..
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

## 2) Deployment steps - Automated Installation/Setup

###	1. Clone the helm-wordpress-example repo
     git clone https://github.com/BJWRD/helm-wordpress-example

### 2. Start the minikube instance within Virtualbox
     minikube start --driver=virtualbox

<img width="475" alt="image" src="https://user-images.githubusercontent.com/83971386/209128607-3da863f9-b5f7-47ab-909c-fee6409a3b55.png">

### 3. Helm Chart Installation
     helm install wordpress wordpress/
     
***NOTE:*** 
* My Application name is wordpress 
* wordpress/ is the directory folder where the templates folder and the Chart.yaml file exists.
* This helm command will install the wordpress app (i.e. it will launch the wordpress/database pods and expose wordpress pod on port 80)
     
### 4. Helm verification
     helm list

## 3) Deployment steps - Utilising Bitnami Wordpress Repository

### 1. Add the Bitnami Repo to your local host
    
    helm repo add bitnami https://charts.bitnami.com/bitnami
    
### 2. Helm Chart Installation

    helm install my-release bitnami/wordpress
    
<img width="620" alt="image" src="https://user-images.githubusercontent.com/83971386/209133608-eeb25fa8-9238-401c-a210-b06050e009b6.png">

## Test Wordpress site accessibility
Now that the Kubernetes Wordpress Cluster has been deployed, it's time to test to see whether we can now access the Wordpress site.

Firstly verify the Minikube Network settings -

    minikube ip
    minikube service list
    
<img width="384" alt="image" src="https://user-images.githubusercontent.com/83971386/209134617-0ccd31a5-4cca-4dda-bf62-f48c7412d5e6.png">

Once confirmed, enter the address within a browser (http://<IP>:<Port>) and you will then be presented with the Wordpress welcome screen -

<img width="521" alt="image" src="https://user-images.githubusercontent.com/83971386/209134776-18a948ff-9a27-4a0d-95c6-0db0bcc4df23.png">

### Teardown steps

### 1. Delete the deployed K8's infrastructure
    helm uninstall wordpress
    
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
