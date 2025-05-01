docker run -it --name somename busybox 

busybox = image name


docker network inspect -> shows network for active containers

all containers are attached with the docker bridge.
bridge -> default nw driver
host -> 

docker network ls -> shows network, bridge, host and none

docker run -it --network=host busybox  -> make this container as host

difference between bridge and host

bridge -> need to expose ports as they are on different nw. (bridge network and your local network)

host -> no need to map port if ran in host mode - host and docker is on same nw so no need to expose the ports.


you can make your custom nw communicating containers between each other. here no ip address is needed and can communicate using the host name only.

docker network create -d bridge name  -> driver bridge mode

docker run -it --network=name --name somename ubuntu  -> connected with name network running ubuntu

docker run -it --network=name --name somename ubuntu  -> connected with name2 network running ubuntu

as above two in same nw they can communicate with each other

ping name -> will be able to ping from one container to another.



we can use this in a project where we create our own nw in bridge mode and then we can create one container with posgres and another with node they can comunicate with each other without worrying about ip address.





volume mounting


container has some memory 
when container is destroyed the memory also is destroyed
to prevent this we have docker volumes to save memory so that we can work later time again.

docker run -it -v /pathtodesktop/work:/container/work ubuntu -> desktop work folder mounts the docker work all data will be saved in the desktop 


create your own  volume

docker volume create vol1  -> create your own volume

docker volume rm vol1 -> remove the volume

docker run -d --name det --mount source=vol1,target=/app nginx:latest  -> mounts the nginx container on the created new volume.


docker inspect det  -> inspects the volume




Efficient caching

layers are cached in dockerfile

any change you make will not run the whole layers it will run only the layer below from the changed code layer, rest the above layers will be run from cache.

the order of layer matters so any change will take more time to build. keep at the bottom the most changing files.

Dockerfile advanced -

COPY . .   -> copies all files from local to image.

WORKDIR /app  -> dont have to mention path if workdir is mentioned

RUN CD app && npm install  -> running 2 command in 1 line




Multi stage build

Typescript .ts -> js here ts is converted to js - write a pipeline

FROM ubuntu

RUN apt-get update
RUN apt-get install -y curl
RUN curl -sL https://deb.nodesource.com/setup_18.x | bash
RUN apt-get upgrade -y
RUN apt-get install -y nodejs
RUN apt-get install typescript

WORKDIR /app

COPY package.json package.json
COPY package-lock.json package-lock.json

RUN npm install













