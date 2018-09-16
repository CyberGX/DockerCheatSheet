
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