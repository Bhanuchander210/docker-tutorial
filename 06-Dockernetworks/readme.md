# Docker Networks
---

## Docker Network Drivers

- It is a piece of software which enables networking for containers.
- Native network drivers are shipped with docker engine.
- IP Address management drivers provide default subnets if not specified by admin.

## Architecture

![Arch](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2018/07/Architecture-of-Container-Networking-Model@2x.png)

*Image Source : [Ref](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2018/07/Architecture-of-Container-Networking-Model@2x.png)*

# Native Network Drivers

## Host Network

Multiple containers running on same native host network are connected using **veth** virtual ethernet which reflects
the capabilities and limitations of host network.

## Bridge Network

It is also a default network for docker containers. If we don't explicitly connect of containers to another network, It
normally connected to its default bridge. It creates a *virtual ethernet bridge*. All of the Containers are connected to the
bridge network via **containers endpoint**. Now this bridge communicates to the host network.

So it can be isolated from host network. Here we can define **IP Range** and **Subnet Mask** for the bridge network.

Or we can use **IPs** to communicate the containers which are provided by IPAM Drivers.

![bridge](http://img.scoop.it/bmExZyvGWidultcwx9hCb7nTzqrqzN7Y9aBZTaXoQ8Q=)

## Overlay Network

This network used for **Swarm Mode** of docker. Multiple cluster of hosts connected in single network. It placed on the 
underlay network which collects the ip related details of all connected containers.

![overlay](http://img.scoop.it/1nNoIXGkJiDax7l5g5GxH7nTzqrqzN7Y9aBZTaXoQ8Q=)

## Macvlan Network

Each containers which connected to this network, should be assigned by a Physical MAC address. See the official [documentation](https://docs.docker.com/network/macvlan/) for more details.

![macvlan](http://img.scoop.it/zD6OR5JZu3qF9dxWL79Gc7nTzqrqzN7Y9aBZTaXoQ8Q=)

## Quick Details

### Creating Docker Network

- To create a bridge network with default info

```commandline
docker network create --driver bridge testbridge
```

- To create a bridge network with specific details by using arguments

```commandline
docker network create --driver bridge --subnet=192.168.0.0/16 --ip-range=192.168.5.0/24 testbridge-1
```

The inspected specification *json* files are attached here.

- [testbridge.json](/assets/files/testbridge.json)
- [testbridge-1.json](/assets/files/testbridge-1.json)


### Connect Disconnect and Inspect

We can connect a container to a network like shown below command,

````commandline
docker network connect testbridge-1 apachecontainer
````
Now it will connect to the new bridge along with the default bridge.

To inspect this:

```text
$ docker container inspect --format "{{.NetworkSettings.Networks}}" apachecontainer
map[bridge:0xc4205fc000 testbridge-1:0xc4205fc0c0]
```

The total inspected specification file is attached here as [connected_apachecontainer.json](/assets/files/connected_apachecontainer.json).


### To get the container ip
 
```commandline
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' containerName
```


### Good-practice to deploy containers
(as per my point of view)

- **Port Mapping** for single container / multi-containerised Local bridge
- **Static IP Assigned Bridge** Local (Ip-range and aux-address assigned) bridge

#### Posts
- https://docs.docker.com/v17.09/engine/userguide/networking/default_network/container-communication/
- http://blog.oddbit.com/2014/08/11/four-ways-to-connect-a-docker/
- https://stackoverflow.com/questions/46951569/how-to-connect-containers-on-two-different-hosts-connected-by-lan
- http://blog.oddbit.com/2018/03/12/using-docker-macvlan-networks/
- https://hicu.be/bridge-vs-macvlan
- https://sreeninet.wordpress.com/2016/05/29/macvlan-and-ipvlan/ 

### Reference

- [Docker network](https://docs.docker.com/network/)
- [Understanding docker networking](https://blog.docker.com/2016/12/understanding-docker-networking-drivers-use-cases/)

[next](/07-Dockerstorage)
[home](/)