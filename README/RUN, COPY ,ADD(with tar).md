# To create tar file (a compressed zip of a file/files), write this command:
```sh
tar -czvf random_name.tar.gz -C foldername .
``` 
# FROM command in Dockerfile is used to specify the base image. base image is the image from which you want to build your new image. so, the new image will have everything that the base image has.
```dockerfile
FROM imageName:8 // to use a specific version of the image
FROM imageName:latest // to use the latest version of the image
```
# COPY command in Dockerfile is used to copy files from the host machine to the container. 
```dockerfile
COPY specificFile /path.../.../...       // to copy a specific file 
COPY . /path.../.../...                 // to copy all files from the current directory
```
# With the ADD command we are able to add to our image a tar file (zip) and extract it in the container.
```dockerfile
ADD random_name.tar.gz /path.../.../...  // to add a tar file to the container
```
# RUN command in Dockerfile is used to run commands in the container. if we have a dependency in our application, we can install it using the RUN command.so if my application needs python to run, i can install it like this:
```dockerfile
RUN apk add python3 //S for alpine linux
RUN apt-get install python3 // for debian based systems
```
# apt-get is a package manager for debian based systems. it is used to install, remove, update and upgrade packages.
# apk is a package manager for alpine linux. it is used to install, remove, update and upgrade packages.