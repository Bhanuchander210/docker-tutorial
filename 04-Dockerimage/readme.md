## DockerImage
---

> Docker Image is a stack of multiple layers created by a dockerfile

**Notes :**

- Built docker images may contain dependent images.
- The top layer of Docker Image is **R/W** type.
- Apart from that layer, other are **R/O** type.
- Docker Image has its unique *Name* and *Id*.
- It can be pushed to and pulled from **Docker Hub**.

###### Layers

```text
Docker Image ----
                |---- CMD Layer
                |---- EXPOSE Layer
                |---- WORKDIR Layer
                |---- RUN Layer
                |---- Base Image Layer
                |---- bootfs
``` 

###### Docker commands

- [`docker search imageName:imageTag`](#docker-search)
- [`docker images`](#docker-images)
- [`docker inspect`](#docker-inspect)
- [`docker history`](#docker-history)
- [`docker pruning`](#)


###### Docker search
**Examples :**

- Total search

```commandline
docker search ubuntu
```

- Applying filter

```commandline
docker search --filter "is-official=true" ubuntu
```

- Applying format

```commandline
docker search --format "table {{.Name}} {{.IsOfficial}}" ubuntu
```
 
###### Docker images

**Example :**

- To list all images
```commandline
docker images repositoryName
```

- Pulling the image from docker hub
```commandline
docker images pull repo:tag
```

- With Specific Tag Pull
```commandline
docker image pull nginx:latest
```

- With all tag pull

```commandline
docker image pull --all-tags nginx
```
It will pull all tags of **nginx**, Finally we will have these images once pulled.

```text
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
nginx               1-perl              ee46d026c719        31 hours ago        148MB
nginx               1-alpine-perl       8022c404c07a        6 days ago          50.5MB
nginx               1-alpine            9a2868cac230        6 days ago          16.1MB
nginx               latest              8c9ca4d17702        6 days ago          109MB
nginx               1.10-alpine         f94d6dd9b576        2 years ago         54MB
nginx               1.10.0              16666ff3a57f        2 years ago         183MB
nginx               1.10.0-alpine       8328c2365672        2 years ago         60.6MB
```

###### Docker Inspect

**Examples :**

- To inspect the repo
```commandline
docker image inspect ubuntu:latest
```

It will show you a description *json* file like this,

```text
    {
        "Id": "sha256:47b19964fb500f3158ae57f20d16d8784cc4af37c52c49d3b4f5bc5eede49541",
        "RepoTags": [
            "ubuntu:latest"
        ],
        "RepoDigests": [
            "ubuntu@sha256:7a47ccc3bbe8a451b500d2b53104868b46d60ee8f5b35a24b41a86077c650210"
        ],
        "Parent": "",
        "Comment": "",
        ....
```

The total output file also attached here as [ubuntu_latest.json](/assets/files/ubuntu_latest.json)

###### Docker history

- To view the history of a docker image

```commandline
docker image history ubuntu:latest 
```

###### Docker pruning

- To remove the not-running things

```commandline
docker container prune
docker image prune
docker volume prune
docker network prune
docker system prune
```

###### Docker Image Size

- To view the Image size

```commandline
docker image inspect imageName:latest --format='{{.Size}}'
```

[next](/05-Container/readme.md)
[home](/readme.md)