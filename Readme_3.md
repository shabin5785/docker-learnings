
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










