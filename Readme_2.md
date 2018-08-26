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


