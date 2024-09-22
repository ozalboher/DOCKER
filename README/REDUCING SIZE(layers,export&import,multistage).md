## This section will cover 4 of the methods we can use in order to reduce the image/container size: 
## 1. LAYERS: Understading that the way docker works is by layering the steps and commands that are described in the dockerfile, so if we can combine the layers into 1 layer it reduces space.
## 2. EXPORT the container to a tar file then import it back as a new image that would be lighter in space.  
## 3. Using a lighter base-image like ALPINE.  
## 4. MULTISTAGE - a process in which one image is being built and all of its artifacts would then transfer to another image that would now use its data as a new image so we would only will be using the latter, saving space.


## VIEWING all LAYERS via HISTORY:

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
# Diffrences between docker export and docker save: Docker export is exporting the container as a snapshot and docker save will essentially do a full backup of the image. So if we would want to save the history of the image, we would use docker save. If we would want to save some space, we would use docker export.

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

# Explanation: export.tar: This is the tar file that contains the exported container. docker import: Imports the exported container (from the tar file) into a new Docker image. imageName:tag: The tag you want to assign to the imported image. Key Notes: If the tar file was created using docker export, it will contain the filesystem of the container but not any Docker metadata (such as layers, history, or tags).The resulting image will be based on the filesystem from the tar file, and you'll need to reconfigure the image (e.g., CMD, ENTRYPOINT, etc.) if needed.

# To reduce image size even further we might consider using alpine as a base image. Alpine is a lightweight Linux distribution that is commonly used in Docker images. It is known for its small size and minimalistic design. Just remember that I would need to write "apk update" instead of the ubuntu image where I would write "apt-get update". and I would need to write "apk add package" instead of "apt-get install package". And with alpine I would no longer need to confirm installation of packages with "y" like I would need to do with ubuntu.

# Multistage IMAGE: Multistage builds are a feature of Docker that allow you to create smaller and more efficient Docker images. They work by using multiple stages in the build process, each with its own set of instructions. This allows you to build your application in one stage and then copy only the necessary files to the final image in another stage. This can help reduce the size of your Docker images and improve build times. So if I would want to build a python application, I would need to install python and then install the application. But I would not need python to run the application. So I would use a multistage build to build the application and then copy the application to a new image that would only have the application. So the Dockerfile would look like this:
```dockerfile
FROM python:3.8 as builder
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .

FROM python:3.8-slim
COPY --from=builder /app /app
WORKDIR /app
CMD ["python", "app.py"]
```
# The first stage is called the builder stage. In this stage, we install the dependencies for the application and copy the application code. The second stage is called the final stage. In this stage, we copy the application code from the builder stage and set the working directory to /app. We then run the application using the CMD command. The --from=builder flag is used to copy files from the builder stage to the final stage. This allows us to create a smaller and more efficient Docker image by only including the necessary files in the final image.