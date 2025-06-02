**Problem:** how would you:
- automate container lifecycle
- scale up / down
- replace containers without downtime
- create cross-node virtual networks
- store passwords / secrets and get them to the right containers

### Swarm
- is a clustering solution built inside Docker 
- isn't enabled by default

**How it works:**
- Docker Swarm is like a leader that tells nodes what containers to run, where to run them, and how many copies of each to run
- In the Swarm group, you have nodes that can either be a **manager (leader)** or a **worker**
- Instead of running containers (with `docker run`), you now run **services** which are just instructions e.g. run 3 copies of this container

#### Initializing Swarm mode
`docker swarm init` - initializes a single-node swarm

**What this does:**
- creates a root signing certificate for the swarm and is issued for the first manager node
- join tokens are created to associate the manager and worker nodes with the swarm
- creates the Raft database to store root CA, configs, and secrets 

#### Running Services
Instead of running individual containers, you now run **services**. This is because the point now of Docker Swarm isn't to run individual containers, but to run workloads. So instead of the `docker run` command, you now use the `docker service create` command in its place.

You can run a service using `docker service create`
```sh
docker service create alpine ping 8.8.8.8
```
And list down the services currently running
```sh
docker service ls
```
You can also list down the tasks of a certain service. **Tasks** represent a single running container in the service and has a status
```sh
docker service ps <service>
```
To scale up a service:
```sh
docker service update <service> --replicas 3 
```

#### Creating a Multi-node cluster
1. Initialize the swarm on the leader
```sh
docker swarm init --advertise-addr <ip-address>
```
2. Copy the join token command and run on the workers
```sh
docker swarm join --token <token> <leader-ip>:2377
```
3. Verify that the worker has joined the cluster
```sh
docker node ls
```
4. Start a new service and test replication across nodes in the cluster
```sh
docker service create --replicas 3 alpine ping 8.8.8.8
```
5. See the deployment status of the service across nodes
```sh
docker service ps <service>
```
6. Check the tasks of a specific node
```sh
docker node ps <node>
```

#### Swarm Networking
Swarm introduces **multi-host networking** with an overlay driver. This makes it so that containers across multiple hosts can talk to each other as if they were in a VLAN. To enable this:
```sh
docker network create --driver overlay <network>
```
An **overlay network** is like a magic tube that makes it seem like 2 containers / hosts can talk to each other on the same subnet, even though they actually aren't.

To test this, create an overlay network and deploy two services, one for each node. You should be able to connect to the postgres database as if it the containers were running on the same host.
```sh
docker service create --name psql --network mydrupal -e POSTGRES_PASSWORD=mypass postgres:14
docker service create --name drupal --network mydrupal -p 80:80 drupal:9
```

#### Routing Mesh
Even though only one node might be running a certain task of a service, other nodes can sort of delegate incoming packets that are addressed to that service to the proper task. This is possible through **routing mesh**. What this means is if you have an app running only on one node, users can still access that app if they go through other nodes even if they aren't running that app.

Routing mesh acts as a load balancer that balances Swarm Services across their Tasks using a virtual IP. This also means that if you publish a port on one node, all nodes will listen to the port.

![[routing-mesh-network.png]]

The Routing Mesh ingress network is also what makes it possible for you to run several copies of a container that seem to be "bound" to the same host port. In reality, only the Swarm ingress network is bound to the host port, while the containers themselves are on an overlay network wherein routing mesh can distribute the load. It distributes the load in a connection-based manner, meaning the container will handle traffic as long as the host receives it. To do true round-robin, you have to use an external proxy to load balance.

#### Docker Swarm Stacks
Swarm Stacks are just Docker Compose for services--they allow you to specify declaratively what your services are in just one file, plus any volumes, networks, and secrets that need to be deployed with them.

The main difference is that when Docker Swarm uses a compose file, it will ignore the `build` section because ideally in production scenarios, it shouldn't be used to build images and should only pull them. You can then specify an additional `deploy` section to set Swarm-specific configuration.

```yaml
deploy:
	replicas: 1
	update_config: # how do you want updates to roll-out?
		parallelism: 2
		delay: 10s
	restart_policy: # what happens when the container fails?
		condition: on-failure
	placement:
		constraints: [node.role === manager] # make sure its deployed on a specific node
```

To deploy the stack:
```sh
docker stack deploy -c <filename.yml> <stack-name>
```
#### Swarm Secrets
- secrets are only stored on disk on manager nodes
- they are assigned to services, and only containers in the assigned services can see them

You can create a secret either from a plaintext file, or by passing the secret value in the command line:
```sh
docker secret create <secret-key> <filename-containing-secret-value>
echo "mypassword" | docker secret create <secret-key> -
```

To assign the secret to a service, use the `--secret` option for each secret key. Assigning secrets creates a file on the containers inside that service so that they can access the secret value
```sh
docker service create --name psql --secret psql_user --secret psql_pass -e POSTGRES_PASSWORD_FILE=/run/secrets/psql_pass -e POSTGRES_USER_FILE=/run/secrets/psql_user pos
tgres
```

#### Rolling Updates
To scale a service:
```sh
docker service scale web=5 api=3
```
To apply an image update / downgrade:
```sh
docker service update --image nginx:1.13.6 web
```
To publish / unpublish ports:
```sh
ocker service update --publish-rm 8088 --publish-add 9090:80 web
```


