### Assign Sudo Privileges to User by using the sudo group
- [doc for refrence](https://linuxize.com/post/how-to-add-user-to-sudoers-in-ubuntu/)
1. Log in as the root user and check the groups the user is assigned to using the id command:
2. Now Add the `sudo` user to the username and it should allow the sudo permission to the user.
```
ubuntu@ip-172-31-86-220:~$ sudo su -

root@ip-172-31-86-220:~# id devops
uid=1001(devops) gid=1001(devops) groups=1001(devops)

root@ip-172-31-86-220:~# usermod -aG sudo devops

root@ip-172-31-86-220:~# id devops
uid=1001(devops) gid=1001(devops) groups=1001(devops),27(sudo)

root@ip-172-31-86-220:~# exit
logout
```
3.  Login to the user and check:


### Assign Sudo Privileges to User by Modifying the sudoers file

1. Log in as the root user and use the command to edit the sudoers file `/etc/sudoers`  : `sudo visudo`

2. Add a new line for the new user using the following fields: `<username> ALL=(ALL:ALL) ALL`
```
[devops@ip-172-31-91-104 ~]$ sudo cat /etc/sudoers | grep -B 2  devops
## Allow root to run any commands anywhere
root    ALL=(ALL)       ALL
devops ALL=(ALL) ALL
```
3. We can check the syntax of the file using command: `sudo visudo -c`
```
[devops@ip-172-31-91-104 ~]$ sudo visudo -c
[sudo] password for devops:
/etc/sudoers: parsed OK
/etc/sudoers.d/90-cloud-init-users: parsed OK
```
5. Login to the user and check
