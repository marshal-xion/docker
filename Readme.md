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


you can run the image by below command

docker run -it -p 8000:8000 image_name 

docker exec -it image_id bash  (you can get image id from container ls, this will open the bash of that container instead of running the file)


you can also use node directly 

FROM node   (choose a base image) (directly loads the node installed container)

COPY package.json /src/appname/package.json
COPY package-lock.json  /src/appname/package-lock.json
COPY main.js /src/appname/main.js

RUN npm install  (installs all the packages from package.json)

ENTRYPOINT ["node", "main.js"]   (runs the main.js file using node command in the container - starts localhost)




TO upload to docker hub - 


create a repository in docker.hub.com and name it as username/projectname

then come to local and create a image with same name 
docker build -t username/projectname
docker push username/projectname

make sure you are logged in with same docker account







Running multiple containers - 

postgres - cont1
redis - cont2
mail - cont3


create a docker-compose.yml file -  (configuration)

version '3.1'

services:
  postgres-cont1:
    image: postgres  #hub.docker.com
    ports:
       - '5432:5432'
   restart: always
   volumes:
       - db_data:/var/lib/postgresql/data
    enviroments:
       POSTGRES_USER: postgres
       POSTGRES_DB: review
       POSTGRES_PASSWORD: password
    healthcheck:
       test: ["CMD-SHELL", "pg_isready"]
       interval: 10s
       timeout: 5s
       retries: 5
    postgres_is_ready:
       image: postgres
       depends_on:
          postgres:
             condition: service_healthy
       


 redis:
   redis-cont2:
      image: redis
      ports: 
         - '6379:6379'
  

save this file and go to root of the project and open a cmd prompt and run 
docker compose up    (starts 2 containers locally using the config file)
use sudo docker if in linux

this will run a stack of containers 

docker compose down  (stops the container)

docker compose up  -d   (detach mode - runs in backgroudn)




