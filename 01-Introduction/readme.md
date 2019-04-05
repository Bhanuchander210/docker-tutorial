# Docker - Introduction
---

Lets start with containers,

## Containers
---

> Containers are an abstraction at application layer that packages the codes and dependencies together, 
ships the application and run-time environment.

Difference between VM and containers are,

![diff](https://techglimpse.com/wp-content/uploads/2016/03/Container-vs-VMs.jpg)

|Virtual Machine|Containers|
|---------------|----------|
|Having Hypervisor.| Docker replaces hypervisor.|
|It consumes more. For example ~1.2Gb/instance|Consumes very less ~2.5Mb|
|So heavier|Lightweight|
|Slower|Faster|
|Needs bigger footprint (RAM and storage)|Smaller footprint (No RAM and individual storage)|


## Docker
---

**A New fish in IT Ocean** - **A Containerization Platform**

> Docker is an open platform for developers and system admins to build, ship and run
containerized applications.

### Benefits of Docker

- Reproducibility
    Like **JVM**, docker also has same facility to run processes in any device. 
    All images built from the same Dockerfile will function identically.
- Isolation
    A container does not going to affect any other installations or configs on the system or on other dockers.
    By using separate containers for each component of an application (for example a web server, front end, and database for hosting a web site), you can avoid conflicting dependencies.
- Security
    Normal ideology of having multiple containers means having security. 
- Docker Hub
    View about [docker hub](https://hub.docker.com)
- Environment Management
- Continuous Integration
    Works well with tools like Travis, Jenkins, and Wercker.
    
    
### Note

Docker is still under development so that getting new features and backward compatibility with previous versions is not guaranteed.
Docker swarm may not be sufficient as standalone cluster manager as announced recently that it has support for kubernetes. 


### Installation on CentOS

Find the proper and detailed steps in official page - [Install docker on CentOS](https://docs.docker.com/install/linux/docker-ce/centos/)

These steps provided here for quick installation.

**Step 1.** Remove Old Version

```commandline
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

**Step 2.** Install Dependencies

```commandline
$ sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```

**Step 3.** Setup the repository

```commandline
$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

**Step 4.** Installation

```commandline
$ sudo yum install docker-ce docker-ce-cli containerd.io
```

**Step 5.** Start the Docker

```commandline
$ sudo systemctl start docker
```

**Step 6.** Check the docker

```commandline
docker -v
```
Output like : `Docker version 18.09.2, build 6247962` 

and

```commandline
docker run hello-world
```

Output like : `Hello from Docker! ....` 

**Step 7.** Pull Image from Docker Hub and run
```commandline
docker image pull nginx:latest
docker container run -itd --name myOwnNginx -p 8080:80 nginx:latest
```

After the pull, we can directly run the container from above shown command with specific `container name` and exposed `port`.

So after that, we can get the nginx in our local `localhost:8080`.

---

![img](/assets/img/nginx.png)

---

Or we can check by command where container is running or not.

```commandline
docker container ls -a
``` 

### Docker commands

- [Official doc](https://docs.docker.com/engine/reference/run/)

### References
---

- [Docker Essentials by Udemy](https://www.udemy.com/docker-essentials)
- [Docker tutorial by techglimpse](https://techglimpse.com/docker-installation-tutorial-centos/)
- [When and Why Dockers by linode](https://www.linode.com/docs/applications/containers/when-and-why-to-use-docker/)

[next](/02-Dockerarchitecture)
[home](/) 