### Docker swarm mode - Routing Mesh

#### Publishing in One port:

- Creating

```
docker service create --name snakegame --publish published=9000,target=80 snakegameimg:latest
```

- Updating

```
docker service update --publish published=9000,target=80 snakegame
```

Output will be like :

```
snakegame
overall progress: 1 out of 1 tasks 
1/1: running   [==================================================>] 
verify: Service converged 
```

#### Configuring External Load balance
---

Two methods,

- Using Routing Mesh
- Without Using Routing Mesh


###### Using Routing Mess

Configure [HAProxy](http://www.haproxy.org/) *The Reliable, High Performance TCP/HTTP Load Balancer*.

###### Without Using Routing Mess

Need to set `--endpoint-mode` to `dnsrr`. 

```
docker service create --name snakegame --endpoint-mode dnsrr --publish published=9000,target=80 snakegameimg:latest
```

##### References:

- [Doc - Key concepts - Load Balancing](https://docs.docker.com/engine/swarm/key-concepts/#load-balancing)
- [External load balance](https://docs.docker.com/engine/swarm/ingress/#configure-an-external-load-balancer)
- [Load Balancing by UpCloud](https://upcloud.com/community/tutorials/load-balancing-docker-swarm-mode/)
- [Load Balancing by Docker labs](https://github.com/docker/labs/blob/master/networking/concepts/10-load-balancing.md)