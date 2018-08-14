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

- by deafult tag name is given is latest. But latest not always means latest. Its just a tag, even though orgs take care to maintain it liek that

- use "docker image push" to push images to repo. To push images. we need to login first. Push then takes the image and puts it under our registry in repo.


