1. Which is the default data directory for Docker?
-    `/var/lib/docker`

2. What component of the docker architecture is responsible for managing containers on Linux on version 1.15 of Docker Engine?
-    LibContainer

3. Which component is responsible for keeping the containers alive when the Docker Daemon goes down?

-    LibContainer
-    Runc
-    Containerd
-    Containerd-Shim 
Answer: d

4. What is the command to start docker daemon manually?

-    `docker`
-    `dockerd`
-    `docker-engine`
-    `docker --start-engine`
Answer: b

5. On what interfaces are the docker daemon made available by default?

-    TCP socket
-    UDP socket
-    Unix Socket
-    192.168.1.10
c

6. What is the port conventionally used to configure un-encrypted traffic on TCP?

-    2345
-    2346
-    2375
-    2376
c

7. What file is used to configure the docker daemon?

-    `/var/lib/docker/docker.conf`
-    `/var/lib/docker/daemon.json`
-    `/etc/docker/daemon.json`
-    `/etc/docker/daemon.conf`
c

8. What flags are used to configure encryption on docker daemon?

-    tlsverify, tlscert, tlskey
-    tlsverify, key, cert
-    key, cert, tls
-    host, key, cert, tls
a

9. What is the default network driver used when a container is created?

-    overlay
-    bridge
-    none
-    host
b

10. What is the command used to list the running containers on the Docker Host?

-    `docker container ls`
-    `docker container start`
-    `docker container stop`
-    None of the above
a

11. Which of the below commands create a container with `nginx` image and name `nginx`?

-    `docker container create nginx --name nginx`
-    `docker container --name nginx nginx`
-    `docker container run nginx`
-    `docker container create --name ngi`
d

12. How to list all running and stopped containers and their status?

-    `docker container ls`
-    `docker container ls -a`
-    `docker container ls -aq`
-    `docker container ls -q`
b

13. How to start a stopped Container?

-    `docker container rm nginx`
-    `docker container start nginx`
-    `docker container create nginx`
-    `docker container run nginxo`
b


14. How do I get only the IDs of running containers?

-    `docker container ls`
-    `docker container ls -a`
-    `docker container ls -aq`
-    `docker container ls -q`
d

15. What is the option used in docker run command to attach to the terminal of the container in an interactive mode?

-    -f
-    -it
-    -i
-    -t
b

16. What is the command to change the container name “httpd” to “webapp”?

-    `docker container rename httpd webapp`
-    `docker container rename webapp httpd`
-    `docker container replace --name httpd webapp`
-    `docker container create --name webapp httpd`
a

17. What is the command to run a “nginx” container in a detached mode with name “webapp”?

-    `docker container run -it --name webapp nginx`
-    `docker container run -it --name nginx webapp`
-    `docker container run -d --name webapp nginx`
-    `docker container run -d --name nginx webapp`
c

18. You cannot start a killed container.

-    True
-    False
b

19. Delete the stopped container named “webapp”.

-    `docker container delete webapp`
-    `docker container remove webapp`
-    `docker container kill webapp`
-    `docker container rm webapp`
d


20. Run a container called `webapp` with image `nginx`, and in an interactive mode.

-    `docker container run -it nginx`
-    `docker container run -it nginx --name webapp`
-    `docker container run nginx`
-    `docker container run -it --name webapp nginx`
d

21. Which combination of keys are used to escape from the shell and keep the container `webapp` running?

-    Ctrl+c
-    Ctrl+p+q
-    exit, Ctrl+p+q
-    Ctrl+c, exit
b

22. Which combination of keys are used to exit from the shell and stop the container `webapp`?

-    Ctrl+c
-    Ctrl+p+q
-    Ctrl+p
-    Ctrl+z
a

23. You have a running container and want to execute a command inside it. Which command will you use?

-    execute
-    run
-    start
-    exec
d. 

24. We deployed a container called `webapp`. Inspect this container to get the IPPrefixLen.

