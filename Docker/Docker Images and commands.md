

#### Image Addressing:
 ![[Pasted image 20220623024637.png]]

- `docker search <image-name>`: this command is used to search for the docker images in the docker hub or regitered registry.

| Command            | Descriptions                                      |
| ------------------ | ------------------------------------------------- |
| ``--limit 2``      | limits the images output                          |
| `--filter star=10` | gets the images onto the popularity of the images |
| `--filter is-official=true`  | will only return the offical images |

```
ubuntu@ip-172-31-10-78:~$ docker search httpd --limit 2
NAME               DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
httpd              The Apache HTTP Server Project                  4052      [OK]
clearlinux/httpd   httpd HyperText Transfer Protocol (HTTP) ser…   1

ubuntu@ip-172-31-10-78:~$ docker search httpd --limit 2  --filter stars=10
NAME      DESCRIPTION                      STARS     OFFICIAL   AUTOMATED
httpd     The Apache HTTP Server Project   4052      [OK]
```

- `docker image pull image-name`: used to pull the image to local registry
- `docker image push image-name`: command used to push the image to local registry
- `docker image tag <old-tag-name>  <new-tag-name>`
- `docker image list` : command is used to list the docker images
- `docker image rm` : command is used to remove the docker images.
	- This happens in 2 steps: 
		- First the soft link to all the tags is removed from the image
		- Then the image is removed.
- `docker image prune -a` : the command is used to remove all the un used images, while this is not much recommended command in dev or prod enviroments.
- `docker system df` : command is used to describe the storage utilized by different docker resources.
	- if a image is used by container, then it is considered as **reclaimable image**
```
ubuntu@ip-172-31-10-78:~$ docker system df
TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          6         3         1.911GB   974.5MB (51%)
Containers      4         1         209.9MB   209.9MB (99%)
Local Volumes   0         0         0B        0B
Build Cache     0         0         0B        0B
```

#### Inspecting a Running docker images:
- `docker image history <image-name`: this will  show all the commands used to created the image.
	- This is helpful **incase we dont have the docker file** for the image, we can know all the commands that were run to create the image.
![[Pasted image 20220623030134.png]]

- `docker image inspect httpd`: command is used inspect and get the details of the docker image in the JSON format.
 
| Command                     | Descriptions                                      |
| --------------------------- | ------------------------------------------------- |
| ``-f {{.Os}}``               | will return the OS variable             |
| `-f {{.Architecture}}`          | will return architecture variable |
| `-f '{{.Architecture}} {{.Os}}'` | will return both the variable in the written sequence               |


 ![[Pasted image 20220623030501.png]]
##### Authentication Private Registry:

- `docker login <registry`: like incase of docker it is `docker.io`  while incase of gcp it is `gcr.io`

##### Pushing image to remote registry:

1. We will tag the image with new name using command: `docker image httpd:alpine gcr.io/company/httpd:customv1`
2. Now we will push to the remote repository `gcr.io` inside the repository `company` using command: `docker image push gcr.io/company/httpd:customv1`

#### Saving and sending a docker image as tar file:

- `docker image save <image-name> -o alpine.tar` : this command will create a tar file for the docker image, which can be extracted and copied to other server and loaded as docker image
- `docker image load <-i> alpine.tar`: we will load the tar file and get the docker images created.
![[Pasted image 20220623031107.png]]

#### Saving running containers into tar files which can be used to create docker images:
- `docker export <container-name>  > testcontainer.tar` :  this will save the container as tar file
- `docker image import testcontainer.tar newimage:latest`:  now using the import command we can create teh docker image, which can be used to create the docker image.


