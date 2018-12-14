# Session 1

## Exercise 1

#### Building a basic image

~~~
# mkdir exercise1
# cd exercise1
~~~

#### Create a file called “Dockerfile” and add the following
~~~
From alpine
RUN apk add openssh
~~~

#### Build the image using the command 
~~~
# sudo docker build -t exercise1 .
~~~

## Exercise 2

#### Building a basic image

~~~
# mkdir exercise2
# cd exercise2
~~~

#### Create a file called “Dockerfile” and add the following
~~~
From alpine
RUN apk add openssh && rm -rfv /lib/apk/db && rm -rfv /etc/apk && rm -rfv /lib/apk/db/lock && rm -rfv /var/cache

~~~

#### Build the image using the command 
~~~
# sudo docker build -t exercise2 .

#sudo dive exercise2
~~~

## Building an apache container

Dockerfile

~~~
FROM centos:latest
USER root

MAINTAINER workshop

# Update image. Line is commented for now to save network bandwidth
#RUN yum update -y
RUN yum install httpd  -y

# Create a test index.html
RUN echo "The Web Server is Running" > /var/www/html/index.html
EXPOSE 80

# Start the service
CMD ["-D", "FOREGROUND"]
ENTRYPOINT ["/usr/sbin/httpd"]
~~~