-    `docker container inspect webapp | grep IPPrefixLen`
-    `docker container top webapp | grep IPPrefixLen`
-    `docker container run webapp | grep IPPrefixLen`
-    `docker container logs webapp | grep IPPrefixLen`
a

25. We have deployed some containers. What command is used to get the container with the highest memory?

-    `docker container stats`
-    `docker container status`
-    `docker container top`
-    `docker container ls`
a

26. How to display the running processes inside the container?

-    `docker container top container-name`
-    `docker container stats container-name`
-    `docker ps container-name`
-    `docker container logs container-name`
a

27. You have a `webapp` container and image `httpd`.  
Inspect the logs of the `webapp` container.  
Which command is used to get the stream logs of the `webapp` container so that you can view the logs live?

-    `docker container log webapp`
-    `docker container log -f webapp`
-    `docker container logs webapp`
-    `docker container logs -f webapp`
d

28. Which command returns only new and/or live events?

-    `docker system info`
-    `docker container events`
-    `docker container events -f`
-    `docker system events`
d

29. Which command returns events since the past 30 minutes?

-    `docker system events since 30m`
-    `docker system events --since 30m`
-    `docker container events --since 30m`
-    `docker container events since 30`
b

30. Which command is used to get the events of the container named `"webapp"`? (This one is for you to read the documentation)

-    `docker system events since 10m`
-    `docker system events --filter 'container=webapp'`
-    `docker system events --filter 'image=webapp`
b


31. Run a container named `webapp` with `nginx` image in detached mode. Select the right answer.

-    `docker container run --detach --name=webapp nginx`
-    `docker container run --detach --name=nginx webapp`
-    `docker container create -d --name=nginx webapp`
-    `docker container create -d nginx`
a

32. Stop the container named `"nginx"`.

-    `docker container halt nginx`
-    `docker container stop nginx`
-    `docker container rm nginx`
-    `docker container pause nginx`
b

33. How do you list running & stopped containers?

-    `docker container ls -a`
-    `docker container ls -q`
-    `docker container ls`
-    `docker container ls -q, docker co`
a

34. Delete the “webapp” Container. Select the right answer.

-    `docker container delete webapp`
-    `docker container remove webapp`
-    `docker container kill webapp`
-    `docker container rm webapp`
d

35. Stop all running containers on the host. Select the right answer.

-    `docker container stop $(docker container ls -a)`
-    `docker container rm $(docker container ls -q)`
-    `docker container stop $(docker container ls -q)`
-    `docker container stop --all`

36. Stop all running containers on the host. Select the right answer.

-    `docker container stop $(docker container ls -a)`
-    `docker container rm $(docker container ls -q)`
-    `docker container stop $(docker container ls -q)`
-    `docker container stop --all`
c

37.  Delete all running and stopped containers on the host. (Explore the documentation to identify an option to force remove running containers)

-    `docker container stop $(docker container ls -q)`
-    `docker container rm $(docker container ls -q)`
-    `docker container stop $(docker container ps -q)`
-    `docker container rm -f $(docker container ls -aq)`
d


38. Which command is used to delete the stopped containers?

-    `docker container remove $(docker container ls -aq)`
-    `docker container rm $(docker container ls -aq)`
-    `docker container prune`
-    `docker container rm --all`
c

39. What is the command to pause a running container?

-    `docker container pause`
-    `docker container --pause`
-    `docker container halt`
-    `docker container SIGSTOP`
a


40. What are the signals sent to a running container when the docker container stop command is executed?

-    SIGSTOP followed by SIGKILL
-    SIGTERM followed by SIGKILL
-    SIGKILL followed by SIGTERM
-    SIGKILL followed by SIGSTOP
b

41. Run a container with image `nginx`, name `nginx` and hostname `webapp`.

