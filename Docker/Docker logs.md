#### Setting up Docker `Debug logs`

1. Edit or create the `/etc/docker/daemon.json`   file and add the line for setting `dubug`  attribute as `true`
```
{
	"debug": true
}
```

2. Now reload the docker service to enable settings on the docker daemon: `systemctl reload docker `
3. Now we can again the check the daemon attributes to confirm the settings were applied or not:
```
ubuntu@ip-172-31-10-78:~$ sudo docker system info
Client:
 Context:    default
 Debug Mode: false

Server:
 Containers: 3
  Running: 0
  Paused: 0
  Stopped: 3
 Images: 10
 Server Version: 22.06.0-beta.0
 Storage Driver: overlay2
 Docker Root Dir: /var/lib/docker
 Debug Mode: true

```

4. Now we can run the container and check the log file like
	- `/var/log/syslog` : Ubuntu
	- `/var/log/messages` :  Centos



## Logging drivers:
![[Pasted image 20220619083301.png]]

- The docker logs for the container is saved in the `/var/lib/docker/containers/<container-id>/<container-id-json.log` file inside the container directory of docker
```
root@ip-172-31-10-78:~# ls /var/lib/docker/containers/
1100977982aae3733be793dcc78f0abb6414a42763a176a42d17354285ca75cb  79e8708416b04410d1f254e2ab50bb0ddf2745ca8da1141b0df692868096f75b
32a6aee322a851a252d0c031d155817721fae3e991f4e2bfff11a083163869b8  e1e48a06dd952f14cc3cbec867822bd698bd6e37b5372c3cd3727d70b7eb1215

root@ip-172-31-10-78:~# docker ps
CONTAINER ID   IMAGE          COMMAND              CREATED         STATUS         PORTS     NAMES
79e8708416b0   httpd:latest   "httpd-foreground"   5 minutes ago   Up 5 minutes   80/tcp    testhttpd

root@ip-172-31-10-78:~# ls /var/lib/docker/containers/79e8708416b04410d1f254e2ab50bb0ddf2745ca8da1141b0df692868096f75b/
79e8708416b04410d1f254e2ab50bb0ddf2745ca8da1141b0df692868096f75b-json.log  config.v2.json   hostname  mounts       resolv.conf.hash
checkpoints                                                                hostconfig.json  hosts     resolv.conf

root@ip-172-31-10-78:~# ls /var/lib/docker/containers/79e8708416b04410d1f254e2ab50bb0ddf2745ca8da1141b0df692868096f75b/
```



- [Doc from docker for configuring logging drivers](https://docs.docker.com/config/containers/logging/configure/) 
- We can set the logs and logging onto different plugins based on the requirement using the `daemon.json`  file
![[Pasted image 20220619083706.png]]