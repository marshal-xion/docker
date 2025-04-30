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











