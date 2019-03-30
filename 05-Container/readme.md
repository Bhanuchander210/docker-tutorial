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

### Container resource allocation

Some examples :

- Memory allocation

|Options|Details|
|-------|-------|
|-m or --memory=|The maximum amount of memory the container can use. If you set this option, the minimum allowed value is 4m (4 megabyte).
|--memory-swap*|The amount of memory this container is allowed to swap to disk. See --memory-swap details.
|--memory-swappiness|By default, the host kernel can swap out a percentage of anonymous pages used by a container. You can set --memory-swappiness to a value between 0 and 100, to tune this percentage. See --memory-swappiness details.
|--memory-reservation|Allows you to specify a soft limit smaller than --memory which is activated when Docker detects contention or low memory on the host machine. If you use --memory-reservation, it must be set lower than --memory for it to take precedence. Because it is a soft limit, it does not guarantee that the container doesn’t exceed the limit.
|--kernel-memory|The maximum amount of kernel memory the container can use. The minimum allowed value is 4m. Because kernel memory cannot be swapped out, a container which is starved of kernel memory may block host machine resources, which can have side effects on the host machine and on other containers. See --kernel-memory details.
|--oom-kill-disable|By default, if an out-of-memory (OOM) error occurs, the kernel kills processes in a container. To change this behavior, use the --oom-kill-disable option. Only disable the OOM killer on containers where you have also set the -m/--memory option. If the -m flag is not set, the host can run out of memory and the kernel may need to kill the host system’s processes to free memory.

Reference: From official [documentation](https://docs.docker.com/v17.12/config/containers/resource_constraints/#configure-the-default-cfs-scheduler).
 
- CPU Allocation

|Options|Details|
|-------|-------|
| --cpus=<value>|Specify how much of the available CPU resources a container can use. For instance, if the host machine has two CPUs and you set --cpus="1.5", the container is guaranteed at most one and a half of the CPUs. This is the equivalent of setting --cpu-period="100000" and --cpu-quota="150000". Available in Docker 1.13 and higher.|
| --cpu-period=<value>|	Specify the CPU CFS scheduler period, which is used alongside --cpu-quota. Defaults to 100 micro-seconds. Most users do not change this from the default. If you use Docker 1.13 or higher, use --cpus instead.|
| --cpu-quota=<value>|	Impose a CPU CFS quota on the container. The number of microseconds per --cpu-period that the container is guaranteed CPU access. In other words, cpu-quota / cpu-period. If you use Docker 1.13 or higher, use --cpus instead.|
| --cpuset-cpus| Limit the specific CPUs or cores a container can use. A comma-separated list or hyphen-separated range of CPUs a container can use, if you have more than one CPU. The first CPU is numbered 0. A valid value might be 0-3 (to use the first, second, third, and fourth CPU) or 1,3 (to use the second and fourth CPU).|
| --cpu-shares| Set this flag to a value greater or less than the default of 1024 to increase or reduce the container’s weight, and give it access to a greater or lesser proportion of the host machine’s CPU cycles. This is only enforced when CPU cycles are constrained. When plenty of CPU cycles are available, all containers use as much CPU as they need. In that way, this is a soft limit. --cpu-shares does not prevent containers from being scheduled in swarm mode. It prioritizes container CPU resources for the available CPU cycles. It does not guarantee or reserve any specific CPU access.|


Notes :

- Docker container lives only for the time of your command process which was declared inside the docker file.

[next](/06-Dockernetworks)
[home](/)