### Docker Registry Notes :

- We can't change the default registry from **docker.io** to **private registry**.[Link](https://stackoverflow.com/questions/33054369/how-to-change-the-default-docker-registry-from-docker-io-to-my-private-registry)
- We need **TLS Certificate** for our own private registry. [Link](https://docs.docker.com/registry/insecure/)
- How to deploy a Local Docker Registry. [Link](https://docs.docker.com/registry/insecure/)


### Steps :

###### Step 1 : Create docker.json for insecure registries

*/etc/docker/daemon.json*

```
{
  "insecure-registries" : ["myregistrydomain.com:5000"]
}
```

###### Step 2 : Run command

```
docker run -d --restart=always --name registry -p 5000:5000 registry:2
```

### Using TLS

###### Step 1 : Creating self signed certificate.

```
openssl req -newkey rsa:4096 -nodes -sha256 -keyout domain.key -x509 -days 365 -out domain.crt
```

###### Step 2 : Run the command 

```
docker run -d \
  --restart=always \
  --name registry \
  -v /home/bhanuchander/myenv/docker-certs:/certs \
  -e REGISTRY_HTTP_ADDR=0.0.0.0:5000 \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key \
  -p 5000:5000 \
  registry:2
```

###### Step 3 : Add certificate in trust store
Copy the **docker.crt** file as **ca.crt**

```
cp docker.crt /etc/docker/certs.d/bhanuchander.nmsworks.co.in:5000/ca.crt
```

### How to Tag and Push an image ?

**docker tag [OPTIONS] IMAGE[:TAG] [REGISTRYHOST/][USERNAME/]NAME[:TAG]**

*Command:*

```
docker tag mysql:5.5 bhanuchanderu.nmsworks.co.in/mysql:5.5
docker push bhanuchanderu.nmsworks.co.in/mysql:5.5
``` 


###### Notes :

- [Running docker container](https://stackoverflow.com/questions/31667160/running-docker-container-iptables-no-chain-target-match-by-that-name)
  
  
###### References
 
 - https://code-maze.com/docker-hub-vs-creating-docker-registry
 - https://hackernoon.com/create-a-private-local-docker-registry-5c79ce912620
 - https://docs.docker.com/registry/insecure/
 - http://port.us.org/