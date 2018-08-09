# docker-learnings

- Containers are consistent across different devices. It runs the same way fundamentaly in any device. So we dont need to worry about the way software runs in different platforms

- migrating app to container doesnt need any code change. It runs the same way as before, but in containers. 

- mac natively doesnt support docker, same as win10. So a small virtual os needs to be run in these os to run docker in them ( dont use brew for docker in mac). So mac/win are similar to this feature.`

- In docker edge version means beta version. Its supported only for a month, after that a new version is realeased.  Stable is supported for around 4 months, after that a new one is released.

- in Windows we can have a linux container or a windows container. windows container is a newer one as MS started supporting it recently. Not all version of docker work for all versions of windows.Old version of windows needs to use legacy docker called as Dokcer tool box. Same with old mac os versions. Dont install docker from linux packages as these are old.  Windows is going to support native linux containers soon. 

- Newer versions of windows in background docker for win is using Hyper-V wiht tiny linux VM for linux containers. This does not work well with virtualbox. as hyper-v doesnt work with virtualbox. Old versions of windows uses a docker-machine which is a linux vm in virtual box, these two doesnt support windows containers. Windows server 2016 support native windows containers. 

- new way of docker command is docker <management command &lt <subcommand &lt , but the old way to just giving the command still works. Docker provided this division as commands were getting too many in number

- a docker image is the application that we run. Container is the instance of the image that we run.we can have many containers based on an image.

- publish command will expose a local port and link it with the port of the container.
publish A:B, where A is local port and B is container port

- detach will run the docker command in background. this will return a unique container id

- container ls will list all containers running ( docker ps does the same thing), ls -a will list all old containers

- container stop <containerid> to stop it. First few chars of id is enough. We can name the containers ourselves, but this needs to be unique. If not specified, docker will generate the name, based on random adjectives and names. name of container is different from id of a container,

-- use logs command to view logs on container (esp running in the background). 

-stopping a container doesnt remove it. We then need to delete it to remove from system. we cannot remove a running container. Stop and remove it or remove it forcefully remove it

-docker on starting a continaer, adds a layer on top of image, sets networking, gives it a virtual ip, opens the required ports and starts it.

- the container runs as a process on the host machine, run as a different user. We can see the process in teh host machine process list. in that way its different from a virtual machine and an app that runs inside the virtual machine.
- use env option while starting containers to pass environment variables to the containers. 

- sample commands
docker container run --publish 80:80 --detach --name mycontainer_nginx nginx
docker container logs <container name&lt
docker container ls
docker container ls -a
docker container stop < container ids>
docker ps
docker container rm -f <container ids &lt //force remove option


- docker has commands that can help us show container meta data and performance like stats command or inspect command or top command

- we dont need a ssh connection to a container to connect and interact with the container.docker can do it for us.

- there is an "it" option, which is combinatin of interactive and tty terminal. This passed to docker while running a container gives us an interactive terminal to work with, connected to teh container.

- docker container run [OPTIONS] IMAGE [COMMAND] [ARG...]
 here after image we can pass commands that needs to be run in the continer. Like passing bash will run bash command within the container. 
 
 This command will override the default command that is run in the container. Now if we specify the command as bash, then later exit the bash, it means the bash is stopped container has no process to run and will stop.
 
 - some continaers like linux distros have bash or a terminal as defautl command to run. So if we just specify the it option, contianer will by default return an interactive terminal. But continaers like nginx, dont have default command as a termianl or bash, or givingn just it wiht no command override, will give a blank interactive terminal 
 
 - continaer start command to start a continaer. Again it has option of "ai" which is attach interactive to get a shell
 
 - to attach shell to a running container, use the exec command to connect terminal with it. exec use "it" option just like run
 
 - when we exit the command run using exec, container is not stopped, as the exec was run in addition to root command and so there is a command running on contianer. but when using run, we are running only one command and so contaienr stops when this is exited.
 
 - and we can only run a command in a container only if that is installed in the continer. Like running bash, the container needs to have bash installed. 
 
 - each contianer has a private network bridge which is "NAT" to the host ip
 
 - all containers with a virtual network can talk to each other. Its best practice to keep logically separate containers in different virtual networks. 
 
 - if we chose not to use the default virtual network , we can create our own virtual networks. attach containers to that etc. We can link container to multiple virtual networks, or even connect container to host ip ( lost some container features)
 
 - docker container port can list ports open within the container
 
 - we can pass a "format " option with a formatting string to extract data from json output, like use it to extract data from inspect command
 
 - Continaers are by default created in their virutal network. This network cannot access host machine network. We then create a tunnel or link by using "-p" which maps a port from host to a port in virtual network. Now traffic is routed from virtual network to host network via this ports. 
 Two ports in hostmachine cannot be used multiple times.
 
 - "bridge" is the name of the default network created by docker that is NAT'ed to host ip
 "host" is the name of the host netwokr. We can attach contianers to this network but not recommended.  there is a . "none" network whihc is not attached to any network.
 
 - network in docekr needs a driver to work. By deafult docker has a driver name "bridge". We can use other drivers as well.
 
 - we can create our network, configure it and attach continaers to it. We can choose network while creating continars as well. We can also disconnect or connect a container to a network.
 connect and disconnect are dynamic, no need to stop teh containers. 
 
 - each network created will have its own ip range. startign with 172.x.x.x, with increasign numbers
 
 - docker supports dns and we should use it ahead of ip. Docker uses container name as dns names.( we can set alias if we wants to)
 
 - default netowork "bridge" doesnt have dns resolution built into it. so on creating containers, we have to use "link" option to link containers and allow dns resolution. 
 Its far easier to creaet a new network( which will have dns built into it). then we by default get dns resolution
