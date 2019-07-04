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
docker run -d -p 3000:80 pjbzz100/project-portal --rm
```
Flag:
1. `--rm`: removes the container and volumes after the container exits
2. `-v %cd%:/src`: mounts the code into the container at "/src"
