### Networks
- by default, each container connects to a virtual private network called the bridge network (has the docker0 interface)
- the best practice is to have the containers that need to talk to each other for one app grouped inside one virtual private network (without exposing them with the `-p` option), and for another unrelated app, their containers will be grouped in another private network that can't talk to other private networks
- just like how you can have multiple NICs inside one computer, a container can connect to 0, 1, or 2 or more virtual private networks
- the ethernet interface on the host has a "firewall" that by default, blocks all incoming traffic coming through (until you publish a port with `-p`), and translates the IP address coming from the bridge network (usually 172.x.x.x) into 192.168.x.x.

#### Useful commands
- `docker inspect --format '{{ .NetworkSettings.IPAddress }}' <container>` - get the IP address of a container
- `network ls` - list down Docker networks
- `network create` - create a new virtual network
	- `--driver` - create the network with a third party driver
- `network connect <network> <container>` - attach a network to a container even while it's running
- `network disconnect <network> <container>` - disconnect a container from a network
- `network inspect <network>` - show the configuration of a network, including what containers are connected to it
- `run --network-alias <alias>` - give a container an alias in the DNS record (CNAME)
- `run --network <network>` - run a container connected to a custom network on startup

### DNS
- containers shouldn't rely on IP addresses for communication because configuration changes so often
- Docker daemon has a built-in DNS server that containers use by default
- if you create a new network, you can ping other containers in that network just by their container name (hostname
	- the bridge server doesn't have this built-in DNS server by default

##### DNS Round Robin
Docker's internal DNS sort of does a round robin when you register the same alias for 2 containers. For example, starting 2 elasticsearch containers:
```sh
docker run -d --net round-robin --net-alias search elasticsearch
docker run -d --net round-robin --net-alias search elasticsearch
```
Now, when you go into a container and execute `nslookup search`, it will give you the records that point to the IP addresses of the 2 containers. So now when you curl `localhost:9200`, either of the 2 containers may answer.