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
 # IMPORTANT TO REMEMBER: RUN command is used when the image is being BUILT. CMD and ENTRYPOINT commands are used when the container STARTS. ENTRYPOINT command is used to specify the main command that will run in the container. CMD command is used to specify the arguments that will be passed to the main command.