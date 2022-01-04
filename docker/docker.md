##Description

This is a collection of commands for configuring and hardening a Docker instance.

##list docker images
```
docker images
```
##run image
```
docker -run -it --rm <repository> /bin/bash
```
##show running images
```
docker ps -a
```
##build
```
docker build . -t <imageid>
```

===========================================================================

##Running as unprivileged user
```
##add to Dockerfile

RUN groupadd -r <user> && useradd -r -g <usergroup> <user>

ENV HOME /home/<user>

##build

##run as unpriv <user>

docker run -u <user> -it --rm <repository> /bin/bash
```
===========================================================================

##Restrict running in privileged mode
```
##run (as <user>)

docker run -u <user> -it --rm --security-opt=no-new-privileges <imageid> /bin/bash
```
===========================================================================

##Block access to root account
```
##add to Dockerfile

RUN chsh -s /usr/sbin/nologin root

##build
```
===========================================================================

##Specify kernel level capabilities
[https://man7.org/linux/man-pages/man7/capabilities.7.html](https://man7.org/linux/man-pages/man7/capabilities.7.html)
```
##run with specified capabilities (drop all, add network-related operations with NET_ADMIN)

docker run --cap-drop all --cap-add NET_ADMIN -it --rm <user> <imageid> /bin/bash
```
===========================================================================

##Prevent container from writing to filesystem
```
##run as read-only(as <user>)
docker run --read-only -u <user> -it --rm

##run with temporary writeable filesystem(as <user>)
docker run --read-only --tempfs /opt -u <user> -it --rm
```
===========================================================================

##Intercontainer isolation
```
##check docker network
docker network ls

##inspect network (<networkname)
docker network inspect <networkname>

##create custom network and disable InterContainerCommunications
docker network create --driver bridge -o "com.docker.network.bridge.enable_icc="false"" <networkname>

##run with custom network <networkname>
docker run -it --rm --network <networkname> /bin/bash
```
===========================================================================

##Docker auditing with docker-bench-security

clone repo
```
git clone [https://github.com/docker/docker-bench-security.git](https://github.com/docker/docker-bench-security.git)
```
run
```
cd docker-bench-security && ./docker-bench-security.sh
```

[https://hackersploit.org/docker-security-course/](https://hackersploit.org/docker-security-course/)

[Docker Security Essentials | How To Secure Docker Containers](https://www.youtube.com/watch?v=KINjI1tlo2w)

[Docker benchmark security](https://github.com/docker/docker-bench-security.git)