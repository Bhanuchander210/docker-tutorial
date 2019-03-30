## Docker-Machine

A tool for swarm mode in docker environment.


### Installation

Official : [Documentation](https://docs.docker.com/machine/install-machine/)


### Simple Demo

- Creating the Manager

```commandline
docker-machine create --drive virtualbox manager 
```
If you face error downloading **boot2docker.iso** use this command download manually, 
`curl -L https://github.com/boot2docker/boot2docker/releases/download/v18.09.4-rc1/boot2docker.iso > $DOCKER_REG_HOME/.docker/machine/cache/boot2docker.iso`.

- Creating workers using the same command,

```commandline
docker-machine create -d virtualbox worker-1
docker-machine create -d virtualbox worker-2
```
- Verify the created machines by, and also get docker machine ip

```commandline
docker-machine ls
docker-machine ip manager
docker-machine ip worker-1
docker-machine ip worker-2
```

- Do ssh to enter inside the created virtual box

```commandline
docker-machine ssh manager
```

It will enter inside the machine and the output will be like this,

```text
   ( '>')
  /) TC (\   Core is distributed with ABSOLUTELY NO WARRANTY.
 (/-_--_-\)           www.tinycorelinux.net
```
`in master`

- Init this machine for `swarm` mode. Before that you need to know and enter correct ip of the container

```commandline
docker swarm init --advertise-addr 192.168.99.104
```
`in master`

Now it will show you a text like this,

```text
Swarm initialized: current node (xjnmvtmmg20c1zdh06748lza8) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-0ketqcvyubeq09l03q3gh4bukf0bu49bzpcjwqfqle77pilfl9-ev4olhsu6hpo4wk5wbmdp4ixx 192.168.99.104:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```

- Set all the workers to this `manager` by entering inside using `docker-machine ssh worker-n`.

```text
docker swarm join --token SWMTKN-1-0ketqcvyubeq09l03q3gh4bukf0bu49bzpcjwqfqle77pilfl9-ev4olhsu6hpo4wk5wbmdp4ixx 192.168.99.104:2377
```
`in worker`

After executing this command you will get notified by this acknowledgement : **This node joined a swarm as a worker**.
If you missed this token command, you can any time get this by executing this command inside the manager,

```commandline
docker swarm join-token worker
```
`in master`

- Check the connected workers for the manager by this command,

```commandline
docker node ls
```
`in master`

Output:

```text
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
xjnmvtmmg20c1zdh06748lza8 *   manager             Ready               Active              Leader              18.09.3
j7b3f1ch9yok47yapv934xr59     worker-1            Ready               Active                                  18.09.3
ji8piabqz3n5cwe6tp5rksko7     worker-2            Ready               Active                                  18.09.3
```

- Run a test job as **3 replicas** from manager and observe the activity is like,

```commandline
docker service create --name web-server -p 8080:80 --replicas 3 nginx:latest
```
`in master`
After that you can check that the running status of the container by the command : `docker service ps web-server`.
It will generate 3 replicas of *web-server* such as web-server.1, web-server.2 and web-server.3 to the three nodes master, worker-1 and worker-2.

### Draining a worker

- we can drain a worker by a command from `master`.

````commandline
docker node update --availability drain worker-2
````
`in master`

After that you can check the status of the **replica** associated to the drained node,

```text
docker@manager:~$ docker service ps web-server
ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE                ERROR               PORTS
qmkbqvmql3pj        web-server.1        nginx:latest        manager             Running             Running about 1 minute ago                        
xpix3ql8buqn         \_ web-server.1    nginx:latest        worker-2            Shutdown            Shutdown about 1 minute ago                       
u4mp5sxb2arn        web-server.2        nginx:latest        manager             Running             Running 15 hours ago                             
4ics5rgabid3        web-server.3        nginx:latest        worker-1            Running             Running 14 hours ago 
```


### Scale and updating the docker-machine

we can scale the service running in cluster manager like shown below command,

```commandline
docker service scale web-server=5

```
Output :

```text
web-server scaled to 5
overall progress: 5 out of 5 tasks 
1/5: running   [==================================================>] 
2/5: running   [==================================================>] 
3/5: running   [==================================================>] 
4/5: running   [==================================================>] 
5/5: running   [==================================================>] 
```
`in master`

After that you can check how these 6 replicas working in cluster by,

```commandline
docker@manager:~$ docker service ps web-server 
ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE                ERROR               PORTS
qmkbqvmql3pj        web-server.1        nginx:latest        manager             Running             Running about an hour ago                        
xpix3ql8buqn         \_ web-server.1    nginx:latest        worker-2            Shutdown            Shutdown about an hour ago                       
u4mp5sxb2arn        web-server.2        nginx:latest        manager             Running             Running 15 hours ago                             
4ics5rgabid3        web-server.3        nginx:latest        worker-1            Running             Running 15 hours ago                             
sgrbgh17cf72        web-server.4        nginx:latest        worker-1            Running             Running 4 minutes ago                            
yfvt1jagfe3u        web-server.5        nginx:latest        worker-1            Running             Running 4 minutes ago 
```

There is no replica assigned to worker-2 even it is inside the cluster. because the manager till not updated.
To update the manager the following command is used.

```commandline
docker service update --force web-server
```
After this command all new joined workers also updated and scaled accordingly.



# Applying Docker stack


### Deploying stack

See the official [documentation](https://docs.docker.com/engine/reference/commandline/stack/) for more information.

- We can deploy a stack in manager node where swarm inited.
```commandline
docker stack deploy -c docker-compose.yaml mystack
```
`in manager`

### Useful -Posts

- https://stackoverflow.com/questions/34336218/is-there-a-way-to-force-docker-machine-to-create-vm-with-a-specific-ip