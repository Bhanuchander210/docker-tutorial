## Containers

It can be simply called as *running instance of a Docker Image*.

- Containers a lot lighter than a virtual machine and provides similar isolation.
- It adds a writable layer on top of the image layers and works on it.
- Can talk to the containers like *process* in linux.
- It uses **copy on write policy**.

![containers](https://iotbytes.files.wordpress.com/2017/06/iot_containers.png?w=541&h=356)

*Image Source : [ref](https://iotbytes.files.wordpress.com/2017/06/iot_containers.png)* 

A Container run provides us,

- Computation (#CPU)
- Memory  (#RAM)
- Storage

## Quick Details

### Creating containers

- Method 1. Using create command

It will create docker container with the status **created**. That means it will not start the instance just
create a ready-up container.

```commandline
docker container create -it --name containerName imageName:tagName
```

- Method 2. Using run command

This method itself do two tasks of **creating** and **running** the container respectively.
After executing this command, the status of the container will be **Up for (n)times**.

```commandline
docker container run --itd --name containerName imageName:tagName 
```

### Working with containers

- Start
- Stop
- Rename
- Restart
- Pause
- UnPause
- Wait
- Kill
- Remove

```text
docker container start containerName/containerId
docker container stop containerName/containerId
docker container rename containerName newName
docker container restart --time 5 containerName
docker container pause containerName/containerId
docker container unpause containerName/containerId
docker container wait containerName/containerId
docker container kill containerName/containerId
docker container rm containerName/containerId
```

### Containers Attach and Exec

It is for attaching the Standard I/O and Standard Error of a container to the terminal.
It will connect to a running container. If we exit it, the container also will be exited.
So that using the command *attach* is not highly recommended. Or You can use `ctrl + p` or `ctrl + q` to detach
the container.

- Attach

```commandline
docker container attach containerName
```

- Exec

In Execute, we can run any command we want with the container. 

```commandline
docker exec -it containerName command
```

Note: options \[i,t\] are used to get interactive and teletype terminal. 


### Container Exposure

As we know that, The port exposure can be done in two ways

Here we need to tell the exact port for expose.
- `docker container run -itd --name contName -p 8080:80 imageName`

This command allow the docker itself to map a port.
- `docker container run -itd -name contName -P imageName`

- Port mapping on live container

```commandline
docker stop containerName
docker commit tempImageName
docker run -p 8080:8080 --name -td tempImageName
```

Notes :

- Docker container lives only for the time of your command process which was declared inside the docker file.

[next](/06-Dockernetworks)
[home](/)