## -d:

# Typing -d in terminal means that the container will run in the background.(detached mode). Typing -P in terminal means a random port will be assigned to the container, when the container runs.

```sh
docker run -d -P myweb:4
```

## -v:

# Adding a -v and a path to the run command, will "listen" to changes in the website or whatever we are working on. so each edit would be reflected instantly in the running container. and I would not need to rebuild the image each time I make a change. so the command would look like this:

```sh
docker container run -p 8080:80 -v /path.../.../...:/usr/local/apache2/htdocs imageName:tag
```

# the destination directory in the container is specified after the colon. so in this case, the data will be stored in /var/www/html in the container.or if we use for example the apache image it would be stored in /user/local/apache2/htdocs.

## docker container ls / docker image ls:

# Typing 'docker container ls' in terminal will show the running containers. So if we want to see the random port that was assigned to the container, we do that and then we can access the web application by typing localhost:randomPortNumber in the browser. we will get "it works!" message (from APACHE).

## docker stop:

# To stop a container, we type

```sh
 docker container stop containerID
```

# To remove a container, we type

```sh
docker container rm containerID
```

# To remove all containers, we type

```sh
 docker container rm $(docker container ls -aq)
```

To remove an image, we type

```sh
 docker image rm imageName:tag
```

To remove all images, we type

```sh
docker image rm $(docker image ls -aq)
```

## EXPOSE PORT:

# EXPOSE line in Dockerfile is used to specify the port that the container will listen to. so if we have a web application that listens to port 80, we can specify the port like this:

```dockerfile
EXPOSE 80
```
