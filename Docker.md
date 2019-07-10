# General process
```
docker pull pjbzz100/project-portal
docker run -d -p 3000:80 pjbzz100/project-portal
docker commit [CONTAINER] pjbzz100/project-portal
docker push pjbzz100/project-portal
```

# Step breakdown
## 1. Pull the image
```
docker pull pjbzz100/project-portal
```

## 2. Run the image and create a container 
```
docker run -d -p 3000:80 pjbzz100/project-portal
```
1. `-d`: runs container in background and print container ID
2. `-it`: creates an interactive bash shell in the container
3. `--rm`: removes the container and volumes after the container exits
4. `-v %cd%:/src`: mounts the code into the container at "/src"

## 3. Build an image
```
docker build -t project-portal .
```
1. `-t`: specifies the name of the image
2. `.` specifies the build context (e.g. the current folder)

## 4. Tag an image to be ready to push to the repo

```
docker tag [LOCAL IMAGE ID] [USERNAME]/[REPO]
docker tag 423d4931369f pjbzz100/project-portal
```

## 5. Push the new image to the repo
```
docker push pjbzz100/project-portal
```

## Clean up 
### All
Clean up unused resource
```
docker system prune
```
Include stopped resources
```
docker system prune -a
```

### Images
Remove single image
```
docker images -a
docker rmi [IMAGE ID/NAME]
```
Remove unused images
```
docker images purge
```

### Containers
Remove single container
```
docker ps -a
docker rm [CONTAINER ID/NAME]
```
