Docker commands

docker run -it ubuntu  -> Run ubuntu in interactive mode

docker container ls -> shows all active docker containers

docker container ls -a  -> shows all active and inactive containers

docker start container_name  -> starts the inactive container

docker stop container_name -> stops an active container

docker exec container_name ls -> runs ls in container_name and returns the result, closes the container after running.

docker exec -it container_name bash  -> runs bash and directly runs the bash from the container_name, keeps running the container

docker images  -> shows images in local machine

docker images ls  -> lists all the docker images

docker run -it -p 9000:9000 image_name  -> to expose containers port 9000 to local machine 

docker run -it -p 1000:9000 -e key=value -e key=value image_name -> pass env values from docker command to be set for the container



Dockerizing an existing project

create a file Dockerfile in the root of the project 
open it and add the commands for example we want to dockerize a 

FROM ubuntu   (choose a base image)

RUN apt-get update  (updates the apt to latest)
RUN apt-get install -y curl  (installs curl)
RUN curl -sL https://deb.nodesource.com/setup.18.x | bash -   (installs node.js)
RUN apt-get upgrade -y
RUN apt-get install -y node.js

COPY package.json /src/appname/package.json
COPY package-lock.json  /src/appname/package-lock.json
COPY main.js /src/appname/main.js

RUN npm install  (installs all the packages from package.json)

ENTRYPOINT ["node", "main.js"]   (runs the main.js file using node command in the container - starts localhost)

can copy project folder also and if you want to ignore some file to copy from a folder put .dockerignore file in the root
and then add the name of the folder that you dont want to copy like below

node_modules/


after this adding dockerfile to your root you need to convert the folder to an docker image so open the folder in a cmd prompt and type

docker build -t "image_name" .  (tag is same like commit -m in github, here . means same directory or path to the file)

after the image is created we can run the image and the output willbe a nodejs prompt running the main.js