-    `docker container run -d --name webapp --hostname=webapp nginx`
-    `docker container run -d --name nginx webapp`
-    `docker container run -d --name nginx --hostname=webapp nginx`
-    `docker container run -d --name webapp nginx`
c

42. What is the hostname set on the container when the following command is run :  
`docker container run -d --name webapp httpd`

-    webapp
-    apache
-    httpd
-    containers unique id
d

43. What is the default restart policy?

-    unless-stopped
-    on-failure
-    no
-    always
c

44. Which policy would restart the containers even after the docker daemon is restarted?

-    unless-stopped
-    on-failure
-    always
-    always, unless-stopped
c

45. Which policy is used to restart a container unless it is explicitly stopped or Docker is restarted.

-    unless-stopped
-    on-failure
-    no
-    always
a

46. Which command can be used to check the restart policy of `webapp` container?

-    `docker container inspect webapp`
-    `docker container info webapp`
-    `docker container check webapp`
-    None of the above
a

47. Restart container unless it is explicitly stopped or Docker is restarted.

-    unless-stopped
-    on-failure
-    no
-    always
a

48. Which command should be used to update the `httpd` container with the always policy?

-    `docker container update --restart always httpd`
-    `docker container unpause --restart always httpd`
-    `docker container upgrade --restart always httpd`
a

49. Which command should be used to update all the running containers with unless-stopped policy?

-    `docker container upgrade --restart unless-stopped $(docker container ls -q)`
-    `docker container update --restart unless-stopped $(docker container ls -q)`
-    `docker container upgrade --restart unless-stopped $(docker container ls -aq)`
-    `docker container update --restart unless-stopped $(docker container ls -aq)`
b

50. Which option is used to reduce container downtime due to daemon crashes, planned outages, or upgrades?

-    Restart Policy
-    Swarm
-    Live Restore
-    Restart Policy, Live
c

51. What is the path file which is used to add the live restore?

-    `/etc/docker/daemon.json`
-    `/var/lib/docker/daemon.json`
-    `/var/log/docker/daemon.json`
-    `/var/lib/docker`
a

52. How to enable the live restore setting to keep containers alive when the daemon becomes unavailable?

-    `echo '{"live-restore": true}' >> /etc/docker/daemon.json`
-    `echo '{"live-restore": true}' >> /var/lib/docker/daemon.json`
-    `echo '{true: "live-restore"}' >> /etc/docker/daemon.json`
-    `echo '{true: "live-restore"}' >> /var/lib/docker/daemon.json`
a

53. **Which of the below commands may be used to copy a file `/web.conf` from a container named `webapp` with id `89683681` to the `/tmp` directory on the host?

-    `docker container cp /tmp/web.conf webapp:/etc/web.conf`
-    `docker container cp webapp:/web.conf /webapp`
-    `docker container cp 89683681:/web.conf /tmp/`
-    `docker container cp webapp:/web.conf /tmp/`
d

54. Copy the `/etc/nginx` directory from the `webapp` container to the docker host under `/tmp/`.

-    `docker container copy webapp:/etc/nginx /tmp/`
-    `docker container cp webapp:/etc/nginx /tmp/`
-    `docker container copy /tmp/ webapp:/etc/nginx`
-    `docker container cp /tmp/ webapp:/etc/nginx`
b

55. What is the command to copy the file `/root/myfile.txt` from the host to `/root/` of the `webapp` container?

-    `docker container copy /root/myfile.txt webapp:/root/`
-    `docker container cp /root/myfile.txt webapp:/root/`
-    `docker container copy webapp:/root/ /root/myfile.txt`
-    `docker container cp webapp:/root/ /root/myfile.t`
b

56. We can copy a file from a stopped container.

-    True
-    False
a

57. Data inside the container is persistent.

-    True
-    False
b

58. You can run multiple instances of the same application on the docker host.

-    True
-    False
a

59. You can map to the same port on the Docker host more than once.

-    True
-    False
b

60. Which option could be used to expose a webapp container to the outside world?

