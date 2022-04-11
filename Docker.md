## Purpose: This will hold the fancy commands/info/thoughts and such I discover about docker (From Research)

---
https://nodejs.org/en/docs/guides/nodejs-docker-webapp/


Basic Docker File for node.js
```
FROM node:16

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

RUN npm install
# If you are building your code for production
# RUN npm ci --only=production

# Bundle app source
COPY . .

EXPOSE 8080 # THIS EXPOSES THE WEB SERVER
CMD [ "node", "server.js" ]
```
Basic Build with Docker:

```
docker build . -t <your username>/node-web-app
```

Check Docker Images

```
$ docker images

# Example
REPOSITORY                      TAG        ID              CREATED
node                            16         3b66eb585643    5 days ago
<your username>/node-web-app    latest     d64d3505b0d2    1 minute ago
```

**Run the Docker Image (so Build and Run are Different)**

```
docker run -p 49160:8080 -d <your username>/node-web-app
```

**Print out Info of App:**
```
# Get container ID
$ docker ps

# Print app output
$ docker logs <container id>
```

**Enter a Docker Container**

```
# Enter the container
$ docker exec -it <container id> /bin/bash
```


## Here is a good way to test your web app (just grab the header for the 200, this should be enough):

```
$ curl -i localhost:49160

HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: text/html; charset=utf-8
Content-Length: 12
ETag: W/"c-M6tWOb/Y57lesdjQuHeB1P/qTV0"
Date: Mon, 13 Nov 2017 20:53:59 GMT
Connection: keep-alive

Hello world
```

---

## Fancy commands to lookup:

https://www.youtube.com/watch?v=DP-2f-fIAPA
https://www.youtube.com/watch?v=fqRr2_nMQ-s

- docker-machine ip default
- docker run -d -P --name web nginx
- docker stop web
- docker run -d -P -v $Home/mysite:/usr/share/nginx/html \
  - -> So this looks like it is ref a docker file that can be changed... so I will need to make a file that I user docker to interface with...
- docker run -d --name course -p 7777:80 -v ${PWD}:/usr/local/apache2/htdocs/ http:2.4
  -v = Storage assign ment {left is os, right is container}
- **Apache has a http container on dockerhub!!!**
- docker stats --> container resorce usage

- docker images 

https://www.youtube.com/watch?v=Tm0Q5zr3FL4

- Swarm manages daemon environments, so you can backup, fix, and scale, rollback all daemon. Thus swarm = docker on many machines or clusters able to fix itself (group of containers with same image).

- Docker deamon, registery, and client

- Manager Node --> controls --> worker nodes
- Worker Node --> Application
 
- **Api can be used to control from 3rd party solutions**

```
sudo docker swarm join --token {TOKEN} {IP:PORT}

sudo docker swarm leave --force
```

https://hub.docker.com/_/httpd

--> How to modify... so it looks like you can change the docker config file, thus I can download this and then use the docker config to do my custom config?


## Custom Container Research

https://docs.docker.com/develop/develop-images/baseimages/

```docker
FROM scratch
COPY hello /
CMD ["/hello"]

```

```docker
FROM mcr.microsoft.com/windows/nanoserver:ltsc2022
COPY hello.txt C:
CMD ["cmd", "/C", "type C:\\hello.txt"]
```

--> So you can run commands in a docker file, cool, this probs can be used to insert certs when a new image is built

```
docker build --tag hello .
```
--> So you have to build an image 

---
# Tutorials

---
## Implement TLS in custom dir on apache and Generate Certs

https://hub.docker.com/_/httpd

https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-apache-in-ubuntu-16-04

https://unix.stackexchange.com/questions/104171/create-ssl-certificate-non-interactively

Build Key (no user input)

```
openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 -subj "/C=US/ST=Hello/L=World/O=World/CN=www.helloworld.com" -keyout out.key -out out.cert
```

Change Apache Config for different dir to store key

```
# So each build of apache has a slight diff cert dir, thus for Dockerhub's httpd image, the config is located at:

/usr/local/apache2/conf/extra/httpd-ssl.conf

# Change Lines to what you want

SSLCertificateFile "/usr/local/apache2/conf/server.crt"
SSLCertificateKeyFile "/usr/local/apache2/conf/server.key"

```

---


# Commands

Enter-Shell

```
# if  you have an os image
docker run -it --name NAME centos:latest

# for other stuff
docker exec -it <container name> /bin/bash
```

Basic Docker File

```
FROM centos:latest

MAINTAINER User

RUN yum -y install httpd

CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]

EXPOSE 80

```

Build From Docker File (Create Image)

Basically find "Dockerfile" and then name it NAME:TAG
```
docker build /test/ -t webserver:version1
```

Start Docker Container

```
docker start {CONTAINER NAME OR ID}
```

Run Docker Image 

```
-d, --detach                         Run container in background and print container ID
-i, --interactive                    Keep STDIN open even if not attached
-t, --tty                            Allocate a pseudo-TTY


docker run -dit -p 3333:80 webserver:version1

# Name it something


docker run -dit --name my-running-app -p 8080:80 my-apache2
```

Remove Container

```
docker rm [OPTIONS] CONTAINER [CONTAINER...]
```

Remove Image

```
docker image rm [OPTIONS] IMAGE [IMAGE...]
```
Remove all containers and custom images

```
docker stop $(docker ps -a | grep webserver | tr -s ' ' | cut -d ' ' -f 1)
docker rm $(docker ps -a | grep webserver | tr -s ' ' | cut -d ' ' -f 1)
docker rmi $(docker images | grep webserver | tr -s ' ' | cut -d ' ' -f 3)
```

**Push to Docker Hub**

```
docker push komquest/webserver:tagname
```

**Apache**

Test Server

```
apachectl configtest
```

Start/Stop Server

```
apachectl stop
apachectl start
```


# Links

- SSH into container

  - https://phase2.github.io/devtools/common-tasks/ssh-into-a-container/

- Reason why "mkdir" doesn't always work in dockerfile
  - https://stackoverflow.com/questions/30741995/cannot-execute-run-mkdir-in-a-dockerfile


# Docker Config Examples

```docker
FROM centos:7
ENV container docker
RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;
VOLUME [ "/sys/fs/cgroup" ]
CMD ["/usr/sbin/init"]

```

```
FROM ubuntu:16.04


RUN apt-get update && apt-get install -y \
curl

FROM openjdk:8-jre-alpine

RUN apk --no-cache add curl


WORKDIR /opt


ADD login.sh /opt


CMD ["chmod 755 login.sh  && ./login.sh"]
```

```
FROM busybox
WORKDIR /app
COPY entrypoint.sh .
COPY more_stuff .
ENTRYPOINT ["/app/entrypoint.sh"]
CMD ["/app/more_stuff/my_app"]
```