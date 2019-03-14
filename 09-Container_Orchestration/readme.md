## Docker Orchestration

All previous chapters almost deals only with single host or like single VM regarding things.

![swarm](https://raw.githubusercontent.com/docker/swarm/master/logo.png)


Docker Swarm Mode means deploying a single app in inner-collaborated and manageable multiple hosts or 
a hosts cluster. This swarm mode can be managed by the **Orchestrator**.

> Orchestrator is a tool used to provision, schedule and manage containers at large scale over one
or more clusters of multiple hosts.

It also works on the hierarchy like **Master-Slave**, can be called as **Manager-worker**.
The manager creates the key for this swarm-mode to share for the workers. In high-level,
Multiple hosts (workers) are going to work for a single host (Manager) which initiated the swarm mode.

![hierarchy](/assets/img/docker-swarm.png)

So these swarm manager and worker has various tools as shown below to run the swarm mode.

![Manager](/assets/img/docker_manager.png)
![Worker](/assets/img/docker_worker.png)

Swarm mode can be achieved by assigning container services as tasks to the workers. The manager is responsible to assign
this tasks as **replicas** to the workers.

If some host failed or disconnected, The manager schedule the task to another worker which is alive. According to the docker,
at-least the half and above of workers needs to be alive for running the swarm mode. So it can be measured as,

> Required No. of working Nodes = (Total Nodes / 2) + 1


#### Swarm Setup

- [Kubernetes](https://kubernetes.io/)
- [Rancher](https://rancher.com/)
- [Docker Machine](https://docs.docker.com/machine/)

#### Kubernetes

<img src="https://github.com/kubernetes/kubernetes/raw/master/logo/logo.png" width="100">

Container Orchestrator which can replace docker-compose as rich in deployment/release wise features. 

[installation](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl)

- Pod
- Replica-set
- Service
- Deployment


##### Pod, Replica-Set and Deployment

<img src="https://github.com/Bhanuchander210/docker-tutorial/raw/master/assets/img/eplicaset.png" width="100">

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

##### Before you start

Initially The communication between a **kubernetes cluster** and our `kubectl` command line tool should be established.
See the official document [here](https://kubernetes.io/docs/tasks/run-application/run-single-instance-stateful-application/#before-you-begin)

Try the kubernetes clusters

- Minikube (Local Setup)
    Use this [Minikube-compose](/09-Container_Orchestration/kubernetes/minikube_comp) for setting up the **Minikube**.
    See the official [Installation](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- [Katacoda](https://www.katacoda.com/courses/kubernetes/playground) (Online)
- [Play with Kubernetes](http://labs.play-with-k8s.com/) (Online)

###### Minikube

- [How to setup and start minikube ?](https://kubernetes.io/docs/tasks/tools/install-minikube/)
- Start minikube once installed

```commandline
minikube start
```

###### kubernetes commands

- [Command page](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)

###### Some quick commands

- Kubernet describe about pod logs

```commandline
kubectl describe pod pod-name
```

#### References

- [Kubernetes vs Rancher vs Docker Machine](https://stackshare.io/stackups/docker-machine-vs-kubernetes-vs-rancher)
- [Kubernetes official doc](https://kubernetes.io/docs/home/)
- [Kubernetes by udemy](https://www.udemy.com/kubernetes-docker)