-    -p
-    -P
-    –publish
-    –expose
a and b

61. Map TCP port 80 in the container to port 8080 on the Docker host for connections to host IP 192.168.1.10 . Select the all right answers

-    -p 192.168.1.10:8080:80
-    -p 192.168.1.10:80:8080
-    -p 192.168.1.10:8080:80/tcp
-    -p 192.168.1.10:8080:8080
a and c

62. Unless specified otherwise, docker publishes the exposed port on all network interfaces.

-    True
-    False
a

63. Map UDP port 80 in the container to port 8080 on the Docker host.

-    -p 8080:80/udp
-    -p 80:8080/udp
-    -P 8080:80/udp
-    None of the above
a

64.  How does the -P option in the docker container run command know what ports to publish on the container?

-    It identifies the ports listening inside the container using netstat command
-    It uses the ExposedPorts field set on the container or the EXPOSE instruction in the Dockerfile
-    It requires the –expose command line argument
-    It assigns random ports between 32768 and 61000
b

65. How does docker map a port on a container to a port on the host?

-    Using an internal load balancer
-    FirewallD Rules
-    Using an external load balancer
-    IPTables Rules
d

66. What IPTables chains does Docker modify to configure port mapping on a host?

-    INPUT
-    FORWARD
-    DOCKER
-    OUTPUT
c

67. How to check the logs of the docker daemon?

-    `journalctl -u docker.service`
-    `less /var/log/messages`
-    `less /var/log/daemon.log`
-    `/var/log/docker.log`
a,b,c,d

68. Enable the debugging mode. Select the right answer

-    `echo '{"debug": true}' > /etc/docker/daemon.json`
-    `echo '{"debug"}' > /etc/docker/daemon.json`
-    `echo '{"debug": true}' > /var/lib/docker/daemon.json`
-    `echo '{"debug"}' > /var/lib/docker/daemon.json`
a

69. How to check if the docker service is running or not?

-    `docker status`
-    `sudo systemctl status docker`
-    `sudo systemctl docker status`
-    `sudo service status docker`
b

70. Which environment variable will be used to connect a remote docker server?

-    DOCKER_REMOTE
-    DOCKER_HOST
-    DOCKER_CONFIG
-    None of the above
b

71. What may be the cause of this error: “unable to configure the Docker daemon with file /etc/docker/daemon.json: the following directives are specified both as a flag and in the configuration file: tls: (from flag: true, from file: false)”?

-    The tls flag is set to true in daemon.json file and false in the command line
-    The tls flag is set to false in daemon.json file and true in the command line
-    The tls flag is not set on the command line
-    The tls flag is not set in the daemon.json file
b

72. What is the default logging driver?

-    json-file
-    syslog
-    journald
-    splunk
a

73. Where is the log of the `webapp` container with id `78373635` on the Docker Host?

-    `/var/lib/docker/containers/78373635/78373635.json`
-    `/var/log/docker/78373635.json`
-    `/etc/docker/78373635.json`
-    `/var/lib/docker/tmp/78373635/78373635.json`
a

74. Which command is used to check the default logging driver?

-    `docker system df`
-    `docker system events`
-    `docker system prune`
-    `docker system info`
d

75. How to change the default logging driver to syslog?

-    `echo '{"log-driver": "syslog"}' > /etc/docker/daemon.json`
-    `echo '{"syslog": "log-driver"}' > /etc/docker/daemon.json`
-    `echo '{"log-driver": "syslog"}' > /var/lib/docker/daemon.json`
-    `echo '{"syslog": "log-driver"}' > /var/lib/docker/daemon.json`
a

76. Run a `webapp` container, and make sure that no logs are configured for this container.

-    `docker run -it --log-driver none webapp`
-    `docker run -it --logging-driver none webapp`
-    `docker run -it webapp`
-    `docker run -it --log none webapp`
a


77. What is the purpose of a private registry?

