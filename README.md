
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
--env VAR1=Value1 / -e VAR1=Value1 : set enviroment variable VAR1 in container equal to enviroment variable in docker system
--name : set name for container
--storage-opt size=120G : set docker partition size
--shm-size 128M : set container shm to 128mb
$Command -b : it's optional and run specified command in binary mode
-v vol1:/opt:ro : Mount vol1 to /opt read-only
-p PublicPort:PrivatePort : -p 3000:3306
-P : Connect random port on host to container nedded ports
```
Example :
```bash
$ docker run -itd --name "MyCentOS" centos:lastest /usr/bin/top -b
```
<div dir="rtl">
Docker Run در اصل ترکیبی از ۳ دستور مختلف می باشد که ایمیج مورد نظر را دانلود و کانتینر آن را ساخته و آن را اجرا می کند.
</div>

- docker pull centos:latest
- docker create -it --name "MyCentOS" centos:lastest /bin/bash
- docker start /MyCentOS

### Create stopped container :
```bash
$ docker create -it centos:lastest /bin/bash
$ docker start -ai $CreatedContainerID
```

### Images :

```bash
# List local Images
$ docker image ls / docker images -a

# Remove Images
$ docker rmi ($ImageID / $ImageName)
$ docker rmi $(docker images -q) # Remove All Images

# Search DockerHub Images
$ docker search $Name

# Download Images
$ docker pull $ImageName

# Create new image from changed image
$ docker commit ($ContainerID/$ContainerName) $NewImageName

# Export docker image
$ docker save -o $FileName.tar.gz ($ImageID / $ImageName:$ImageTag)

# Import docker image
$ docker load -i $FileName.tar.gz

# Add tag to loaded image
$ docker tag $ImageID $ImageName:$ImageTag
```


<div dir="rtl">
نکته : در داکر در صورتی که محتویات یک کانتینر تغییراتی اعمال می کنیم می بایست ایمیج مورد نظر را Commit کرده و در صورتی که تمایل داریم آن را به دستگاه دیگری انتقال دهیم با Save و Load از آن خروجی گرفته و آن را در داکر دستگاه دیگر لود می کنیم.
</div>

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

### Change container states :
```bash
$ docker start $ContainerID
$ docker stop $ContainerID
$ docker attach $ContainerID
$ docker pause $ContainerID 
$ docker unpause $ContainerID 
$ docker kill $ContainerID 
```
```
Tip : to deattach from container and prevent container status to down you must use CTRL+P+Q
```

### Remove containers :
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

### Container extra commands :
```bash
# Docker execute command
$ docker exec -it ($ContainerID / $ContainerName) /bin/bash

# Export Container
$ docker export ($ContainerID / $ContainerName) MyContainer.tar

# import Container
$ docker import MyContainer.tar
```

### Container Network : 
```bash
$ docker port ($ContainerID / $ContainerName)
$ docker network ls
$ docker network create --subnet 192.168.20.0/24 mynet
$ docker network connect --ip 192.168.20.100 mynet ($ContainerID / $ContainerName)
$ docker network disconnect mynet ($ContainerID / $ContainerName)
$ docker network rm mynet
```

### Volumes :
```bash
# Real volume location in host is /var/lib/docker/volumes/vol1/
$ docker volume create --name vol1
```

### Docker Inspect :
Show information about container or images by id
```bash
$ docker inspect ( $ContainerID / $ImageID )
```

### Docker File :
```bash
# Build DockerFile To Image
$ docker build -t MyImageName .
```

# Create Local Registry :
```bash
# download registry image from dockerhub
$ docker pull registry

# create registry container
$ docker run -dlt -p 5000:5000 -name registry registry
```
```
Edit file : /usr/lib/systemd/system/docker.service

ExecStart=/usr/bin/dockerd

To:

ExecStart=/usr/bin/dockerd --insecure-registry $RegistryContainerIP:5000
```

```bash
# Reload services
$ systemctl daemon-reload
$ systemctl restart docker

# Upload image to remote registry
$ docker tag centos $RegistryContainerIP:5000/myimage
$ docker pull $RegistryContainerIP:5000/myimage

# Optional : add web interface to registry
$ docker run -it -p 8080:8080 --name registry-web --link $RegistryConrainerName -e REGISTRY_URL=http://registry-srv:5000/v2 -e REGISTRY_NAME=localhost:5000 hyper/docker-registry-web
```

# Docker Compose
Docker compose is official plugin for docker to deploy mulptile container by one yml file.
```bash
# Install Docker Compose
$ yum install epel-release
$ yum install python-pip
$ yum upgrade python*
$ yum install docker-compose

# Check installation
$ docker-compose --version

# Show composed containers status
$ docker-compose ps

# Start composed containers
$ docker-compose up -d

# Stop composed containers
$ docker-compose stop

# Force-Kill composed containers
$ docker-compose kill

# Remove composed containers
$ docker-compose rm

# Stop composed container & remove
$ docker-compose down

# Check compose file before start
$ docker-compose config
```

## Create docker compose file
```bin
$ nano /usr/local/bin/docker-compose.yml
```
```
my-test:
  image: nginx:latest
```
```bash
$ docker-compose up -d
```

for more information [click here](https://docs.docker.com/compose/gettingstarted/#step-3-define-services-in-a-compose-file).

# Docker Cluster Swarm
```bash
# Node 1 - Leader ( IP : 192.168.1.1 )
$ docker swarm init --advertise-addr 192.168.1.1
# To join workers run this command ( Token get from top command )
$ docker swarm join --token $TOKEN $MasterIP:2377
# Get Nodes from leader
$ docker node ls
# To prevend single point of failure promote all nodes
# now if leader going down other nodes can be leader
$ docker node promote $NodeName