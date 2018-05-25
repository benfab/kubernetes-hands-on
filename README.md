# Kubernetes-hands-on

This repo aims to provide a quick hands-on for Kubernetes beginners.

## Pre-requisites

- Google Cloud account
- Be familiar with Docker 

## Create your K8s cluster

 - Login to the [Google Cloud console](https://console.cloud.google.com)
 - Create a new project  
 - Browse Kubernetes Engine
 - Create a cluster using the default values
 - Activate the Google Cloud Shell 
 - Optional: install extension SSH for Google Cloud Platform

## Explore your cluster with kubectl
From Google Cloud Shell, run the following command to get the kube config:

```
gcloud container clusters get-credentials [CLUSTER-NAME] --zone [ZONE-NAME] --project [PROJECT-NAME]
```
Notice, the command is automatically generated when pressing the `Connect` button from the cluster list.

Display your kube config 

`cat .kube/config`

Play with kubectl to explore some Kubernetes resources

list nodes  
`kubectl get nodes` 

list services   
`kubectl get services`

list pods   
`kubectl get pods` 

list deployment   
`kubectl get deployment`

list replicas set   
`kubectl get replicasets`

list replicas persistent volume    
`kubectl get persistentvolumes`

list namespaces   
`kubectl get namespace` 

list secrets   
`kubectl get secret`

Notice, that you can use short-names   
`kubectl get no`  
`kubectl get svc`  
`kubectl get ns`  

List everything on your cluster  
`kubectl get all`

Use the describe command to get more details about a specific resource  
`kubectl describe node <node-name>`

## Deploy an app

Deploy a simple app
```
kubectl run hello-server --image gcr.io/google-samples/hello-app:1.0 --port 8080
```
Add a load balancer in front
```
kubectl expose deployment hello-server --type "LoadBalancer"
```
Fetch the public IP assigned to the Loadbalancer, notice it might take some time:
```
kubectl expose deployment hello-server --type "LoadBalancer"
```
Browse the service.

`curl http://<ip>:8080/`

Get the logs  
`kubectl logs deploy/hello-server -f`

## Scale 

Scale up your deployment  
`kubectl scale deploy/hello-server --replicas 3`

Run the cURL command multiple time and attention that the hostname change, as the traffic get load balanced accross your nodes.

`curl http://<ip>:8080/`

Watch the pods by runnig this comamand in another Shell  
`kubectl get pods -w`

In another Google Shell windows, kill one pods  
`kubectl delete pod <pod-name>`

Notice that the pod get recreated!

## Destroy one VM

Browse the VM instance service
Delete on VM from your worker pool

Watch the pods and the pods beeing automatically recreated, run the commands in separated Shell
`kubectl get pods -w`  
`kubectl get nodes -w`  


## Delete cluster
`
gcloud container clusters delete [CLUSTER_NAME] --zone [ZONE-NAME] --project [PROJECT-NAME]
`

### References
* https://cloud.google.com/kubernetes-engine/docs/quickstart
* https://labs.play-with-k8s.com/


