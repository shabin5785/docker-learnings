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
publis A:B, where A is local port and B is container port

- detach will run the docker command in background. this will return a unique container id

- container ls will list all containers running ( docker ps does the same thing), ls -a will list all old containers

- container stop <containerid> to stop it. First few chars of id is enough. We can name the containers ourselves, but this needs to be unique. If not specified, docker will generate the name, based on random adjectives and names. name of container is different from id of a container,

-- use logs command to view logs on container (esp running in the background). 

-stopping a container doesnt remove it. We then need to delete it to remove from system. we cannot remove a running container. Stop and remove it or remove it forcefully remove it

-docker on starting a continaer, adds a layer on top of image, sets networking, gives it a virtual ip, opens the required ports and starts it.

- the container runs as a process on the host machine, run as a different user. We can see the process in teh host machine process list. in that way its different from a virtual machine and an app that runs inside the virtual machine.
- use env option while starting containers to pass environment variables to the containers. 

