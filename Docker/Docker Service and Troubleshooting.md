
**Docker certified pdf:** ([PowerPoint Presentation (kodekloud.com)](https://kodekloud.com/wp-content/uploads/2021/08/Docker-Certified-Associate.pdf))
#docker_service

### Docker Service and Troubleshooting in Linux:

 1. `journalctl -u docker.service` : command is used to check the docker logs
```
ubuntu@ip-172-31-10-78:~$ journalctl -u docker.service
Jun 17 13:11:51 ip-172-31-10-78 systemd[1]: Starting Docker Application Container Engine...
Jun 17 13:11:51 ip-172-31-10-78 dockerd[672]: time="2022-06-17T13:11:51.780280501Z" level=info msg="Starting up"
Jun 17 13:11:51 ip-172-31-10-78 dockerd[672]: time="2022-06-17T13:11:51.788660409Z" level=info msg="detected 127.0.0.53
```

2. Check the docker configuration file in `/etc/docker/daemon.json`  and check the custom variable values set
3. Check the free space available on the host using command: `df -h`
4. We can also delete all the unused containers and space occupied by them using command:  `docker container prune`



- We can use the systemctl commands to start the docker engine as shown bellow:
```
ubuntu@ip-172-31-10-78:~$ sudo dockerd --debug
INFO[2022-06-17T12:47:13.003346629Z] Starting up
DEBU[2022-06-17T12:47:13.003987519Z] Listener created for HTTP on unix (/var/run/docker.sock)
INFO[2022-06-17T12:47:13.004567706Z] detected 127.0.0.53 nameserver, assuming systemd-resolved, so using resolv.conf: /run/systemd/resolve/resolv.conf
DEBU[2022-06-17T12:47:13.004896063Z] Golang's threads limit set to 28080
INFO[2022-06-17T12:47:13.005326940Z] [core] original dial target is: "unix:///run/containerd/containerd.sock"  module=grpc
INFO[2022-06-17T12:47:13.299663194Z] API listen on /var/run/docker.sock  

```
  

- Incase the **docker service is not starting**, we can use the `dockerd`  command to start docker and check logs using `--debug`  flag.
```
ubuntu@ip-172-31-10-78:~$ sudo dockerd --debug
INFO[2022-06-17T12:47:13.003346629Z] Starting up
DEBU[2022-06-17T12:47:13.003987519Z] Listener created for HTTP on unix (/var/run/docker.sock)
INFO[2022-06-17T12:47:13.004567706Z] detected 127.0.0.53 nameserver, assuming systemd-resolved, so using resolv.conf: /run/systemd/resolve/resolv.conf
DEBU[2022-06-17T12:47:13.004896063Z] Golang's threads limit set to 28080
INFO[2022-06-17T12:47:13.005326940Z] [core] original dial target is: "unix:///run/containerd/containerd.sock"  module=grpc
INFO[2022-06-17T12:47:13.299663194Z] API listen on /var/run/docker.sock  

```


#### Troubleshooting  `docker.service failed`  issue: 
- https://stackoverflow.com/questions/55906503/docker-how-to-fix-job-for-docker-service-failed-because-the-control-process-ex 

I ran the command below to inspect the issue following [Tâmer Pinheiro answer](https://stackoverflow.com/a/55911400/10907864):

```
sudo dockerd --debug
```

And it gave me this information:

```
INFO[2020-09-02T11:46:15.272833297Z] Starting up                                  
DEBU[2020-09-02T11:46:15.273547501Z] Listener created for HTTP on unix (/var/run/docker.sock) 
failed to start daemon: Unable to get the TempDir under /var/lib/docker: mkdir
/var/lib/docker/tmp: no space left on device
```

I also checked the disk space of the server:

```
df -h
```

which showed that there was no space left on the drive (`/dev/sda1 filesystem`):

```
Filesystem      Size  Used Avail Use% Mounted on
udev            2.0G     0  2.0G   0% /dev
tmpfs           396M   12M  384M   4% /run
/dev/sda1       9.8G  9.8G     0 100% /
tmpfs           2.0G     0  2.0G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock

tmpfs           2.0G     0  2.0G   0% /sys/fs/cgroup
tmpfs           396M     0  396M   0% /run/user/1000
```

And I realized that the issue had to do with no space left on device.

I had to run the following commands to clear some space for the filesystem:

```
sudo find /var/log -type f -delete
sudo rm -rf /var/cache/apt/*
sudo apt clean all
```

And then finally, I removed the `/var/lib/docker.bk` file, because it was too large for the **9.8GB filesystem** of the server:

```
sudo rm -rf /var/lib/docker.bk
```

This time when I ran the command:

```
sudo systemctl restart docker
```



##### How to enable docker daemon to listen outside the docker host and enable other host to spin containers


![[Pasted image 20220617182112.png]]

- To Let the docker listen to outside traffic by default, we can use the `/etc/docker/daemon.json` file as shown in the bellow picture.
  ![[Pasted image 20220617182357.png]]
	- The `dockerd --debug=false` command gives a error as the configuration file above in define `--debug=true`  , thus docker is not able to decide, which configuration to use.


Basic Docker commands:

| Command              | Descriptions                                                                                                |
| -------------------- | ----------------------------------------------------------------------------------------------------------- |
| ``docker version``   | This command is used to check the version of container                                                      |
| `docker system info` | This command is used to check the number of containers, running , Paused, stopped, Images and other details ![[Pasted image 20220617174521.png]]|
|                       |                                                                                                             |

