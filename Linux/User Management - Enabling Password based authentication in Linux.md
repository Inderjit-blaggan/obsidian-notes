
1. SSh Into the Linux box: 
```
v@DESKTOP-JE0IJPU:~$ ssh ec2-user@54.146.140.206 -i eks-terraform-key.pem
The authenticity of host '54.146.140.206 (54.146.140.206)' can't be established.
ECDSA key fingerprint is SHA256:ftcsNtQWIqbQyzkpr6eCAuApMGufdmkGFh8XAFTwGDE.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '54.146.140.206' (ECDSA) to the list of known hosts.

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/
[ec2-user@ip-172-31-91-104 ~]$ Connection to 54.146.140.206 closed by remote host.
Connection to 54.146.140.206 closed.
v@DESKTOP-JE0IJPU:~$ ssh ubuntu@54.89.208.127 -i eks-terraform-key.pem
The authenticity of host '54.89.208.127 (54.89.208.127)' can't be established.
ECDSA key fingerprint is SHA256:skrCx9GqLMBx72oD0VXZBThvbQx+raU1kOQGWOexTZI.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '54.89.208.127' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.13.0-1029-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Thu Jul  7 02:28:13 UTC 2022

  System load:  0.35              Processes:             105
  Usage of /:   19.2% of 7.58GB   Users logged in:       0
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

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

ubuntu@ip-172-31-86-220:~$ sudo su

```

2. Elevate your access to root user and set the password for the user using command: `passwd <username>`
```
root@ip-172-31-86-220:/home/ubuntu# passwd ubuntu
New password:
Retype new password:
passwd: password updated successfully
```

3. Now Get inside the `/etc/ssh/sshd_config` file and add edit the `PasswordAuthentication` field to `yes` from `no`
   ![[Pasted image 20220707080212.png]]
```
root@ip-172-31-86-220:/home/ubuntu# cd /etc/ssh/

root@ip-172-31-86-220:/etc/ssh# cat ssh
ssh_config                ssh_host_dsa_key.pub      ssh_host_ed25519_key      ssh_host_rsa_key.pub      sshd_config.d/
ssh_config.d/             ssh_host_ecdsa_key        ssh_host_ed25519_key.pub  ssh_import_id
ssh_host_dsa_key          ssh_host_ecdsa_key.pub    ssh_host_rsa_key          sshd_config

root@ip-172-31-86-220:/etc/ssh# cat sshd_config  | grep PasswordAuthentication
# To disable tunneled clear text passwords, change to no here!
PasswordAuthentication no
#PermitEmptyPasswords no

root@ip-172-31-86-220:/etc/ssh# vi sshd_config
```

4. Restart the `sshd` services using command:  `systemctl restart sshd`
```
root@ip-172-31-86-220:/etc/ssh# systemctl restart sshd

root@ip-172-31-86-220:/etc/ssh# exit
exit
ubuntu@ip-172-31-86-220:~$ exit
logout
Connection to 54.89.208.127 closed.

v@DESKTOP-JE0IJPU:~$ ssh ubuntu@54.89.208.127
ubuntu@54.89.208.127's password:
Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.13.0-1029-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Thu Jul  7 02:30:59 UTC 2022

  System load:  0.03              Processes:             105
  Usage of /:   19.5% of 7.58GB   Users logged in:       0
  Memory usage: 20%               IPv4 address for eth0: 172.31.86.220
  Swap usage:   0%


```


