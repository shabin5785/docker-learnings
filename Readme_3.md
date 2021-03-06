
**Docker Compose**
- configure relation ship between containers or make them work together. We can use it to create our entire setup of the app, including all required containers. 

- It uses a YAML file configuration containing all continers, networks, volumes etc. then user docker-compose-cli to build the entire thing

- yaml file can be used with docker compose to create local setup and from version 1.13 can be used in prod with docker swarm

- docker-compose.yml is default name, use -f option for other files.

- atleast version 2 for docker compose file is recommended. 

- docker compose file has services section for continaers (including run, volume,image, commands, env, ports etc), volume section and network section.

- compose has a depends on section that can be used to build hierarchy and relationships

- compose is not prod ready and should be used for local development and testing.

- we can build a custom image inside comopse as well. Compose will look for image in cahce, if not found build it using our commands. next time the same file is composed, image is taken from cache.
for this we use build command within compose and refer it to a docker file.

**Docker Swarm**

- how do we ensure containers run in trusted server?how to store secrets, configs etc

- Swarm mode is a clustering solution built in docker. at its code swarm is a server-client arch whichh we can orchestrate and manage it.

- Swarm is different from swarm classic. 

- Swarm is not enabled by default. It has to be manually enabled to work with. it gives new commands like docker swarm, docker node, docker service, docker secret, docker stack

- Swarm has a bunch of manager nodes and worker nodes, Managers keep a copy of swarm config in a db (raft db? ) . Also it encrypts the communication between slaves. There can be multiple manager nodes and many many slave nodes. But among managers, only one can be a leader. Each of the manager and workers can be a virutal machien or nodes

- There is a single raft db for all manager nodes ( raft consensus group)

- mangers can be workers. and we can demote or promote nodes as managers or workers. A manager is a worker node with permission to control the swarm .

-with docker container we can start one container at a time. Swarm has service command using which we can design tasks, and declare teh number of containers and replicas that we need of them (like 3 nginx nodes). The manager nodes then decide where to run the tasks and create the worker nodes.

- swarm will accept commands to create a service. It will then orchestrate it and create tasks, then assign ip to the tasks , then assign nodes to the taks and then publishes to dispatcher.
Worker will connect to dispatcher and ask for tasks, once it gets a task it will execute it brinign the node up to life 

- docker swarm init to enable swarm ( will create a single node swarm). This will init the swarm, create certificates etc .In short set up a swarm. the single node is a manager node. It also creates the raft database and stores config ( raft is a protocol that ensure consistency across multiple nodes especially in cloud env)

- raft can be used to store config and secrets, no need for a separate db to store these values. 

- "docker node ls" will list nodes in swarm and manage nods within swarm.

- docker service command replaces run command in swarm. ( may be done to not break run command)

- "docker service create" to create a new service based on an image. 
docker service create alpine ( create a service based on alpine linux)
as we have only one node as of now the above one creates a service wiht one replica
service command will create the container and name it based on service name and take care of configuring it.

"docker service upate <service id> -replica 3" will update the above service (based on id) to have 3 replicas. 
  
- similar to this docker update can be used to modify the details or config of a running container. but difference between swarm update to container update is that we can do swarm update without brining the cluster down. So we can do a lot more update operations compared to a plain container. swarm will make sure that updates happens in a way that doestn bring down the cluster.

- service will take care of recovey and automation of conainter life cycles. so to remove all containers in a service, we have to remove the service. else if we remove a container service wil bring it back up

- inside swarm use the network with driver "overlay". this is for container traffic within the swarm . this comes out as a single vpn for all nodes in the swarm. So all nodes in swarm in different networks can talk to each other over the overlay network. 

each node in swarm can be connected to one or more overlay networks. Like db in one network, server in another one or two etc

- how overlay works is by using a routing mesh. this works within the swarm and routes the packets arriving to the service to the correct task. its a feature of linux kernal with a fancy name.
it also . load balances the network load of tasks.

- so if we have two nodes for a backend service, docker puts a VIP(virtual IP) in frnt of them and hanldes the load to the two nodes. We dont have to put a load balancer for this. Similar for extrnal traffic to the swarm.

- currenty the load balancer is a stateless loadbalancer

- stacks are an abstraction over services. Stacks accept compose files for services, networks and volumes and secrets.
stack will create all of the defined items, we can use exisitng items like networks usign external command in stack file. 

- stack cannot build images ( partly we shoudlnt be doing builds in prod). Similary compose ignores deploy.

**Secrets in Docker**

- Docker swarm raftdb as of 1.13 is encrypted disk by default. it is stored only in master node. 
We have store secrets inside this. Only containers within the swarm can see and retrieve it. They are not stored in disk, though it appears like that, but its actually in-memory. 

- after creating secret, we can map the secret files ( in-memory) while creating service and then assign values in secrets to the service.



-Docker override yaml file can be used to change docker file values. Dokcer will automatically read the file and apply the values. 

- docker service update command can be used to update a running contianer. it can be used to change container settings and bring it up again.

- we cannot do update and remove ports. we can only update with add and remove ports option

- we can also do healthchecks for containers. 