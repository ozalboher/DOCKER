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