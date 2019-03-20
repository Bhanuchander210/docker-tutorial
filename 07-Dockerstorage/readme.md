## Containers Storage

### Intro

- Container data needs to be backed up some where as a permanent storage.
- As we studied it has two types of layers shown below,
    - Read and Write Layers (Volatile Data)
    - Read Only Layers (Permanent Data)
    
```text
******* Read and Write Layer *******
|--- Union Mount Point Layer
|--- Container Layer
|
|******* Read Only Layers  *******
|---- CMD Layer
|---- EXPOSE Layer
|---- WORKDIR Layer
|---- RUN Layer
|---- Base Image Layer
|---- bootfs
```

So here if a container dies, The volatile data also should be vanished.
We can store this important data at anywhere in such as 
- Host machine 
- Other server machine
- Cloud

These data are stored as **Docker Volumes** which is completed isolated from the host file system.
It is also controlled by Docker command line, secure to ship and more reliable.

### Docker Volumes

- Storage objects of docker
- Mounted to containers
 
![Docker volume](/assets/img/docker_volume.png)
 
As shown in the figure,
 
1. Docker container provides data docker engine
1. User also provides commands to the docker engine
1. According to the commands the data would be saved as volumes as Docker volumes at anywhere in the container network.
 
### TMPFS
 
- Temporary File System Mount, This is the third option if the container runs on **linux**.
- Here the container can store the files on outside of the container's writable layers.
- It only persist on the host memory, that allows to store the sensible data which you don't want to persist after container stops.
 
 For example : We can store the `ssh` keys inside this tmpfs for security purpose.
 
### Commands

- Docker volume create volName
- Docker volume ls volName
- Docker volume rm volName
- Docker volume inspect volName
- Docker volume create

## Quick Details

- Mounting the volume to the Container

```commandline
docker run -d --volume volName:/tmp containerName
```

- To List the volumes not Mounted

```commandline
docker volume ls --filter "dangling=true"
```

- Check using containers

```commandline
docker container inspect --format "{{json .Mounts}}" containerName | python -m json.tool
```

So these kind of mounted volumes can be found in `/var/lib/docker/volumes`

[next](/07-Dockerstorage)
[home](/)