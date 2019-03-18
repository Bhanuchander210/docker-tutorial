## Architecture

### Stages of Docker

|Stages|Format|
|-----|------|
|Build| Dockerfile|
|Ship | Docker Image|
|Run  | Containers|

### Eco-System

- Docker Client
- Docker Host
- Docker Registry

### Docker Client

It is the medium or machine through which we as user interact with **docker**.
There are two ways to interact,

- Docker CLI
- Docker API 

### Docker Host

It is the machine which really performs the containerization by running the **docker daemon**.
Docker daemon listens and performs the regarding actions asked by docker client.

**Important Notes:**

- Docker daemon builds *dockerfile* and *dockerimage*.
- So that both of them can directly communicate with docker daemon.
- These docker images can be pushed and pulled by **Docker Hub** like GitHub (But the concept is not about VCS).
- Containers communicate with docker daemon via **docker images**, that means changes in container reflects in image temporarily.

```
Container <-----> Docker Image <-----> Docker Daemon
```

### Docker Registry

Docker registry is a simple hub which stores the docker images and serves for others. Normal command `docker pull` pulls the
image from registry.


So by summarizing all of this would be like this,

| Docker Client| Docker Host | Docker Registry|
|--------------|-------------|----------------|
|Docker CLI|Daemon|Images|
|Docker API|Containers||
||Images||

[next](/03-Dockerfile)
[home](/) 