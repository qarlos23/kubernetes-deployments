# Jenkins on minikube
This project contains an example on how to configure jenkins in a local minikube.

## Prerequisites
1. minikube should be installed [Installation](https://kubernetes.io/docs/setup/minikube/#installation)
2. Kubectl should be installed [Installation](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
3. Helm should be installed [Installation](https://docs.helm.sh/using_helm/)


### Usage
First it is required that minikube is running on the local machine, it can be started running the following command:
```
minikube start
```
Moreover it is a good idea to enable the usage of the Ingress in order to access the deployed services, minikube offers a ingress controller and for that it is only necessary to enable the following plugin:
```
minikube addons enable ingress
```
Once this is started, it order to be able to deploy using  [Helm](https://docs.helm.sh/), it is required to install the server part of it (tiller) in the cluster. For that run the following command:
```
Helm init
```
Now it is everything ready to start with the deployment.

1. **Create a namespace**
    ```
    kubectl create -f namespace.yaml
    ```
2. **Create a persistence volume**

    This will help to keep the configuration of the jenkins independently if the Pod is deleted or updated.
    ```
    kubectl create -f persistentVolume.yaml
    ```
3. **Install Jenkins**

   Execute following command for installing jenkins
    ```
    helm install -f jenkins/jenkins-values.yaml --namespace jenkins-project jenkins stable/jenkins
    ```
4. **Create ingress rules**
    ```
    kubectl apply -f ../../ingress.yaml 
    ```
At this timejenkins  should be now up and running in the minikube cluster and can be access by:
```
minikube service jenkins
```
or
using the cluster api, this is obtained with:
```
minikube ip
```
and requesting in the browse http://{minikube-ip}/jenkins