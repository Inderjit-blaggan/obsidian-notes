## Docker Commands:
#docker_commands
##### Docker images commands:




###### Docker containers commands:


- `docker container create image` :  the command will create the container but not start it.
		- Once the command is executed we can see a new directory by `container-id` be created inside `/var/lib/docker/containers/`
		  ![[Pasted image 20220617185755.png]]

- `docker container ls -a` : command is used to list the container
	
     | Flags       | Descriptions                                          |
     | ------------- | ----------------------------------------------------- |
     | `a` | all |

- `docker container start <container-id>` : command is used to start the container.
- `docker container run <image>`  Now to combine **container creation + starting containers**, we use the docker container run command.
  - ` docker run -it ubuntu bash`  : this command is used to run a docker containers in interactive mode.

     | Flags    | Descriptions                                         |
     | -------- | ---------------------------------------------------- |
     | `-i`     | interactive                                          |
     | `-t`     | terminal mode                                        |
     | `--name` | name of the container                                |
     | `-d`     | detached Mode                                        |
     | `-rm`    | remove the container as soon as it exits the process |
     | `--hostname`         | used to set the hostname of the container                                                     |

- `docker container attach <first-4-character-container-id` : we can use the attach command to get inside the container
- `ctrl + p + q` : Use to enter in the **deattach mode**, the keys to exit out of the containers without existing it. 
- `docker container exec -it <container-id> <command>` : the command is used to run a command inside the containers


#### Troubleshooting docker commands

- `docker container inspect <container-id>`: to inspect the container and check its configuration we can use the command.
- `docker container stats` : this command is used to check the CPU, Memory , Nw IO information about the docker container.
	![[Pasted image 20220618040355.png]]
-  `docker container top <container-id>` : to list the processes running inside docker containers and there variable and attributes.
	![[Pasted image 20220618040752.png]]
- `docker container logs <container-id>`: to get the logs from the running container, to get the real time logs we can use the flag `-f`
	![[Pasted image 20220618040853.png]]
- `docker system events --since <time>` : command is used to get the details of event that occured in the specified duration. 
	  ![[Pasted image 20220618041137.png]]

     | Flags    | Descriptions          |
     | -------- | --------------------- |
     | `-f, --filter`      | Filter output based on conditions provided      |
     | `--since`     | Show all events created since timestampe         |
     | `--until ` | Stream events until this timestamper |
     | `-d`         |  detached Mode                     |


#### Container-level management:
- `docker container pause <container-id>`: command is used to pause the container.
- `docker container unpause <container-id>`: command is used to unpause the container. 
	- It used `freezer cgroup` to isolate the container from getting any other commands from docker.
- `docker container stop <container-id>`: command is used to stop the container.
- `docker container rm <container-id>`: command is used to remove the container. 
	- Docker first tries to do gracefull stop on containers and issues a `-SIGTERM`  Signal, incase the container doesnt stop in specified time, it then goes and run the `-SIGKILL` signal to forcefully stop the container.
- `docker container stop $(docker container ls -q)` : To stop all containers using single command we can use.
		![[Pasted image 20220618043000.png]]
- `docker container rm $(docker container ls -aq)` : to remove all the stopped containers.
- `docker container prune`: this command is used to clean up space and remove all stopped container.


##### Copy the File from docker host to docker contiainer
- `docker container cp <source file in the host> <container-name>:<path in container` : command used to copy file from docker host to container
![[Pasted image 20220619042032.png]]

##### Docker Networking Commands:
