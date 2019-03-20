## Dockerfile

### Introduction

- contains sequential sets of instructions.
- Prime way to interact with docker daemon.
- Each instruction creates a layers.
- A stack of such sequenced layers managed by a file system called **docker image**.
- So it has enabled caching and troubleshooting facility.
- Docker daemon can reuse the pre-created layer for a docker file layer.


### Structure

**Rule of Thumb : The name should be `Dockerfile`**

It has sequential instructions, that can be categorized as

- Fundamental instructions
- Configuration instructions
- Execution instructions

**Fundamental Instructions**

|Command| Details|
|-------|--------|
|FROM| Sets the Base Image for subsequent instructions.|
|ARG| defines a build-time variable.|
|MAINTAINER| (deprecated - use LABEL instead) Set the Author field of the generated images.|
|ENV| sets environment variable.|
|LABEL| apply key/value metadata to your images, containers, or daemons.|

**Configuration Instructions**

|Command| Details|
|-------|--------|
|RUN| execute any commands in a new layer on top of the current image and commit the results.|
|ADD| copies new files, directories or remote file to container. Invalidates caches. Avoid ADD and use COPY instead.|
|COPY| copies new files or directories to container. Note that this only copies as root, so you have to `chown` manually regardless of your USER / WORKDIR setting, as same as ADD.|
|VOLUME| creates a mount point for externally mounted volumes or other containers.|
|USER| sets the user name for following RUN / CMD / ENTRYPOINT commands.|
|WORKDIR| sets the working directory.|


**Execution Instructions**

|Command| Details|
|-------|--------|
|CMD| provide defaults for an executing container.|
|EXPOSE| informs Docker that the container listens on the specified network ports at runtime. NOTE: does not actually make ports accessible.|
|ONBUILD| adds a trigger instruction when the image is used as the base for another build.|
|STOPSIGNAL| sets the system call signal that will be sent to the container to exit.|
|ENTRYPOINT| configures a container that will run as an executable.|


## Quick Details

### Build Docker Filer

**Step 1.** Create a dockerfile

```
ARG CODE_VERSION=16.04
FROM ubuntu:${CODE_VERSION}
RUN apt-get update -y
CMD ["bash"]
```

**Step 2.** Build the dockerfile

```commandline
docker build -t imageName .
```

This command takes some time to download the base image and executes the commands step by step and creates **Image file**.

**Step 3.** Check the image availability

```commandline
docker images
```

The output would be like this,
```
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
imageName             latest              92e0fbf67907        10 seconds ago      142MB
```

Here we can also use configuration instructions,

### Example Docker file

```text
FROM ubuntu:16.04
LABEL Creator: "Bhanuchander"
RUN apt-get update && apt-get install -y curl \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN mkdir /home/Examples
ENV USER bhanuchander 
ENV SHELL /bin/bash
ENV LOGNAME bhanuchander
CMD ["bash"]
```

### Quick Commands

It uses the command `RUN` with some configuration commands such as creating directories, update and installations.

- How to run image

```commandline
docker run -itd --name containerName imageName
```

- Check Container information

```commandline
docker ps -a
```

- Docker execution

```commandline
docker exec -it configcontainer bash
```

The <mark>bash</mark> command is running in the background due to the detached flag used `-d`. So the current
console will be like this,

```
root@4714611e4d92:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@4714611e4d92:/# echo $USER
bhanuchander
root@4714611e4d92:/# echo $SHELL
/bin/bash
root@4714611e4d92:/# echo $LOGNAME
bhanuchander
root@4714611e4d92:/# ls /home/         
Examples
```


### Exposing technique

- Dockerfile Instruction
- Docker run command

we can use `expose` instruction in Docker file like shown below file **expose/Dockerfile**.

```text
FROM ubuntu:16.04
RUN apt-get update && apt-get install nginx -y \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

Build this **dockerfile** as image and run as container by exposing the port as shown below,

```commandline
docker run -itd --name expcontainer -p 8080:80 exposeimage
```

we can view the exposed port from the command

```text
 docker container list -a
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS                        PORTS                  NAMES
76e4ef5ebb7b        exposeimage         "nginx -g 'daemon of…"   About a minute ago   Up About a minute             0.0.0.0:8080->80/tcp   expcontainer
```


### ADD vs COPY

Docker discourages of using `ADD` command because of the image size that matters. It always copy or download remote files
and extract compressed files within the docker image. So that use other strategies to add other files.

We can avoid using the `ADD` command shown below,

```commandline
ADD http://example.com/big.tar.xz /usr/src/things/
RUN tar -xJf /usr/src/things/big.tar.xz -C /usr/src/things
RUN make -C /usr/src/things all
```

Instead of this we can use it like this,

```commandline
RUN mkdir -p /usr/src/things \
    && curl -SL http://example.com/big.tar.xz \
    | tar -xJC /usr/src/things \
    && make -C /usr/src/things all
```

See more [Docker best practices](https://docs.docker.com/v17.09/engine/userguide/eng-image/dockerfile_best-practices/)

### Containerizing apache web-server

Apache web-serve has been containerized from the file `apache/Dockerfile`

```Dockerfile
FROM ubuntu:12.04
LABEL Creator: "Bhanuchander"
RUN apt-get update && apt-get install -y apache2 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE USER bhanuchander
EXPOSE 80
CMD ["/usr/sbin/apache2", "-D", "FOREGROUND"]
```

Run the command to crate and start the container

```commandline
docker run -itd --name apachecontainer -p 8080:80 apacheimage
```

Here, we can see the apache server in `localhost:8080`,

---
![apache](/assets/img/apache.png)
---

Note: Check the port availability in your local, before running the docker with port mapping.  

### Containerization of JAVA App

A Java file **Example.java** compiled as [Example.class](/03-Dockerfile/javatest/Example.class) and stored here.

```java
import java.util.Date;
import java.lang.Thread;
import java.lang.String;
import java.io.*;
class Example{
        public static void main(String[] args) throws Exception{
                String fileName = "Log-file.txt";
                PrintWriter writer = new PrintWriter(new File(fileName));
                for (int i=1 ; i < 5; i++){
                        Thread.sleep(1000);
                        writer.println("[INFO] {}".format(new Date().toString()));
                }
                writer.close();
        }
}
```

The [Dockerfile](/03-Dockerfile/javatest/Dockerfile) for this java application created as shown below,

```text
FROM store/oracle/serverjre:8
LABEL Creator: "Bhanuchander"
WORKDIR /usr
COPY Example.class .
RUN java Example
CMD ["bash"]
```

It will create a **Log-file.txt** file while the container starts.

### Reference:

- [Docker Doc - Dockerfile](https://docs.docker.com/engine/reference/builder/)
- [Docker Cheat Sheet by wsargent](https://github.com/wsargent/docker-cheat-sheet)
- [Docker copy and add key difference by ryanwhocodes](https://medium.freecodecamp.org/dockerfile-copy-vs-add-key-differences-and-best-practices-9570c4592e9e)


[next](/04-Dockerimage)
[home](/)