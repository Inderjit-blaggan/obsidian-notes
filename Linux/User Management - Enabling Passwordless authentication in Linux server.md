In this Example we have 2 machines `Amazon Linux server ` and  `Ubuntu Server` 
- Now we need to setup passwordless authentication from `Amazon Linux server ` to  `Ubuntu Server`

Procedure:
1. Login into the Amazon machine and create a key pair of private and public key as the root user or the common user between the machines using command: ` ssh-keygen -t rsa`
2. Now copy the create public key like `filename.pub` to the ubuntu server, so as when the private key is used from the amazon server, the public key or public lock can be unlocked.
3. Now Add this public key file to the `.ssh/.authorised`  file inside the root user.
4. Restart the sshd service and check the access from  `Amazon Linux server ` to  `Ubuntu Server`

```
v@DESKTOP-JE0IJPU:~$ ssh ec2-user@54.197.14.78  -i eks-terraform-key.pem
Last login: Thu Jul  7 02:25:15 2022 from 49.36.224.139

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/
[ec2-user@ip-172-31-91-104 ~]$ sudo su -

[root@ip-172-31-91-104 ~]# cd ~

[root@ip-172-31-91-104 ~]# ls -a
.  ..  .bash_logout  .bash_profile  .bashrc  .cshrc  .ssh  .tcshrc
[root@ip-172-31-91-104 ~]# ls -a .ssh/
.  ..  authorized_keys

[root@ip-172-31-91-104 ~]# ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:IIf1oXEQqFBIKSC/k1qt8H2rZqim8kmqVUdA1eB2okk root@ip-172-31-91-104.ec2.internal
The key's randomart image is:
+---[RSA 2048]----+
|=+o.ooB=o        |
|=o  .= =..       |
|....E O o        |
|  .= B +         |
|. = = . S        |
| = = .           |
|. =.. .          |
|.=..o. .         |
|Xooo...          |
+----[SHA256]-----+

[root@ip-172-31-91-104 ~]# echo "Copy the public key file into the ubuntu machine so we can get authenticated on ubuntu side"
Copy the public key file into the ubuntu machine so we can get authenticated on ubuntu side

[root@ip-172-31-91-104 ~]# ls -a
.  ..  .bash_logout  .bash_profile  .bashrc  .cshrc  .ssh  .tcshrc

[root@ip-172-31-91-104 ~]# cat .ssh/id_rsa.pub .
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDggE4HN/9N0rW1f5mDjxSAgXqV2Otz8pjk0GyYhIE+wXPHZ/QmeGENlt59YMonEXBdZM+2xcZXlpL2anfiT6LwcIlBa5M6k9JJnWp0ZeK0Ge5IV8P28fNbrHfKGIgw2nuLALowPW1hyj3BdLSHg78b2Q2RGkIoGFM0b1d+GjkOpSLWnhFV6Tnaitjh3Mv7t/0Ntf1FWA5DjcQNU+E8ZP0iDxsy6iH/+TX9VyG0JaZMl6VO9t++PS1/dfcy4ha5UoFpwrkdjWtFV9KFnz6t7FnCoO+lB7xr4SC3rKmC9HeYXwwRP+Gcq/CL6bRyBTx/B8Zz5CubLnS/S8zjdghqc0mR root@ip-172-31-91-104.ec2.internal

```

2. Login to the `Ubuntu machine` and add the public key to the `~/.ssh/authorized_keys`  file for the root user.
```
ubuntu@ip-172-31-86-220:~$ sudo su -

root@ip-172-31-86-220:~# cd ~

root@ip-172-31-86-220:~# ls -a
.  ..  .bash_history  .bashrc  .profile  .ssh  .viminfo  snap

root@ip-172-31-86-220:~# cd .ssh/

root@ip-172-31-86-220:~/.ssh# ls
authorized_keys

root@ip-172-31-86-220:~/.ssh# cat authorized_keys
no-port-forwarding,no-agent-forwarding,no-X11-forwarding,command="echo 'Please login as the user \"ubuntu\" rather than the user \"root\".';echo;sleep 10;exit 142" ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCCdqMP0zeuqKQjoNyzRbl0xQ5RUF48WUIVcPbGaCbexPjamjymBCqd/AalhrADGv2MklsFUcLN6hjktqZwO7OlxM3F/5HHWKuvsU2aYPG8bIdbzcwgX8PGB5k34FzQOhZDO2rl2VY8iglmo+GXqqSYL/5krdTPtJ2HWkDY9UxGD4im/R7Y4s3kawYAEdS4NmB0Mjjqe5u6A/O1qZX4QhJ6UCLEFE3LJQjGhhJ/45sc/PQ3QlHHyGWWASZ/BP0aO5QYrSOkjx3bWh8tcOGWFHlnqgxyFefXQX87ID71UN/I/CuabRbD9QZbSO053Q8Ppt5DVUkgrGAUcaY85U7CWyiB eks-terraform-key

root@ip-172-31-86-220:~/.ssh# vi authorized_keys

root@ip-172-31-86-220:~# cat ~/.ssh/authorized_keys
no-port-forwarding,no-agent-forwarding,no-X11-forwarding,command="echo 'Please login as the user \"ubuntu\" rather than the user \"root\".';echo;sleep 10;exit 142" ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCCdqMP0zeuqKQjoNyzRbl0xQ5RUF48WUIVcPbGaCbexPjamjymBCqd/AalhrADGv2MklsFUcLN6hjktqZwO7OlxM3F/5HHWKuvsU2aYPG8bIdbzcwgX8PGB5k34FzQOhZDO2rl2VY8iglmo+GXqqSYL/5krdTPtJ2HWkDY9UxGD4im/R7Y4s3kawYAEdS4NmB0Mjjqe5u6A/O1qZX4QhJ6UCLEFE3LJQjGhhJ/45sc/PQ3QlHHyGWWASZ/BP0aO5QYrSOkjx3bWh8tcOGWFHlnqgxyFefXQX87ID71UN/I/CuabRbD9QZbSO053Q8Ppt5DVUkgrGAUcaY85U7CWyiB eks-terraform-key
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDggE4HN/9N0rW1f5mDjxSAgXqV2Otz8pjk0GyYhIE+wXPHZ/QmeGENlt59YMonEXBdZM+2xcZXlpL2anfiT6LwcIlBa5M6k9JJnWp0ZeK0Ge5IV8P28fNbrHfKGIgw2nuLALowPW1hyj3BdLSHg78b2Q2RGkIoGFM0b1d+GjkOpSLWnhFV6Tnaitjh3Mv7t/0Ntf1FWA5DjcQNU+E8ZP0iDxsy6iH/+TX9VyG0JaZMl6VO9t++PS1/dfcy4ha5UoFpwrkdjWtFV9KFnz6t7FnCoO+lB7xr4SC3rKmC9HeYXwwRP+Gcq/CL6bRyBTx/B8Zz5CubLnS/S8zjdghqc0mR root@ip-172-31-91-104.ec2.internal
```

