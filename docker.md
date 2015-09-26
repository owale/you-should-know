**Imagine that our Linux kernel is layer zero. Whenever we run a Docker image, a layer is put on top of our kernel layer. This image, layer one, is a read-only image and cannot be changed or cannot hold a state. A Docker image can build on top of another Docker image that builds on top of another Docker image and so on. The first image layer is called a base image, and all other layers except the last image layer are called parent images. They inherit all the properties and settings of their parent images and add their own configuration in the Dockerfile**

**When we have a container running, all the changes we make to its filesystem are permanent between start and stop. Remember that changes made to the container's filesystem are not written to the underlying Docker image**


**Dockerfile :** _This is the image description that runs when an image is being created._

```bash
# pull mysql image if not existed locally
# run it with name some-mysql with enviroment varibale MYSQL_ROOT_PASSWORD
docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=mysecretpassword -d mysql
```

```bash
#run conatiner wih image wordpress and name it some-wordpress
#--link parameter exposes the some-mysql containers' environment variables, interface, and exposed ports via the environment variables injected to the some-wordpress container
#-p expose port 80 to outside,it will mapped to host port and will accessed as host_port:80
docker run --name some-wordpress --link some-mysql:mysql –p 80 -d wordpress
```

**The Docker image can be seen as a read-only template for containers, specifying what's supposed to be installed, copied, configured, and exposed when a container is started.**

**Dockerfile sample :**
```docker
  FROM busybox:latest
  MAINTAINER Oskar Hane <oh@oskarhane.com>
  RUN mkdir /var/lib/mysql && mkdir /var/www/html
  VOLUME ["/var/lib/mysql", "/var/www/html"]
```
to build this Dockerfile
```bash
  #will create dockerimage with name/tag data-test from Dockerfile in current directory
  docker build -t data-test
```
