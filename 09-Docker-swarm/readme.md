## Docker Swarm Mode

![swarm](https://raw.githubusercontent.com/docker/swarm/master/logo.png)

> Multi-container, multi-machine applications are made possible by joining multiple machines into a “Dockerized” cluster called a swarm.

For it is ad-hoc like clusters which will contain managers and workers. The working principle of the cluster is **Raft consensus**.

See the awesome demo of **[Raft consensus - Demo](http://thesecretlivesofdata.com/raft/)**.

The vision of docker swarm mode and container orchestration both are common. But the way of doing differs.

### Important rules:

- Don't use even number of managers in cluster and especially 2 because see the [comment by xpepermint](https://github.com/moby/moby/issues/34384)
- docker swarm mode fails while the number of managers lost is greater than **Half of the total managers**.

### Utils

- Docker stack
- Docker services

## Docker services

> In a distributed application, different pieces of the app are called “services”

### Must reads

- [Raft Consensus in swarm mode](https://docs.docker.com/engine/swarm/raft/)
- [What are services in swarm](https://docs.docker.com/get-started/part3/)
- [Swarm Mode Guide Official](https://docs.docker.com/engine/swarm/admin_guide/)

## Docker stack

It is the top hierarchy of the distributed applications. It is a group of group of interrelated services 
that share dependencies, and can be orchestrated and scaled together.
Docker stack is also like docker compose but it differs,

### How ?

- Docker stack is ignoring “build” instructions
- It only works on swarm mode. that means it was created for that.
- So that scaling and updating the services is not a big deal.

Some of the utils shown in references are,

- Add a new service and redeploy
- Persist the data

### References

- https://docs.docker.com/get-started/part5/
- https://vsupalov.com/difference-docker-compose-and-docker-stack/