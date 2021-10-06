Resources used : 
1. https://madhuakula.com/content/introduction-to-docker-containers/#/4
2. https://www.youtube.com/watch?v=gAkwW2tuIqE : Learn Docker in 7 Easy Steps : Fireship

Docker is a container based virtualization approach where we use the kernel on the host's OS to run multiple guest instances whcih are called containers.

Each container has : Root File System, Process, Memory, Devices, Network ports.

Containers help us wrap a piece of software in a complete filesytstem that contains everything we need to be installed and run the application as we would like it to for everyone and hence not have a "It ran good on my computer" situation and it runs on every system.

Main Tools: 
1. Image : Read only with OS. libraries and apps, Its the blueprint of the containers and what containers are built from. 
2. Container : Statefull instances of image with a writeable layer basically think of it as a house built with the image used as its blueprint.
3. Docker Registry : This a repository of images like Github is for code for example and one fo the major public docker registry is Docker Hub (https://hub.docker.com/)
4. Dockefile : These are the steps that are taken by Docker to create the Blueprint(Image). So basically you are drawing out the blueprint using this Dockerfile. 
   
Main tool/products in Docker;

Docker Engine : Core functions to Docker image4s to create and start Docker Container.
Docker Machine : Automated Contrianer Provisioning 
Docker Swarn : Allows Container clustering and scheduling 
Docker Compose : Allows to define muli-container enviornments and operate them.


Then some basic commands to use in Docker : 

To Build : 
1. To build a image using the Dockerfile in the current directory with a tag and version number 1.0 :
   
`docker build -t imagename:1.0`

2. List all the images that are locally stored with Docker Engine :
`docker images ls`

3. Delete an image from the local image store :
`docker image rm alpine:3.4`

To Run : 
1. To show all running containers :  `docker container ls`
2. Run a container with an image and name, then expose port 500 externally mapped to a port 80 inside the container : `docker container run --name web -p 3000:80 image:1.0`
3. List the networks  : `docker network ls`
4. Stop a running container through SIGTERM : `docker container stop containername` 