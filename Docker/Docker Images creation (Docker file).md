#dockerfile
![[Pasted image 20220624061441.png]]


![[Pasted image 20220624061535.png]]


![[Pasted image 20220624061623.png]]

#### Docker File Instructions


| Command      | Descriptions                                          |
| ------------ | ----------------------------------------------------- |
| ``FROM``     | This command is used to check the contiainers running |
| `RUN`        |                                                       |
| `COPY`       |                                                       |
| `ARG`       |                                                       |
| `WORDIR`       |                                                       |
| `ENVIROMENT` |                                                       |
| `EXPOSE`     |                                                       |
| `CMD`        |                                                       |
| `ENTRYPOINT` |                                                       |


- Docker file to add mirror list for centos incase we get error `# [Error: Failed to download metadata for repo 'appstream': Cannot prepare internal mirrorlist: No URLs in mirrorlist](https://stackoverflow.com/questions/70963985/error-failed-to-download-metadata-for-repo-appstream-cannot-prepare-internal)`
	- [Link to refrence doc](https://stackoverflow.com/questions/70963985/error-failed-to-download-metadata-for-repo-appstream-cannot-prepare-internal)
```
FROM centos

RUN cd /etc/yum.repos.d/
RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*

RUN yum -y install java

CMD /bin/bash
```


- Docker file to create apache webserver over centos
	- `Dockerfile`
```
#This Docker file is for HTTPD web server
FROM  centos:7
RUN   yum -y update
RUN yum  install httpd -y
COPY  ./index.html /var/www/html/index.html
EXPOSE 80
CMD ["httpd","-D","FOREGROUND"]
```
Command line output: 
```
ubuntu@ip-172-31-10-78:~/docker/httpd$ sudo cat  Dockerfile
#This Docker file is for HTTPD web server
FROM  centos:7
RUN   yum -y update
RUN yum  install httpd -y
COPY  ./index.html /var/www/html/index.html
EXPOSE 80
CMD ["httpd","-D","FOREGROUND"]

ubuntu@ip-172-31-10-78:~/docker/httpd$ ls
Dockerfile  index.html

ubuntu@ip-172-31-10-78:~/docker/httpd$ docker build -t httpd-test:v4 .
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  3.072kB
Step 1/6 : FROM  centos:7
 ---> eeb6ee3f44bd
Step 2/6 : RUN   yum -y update
 ---> Using cache
 ---> a8d95472aba2
Step 3/6 : RUN yum  install httpd -y
 ---> Using cache
 ---> 7e59a4871442
Step 4/6 : COPY  ./index.html /var/www/html/index.html
 ---> Using cache
 ---> 0819e4bc029b
Step 5/6 : EXPOSE 80
 ---> Using cache
 ---> bca2a4290771
Step 6/6 : CMD ["httpd","-D","FOREGROUND"]
 ---> Running in d903d2fdc498
Removing intermediate container d903d2fdc498
 ---> fe988522d578
Successfully built fe988522d578
Successfully tagged httpd-test:v4

ubuntu@ip-172-31-10-78:~/docker/httpd$ docker image ls
REPOSITORY                TAG       IMAGE ID       CREATED          SIZE
httpd-test                v4        fe988522d578   32 seconds ago   714MB
httpd-test                v2        ea1ba0e3bc81   15 minutes ago   714MB
httpd-test                v1        19efb20dafee   17 minutes ago   714MB

ubuntu@ip-172-31-10-78:~/docker/httpd$ docker run -itd --name=httpd-test-1 -p 82:80 httpd-test:v4
fcf04784066a2e94df585539b377033852046b2477f7e8fbed154d100b60bb56

ubuntu@ip-172-31-10-78:~/docker/httpd$ docker ps
CONTAINER ID   IMAGE           COMMAND                 CREATED         STATUS         PORTS
       NAMES
fcf04784066a   httpd-test:v4   "httpd -D FOREGROUND"   5 seconds ago   Up 4 seconds   0.0.0.0:82->80/tcp, :::82->80/tcp   httpd-test-1

ubuntu@ip-172-31-10-78:~/docker/httpd$ curl localhost:82
Test webserver created using Docker file

```