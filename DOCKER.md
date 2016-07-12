** Common commands **

#Install and run a container
docker run -p 27017:27017 --name *container_name_you_like* -d mongo:3 --smallfiles --nojournal

#Attach a shell to a container
docker exec -it *container_name* bash