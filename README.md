
This repo contains collection of docker commands, It was just created and will be actively updated. 

Please fork and submit your pull-requests, if you would like to contribute. Thanks!

### Get Docker status :
```bash
$ docker info 
```

### Download Image, Create Container, Start Container : 
```bash
$ docker run -itd $ImageName:$Version $Command -b
```
```
-i : interactive
-t : tty
-d : detached - start container in background
--cpuset-cpus="1,3" : container can use only cpu #1 and #3
--env VAR1 : set enviroment variable VAR1 in container equal to enviroment variable in docker system
--name : set name for container
--storage-opt size=120G : set docker partition size
--shm-size 128M : set container shm to 128mb
$Command -b : it's optional and run specified command in binary mode
```
Example :
```bash
$ docker run -itd --name "MyCentOS" centos:lastest /usr/bin/top -b
```


### Create stopped container
```bash
$ docker create -it centos:lastest /bin/bash
$ docker start -ai $CreatedContainerID
```

### Images
```bash
# List local Images
$ docker image ls / docker images -a
# Remove Images
$ docker rmi ($ImageID / $ImageName)
$ docker rmi $(docker images -q)
# Search DockerHub Images
$ docker search $Name
# Download Images
$ docker pull $ImageName
```

### List containers :
```bash
$ docker ps -as
```
```
-a : show All Containers
-s : show Containers size
-q : show only ContainerID
-n 3 : just show 3 of Containers
-f status=exited : show only container with exited status
```

### Change container states
```bash
$ docker start $ContainerID
$ docker stop $ContainerID
$ docker attach $ContainerID
```
```
Tip : to deattach from container and prevent container status to down you must use CTRL+P+Q
```

### Remove containers
```bash
$ docker rm -f ($ContainerID / $ContainerName)
$ docker rm -f $(docker ps -aq) # Remove All Containers
```
```
-f : Force Up Container to remove
```

### Copy file to container :
```bash
$ docker cp $FilePath ($ContainerID / $ContainerName):/root
```
```
-a : copy file with all attributes
```

### Docker execute command :
```bash
$ docker exec -it ($ContainerID / $ContainerName) /bin/bash
```