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