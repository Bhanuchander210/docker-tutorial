## Kubernetes

<img src="https://github.com/kubernetes/kubernetes/raw/master/logo/logo.png" width="100">

Container Orchestrator which can replace docker-compose as rich in deployment/release wise features. 

[installation](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl)

- Pod
- Replica-set
- Service
- Deployment


### Pod, Replica-Set and Deployment

<img src="https://github.com/Bhanuchander210/docker-tutorial/raw/master/assets/img/replicaset.png" width="500">

- Pod, basically contains one or more than one containers.
- Pod is the most basic entity in kubernetes.
- ReplicaSet is like a manger of Pod which ensures the Pods activity. It always sees Pod as Replica (also like a Job). So that
 it is called as *ReplicaSet* (Set of replicas or Set of Pods).
 
 Why running Pod alone is dangerous ?
 
 - When the machine crashes or some thing happens related to it, Pod will be deleted.
 - That's why ReplicaSets are used to provide guarantee for Pod's life.   
 
The Importance of Deployment,

- It can update the replicas with zero down time.
- Deployment controller contains deployment objects which can create new Replicas, remove old replicas excluding their resources and adopt it with the updated one.

### Before you start

Initially The communication between a **kubernetes cluster** and our `kubectl` command line tool should be established.
See the official documentation,
 
 - [Run single instance stateful application](https://kubernetes.io/docs/tasks/run-application/run-single-instance-stateful-application/#before-you-begin)

Try the kubernetes clusters

- Minikube (Local Setup)
    Use this [Minikube-compose](/09-Container_Orchestration/kubernetes/minikube_comp) for setting up the **Minikube**.
    See the official [Installation](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- [Katacoda](https://www.katacoda.com/courses/kubernetes/playground) (Online)
- [Play with Kubernetes](http://labs.play-with-k8s.com/) (Online)

### Minikube

It is a lightweight Kubernetes implementation that creates a VM on your local machine and deploys 
a simple cluster containing only one node. Minikube is available for Linux, macOS, and Windows systems. 
The Minikube CLI provides basic bootstrapping operations for working with your cluster, including start,
stop, status, and delete.

See the [documentation](https://kubernetes.io/docs/tutorials/kubernetes-basics/create-cluster/cluster-intro/)


- [How to setup and start minikube ?](https://kubernetes.io/docs/tasks/tools/install-minikube/)
- Start minikube once installed

```commandline
minikube start
```

### kubernetes commands

- [Command page](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)


## Quick Details

### Kubernetes list running pods

```commandline
kubectl get pod
```
Output will contains pod name and current status.

```text
NAME                          READY   STATUS    RESTARTS   AGE
devops-6b99f8d648-z6mzq       1/1     Running   0          2d17h
mysql-devop-96cd8596f-gmlg9   1/1     Running   3          2d19h
wordpress-5b8b869d84-vlq57    1/1     Running   3          2d19h
```

### Kubernetes describe about pod logs

```commandline
kubectl describe pod pod-name
```

### Update the deployment with zero-down time

```commandline
kubectl apply -f config-file.yaml
```

### Import local image into kubernetes

```commandline
# Start minikube
minikube start

# Set docker env
eval $(minikube docker-env)

# Build image
docker build -t imageName:version

# Run in minikube
kubectl run hello-foo --image=foo:0.0.1 --image-pull-policy=Never

# Or it can be set inside the yaml config file like shown below
# - image: imageName:latest
#   name: imageName
#   imagePullPolicy: Never
```

### kubernetes objects

```commandline
kubectl api-resources
```

In my system around **53** types of kubernetes objects are available.

```text
NAME                              SHORTNAMES   APIGROUP                       NAMESPACED   KIND
bindings                                                                      true         Binding
componentstatuses                 cs                                          false        ComponentStatus
configmaps                        cm                                          true         ConfigMap
endpoints                         ep                                          true         Endpoints
events                            ev                                          true         Event
``` 

### References

- [Kubernetes official doc](https://kubernetes.io/docs/home/)
- [Kubernetes by udemy](https://www.udemy.com/kubernetes-docker)
- [kubernetes cluster - 50 useful tools](https://caylent.com/50-useful-kubernetes-tools/)
- [Training by Google](https://cloud.google.com/training/#intros)