-    tightly control where your images are being stored
-    fully own your images distribution pipeline
-    integrate image storage and distribution tightly into your in-house development workflow
-    All of the above
d

78. What is the default public registry for docker?

-    Docker Hub
-    Amazon Container Registry
-    Google Container Registry
-    Docker Trusted Registry
a

79. What is the default tag if not specified when building an image with the name `webapp`?

-    none
-    default
-    latest
-    v1
c

80. Run ubuntu container with the trusty tag.

-    `docker run ubuntu`
-    `docker run ubuntu:latest`
-    `docker run ubuntu:trusty`
-    `docker run ubuntu -t trusty`
c

81. Select the right answer. Which command is used to list the local images?

-    `docker image ls`
-    `docker images ls`
-    `docker container image ls`
-    `docker container images ls`  
a

82. List the full length image IDs. (Please explore documentation)

-    `docker image ls --digests`
-    `docker images --digests`
-    `docker images --no-trunc`
-    None of the above
d
```
ubuntu@ip-172-31-10-78:~$ docker image ls -q
b260a49eebf9
27941809078c
bdc66f653f56
bdc66f653f56
d0c8d2556876
8893b8485921
66cd53172aac
66cd53172aac

ubuntu@ip-172-31-10-78:~$ docker image ls
REPOSITORY                                                       TAG       IMAGE ID       CREATED        SIZE
httpd                                                            latest    b260a49eebf9   8 days ago     145MB
ubuntu                                                           latest    27941809078c   2 weeks ago    77.8MB
ec2-sre-cask-test                                                latest    bdc66f653f56   2 weeks ago    967MB
000699977340.dkr.ecr.us-east-2.amazonaws.com/ec2-sre-cask-test   latest    bdc66f653f56   2 weeks ago    967MB
node                                                             14        d0c8d2556876   3 weeks ago    946MB
salrashid123/squidproxy                                          latest    8893b8485921   5 months ago   713MB
gkoenig/simplehttp                                               latest    66cd53172aac   2 years ago    7.43MB
000699977340.dkr.ecr.us-east-2.amazonaws.com/ec2-sre-cask-test   <none>    66cd53172aac   2 years ago    7.43MB
```


83. Display images with a name containing `postgres`, at least 12 stars.

-    `docker find --filter=stars=12 postgres`
-    `docker search --filter=stars=12 postgres`
-    `docker find --limit=12 postgres`
-    `docker search --limit=12 postgres`
b

84. Download `nginx` image from the Google Container Registry hub registry.

-    `docker image pull nginx`
-    `docker image build nginx`
-    `docker image load nginx`
-    `docker pull gcr.io/kodekloud/nginx`
d

85. Display images with a name containing `busybox`, at least 3 stars and are official builds.

-    `docker find --filter is-official=true --filter stars=3 busybox`
-    `docker search --filter is-official=true --filter stars=3 busybox`
-    `docker find --filter is-official=true --limit=3 busybox`
-    `docker search --filter is-official=true --limit=3 busybox`
b

86. What is the command to change the tag of `"httpd:latest"` to `"httpd:v1"` ?

