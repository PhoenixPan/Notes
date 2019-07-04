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
docker build -t react-docker-app .
```
1. `-t`: specifies the name of the image
2. `.` specifies the build context (e.g. the current folder)
