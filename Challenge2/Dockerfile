FROM fedora:latest 

# fedora is the base image for this dockerfile
# dnf is a command to use the package manager for fedora
# -y is to accept the installation
# install httpd is to install the apache server on the fedora image

RUN dnf -y install httpd 


# copy the index.html file from the local directory to the docker container
# /var/www/html is the default directory for the apache server to host the webpages
# in this case we are using a tar file to copy the index.html file to the container
ADD webserver.tar.gz /var/www/html/

EXPOSE 80 

# start the apache server
CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]
# the "/usr/sbin/httpd" is the path to the apache server executable file in the fedora image
# the "-D" is to run the apache server in the foreground
# the "FOREGROUND" is to run the apache server in the foreground

# Or with ENTRYPOINT like this:
# CMD ["-D", "FOREGROUND"]
# ENTRYPOINT ["/usr/sbin/httpd"]

# The CMD instruction has three forms:
# CMD ["executable","param1","param2"] (exec form, this is the preferred form)
# CMD ["param1","param2"] (as default parameters to ENTRYPOINT)
# CMD command param1 param2 (shell form)

# The ENTRYPOINT instruction has two forms:
# ENTRYPOINT ["executable", "param1", "param2"] (exec form, preferred)
# ENTRYPOINT command param1 param2 (shell form)

# Build the image using the following command:
# docker build -t challenge:2 . 

# Run the container using the following command:
# docker run -d -p 8080:80 challenge:2

# Access the website using the following URL:
# http://localhost:8080/index.html

# The website will display the following message:
# "Hello World! This is Challenge 2"

# The website is hosted on the apache server running on the docker container.
# This is just a basic concept and will not be used.