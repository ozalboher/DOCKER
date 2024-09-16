# Build image: 
```sh
docker build . -t helloworld:1
```
# Build image when the file name is not Dockerfile, or if it has a different type of file (like a .txt file): 
```sh
docker build . -t helloworld:1 -f filename
# OR TYPE:
docker build --file="filename" . -t helloworld:1   
```
# Build image from github, click on clone repository, copy the link, and type this command:
```sh
docker build paste_the_link -t helloworld:1
```
# Run image: 
```sh
docker run helloworld:1 
```
# Run image with port 8080 exposed to the host: 
```sh
docker run -p 8080:8080 helloworld:1
```
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

# ENTRYPOINT command in Dockerfile is used to specify the command that will run when the container starts. it is used to specify the main command that will run in the container. SO if we have a python application, we can specify the python command that will run the application. Lets say we wanna ping 5 times to google.com, we can specify the command like this:
```dockerfile
ENTRYPOINT ["ping", "-c", "5", "google.com"]
```
or we can specify the command like this:
```dockerfile
CMD ["google.com"]
ENTRYPOINT ["ping", "-c", "5"]
```
# The second method is useful when we want to pass arguments to the command. so if we want to ping 5 times to a different website - lets say apple.com, we can pass the website as an argument to the command. so we can run the container like this:
```sh 
docker run imageName:tag apple.com
```
* Side note: RUN command is used when the image is being BUILT.
 CMD and ENTRYPOINT commands are used when the container STARTS.

 ENTRYPOINT command is used to specify the main command that will run in the container. CMD command is used to specify the arguments that will be passed to the main command.

# Typing -d in terminal means that the container will run in the background.(detached mode). Typing -P in terminal means a random port will be assigned to the container, when the container runs.
```sh
docker run -d -P myweb:4 
```
# Typing 'docker container ls' in terminal will show the running containers. So if we want to see the random port that was assigned to the container, we do that and then we can access the web application by typing localhost:randomPortNumber in the browser. we will get "it works!" message (from APACHE).

# To stop a container, we type 'docker container stop containerID'. To remove a container, we type 'docker container rm containerID'. To remove all containers, we type 'docker container rm $(docker container ls -aq)'. To remove an image, we type 'docker image rm imageName:tag'. To remove all images, we type 'docker image rm $(docker image ls -aq)'.


# EXPOSE command in Dockerfile is used to specify the port that the container will listen to. so if we have a web application that listens to port 80, we can specify the port like this:
```dockerfile
EXPOSE 80
```

# Adding a -v and a path to the run command, will "listen" to changes in the website or whatever we are working on. so each edit would be reflected instantly in the running container. and I would not need to rebuild the image each time I make a change. so the command would look like this:
```sh
docker container run -p 8080:80 -v /path.../.../...:/usr/local/apache2/htdocs imageName:tag
```
# the destination directory in the container is specified after the colon. so in this case, the data will be stored in /var/www/html in the container.or if we use for example the apache image it would be stored in /user/local/apache2/htdocs.

# To rename a tag of an image, we type 'docker tag imageName:tag newImageName:tag'. And if instead of writing a number as a tag, we write 'imageName:latest', we can just write 'imageName' and it will be the same as 'imageName:latest'.

# The command below is used to enter the container. The -it flag is used to enter the container in interactive mode. The /bin/sh is the shell that we will use to enter the container. So if we want to install a package in the container, we can enter the container and install the package like this:
```sh
docker container exec -it containerID /bin/sh
```
# To install a package in the container, we type 'apk add package'. So if we want to install python in the container, we can install it like this:
```sh
apk add python3
```
# To exit the container, we type 'exit'.

# Docker repositories are used to store images. So if we want to push an image to a repository, we can do that like this:
```sh
docker image push ozalboher/mywebserver:latest
```
# To pull an image from a repository, we can do that like this:
```sh
docker image pull ozalboher/mywebserver:latest
```
# To login to a repository, we can do that like this:
```sh
docker login
```

# Each docker instruction command creates a new Layer with a unique ID. Only the top layer would be writeable and the rest would be read-only. The top layer is called the container layer. We can see the layers of an image by typing:
```sh
docker image history imageName:tag
```
# REDUCING LAYERS: Because Docker is caching each command as a layer, it can save up space and time. So if I would build a new image that uses some command of a previous image it would not need to download and save it sepereately. It would just use the cached layer. But also remember that each layer takes up a chunk of space, so I can also combine my commands as one and docker would treat it as only one layer. So if I would install python and then install a package, I can combine it like this:
```dockerfile
RUN apk add python3 && \ 
    apk add package && \
    apk add package2
```
# IMPORT & EXPORT: To export an image to a tar file, we can do that like this:
```sh
docker export 36975d25bf49 > ./export.tar
```
# The number afrer the export is the container ID. to view a container ID, we can type 'docker container ls -a'. the -a flag is used to show all containers, not just the running ones.(we would obviously need to have a container running at some point to view the ID).Exporting a container as a tarball file would mean the history of the image would be lost(Unlike exporting Image as a tarball that would also keep the history) But this would be useful to do if we would want to save some extra space.
# To import an image from a tar file, we can do that like this:
```sh
cat export.tar | docker import - imageName:tag
```
# 'cat' command Reads the contents of the export.tar file and outputs it to the standard output  and we pipe it to the docker import command with the '|' symbol and the '-' symbol is used to specify the standard input.