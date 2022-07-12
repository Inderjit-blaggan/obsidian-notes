#### Commands used to create the Apache server

1. ` sudo su`
3.  `apt update`
4.  `apt install apache2 -y`
5.  `ls /var/www/html`
6.  `echo "Hello World!"`
7.  `echo "Hello World!" > /var/www/html/index.html`
8.  `echo $(hostname)`
9. ` echo $(hostname -i)`
10. ` echo "Hello World from $(hostname)"`
11. ` echo "Hello World from $(hostname) $(hostname -i)"`
12.  `echo "Hello world from $(hostname) $(hostname -i)" > /var/www/html/index.html`
13.  `sudo service apache2 start`

```
inderjitsinghblaggan@instance-1:~$ bash

inderjitsinghblaggan@instance-1:~$ whoami
inderjitsinghblaggan

inderjitsinghblaggan@instance-1:~$ pwd
/home/inderjitsinghblaggan
inderjitsinghblaggan@instance-1:~$ cat /etc/*release
PRETTY_NAME="Debian GNU/Linux 11 (bullseye)"
NAME="Debian GNU/Linux"
VERSION_ID="11"
VERSION="11 (bullseye)"
VERSION_CODENAME=bullseye
ID=debian
HOME_URL="https://www.debian.org/"
SUPPORT_URL="https://www.debian.org/support"
BUG_REPORT_URL="https://bugs.debian.org/"

inderjitsinghblaggan@instance-1:~$ sudo apt update
Get:1 http://packages.cloud.google.com/apt cloud-sdk-bullseye InRelease [6778 B]
0 upgraded, 23 newly installed, 0 to remove and 1 not upgraded.
Need to get 19.7 MB of archives.
After this operation, 98.9 MB of additional disk space will be used.
Do you want to continue? [Y/n] y
Building dependency tree... Done
Reading state information... Done
1 package can be upgraded. Run 'apt list --upgradable' to see it.

inderjitsinghblaggan@instance-1:~$ sudo apt install apache2
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
inderjitsinghblaggan@instance-1:~$ ls /var/www/html/
index.html

inderjitsinghblaggan@instance-1:~$ sudo echo "hello blaggan" > /var/www/html/index.html 
bash: /var/www/html/index.html: Permission denied

inderjitsinghblaggan@instance-1:~$ sudo su

root@instance-1:/home/inderjitsinghblaggan# echo "hello blaggan" > /var/www/html/index.html 

root@instance-1:/home/inderjitsinghblaggan#  sudo service apache2 start

root@instance-1:/home/inderjitsinghblaggan# curl localhost
hello blaggan

root@instance-1:/home/inderjitsinghblaggan# hostname
instance-1

root@instance-1:/home/inderjitsinghblaggan# hostname -i
10.128.0.2
```
![[Pasted image 20220623163727.png]]

