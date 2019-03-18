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


### Swarm Setup

- [Kubernetes](https://kubernetes.io/)
- [Rancher](https://rancher.com/)
- [Docker Machine](https://docs.docker.com/machine/)

### References

- [Kubernetes vs Rancher vs Docker Machine](https://stackshare.io/stackups/docker-machine-vs-kubernetes-vs-rancher)


[home](/readme.md)