  - ` docker container run -it ubuntu bash`  : this command is used to run a docker containers in interactive mode.

     | Flags        | Descriptions                                                                                                                                  |
     | ------------ | --------------------------------------------------------------------------------------------------------------------------------------------- |
     | `-i`         | interactive                                                                                                                                   |
     | `-t`         | terminal mode                                                                                                                                 |
     | `--name`     | name of the container                                                                                                                         |
     | `-d`         | detached Mode                                                                                                                                 |
     | `--rm`       | remove the container as soon as it exits the process                                                                                          |
     | `--hostname` | used to set the hostname of the container                                                                                                     |
     | `--restart`  | The option is used to define the [[#Contianer Restart Policy]] to the containers `no` ``on-failure[:max-retries]``  `always` `unless-stopped` |
     | `-p --PORT`  | used to expose ports for the containers [[#Exposing containers using ports]]                                                                  |
     | `--log-driver`    |  This is used to set the custom logging for the containers. [[Docker logs]] 


##### Exposing containers using ports
- `-p --PORT`:  
	- Concept: Docker is dependent on the `IP table` which is used to define the rules and thus help in mapping the docker. 
	![[Pasted image 20220619044223.png]]
	- ![[Pasted image 20220619042450.png]]
	- Incase of **multiple interfaces:**
		- Now incase a docker host has 3 interfaces having its public IP addresses, then once we run flag `-p 8000:5000` the application will get exposed over all the interfaces over the `8000` port. 
				- To limit it to only one of the interfaces we can add the IP before the port `<IP>:<PORT>:<Container-PORT>`  ![[Pasted image 20220619042900.png]]

	- Incase we have **multipe application on container having different port** and we want **docker to automatically expose** those to random ports we can use `-P ` Flag which checks the dockerfile and then expose the specified port on the defined ports
		-  Every time the random port will change and this it not usefull for production use cases ![[Pasted image 20220619043907.png]]

| Command                                                 | Descriptions                                                                                                                                               |
| ------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ``docker run -p 5000 image``                            | This command will expose the container over a random port accross all 3 containers ![[Pasted image 20220619043023.png]]                                    |
| `docker run -p 127.0.0.1:8000:5000 image`               | THis command will expose the container over localhost at 5000 and the application will not be avalaible outside host  ![[Pasted image 20220619043321.png]] |
| `docker run -p <IP>:<Host-PORT>:<Container-PORT> image` | This will expose port over the host IP of the interface specified in the command ![[Pasted image 20220619042900.png]]                                      |
| `docker run -P image` | This will automatically expose the port defined in the dockerfile or the image section of the container to random ports.  Every time the **random port will assigned on restart of container** and this it not usefull for production use cases ![[Pasted image 20220619044038.png]]                                                                                                                                                            |



#### Contianer Restart Policy
- Docker provides [restart policies](https://docs.docker.com/engine/reference/run/#restart-policies---restart) to control whether your containers start automatically when they exit, or when Docker restarts. 
	- Restart policies ensure that linked containers are started in the correct order. 
	- Docker recommends that you use restart policies, and avoid using process managers to start containers.
	- To configure the restart policy for a container, use the `--restart` flag when using the `docker run` command. The value of the `--restart` flag can be any of the following:
	
| Command                      | Descriptions                                                                                                                                                                                                                                                                                                                      |
| ---------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ``no``                       | Do not automatically restart the container. **(the default)**                                                                                                                                                                                                                                                                     |
| ``on-failure[:max-retries]`` | Restart the container if it exits due to an error, which manifests as a non-zero exit code. Optionally, limit the number of times the Docker daemon attempts to restart the container using the `:max-retries` option.                                                                                                            |
| `always`                     | Always restart the container if it stops. If it is manually stopped, it is restarted only when Docker daemon restarts or the container itself is manually restarted. (See the second bullet listed in [restart policy details](https://docs.docker.com/config/containers/start-containers-automatically/#restart-policy-details)) |
| `unless-stopped`     |    Similar to `always`, except that when the container is stopped (manually or otherwise), it is not restarted even after Docker daemon restarts.                                                                                                                                                                                                                                        |

- This command changes the restart policy for an already running container named `redis`. : `docker update --restart unless-stopped redis `  
- And this command will ensure all currently running containers will be restarted unless stopped. `docker update --restart unless-stopped $(docker ps -q)


##### Restart policy details[](https://docs.docker.com/config/containers/start-containers-automatically/#restart-policy-details)

Keep the following in mind when using restart policies:

-   A restart policy only takes effect after a container starts successfully. In this case, starting successfully means that the container is up for at least 10 seconds and Docker has started monitoring it. This prevents a container which does not start at all from going into a restart loop.
    
-   If you manually stop a container, its restart policy is ignored until the Docker daemon restarts or the container is manually restarted. This is another attempt to prevent a restart loop.
    
-   Restart policies only apply to _containers_. Restart policies for swarm services are configured differently. See the [flags related to service restart](https://docs.docker.com/engine/reference/commandline/service_create/).

![[Pasted image 20220619034643.png]]

**- What happens when the docker daemon stops?**
	- By default, when the **Docker daemon terminates, it shuts down running containers**. 
	- You can **configure the daemon so that containers remain running if the daemon becomes unavailable**. This functionality is called _live restore_. 
	- The live restore option helps reduce container downtime due to daemon crashes, planned outages, or upgrades.

There are two ways to enable the live restore setting to keep containers alive when the daemon becomes unavailable. **Only do one of the following**.

1. Add the configuration to the daemon configuration file. On Linux, this defaults to `/etc/docker/daemon.json`. On Docker Desktop for Mac or Docker Desktop for Windows, select the Docker icon from the task bar, then click **Preferences** -> **Daemon** -> **Advanced**.
    
    -   Use the following JSON to enable `live-restore`.
        
        ```
        {
          "live-restore": true
        }
        ```
        
    -   Restart the Docker daemon. On Linux, you can avoid a restart (and avoid any downtime for your containers) by reloading the Docker daemon. If you use `systemd`, then use the command `systemctl reload docker`. Otherwise, send a `SIGHUP` signal to the `dockerd` process.
        
-   If you prefer, you can start the `dockerd` process manually with the `--live-restore` flag. This approach is not recommended because it does not set up the environment that `systemd` or another process manager would use when starting the Docker process. This can cause unexpected behavior.
![[Pasted image 20220619034816.png]]