-    `docker container image retag httpd:latest httpd:v1`
-    `docker container image tag httpd:latest httpd:v1`
-    `docker image retag httpd:latest httpd:v1`
-    `docker image tag httpd:latest httpd:v1
d

87. You have an `nginx:v1` image with size 100M. You’ve now created your own version of the image – `nginx:v2` by retagging the first image, what is the total size of both?

-    50M
-    100M
-    150M
-    200M
b

88. Which command should be used to get the total size consumed by all images on a host?

-    `docker image list`
-    `docker image df`
-    `docker system df`
-    `docker system list`
c

89. In the output of the `"docker system df"` command what does the ACTIVE field indicate on the images row?

-    Number of Images currently available on the system
-    Number of Images built on the system
-    Number of Images with containers
-    Number of containers running on the system

90. In the output of the `"docker system df"` command what does the ACTIVE field indicate on the images row?

-    Number of Images currently available on the system
-    Number of Images built on the system
-    Number of Images with containers
-    Number of containers running on the system
c

91. What command might have generated the above output?
```
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
nginx               latest              c39a868aad02        4 days ago          150MB
redis               latest              4760dc956b2d        2 years ago         100MB
ubuntu              trusty              f975c5035748        2 years ago         100MB
webapp              latest              c39a868aad02        4 days ago          150MB
alpine              latest              3fd9065eaf02        2 years ago           5MB
```

-    `docker container ps`
-    `docker ps`
-    `docker image ps`
-    `docker image list`
d

92. What is the total space consumed by images on this system?

-    355 MB
-    505 MB
-    405 MB
-    455 MB
c

93. What image might the `webapp` image be made of?

-    redis
-    ubuntu
-    alpine
-    nginx
d

94. When you run the docker image inspect ubuntu command it gives the error “No such image”. Why is that?

-    Must run the command docker inspect ubuntu/ubuntu
-    Image Ubuntu does not have the latest tag
-    Must authenticate to docker hub first before running this command
-    Must run the command docker image history ubuntu
b


95. What is the user/account and image/repository name for the image company/nginx?

-    image=company, user=nginx
-    image=company, user=company
-    image=nginx, user=nginx
-    image=nginx, user=company
- d

96. Choose the right command to pull `ubuntu` image from a private registry at gcr.io

-    `docker pull ubuntu`
-    `docker pull kk/ubuntu`
-    `docker pull gcr.io/kk/ubuntu`
-    All of the above
c


97. Which command is used to authenticate with azr.com registry which listens on port 5000?

-    `docker auth azr.com:5000`
-    `docker login azr.com:5000`
b

98. You are required to store a copy of the official alpine image in your company’s internal docker registry. What would be your approach?

-    Create a Dockerfile similar to the official image and build an image
-    Pull the official image, tag it with the address of the internal docker registry and push to the internal docker registry
b


99. When you log in to a registry, the command stores credentials in … (Please explore the documentation pages for this)

-    `$HOME/.docker/config.json`
-    `/etc/docker/.docker/config.json`
-    `/var/lib/docker/.docker/config.json`
-    `/var/lib/docker/containers/.docker/config.json`
a


100. While trying to delete image postgres, you got an error “conflict: unable to remove repository reference “postgres” (must force) – container 1a56b95e073c is using its referenced image adf2b126dda8″. What may be the cause of this error?

-    A container is using this image
-    Must use force option to delete an image
-    Another image is using layers from this image
-    The image was built locally on this host 
a

101. Which command is used to remove `webapp:v1` image locally?

-    `docker image rm webapp`
-    `docker image rm webapp:v1`
-    `docker image remove webapp:v1`
-    `docker image del webapp:v1`
b and c


102. Remove all unused images on the Docker host

-    `docker image prune -a`
-    `docker image rm -a`
-    `docker image delete -a`
-    None of the above
a


103. Display all layers of `httpd` image along with the size on each layer.

-    `docker image layers httpd`
-    `docker image history httpd`
-    `docker image inspect httpd`
-    `docker images history httpd`
b


104. Which command can be used to get the ExposedPorts of a `webapp` image?

-    `docker container ls`
-    `docker image inspect webapp`
-    `docker container inspect webapp`
-    `docker image ls`
b

105. How to get the Os field alone of the `httpd` image?

-    `docker image inspect httpd -f '{{.Os}}'`
-    `docker image ls | grep Os`
-    `docker image history | grep Os`
-    `docker image inspect httpd -f '{{.OperatingSystem}}'`
a


106. Which subcommand will be used to get more info about images?

-    inspect
-    load
-    import
-    ls
a


107. Print the value of ‘Architecture’ and ‘Os’ for a `'webapp'` image.

-    `docker image inspect webapp -f '{{.Os}}' -f '{{.Architecture}}'`
-    `docker image inspect webapp -f '{{.Os}} {{.Architecture}}'`
-    `docker image inspect webapp -f '{{.Os}}', -f '{{.Architecture}}'`
-    `docker image inspect webapp -f '{{.Os .Architecture}}'`
b


108. Which command can be used to get a backup of image `webapp`?

-    `docker image backup webapp -o webapp.tar`
-    `docker image save webapp -o webapp.tar`
-    `docker container save webapp -o webapp.tar`
-    `docker container backup webapp -o webapp.tar`
b


109. A tarfile – nginx.tar – has been created using the docker image save command. Which command can be used to extract it into your docker host.

-    `docker image import -i nginx.tar`
-    `docker image restore -i nginx.tar`
-    `docker container restore -i nginx.tar`
-    `docker image load -i nginx.tar`
d


110. A government facility runs a secure data center with no internet connectivity. A new application requires access to docker images hosted on docker hub. What is the best approach to solve this?

-    Get the Dockerfile of the image and build a local version from within the restricted environment.
-    Establish a secure link between the host in the restricted environment and docker hub
-    Pull docker images from a host with access to docker hub, convert to a tarball using docker image save command, and copy to the restricted environment and extract the tarball
-    Pull docker images from a host with access to docker hub, then push to a registry hosted within the restricted environment.
c.

111. You have created a `nginx` container and customized it to create your own webpage. How can you create an image out of it to share with others?

-    `docker image save`
-    `docker image export`
-    `docker export`
-    You can only create an image using a Dockerfile
c


112. How do you restore an image created from the `docker export` command?

-    `docker container import`
-    `docker image import`
-    `docker image load`
-    `docker image restore`
b


113. The “export” command works with Docker images.

-    True
-    False
b

114. Export `webapp` container’s filesystem as a tar archive. Select the right answer

-    `docker export webapp mywebapp.tar`
-    `docker image export --output="mywebapp.tar" webapp`
-    `docker image save -i mywebapp.tar`
-    `docker container export webapp > mywebapp.tar`
d

115. Which of the following commands is used to list the docker images on the Docker Host?

-    `docker images`
-    `docker image ls`
-    `docker image get`
-    `docker ls image`
a and b

116. Which of the following commands used to match all images with the com.example.version label?

-    `docker images --label="com.example.version"`
-    `docker images --filter "com.example.version"`
-    `docker images --filter "label=com.example.version"`
-    `docker images --format "label=com.example.version"`
c


117. Which of the following is not an instruction supported in the Dockerfile? Select the all right answers.

-    EXPOSE
-    ADD
-    WORKDIR
-    EXEC
d

118. The … is a text document that contains all the commands a user could call on the command line to assemble an image.

-    Dockerfile
-    Docker Compose
-    .dockerignore
-    build context
a

119. Which method can be used to build an image using existing containers?

-    `docker commit`
-    `docker export`
-    `docker save`
-    `docker load`
a and b

120. The container being committed and its processes will be paused while the image is committed.

-    True
-    False
a

121. We have a running container named `webapp` with the `nginx` image. We added a custom html file to this container. How do we create an image named `mynginx` from this container?

-    `docker container commit webapp mynginx`
-    `docker container commit mynginx webapp`
-    `docker container update webapp mynginx`
-    None of the above
a

122. The docker container commit is the recommended approach for building a custom image.

-    True
-    False
- b
- 
123. You are required to create an image from an existing image. What is the recommended approach?

-    Use docker image export and docker image import command
-    Use docker container export and docker container import command
-    Use docker image save and docker image load command
-    Use docker container commit command
c

124. You are required to create an image from an existing container. What is the recommended approach?

-    Use docker image export and docker image import command
-    Use docker container export and docker container import command
-    Use docker container commit command
-    Use docker container export and docker image import command
d