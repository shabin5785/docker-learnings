- image has the application binaries and dependencies and the meta data about image and how to run them. No need for kernal or drivers, as the host kernal is reused by the containers hence light weight.

- docker images are made up of union file system. docker image history wil show the historical changes to that image.

- every image starts with a blank layer called "scratch". Images can have multiple layers. so layers are jsut metadata change, with no files uploaded. every layers has a unique SHA to identify it uniquely.

- Images are pulled in layers. So when we pull a image that uses the same layer as an existing image, docker uses the image layer from cache. ( provided it has same SHA as required). So now we have two images sharing same layer ( they will be readonly based on union file system concept)

- so if we change the file in base layer, docker will copy that file to the top most container level. This is known as **copy of write** and while accessing the top most file is returned by docker. 

- we can inspect an image and see the metadata, like exposed ports, the startup command and lot of these things.

- so a container is just a single read/write image on top of an image. 

- images dont have a name, they have an id, a tag, and a repo. Repo is usually the organization or the user name. a combination of repo and tag name is what we use to identify the images. 

- an image can have many tags, and if we are to download them again, they can be served from cache,if we already have a image with that tag in system.

- we can retag existing docker images. use "docker image tag "for this

- by deafult tag name is given is latest. But latest not always means latest. Its just a tag, even though orgs take care to maintain it liek that. So when we omit tag during creation, docker tags it as "latest".

- use "docker image push" to push images to repo. To push images. we need to login first. Push then takes the image and puts it under our registry in repo.

- all docker images are created using a docker file. default file name for building images is Dockerfile. but we can use -f flag to give our own file name

- "FROM" command is required in all docker files.Its the base image to start.Useful to use as small as a base image as possible line alpine instead of ubuntu. but using a main repo like ubuntu will get all packages and security updates.

- "ENV" command is then used to environment varialbles ( key-value pairs). One main reason is that they work the same everywhere

- order of commands in dockerfile is the order of layers in images. So the order of commands is very important. Even the ENV command is a layer in image. Any command in docker file builds an iamge. 

- "RUN" command is used to run shell scripts or any scripts inside the base image. 

- Now runnign too many commands will build too many layers and hence hte size will increase. So we can link commands under one RUN command using the "&&" option so that they all become one command and builds just one layer.

- docker handles all logging for us. we need to ensure to send our logs to stdout and stderr. Our own logging will lead to overhead and complexity. We can map/link the stdout and stderr to different files within container.

- "EXPOSE" command is user to expose ports out of container. By default no ports are opened in the container. But this expose does not expose ports in the host, for that we need the --publish option.

- "CMD" command is hte last command and its required. This is hte command that is run every time we lauch our container or run it. A docker file needs only one CMD command. So if there are multiple the last one wins.

- use docker image command to build an image from docker file. 
docker image build -t <name&gt .

-t is used to tag image wiht name. If pbulish to docker hub we need unique image names. Else for local we can use any name. Lsat is the directory to build from. ( the above command is current directory). if our file is not named Dockerfile, need to use -f optin as well to specify the file name .

- WHile building each layer is given an unique hash. SO next time we build it and line has not changed, then the cached one is used. 

- once built images go to local dokcer image repo. image is not put in the local directory that we built from.

- as image has layers and while building docker uses cached layers,we should use layers that change as last as possible. Docker needs to build all layers since the changed one. So its better to keep the layers that change as last as possible. Like copying code to run in a server, we should put the copy code as last. Else every time we change the code, the entire layers needs to be rebuilt. 

- instead of building from scracth, like building a server by using ubuntu, then nginx and then our code, we can extend official images. We can start frmo nginix image and add code. In this way we can be better proctected and safer and easier. So we define the official image as base image, add commands to docker file to extend it, build it and get a custom image. Now official images,like nginix will already have ports exposed. So if we dont want additioanl ports we dont need to expose it. Like that official images will already have default cmds to run,so we dont to need to specify them as well. 

- "WORKDIR" is the command used in Dockerfile to chagne the directory to work with inside image. Its same as using "RUN cd.. ", docker workdir is the best practice. 

- "COPY" command is used to copy files from local directory to the WORKDIR in image.(or any directory that is currently being worked on in image). images may have a default directory set, so we might not need to change dir as well. BUt its safer to change and work as this avoids confusion.

- One important thing is that we dont inherit "ENV" values from base image. We need to specify them ourselves.

- docker image tag can be used to tag images with names and docker image push can be used to push images to central repo. remeber we need unique names. So use a namespace based naming pattern.

- && in command line is a conditional . Second command is run only if first is successfull. a semi colon(;) also does almost same, but run second command irrespective of the status of first. 

- "COPY . ." copy all files from current directory to current working director in image

**Persistent Data**

- Containers are usually meant for immutable and ephemeral. So we just throw away containers and start a new one. But there are cases where we need to store data, like a database or key-value pairs. Standard containers are persistent in nature. Data is retained once we stop and start the container. 

- Ideally container shouldnt contain unique data, this is known as separation of concern. If we have unique data then we need to ensure that other containers that we start also has that data. 
But this data is lost when we recycle contianers, or new contianers started wont have this data. 

- Dokcer has two solutions for persistant data, docker volumes and bind mounts. 
volume creates a special volume outside container UFS file system, but within in the docker , and not in host machine to store unique data. this is preserved across container removals and allows us to attach it to what ever contianer that we want. Contianer see this as a local file path.
Bind mounts is to bind a local direclty to a continaer directory. 

- Volume is configured in docker file . It is a direcetory specified in docker file. So on container boot, this directiry is created and used for storage. This is persisted until we manually remove hte container. 
Volumes need manual deletion. We cannot delete it by removing containers . 
docker volume prune is used for this 

- In background, the volume is physically created in host machine with unique folder name and then mounted to the volume path for the container. 
We can see the data in host machine.

**- Docker for windows and mac runs a linux vm to run docker.**

- each container will get its own volume and a unnique directry created in host machine . this is retained even after continer is removed. 

- we can have named volumes when creating containers to help with volumes. 
the "-v" option is same as specifying the volume in docker file. 
now "-v abcd:/var/lib/mysql", here abcd is the volume name and part after colon is volume
this is a named volume and will have a friendly name for us to identify. 

- now advantage of naming is that we can use the same nameed volume while creatng containers. Now all these containers will share the same volume and data.

- there are few cases where we need to create volume before creating containers, like when we are using custom drivers. 

- volume skips the ufs file system, like bind mounts. 

- bind mounts map a host file or directory to a container file or directory.
- bind mounts are host specific so cannot be specified in docker file, but when during contianer run.

- option for bind mount is same "-v" option but with a full path to host directory or file.
 "-v /home/shabin/server:/usr/share/nginx/html"
 above one maps local dir to container one. We basically has two names pointing to same locatin. 
 We can edit files in host machine and see changes in container. 
 
 











