Cazton : Microservices Training

# Kubernetes Fundamentals

Workshop material for Kubernetes Fundamentals that supplements the slides.

### Intro

Walkthrough setting up docker kubernetes and kubectl.


### Module 1 - Creating a Pod

We will be deploying a image from dockerhub into the cluster as a stand-alone pod with a deployment.

```bash
kubectl run hello-world --image=csdhall/k8s-hello-world:1.0 --port=8080
kubectl get deployments
kubectl get pods --all-namespaces
kubectl proxy
http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME/proxy/
```

### Module 2 - Describing our Pod and Deployment

We will be describing the deployed pod and deployment.

```
kubectl describe deployment hello-world
kubectl describe pod $POD_NAME
```

### Module 3 - Exposing our Pod with a Service

We will be creating a service to expose our pod outside the cluster as a NodePort type. This will
allow us to view the web application on our local machine.

```
kubectl expose deployment/hello-world --type="NodePort" --port 80
kubectl get services
kubectl describe services/hello-world
curl http://localhost:$NODEPORT
```

### Module 4 - Scaling 

We will scale up/down the number of pods behind the service.

```
kubectl scale deployments/hello-world --replicas=4
kubectl get deployments
kubectl get pods -o wide
kubectl describe deployments/hello-world
kubectl scale deployments/hello-world --replicas=4
kubectl get pods -o wide
```

### Module 5 - Rolling Updates

We will update our deployment image from v1 to v2 to show rolling updates

```
kubectl set image deployments/hello-world hello-world=csdhall/k8s-hello-world:2.0
kubectl rollout status deployments/hello-world
kubectl describe deployments/hello-world
```

### Module 6 - Rollback Updates

We will update our pod to a non-existent image:tag, showing that the broken pods are not routed (previous version still runs). We will then rollback the update and show that there was no downtime.
 
```
kubectl set image deployments/hello-world hello-world=csdhall/k8s-hello-world:3.0
kubectl describe deployments/hello-world
kubectl rollout undo deployments/hello-world
```

### Module 7 - Cleaning Up

We will cleanup the work we've done to make space for the later examples using yaml manifests.

```
kubectl delete services/hello-world
kubectl delete deployments/hello-world
```

### Module 8 - Installing Dashboard

We will install the kubernetes dashboard UI to allow students to see changes without
knowing all of the CLI commands.

```
helm init
helm install stable/kubernetes-dashboard --name kubernetes-dashboard
localhost:8001/api/v1/namespaces/default/services/https:kubernetes-dashboard:/proxy/
```

### Module 9 - Deploying our Pod with the API 

We will deploy our pod using a manifest and show progress in the dashboard UI.

```
kubectl create -f ./manifests/hello-world/pod.yaml
```

### Module 10 - Creating our Service with the API

We will deploy our service using a manifest and show progress in the dashboard UI.

```
kubectl create -f ./manifests/hello-world/service.yaml
```

### Module 12 - Creating a ReplicateSet Workload

We will create a replicaset for our service for scaling

```
kubectl create -f ./manifests/hello-world/service.yaml
```

### Module 13 - Creating a StatefulSet Workload

We will create a statefulset for our service

```
kubectl create -f ./manifests/hello-world/statefulset.yaml
kubectl get pods -w -l app=hello-world
```

### Module 14 - Creating a ConfigMap

We will create a configmap to support our service

```
kubectl create -f ./manifests/hello-world/configmap.yaml
```

### Module 15 - Creating a Secret

We will create a secret to support our service

```
kubectl create -f ./manifests/hello-world/secret.yaml
```