3. Try to login to the `Ubuntu` machien using the new private key created in the 1 step.
- Note: Incase it doesnt work please check the **key permission** and **ownership** and the user used to **ssh is correct**.
```

[root@ip-172-31-91-104 ~]# ssh root@172.31.86.220 -i .ssh/id_rsa
The authenticity of host '172.31.86.220 (172.31.86.220)' can't be established.
ECDSA key fingerprint is SHA256:skrCx9GqLMBx72oD0VXZBThvbQx+raU1kOQGWOexTZI.
ECDSA key fingerprint is MD5:8d:b6:df:be:f4:95:47:f5:1c:35:81:0e:2d:6b:21:22.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '172.31.86.220' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.13.0-1029-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Thu Jul  7 03:41:36 UTC 2022

  System load:  0.0               Processes:             111
  Usage of /:   19.5% of 7.58GB   Users logged in:       1
  Memory usage: 20%               IPv4 address for eth0: 172.31.86.220
  Swap usage:   0%


1 update can be applied immediately.
To see these additional updates run: apt list --upgradable


The list of available updates is more than a week old.
To check for new updates run: sudo apt update


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

root@ip-172-31-86-220:~# exit
logout
Connection to 172.31.86.220 closed.
```


## Create a new user and setup a passwordless based authentication for it.
1. Create a new user called `devops` and configure the password for the user
```
[root@ip-172-31-91-104 ~]# useradd devops

[root@ip-172-31-91-104 ~]# passwd devops
Changing password for user devops.
New password:
Retype new password:
passwd: all authentication tokens updated successfully.
```
- For ubuntu : as we need to define the `-m`  home directory and path 
```
root@ip-172-31-86-220:~# useradd -m -d /home/devops devops
root@ip-172-31-86-220:~# passwd devops
New password:
Retype new password:
passwd: password updated successfully
```

2. Login into the `devops` user  and create the `.ssh`  directory inside the home directory for the new user and give `700` permissions to the `.ssh` directory
```
[root@ip-172-31-91-104 ~]# su - devops

[devops@ip-172-31-91-104 ~]$ whoami
devops

[devops@ip-172-31-91-104 ~]$ cd ~

[devops@ip-172-31-91-104 ~]$ pwd
/home/devops

[devops@ip-172-31-91-104 ~]$ ls -a | grep .s
.bash_logout
.bash_profile
.bashrc

[devops@ip-172-31-91-104 ~]$ mkdir .ssh

[devops@ip-172-31-91-104 ~]$ chmod 700 .ssh/

[devops@ip-172-31-91-104 ~]$ ls -la
total 12
drwx------ 3 devops devops  74 Jul  7 04:00 .
drwxr-xr-x 4 root   root    36 Jul  7 03:56 ..
-rw-r--r-- 1 devops devops  18 Jul 15  2020 .bash_logout
-rw-r--r-- 1 devops devops 193 Jul 15  2020 .bash_profile
-rw-r--r-- 1 devops devops 231 Jul 15  2020 .bashrc
drwx------ 2 devops devops   6 Jul  7 04:00 .ssh
```

3. Now login as the user, which you have the private key file and copy the  public key to the ` .ssh/authized_keys`  and set permissions to `600`  i.e `rw`  for the file
```
[ec2-user@ip-172-31-91-104 ~]$ sudo su -
Last login: Thu Jul  7 04:06:36 UTC 2022 on pts/1

[root@ip-172-31-91-104 ~]# cp /home/ec2-user/.ssh/authorized_keys /home/devops/.ssh/
cp: overwrite ‘/home/devops/.ssh/authorized_keys’? yes

[root@ip-172-31-91-104 ~]# ls -l /home/devops/.ssh/
total 4
-rw------- 1 devops devops 399 Jul  7 04:09 authorized_keys

[root@ip-172-31-91-104 ~]# exit
```

4. Login to the server using the old private key file
```
v@DESKTOP-JE0IJPU:~$ ssh devops@54.197.14.78 -i eks-terraform-key.pem
Last login: Thu Jul  7 04:10:00 2022 from 49.36.224.139

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/
[devops@ip-172-31-91-104 ~]$ whoami
devops
```