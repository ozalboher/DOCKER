## RENAME a file:

# To rename a tag of an image, we type 'docker tag imageName:tag newImageName:tag'. And if instead of writing a number as a tag, we write 'imageName:latest', we can just write 'imageName' and it will be the same as 'imageName:latest'.

## ENTER EXEC mode:
# To run and expose an interactive session with a container i can use the command:
```sh
docker run  -it imageName:tag
```
# The command below is used to enter the container. The -it flag is used to enter the container in interactive mode. The /bin/sh is the shell that we will use to enter the container. So if we want to install a package in the container, we can enter the container and install the package like this:
```sh
docker container exec -it containerID /bin/sh
```
# When in EXEC mode in order to install a package in the container, we type 'apk add package'. So if we want to install python in the container, we can install it like this:
```sh
apk add python3
```
# To exit the container, we type 'exit'.
