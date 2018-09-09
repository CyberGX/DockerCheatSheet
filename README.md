
### Get Docker status :
```bash
docker info 
```

### Download, Install, Run target image : 
```bash
docker run -itd $ImageName:$Version
```
```
-i : interactive
-t : tty
-d : deattach - run in background
```

### 
Get Docker image list :
```bash
docker image ls | docker images
```

### List containers :
```bash
docker ps -as
```
```
-a : show All Containers
-s : show Containers size
```

### Change container states
```bash
docker start $ContainerID
docker stop $ContainerID
docker attach $ContainerID
```

### Remove containers
```bash
docker rm -f $ContainerID|$ContainerName
docker rm -f $(docker ps -aq) # Remove All Containers
```
```
-f : Force Up Container to remove
```

### Copy file to container :
```bash
docker cp $FilePath $ContainerID|$ContainerName:/root
```
```
-a : copy file with all attributes
```

### Docker Attach :
```bash
docker exec -it $ContainerID|$ContainerName /bin/bash
```
