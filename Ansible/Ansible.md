Ansible Installation:





**Ansible Configuration File:**
![[Pasted image 20220529123410.png]]
![[Pasted image 20220529123336.png]]

See [The configuration file](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#the-configuration-file). **Ansible searches configuration files in this order**

1.  ansible.cfg (in the current directory)
2.  ~/.ansible.cfg (in the home directory)
3.  /etc/ansible/ansible.cfg

**Ansible keys for controling managed node from control server:** 

1. **Create a pair of ssh keys** id_rsa and id_ras.pub and transfer those to the remote vms.
	- Either you can copy the files or use **ssh-copy-id** to **send the files to the remote server** for a user using command: ``ssh-copy-id -i <public-key> user@<server-hostname>``

2. Now we can pass the key_file in the inventory file witht the variable ``ansible_ssh_private_key_file=<path>``
   ![[Pasted image 20220529131017.png]]



**Ansible Questions:**

1. **Install Ansible package using yum on ansible controller.**
   - We need to run command : ``sudo yum install epel-release -y ; sudo yum install ansible -y``

2. Among the following options, which file stores the SSH public keys of remote users that are allowed to establish an SSH connection to this host.
- /etc/ssh/sshd_config
- /etc/ssh/ssh_config
- ~/.ssh/config
- ~/.ssh/authorized_keys

Answer: d

3. We want to establish **password-less SSH connectivity** for a user from **Ansible controller** to **web1** managed host, what exactly needs to be done.
	- Add Ansible managed host’s user’s SSH private key in Ansible controller’s user’s “authorized_keys” 
	- Add Ansible controller’s user’s SSH public key in Ansible managed node’s user’s “authorized_keys”
	- Add Ansible controller’s user’s SSH private key in Ansible managed node’s user’s “authorized_keys” 
	- Add Ansible managed host’s user’s SSH public key in Ansible controller’s user’s “authorized_keys”
 Answer: b

4. On Ansible controller node generate an SSH key with filename `ansible` under default location (`~/.ssh`).

  ```
[thor@ansible-controller ~]$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/thor/.ssh/id_rsa): /home/thor/.ssh/ansible
Created directory '/home/thor/.ssh'.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/thor/.ssh/ansible.
Your public key has been saved in /home/thor/.ssh/ansible.pub.
The key fingerprint is:
SHA256:hYDqZjttmlXRM4szK5inbzkFWkW8vYJAQcwVIuG3ECg thor@ansible-controller
The key's randomart image is:
+---[RSA 2048]----+
|+*+oo=o          |
|E.=.. oo .       |
|.+ o ..o= .      |
|  = + .o.=       |
| . = o= S.       |
|  =o..o+.        |
| ooo+o..         |
|  o=*.           |
|  +*..           |
+----[SHA256]-----+

[thor@ansible-controller ~]$ ls -l /home/thor/.ssh/ansible
-rw------- 1 thor thor 1679 May 29 07:49 /home/thor/.ssh/ansible
  
```


5. We would like to establish password-less secure authentication between Ansible controller and `web1` node. Use the keys generated in the previous step and do the needful.  
	User `ansible`'s SSH password for `web1` node is `ansible` and during testing the SSH connection use `-i <path-to-your-ssh-private-key>` with `ssh` command.

```
[thor@ansible-controller ~]$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/thor/.ssh/id_rsa): /home/thor/.ssh/ansible
/home/thor/.ssh/ansible already exists.
Overwrite (y/n)? y
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/thor/.ssh/ansible.
Your public key has been saved in /home/thor/.ssh/ansible.pub.
The key fingerprint is:
SHA256:Ocvj4pPfVdaHic+DMZW1HKBLFFNoxKaLM67O/x+96yo thor@ansible-controller
The key's randomart image is:
+---[RSA 2048]----+
|          o=+o...|
|          .=o .oo|
|          +o  oo |
|         o. .o + |
|        S ..+ = o|
|       = +  .O  .|
|      ..*  .o.+  |
|    . +o E .. .. |
|    .=+=+.+oo+.  |
+----[SHA256]-----+

[thor@ansible-controller ~]$ ssh-copy-id -i ~/.ssh/ansible ansible@web1
/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/thor/.ssh/ansible.pub"
/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
ansible@web1's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'ansible@web1'"
and check to make sure that only the key(s) you wanted were added.

[thor@ansible-controller ~]$ ssh ansible@web1
ansible@web1's password: 
Last login: Sun May 29 07:54:16 2022 from ansible-runner.ansible
```

6. An inventory file is given at `/home/thor/playbooks/inventory`. Configure it to use the private ssh key.
Use the ping module to test connectivity through Ansible - `ansible -m ping -i inventory web1`

```
thor@ansible-controller ~]$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/thor/.ssh/id_rsa): /home/thor/.ssh/ansible
/home/thor/.ssh/ansible already exists.
Overwrite (y/n)? y
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/thor/.ssh/ansible.
Your public key has been saved in /home/thor/.ssh/ansible.pub.
The key fingerprint is:
SHA256:Ocvj4pPfVdaHic+DMZW1HKBLFFNoxKaLM67O/x+96yo thor@ansible-controller
The key's randomart image is:
+---[RSA 2048]----+
|          o=+o...|
|          .=o .oo|
|          +o  oo |
|         o. .o + |
|        S ..+ = o|
|       = +  .O  .|
|      ..*  .o.+  |
|    . +o E .. .. |
|    .=+=+.+oo+.  |
+----[SHA256]-----+

[thor@ansible-controller ~]$ ssh-copy-id -i ~/.ssh/ansible ansible@web1
/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/thor/.ssh/ansible.pub"
/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
ansible@web1's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'ansible@web1'"
and check to make sure that only the key(s) you wanted were added.

[thor@ansible-controller ~]$ ssh ansible@web1
ansible@web1's password: 
Last login: Sun May 29 07:54:16 2022 from ansible-runner.ansible
[ansible@web1 ~]$ ^C
[ansible@web1 ~]$ exit
logout
Connection to web1 closed.

[thor@ansible-controller ~]$ cat ~/playbooks/inventory 
web1 ansible_host=172.20.1.100 ansible_user=ansible

[thor@ansible-controller ~]$ vi  ~/playbooks/inventory        

[thor@ansible-controller ~]$ cat ~/playbooks/inventory 
web1 ansible_host=172.20.1.100 ansible_user=ansible  ansible_ssh_private_key_file=/home/thor/.ssh/ansible

[thor@ansible-controller ~]$ ansible -m ping -i ~/playbooks/inventory web1
web1 | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}
```



**Adhoc Commands:** 

- An Ansible ad hoc command **uses** the ``/usr/bin/ansible`` command-line **tool to automate a single task on one or more managed nodes**. 
	- ad hoc commands are quick and easy, but they are not reusable.

**Why use ad hoc commands?**
- ad hoc commands are **great for tasks you repeat rarely**. 
	- For example, if you want to power off all the machines in your lab for Christmas vacation, you could execute a quick one-liner in Ansible without writing a playbook. An ad hoc command looks like this:
**Command Syntax:** ``ansible [pattern] -m [module] -a "[module options]"``

**Use cases for ad hoc tasks**
- ad hoc tasks can be **used to reboot servers, copy files, manage packages and users,** and much more. 
- You can use any **Ansible module in an ad hoc task.** ad hoc tasks, like playbooks, use a declarative model, calculating and executing the actions required to reach a specified final state. 
- They **achieve a form of idempotence by checking the current state before they begin and doing nothing unless the current state is different** from the specified final state.

1. **Rebooting servers**
	- The default module for the ansible command-line utility is the ansible.builtin.command module. 
	- You can use an ad hoc task to call the command module and reboot all web servers in Atlanta, 10 at a time. 
	- Before Ansible can do this, you must have all servers in Atlanta listed in a group called [atlanta] in your inventory, and you must have working SSH credentials for each machine in that group. 
	- To reboot all the servers in the [atlanta] group: ``ansible atlanta -a "/sbin/reboot"``

	**By default Ansible uses only 5 simultaneous processes.** If you have more hosts than the value set for the **fork count**, Ansible will talk to them, but it will take a little longer. 
	- To reboot the [atlanta] servers with 10 parallel forks:  ``ansible atlanta -a "/sbin/reboot" -f 10``

You check other usecase over [ansible_adhoc_doc_usecase](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html) 



Scenario Based Questions:

1. Run the ansible command to gather facts of the `localhost` and save the output in `/tmp/ansible_facts.txt` file.
   
```
[thor@ansible-controller ~]$ ls
playbooks
[thor@ansible-controller ~]$ cat playbooks/inventory 
web1 ansible_host=172.20.1.100 ansible_ssh_pass=Passw0rd ansible_user=root
web2 ansible_host=172.20.1.101 ansible_ssh_pass=Passw0rd ansible_user=root

[thor@ansible-controller ~]$ ansible -m setup localhost > /tmp/ansible_facts.txt 
 [WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'
 
[thor@ansible-controller ~]$ cat /tmp/ansible_facts.txt 
localhost | SUCCESS => {
    "ansible_facts": {
        "ansible_apparmor": {
            "status": "disabled"
        }, 
        "ansible_architecture": "x86_64", 
        "ansible_bios_date": "01/01/2011", 
        "ansible_bios_version": "Google", 
        "ansible_cmdline": {
            "BOOT_IMAGE": "/boot/vmlinuz-5.4.0-1073-gcp", 
            "console": "ttyS0", 
            "ro": true, 
            "root": "UUID=f29ff51b-3bd9-4b0f-81c0-d7378763a3bd"
        },
```


2. Execute an ad-hoc command to perform a `ping` connectivity test for the `localhost` and save the output in `/tmp/ansible_ping.txt` file
```
[thor@ansible-controller ~]$ ansible -m ping localhost > /tmp/ansible_ping.txt 
 [WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'
[thor@ansible-controller ~]$ cat /tmp/ansible_ping.txt 
localhost | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}
[thor@ansible-controller ~]$ 
```

3. Run an adhoc command to perform a `ping` connectivity to `all` hosts in the `/home/thor/playbooks/inventory` file and save the output in `/tmp/ansible_all.txt` file.
```
[thor@ansible-controller ~]$ cat ~/playbooks/inventory 
web1 ansible_host=172.20.1.100 ansible_ssh_pass=Passw0rd ansible_user=root
web2 ansible_host=172.20.1.101 ansible_ssh_pass=Passw0rd ansible_user=root

[thor@ansible-controller ~]$ ansible -m ping -i ~/playbooks/inventory  all > /tmp/ansible_all.txt 

[thor@ansible-controller ~]$ cat /tmp/ansible_all.txt 
web2 | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}
web1 | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}
[thor@ansible-controller ~]$ 
```


4. Run an adhoc command to run a command on host `web1` to print the `date` and save the output in `/tmp/ansible_date.txt` file on the ansible controller. 
	Use the `command` module and argument `date`. Inventory file is available at `/home/thor/playbooks/`
```
[thor@ansible-controller ~]$ ansible -m command -a date -i ~/playbooks/inventory  web1 > /tmp/ansible_date.txt 

[thor@ansible-controller ~]$ cat /tmp/ansible_date.txt 
web1 | CHANGED | rc=0 >>
Sun May 29 08:26:28 UTC 2022
[thor@ansible-controller ~]$ 
```


5. Create a shell script called `host_details.sh` under `~/playbooks/` directory and make it executable.
The shell script should run ad-hoc ansible commands to:
1.  Print the `hostname` of all managed nodes in the inventory file `~/playbooks/inventory`
2.  Using `copy` module copy the `/etc/resolv.conf` file from ansible controller to `/tmp/resolv.conf` on `node00` host. Use the inventory file `~/playbooks/inventory`.
```
[thor@ansible-controller playbooks]$ vi host_details.sh 

[thor@ansible-controller playbooks]$ cat host_details.sh 
ansible all -m command -a hostname -i ~/playbooks/inventory
ansible  node00 -m copy -a "src=/etc/resolv.conf  dest=/tmp/resolv.conf" -i ~/playbooks/inventory 

[thor@ansible-controller playbooks]$ chmod +x host_details.sh 

[thor@ansible-controller playbooks]$ sh host_details.sh 
node00 | CHANGED | rc=0 >>
node00

node01 | CHANGED | rc=0 >>
node01

node00 | CHANGED => {
    "changed": true, 
    "checksum": "ac5732980258ed1b8ad2480d53c3d3811fa3fd5b", 
    "dest": "/tmp/resolv.conf", 
    "gid": 0, 
    "group": "root", 
    "md5sum": "84a2ae80097527ab769a8846fcd41003", 
    "mode": "0644", 
    "owner": "root", 
    "size": 76, 
    "src": "/root/.ansible/tmp/ansible-tmp-1653814848.89-111173639387348/source", 
    "state": "file", 
    "uid": 0
}
```


6. Create a shell script called `host_data.sh` under `~/playbooks/` directory and make it executable.
The shell script should:
1.  Set `ANSIBLE_GATHERING` to `False`
2.  Run an ad-hoc command to print the `uptime` of all managed nodes in the inventory file `~/playbooks/inventory`
3.  Create and run a playbook `~/playbooks/playbook.yml` to cat the file `/etc/redhat-release` on all managed nodes in the inventory file `~/playbooks/inventory`. Also please make sure to run this playbook in verbose mode i.e just add `-vv` at the end of your ansible-playbook command.
```
[thor@ansible-controller playbooks]$ ls
host_data.sh  host_details.sh  inventory  playbook.yml

[thor@ansible-controller playbooks]$ vi host_details.sh 

[thor@ansible-controller playbooks]$ mv host_details.sh host_data.sh 

[thor@ansible-controller playbooks]$ cat host_data.sh 
export ANSIBLE_GATHERING=False
ansible all -m shell -a uptime -i ~/playbooks/inventory
ansible-playbook -i ~/playbooks/inventory  playbook.yml -vv

[thor@ansible-controller playbooks]$ chmod +x host_data.sh 

[thor@ansible-controller playbooks]$ sh host_data.sh 
node00 | CHANGED | rc=0 >>
 09:11:17 up 55 min,  1 user,  load average: 13.30, 9.73, 8.26

node01 | CHANGED | rc=0 >>
 09:11:17 up 55 min,  1 user,  load average: 13.30, 9.73, 8.26

ansible-playbook 2.7.13
  config file = /etc/ansible/ansible.cfg
  configured module search path = [u'/home/thor/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python2.7/site-packages/ansible
  executable location = /bin/ansible-playbook
  python version = 2.7.5 (default, Jun 20 2019, 20:27:34) [GCC 4.8.5 20150623 (Red Hat 4.8.5-36)]
Using /etc/ansible/ansible.cfg as config file
/home/thor/playbooks/inventory did not meet host_list requirements, check plugin documentation if this is unexpected

PLAYBOOK: playbook.yml ****************************************************************************************************************************************************************
1 plays in playbook.yml

PLAY [all] ****************************************************************************************************************************************************************************
META: ran handlers

TASK [shell] **************************************************************************************************************************************************************************
task path: /home/thor/playbooks/playbook.yml:4
changed: [node00] => {"changed": true, "cmd": "cat /etc/redhat-release", "delta": "0:00:00.081620", "end": "2022-05-29 09:11:19.786596", "rc": 0, "start": "2022-05-29 09:11:19.704976", "stderr": "", "stderr_lines": [], "stdout": "CentOS Linux release 7.6.1810 (Core) ", "stdout_lines": ["CentOS Linux release 7.6.1810 (Core) "]}
changed: [node01] => {"changed": true, "cmd": "cat /etc/redhat-release", "delta": "0:00:00.027988", "end": "2022-05-29 09:11:19.804103", "rc": 0, "start": "2022-05-29 09:11:19.776115", "stderr": "", "stderr_lines": [], "stdout": "CentOS Linux release 7.6.1810 (Core) ", "stdout_lines": ["CentOS Linux release 7.6.1810 (Core) "]}
META: ran handlers
META: ran handlers

PLAY RECAP ****************************************************************************************************************************************************************************
node00                     : ok=1    changed=1    unreachable=0    failed=0   
node01                     : ok=1    changed=1    unreachable=0    failed=0  
```


**Privilege Escalations:**

- To esclate to the user privelege while on fly use the ``become`` attribute to **enable it to assume sudo** 
- if incase you have some other user, who you want to esclate privilege to, use the ``become_user``  
- To set them in the inventory file we can use ``ansible_become=yes`` and ``ansible_become_user=nginx `` 
- At time your priveleges will require password, to prompt for password while running playbook use the attribute ``--ask-become-pass``
  
  **Priority order for different file for privileges**
  ![[Pasted image 20220529145852.png]]

**Questions and Answers**

1. Setting `become_user` directive automatically sets `become` as well.
   False

2. What is the default value of `become_user` directive?
	root

3. Which of the following can be passed in as an inventory variable to activate privilege escalation?
	- ansible_become_method
	- become
	- ansible_become
	- become_user
Answer: c

4. We want to change the login shell for a remote user while running an Ansible task, which directive will be used to do so?
	- become
	- become_method
	- become_shell
	- become_flags
Answer: d

5. We have a playbook `file.yml` under `~/playbooks/web1` that simply creates a file `test.txt` on node `web1`. The user used to connect to the host does not have sufficient privileges to create the file on the desired location but has `sudo` access. Make the appropriate changes so that the user's privileges as elevated when the playbook is run.
		You can use the command `ansible-playbook -i inventory file.yml` to run the playbook.
```
[thor@ansible-controller web1]$ ls
file.yml  inventory

[thor@ansible-controller web1]$ cat file.yml 
---
- hosts: all
  gather_facts: no
  tasks:
    - name: Create a blank file
      file:
        path: /home/admin/test.txt
        state: touch

[thor@ansible-controller web1]$ vi inventory 

[thor@ansible-controller web1]$ cat file.yml 
---
- hosts: all
  gather_facts: no
  become: true
  tasks:
    - name: Create a blank file
      file:
        path: /home/admin/test.txt
        state: touch


fatal: [web1]: FAILED! => {"msg": "Using a SSH password instead of a key is not possible because Host Key checking is enabled and sshpass does not support this. Please add this host's fingerprint to your known_hosts file to manage this host."}

sudo vi /etc/ansible/ansible.cfg

echo "Added host_key_checking = False"

[thor@ansible-controller web1]$ cat /etc/ansible/ansible.cfg | grep host_key
host_key_checking = False
#record_host_keys=False
#host_key_auto_add = True

[thor@ansible-controller web1]$ ansible-playbook -i inventory file.yml

PLAY [all] ****************************************************************************************************************************************************************************

TASK [Create a blank file] ************************************************************************************************************************************************************
fatal: [web1]: FAILED! => {"changed": false, "msg": "Error, could not touch target: [Errno 13] Permission denied: '/home/admin/test.txt'", "path": "/home/admin/test.txt", "state": "absent"}
        to retry, use: --limit @/home/thor/playbooks/web1/file.retry

PLAY RECAP ****************************************************************************************************************************************************************************
web1                       : ok=0    changed=0    unreachable=0    failed=1   

[thor@ansible-controller web1]$ vi file.yml 
[thor@ansible-controller web1]$ ansible-playbook -i inventory file.yml

PLAY [all] ****************************************************************************************************************************************************************************

TASK [Create a blank file] ************************************************************************************************************************************************************
changed: [web1]

PLAY RECAP ****************************************************************************************************************************************************************************
web1                       : ok=1    changed=1    unreachable=0    failed=0  
```


6. When the file was created on the host, the owner of the file became `root` user. However, file was to be created for the `admin` user. Please make the appropriate changes to the `file.yml` playbook so that the file is created as user `admin`.
		You can use the command `ansible-playbook -i inventory file.yml` to run the playbook. To test you may have to delete the file manually once. However if you are confident about your solution then hit the `Check` button and we will test that for you.
- Consider using `become_user` directives.
```
[thor@ansible-controller web1]$ cat file.yml 
---
- hosts: all
  gather_facts: no
  become: true
  become_user: admin
  tasks:
    - name: Create a blank file
      file:
        path: /home/admin/test.txt
        state: touch
```


7. There is a playbook `file.yml` under `~/playbooks/web2/` directory. We want to run `file.yml` playbook as `admin` user on `web2` node, so modify the playbook accordingly. To run the playbook we have created a script `web2.sh` on the same location, so you can execute the script with command `sh web2.sh`. We don’t want to store the sudo password in any file due to security reasons. Make the necessary changes so that when the script is run, the playbook must prompt for the become password.
		`ansible` user's password on `web2` is `Passw0rd`
```

[thor@ansible-controller web2]$ cat file.yml 
---
- hosts: all
  gather_facts: no
  become: true
  become_user: admin
  tasks:
    - name: Create a blank file
      file:
        path: /home/admin/test.txt
        state: touch
        
[thor@ansible-controller web2]$ cat web2.sh 
#!/bin/bash
ansible-playbook -i inventory file.yml --ask-become-pass 

[thor@ansible-controller web2]$ cat inventory 
[web]
web2

[web:vars]
ansible_host=172.20.1.101
ansible_ssh_pass=Passw0rd
ansible_user=ansible
ansible_become_user=admin
ansible_ssh_pipelining=true
```


8. We need privilege escalation to be enabled across all playbooks without having to specify in each play, make the necessary changes in `/etc/ansible/ansible.cfg` file to activate privilege escalation.
```
[thor@ansible-controller web2]$ cat /etc/ansible/ansible.cfg  | grep -A 2 privilege
[privilege_escalation]
become=True


```





9. Our organization recently introduced changes in security. Going forward we'd like to use `doas` as privilege escalation tool for all managed nodes without having to update inventories or passing in command line parameters for each node. Make the necessary changes.

Edit ansible configuration file and set `become_method=doas`
```unix
sudo vi /etc/ansible/ansible.cfg
```
It should look like.
```
[privilege_escalation]
become=True
become_method=doas
```


**YAML Syntax in playbooks:**


Questions and Answers:

1. Let us explore the environment for our `KodeKloud e-commerce LAMP stack` application. There are 2 servers - `lamp-web` and `lamp-db`. Let us setup the inventory files for that. Create an inventory file at `/home/thor/playbooks/lamp-stack-playbooks/inventory` to include the following data:
**Hosts:** `lamp-web, lamp-db`  
**Groups:** `db_servers` contains `lamp-db`; `web_servers` contains `lamp-web`  
**IP Addresses:** `lamp-web: 172.20.1.100; lamp-db: 172.20.1.101`  
**Credentials for lamp-web:** `Username=john Password=john`  
**Credentials for lamp-db** `Username=maria Password=maria`
- Files and text in the beggining of questions:
```
[thor@ansible-controller playbooks]$ ls
lamp-stack-playbooks

[thor@ansible-controller playbooks]$ cd lamp-stack-playbooks/ 

[thor@ansible-controller lamp-stack-playbooks]$ ls
deploy-lamp-stack.yml  inventory

[thor@ansible-controller lamp-stack-playbooks]$ cat inventory 
# Inventory File

[thor@ansible-controller lamp-stack-playbooks]$ cat deploy-lamp-stack.yml 
- name: Deploy lamp stack application
  hosts: all
  tasks:
    - name: Install common dependencies
      yum:
        name:
          - libselinux-python
          - libsemanage-python
          - firewalld
        state: installed

[thor@ansible-controller lamp-stack-playbooks]$ 


```

**Solutions:**
```
[thor@ansible-controller lamp-stack-playbooks]$ cat inventory 
# Inventory File

[web_servers]
lamp-web ansible_host=172.20.1.100 ansible_user=john ansible_password=john

[db_servers]
lamp-db ansible_host=172.20.1.101 ansible_user=maria ansible_password=maria
```

2. Let's add some additional data required for setting up the database and web servers. The data should be associated with the respective servers.
**Database Info:**  
`mysqlservice=mysqld`  
`mysql_port=3306`  
`dbname=ecomdb`  
`dbuser=ecomuser`  
`dbpassword=ecompassword`  
**Web Info:**  
`httpd_port=80`  
`repository=https://github.com/kodekloudhub/learning-app-ecommerce.git`

Solutions:
```
[thor@ansible-controller lamp-stack-playbooks]$ cat inventory 
# Inventory File

[web_servers]
lamp-web ansible_host=172.20.1.100 ansible_user=john ansible_password=john

[db_servers]
lamp-db ansible_host=172.20.1.101 ansible_user=maria ansible_password=maria
[thor@ansible-controller lamp-stack-playbooks]$ vi inventory
[thor@ansible-controller lamp-stack-playbooks]$ cat inventory 
# Inventory File

[web_servers]
lamp-web ansible_host=172.20.1.100 ansible_user=john ansible_password=john

[web_servers:vars]
httpd_port=80
repository=https://github.com/kodekloudhub/learning-app-ecommerce.git

[db_servers]
lamp-db ansible_host=172.20.1.101 ansible_user=maria ansible_password=maria

[db_servers:vars]
mysqlservice=mysqld
mysql_port=3306
dbname=ecomdb
dbuser=ecomuser
dbpassword=ecompassword
```

3. Let us setup password less authentication between `Ansible Controller` and the web/db servers.
Create a pair of SSH keys for each user (without any passphrase) at `/home/thor/.ssh/maria` and `/home/thor/.ssh/john`  
And distribute the public keys to the web and database servers - `lamp-db` and `lamp-web`.
DB server user is `maria` and its password is `maria`. Web server user is `john` and its password is `john`.
**Solutions:**
```
[thor@ansible-controller lamp-stack-playbooks]$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/thor/.ssh/id_rsa): /home/thor/.ssh/maria
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/thor/.ssh/maria.
Your public key has been saved in /home/thor/.ssh/maria.pub.
The key fingerprint is:
SHA256:nYsZGmhIWnm0K2JuMdBzKKGLVaWjJhmK8FQWeF3ntDE thor@ansible-controller
The key's randomart image is:
+---[RSA 2048]----+
|.  .*+... E      |
|.o.B.o.  + +     |
|* X.*     o      |
|*% * +   . .     |
|X+* + . S o      |
|o+oo   o + .     |
| o    . o .      |
|.                |
|                 |
+----[SHA256]-----+

[thor@ansible-controller lamp-stack-playbooks]$ ssh-copy-id -i ~/.ssh/maria maria@lamp-db
/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/thor/.ssh/maria.pub"
The authenticity of host 'lamp-db (172.20.1.101)' can't be established.
ECDSA key fingerprint is SHA256:uNTmVywMAW6xMJp4vFGlRAsQelwcEBOr/ThU7LexNbc.
ECDSA key fingerprint is MD5:83:49:cd:9f:e7:ef:a1:c8:73:ef:c0:76:7b:ba:48:d7.
Are you sure you want to continue connecting (yes/no)? yes
/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
maria@lamp-db's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'maria@lamp-db'"
and check to make sure that only the key(s) you wanted were added.

[thor@ansible-controller lamp-stack-playbooks]$ ssh-keygen 
Generating public/private rsa key pair.
Enter file in which to save the key (/home/thor/.ssh/id_rsa): /home/thor/.ssh/john
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/thor/.ssh/john.
Your public key has been saved in /home/thor/.ssh/john.pub.
The key fingerprint is:
SHA256:9FMSy3JwWQ+93be/XpjQ2fl3PbAqk4vnL3jubw61EFc thor@ansible-controller
The key's randomart image is:
+---[RSA 2048]----+
|        . oooE   |
|         +.o.o.  |
|        o.=.. .o.|
|       . +oo ..o=|
|        S.o...o.+|
|          o...o+o|
|        .... .o.B|
|       ..B...   *|
|       .*=X=  .o.|
+----[SHA256]-----+

[thor@ansible-controller lamp-stack-playbooks]$ ssh-copy-id -i ~/.ssh/john  john@lamp-web
/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/thor/.ssh/john.pub"
The authenticity of host 'lamp-web (172.20.1.100)' can't be established.
ECDSA key fingerprint is SHA256:uNTmVywMAW6xMJp4vFGlRAsQelwcEBOr/ThU7LexNbc.
ECDSA key fingerprint is MD5:83:49:cd:9f:e7:ef:a1:c8:73:ef:c0:76:7b:ba:48:d7.
Are you sure you want to continue connecting (yes/no)? yes
/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
john@lamp-web's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'john@lamp-web'"
and check to make sure that only the key(s) you wanted were added.
```


4. Update the inventory file to use the newly created private keys for the respective hosts
 ```
 Inventory File

[web_servers]
lamp-web ansible_host=172.20.1.100 ansible_user=john ansible_ssh_private_key_file=/home/thor/.ssh/john

[web_servers:vars]
httpd_port=80
repository=https://github.com/kodekloudhub/learning-app-ecommerce.git

[db_servers]
lamp-db ansible_host=172.20.1.101 ansible_user=maria ansible_password=maria ansible_ssh_private_key_file=/home/thor/.ssh/maria

[db_servers:vars]
mysqlservice=mysqld
mysql_port=3306
dbname=ecomdb
dbuser=ecomuser
dbpassword=ecompassword
```  


5. A playbook `deploy-lamp-stack.yml` is given with a basic tasks to install basic libraries. Execute the playbook and fix any issues.
You are not required to add any tasks or plays. Only fix the issue with execution.
```
thor@ansible-controller lamp-stack-playbooks]$ cat deploy-lamp-stack.yml 
- name: Deploy lamp stack application
  hosts: all
  tasks:
    - name: Install common dependencies
      yum:
        name:
          - libselinux-python
          - libsemanage-python
          - firewalld
        state: installed
```

Solution:
- Check privilege escalation. Enable `become` mode in the playbook.
- Add `become` mode in the playbook as follows:-  

```
- name: Deploy lamp stack application
  hosts: all
  become: yes 
  tasks:
    - name: Install common dependencies
      yum:
        name:
          - libselinux-python
          - libsemanage-python
          - firewalld
        state: installed
```



**Ansible Modules:**

**Installation of packages using playbooks:**
- [Doc for yum module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/yum_module.html)

1. Create a playbook `httpd.yml` under `~/playbooks/` to install `httpd` package on `web1` node using Ansible’s `yum` module.
```

[thor@ansible-controller playbooks]$ cat inventory 
web1 ansible_host=172.20.1.100 ansible_ssh_pass=Passw0rd ansible_user=root

[thor@ansible-controller playbooks]$ cat httpd.yaml
---
- hosts: web1
  sudo: yes
  tasks:
    - name: install httpd package
      yum: name=httpd state=installed

[thor@ansible-controller playbooks]$ ansible-playbook httpd.yaml -i inventory 
[DEPRECATION WARNING]: Instead of sudo/sudo_user, use become/become_user and make sure become_method is 'sudo' (default). This feature will be removed in version 2.9. Deprecation 
warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.

PLAY [web1] ***************************************************************************************************************************************************************************

TASK [install httpd package] **********************************************************************************************************************************************************
changed: [web1]

PLAY RECAP ****************************************************************************************************************************************************************************
web1                       : ok=1    changed=1    unreachable=0    failed=0   

```



2. We have an rpm available for `wget` package on URL `http://mirror.centos.org/centos/7/os/x86_64/Packages/wget-1.14-18.el7_6.1.x86_64.rpm`. Create a playbook with name `wget.yml` under `~/playbooks` to install that rpm on `web1` node using `yum` module.
   
```
[thor@ansible-controller playbooks]$ vi wget.yml

[thor@ansible-controller playbooks]$ cat inventory 
web1 ansible_host=172.20.1.100 ansible_ssh_pass=Passw0rd ansible_user=root

[thor@ansible-controller playbooks]$ cat wget.yml 
- hosts: web1
  name: Install the wget and then nginx rpm from a remote repo
  tasks:
  - yum:
      name: wget
      state: latest
  - yum:
      name: http://nginx.org/packages/centos/6/noarch/RPMS/nginx-release-centos-6-0.el6.ngx.noarch.rpm
      state: present

[thor@ansible-controller playbooks]$ ansible-playbook wget.yml -i inventory 

PLAY [Install the nginx rpm from a remote repo] ***************************************************************************************************************************************

TASK [yum] ****************************************************************************************************************************************************************************
ok: [web1]

PLAY RECAP ****************************************************************************************************************************************************************************
web1                       : ok=1    changed=0    unreachable=0    failed=0   

[thor@ansible-controller playbooks]$ 
```

3. There is a playbook under `~/playbooks` named as `unzip.yml` to install `unzip` package on `web1` node. We want to install `unzip-5.52` version of this package so before running the playbook make the required changes.
```
[thor@ansible-controller playbooks]$ cat unzip.yml 
---
- hosts: all
  tasks:
    - name: Install unzip package
      yum:
        name: unzip-5.52
        state: present

[thor@ansible-controller playbooks]$ cat inventory 
web1 ansible_host=172.20.1.100 ansible_ssh_pass=Passw0rd ansible_user=root

[thor@ansible-controller playbooks]$ ansible-playbook unzip.yml -i inventory 

PLAY [all] ****************************************************************************************************************************************************************************

TASK [Install unzip package] **********************************************************************************************************************************************************
ok: [web1]

PLAY RECAP ****************************************************************************************************************************************************************************
web1                       : ok=1    changed=0    unreachable=0    failed=0   
```

4. Our playbook - `iotop.yml` - to install the latest version of `iotop` package keeps failing. Please fix the issue so that playbook can work.
	The playbook is located under `~/playbooks` directory. And the inventory file is `inventory`
```

[thor@ansible-controller playbooks]$ cat inventory 
web1 ansible_host=172.20.1.100 ansible_ssh_pass=Passw0rd ansible_user=root

[thor@ansible-controller playbooks]$ cat iotop.yml 
---
- hosts: web1
  tasks:
    - name: Install iotop package
      yum:
        name: iotop
        state: latest
        

[thor@ansible-controller playbooks]$ ansible-playbook iotop.yml -i inventory

PLAY [web1] ***************************************************************************************************************************************************************************

TASK [Install iotop package] **********************************************************************************************************************************************************
changed: [web1]

PLAY RECAP ****************************************************************************************************************************************************************************
web1                       : ok=1    changed=1    unreachable=0    failed=0   

```

5. We want to install some more packages on `web1` node. Create a playbook `~/playbooks/multi-pkgs.yml` to install the latest version of `sudo` package, moreover we already have `vsftpd` `v3.0.2` installed but due to some compatibility issues we want to install `vsftpd` `v2.2.2` so add a task in same playbook to do so.
```
[thor@ansible-controller playbooks]$ cat inventory 
web1 ansible_host=172.20.1.100 ansible_ssh_pass=Passw0rd ansible_user=root

[thor@ansible-controller playbooks]$ cat multi-pkgs.yml 

- hosts: web1
  name: Install the multiple packages
  tasks:
  - yum: name=sudo state=latest
  - yum: name=vsftpd-2.2.2    state=present  allow_downgrade=yes


[thor@ansible-controller playbooks]$ ansible-playbook multi-pkgs.yml -i inventory

PLAY [Install the multiple packages] **************************************************************************************************************************************************

TASK [yum] ****************************************************************************************************************************************************************************
changed: [web1]

TASK [yum] ****************************************************************************************************************************************************************************
ok: [web1]

PLAY RECAP ****************************************************************************************************************************************************************************
web1                       : ok=2    changed=1    unreachable=0    failed=0   

```
   

**Ansible Services Module:**

- [Ansible services doc](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/service_module.html)

1. Create a playbook `httpd.yml` under `~/playbooks` directory to make sure `httpd` service is started on `web1` node. You can use `~/playbooks/inventory` 
```
[thor@ansible-controller playbooks]$ cat inventory 
web1 ansible_host=172.20.1.100 ansible_ssh_pass=Passw0rd ansible_user=root

[thor@ansible-controller playbooks]$ cat httpd.yml 
---
- name: Start httpd
  hosts: web1
  gather_facts: no
  tasks:
    - name: Start httpd service
      service:
        name: httpd
        state: started

[thor@ansible-controller playbooks]$ ansible-playbook httpd.yml -i inventory 

PLAY [Start httpd] ********************************************************************************************************************************************************************

TASK [Start httpd service] ************************************************************************************************************************************************************
ok: [web1]

PLAY RECAP ****************************************************************************************************************************************************************************
web1                       : ok=1    changed=0    unreachable=0    failed=0   

[thor@ansible-controller playbooks]$ 

```


2. We have a playbook `~/playbooks/file.yml` to copy a file with a welcome message under httpd server's document root on `web1` node. Make changes in the playbook so that httpd server reloads after copying the file, make sure it does not restart the httpd server.
```
[thor@ansible-controller playbooks]$ cat inventory 
web1 ansible_host=172.20.1.100 ansible_ssh_pass=Passw0rd ansible_user=root

[thor@ansible-controller playbooks]$ cat file.yml 
---
- hosts: web1
  gather_facts: no
  tasks:
    - name: Copy Apache welcome file
      copy:
        src: index.html
        dest: /var/www/html/index.html

    - name: Start service httpd, if not started
      service:
        name: httpd
        state: reloaded

[thor@ansible-controller playbooks]$ ansible-playbook  file.yml -i inventory 

PLAY [all] ****************************************************************************************************************************************************************************

TASK [Copy Apache welcome file] *******************************************************************************************************************************************************
ok: [web1]

TASK [Start service httpd, if not started] ********************************************************************************************************************************************
ok: [web1]

PLAY RECAP ****************************************************************************************************************************************************************************
web1                       : ok=2    changed=0    unreachable=0    failed=0   

[thor@ansible-controller playbooks]$ 
```


3. We would like the `httpd` service on `web1` node to always start automatically after the system reboots. Update the `httpd.yml` playbook you created earlier with the required changes.
   
```
[thor@ansible-controller playbooks]$ cat httpd.yml 
---
- name: Start httpd
  hosts: web1
  gather_facts: no
  tasks:
    - name: Start httpd service
      service:
        name: httpd
        state: started
        enabled: yes
[thor@ansible-controller playbooks]$ ansible-playbook  httpd.yml -i inventory 

PLAY [Start httpd] ********************************************************************************************************************************************************************

TASK [Start httpd service] ************************************************************************************************************************************************************
ok: [web1]

PLAY RECAP ****************************************************************************************************************************************************************************
web1                       : ok=1    changed=0    unreachable=0    failed=0   

[thor@ansible-controller playbooks]$ 
```

4. We created a playbook `~/playbooks/config.yml` to enable port `443` for httpd on `web1` node as we want to run nginx on the default port `80` so port `80` needs to be free. Make changes in the playbook so that httpd service restarts after making these change.
```
[thor@ansible-controller playbooks]$ cat config.yml 
---
- hosts: web1
  gather_facts: no
  tasks:
    - name: Make changes in Apache config
      replace:
        path: /etc/httpd/conf/httpd.conf
        regexp: "^Listen 80"
        replace: "Listen 443"

    - name: Restart service httpd, in all cases
      service:
        name: httpd
        state: restarted

 
```

5. Create a playbook `~/playbooks/nginx.yml` to install nginx on `web1` node and make sure nginx service is started and should always start even after the system reboots.
```
[thor@ansible-controller playbooks]$ cat nginx.yml 
---
- name:  install and start nginx
  hosts: web1
  gather_facts: no
  tasks:
    - name: Install nginx
      yum:
        name: nginx
        state: latest
    - name: Start nginx service
      service:
        name: nginx
        state: started
        enabled: yes
```


**Firewall Service and Rules:**
1. Using an Ansible playbook install `firewalld` on `web1` node, start and enable its service as well. Name the playbook as `firewall.yml` and keep it under `~/playbooks`.
   
```
[thor@ansible-controller ~]$ cd  playbooks/

[thor@ansible-controller playbooks]$ ls
ansible.cfg  inventory  web2-config.yml

[thor@ansible-controller playbooks]$ vi firewall.yml

[thor@ansible-controller playbooks]$ cat inventory 
web1 ansible_host=172.20.1.100 ansible_ssh_pass=Passw0rd ansible_user=root

[thor@ansible-controller playbooks]$ cat firewall.yml 
- name:  install and start nginx
  hosts: web1
  gather_facts: no
  tasks:
    - name: Install firewall
      yum:
        name: firewalld
        state: latest
    - name: Start firewalld service
      service:
        name: firewalld
        state: started
        enabled: yes
```


2. We have a requirement on `web1` node to white list `web2` node's IP address `172.20.1.101` in firewall. Create and run a playbook `~/playbooks/whitelist.yml` to do so. Add IP in `internal` zone.
```
[thor@ansible-controller playbooks]$ cat whitelist.yml 
- name:  enable custom IP over firewall
  hosts: web1
  gather_facts: no
  tasks:
    - firewalld:
        source: 172.20.1.101/24
        zone: internal
        state: enabled
        permanent: yes
        immediate: yes
```


3. We want to block `161/udp` port on `web1` node permanently. Make a playbook `block.yml` under `~/playbooks/` directory to do so. Use `zone: block`
```
- name:  enable custom IP over firewall
  hosts: web1
  gather_facts: no
  tasks:
    - firewalld:
        port: 161/udp
        permanent: yes
        immediate: yes
        state: enabled
        zone: block
```
- To verify, SSH to `web1` server and run the following command:-
``firewall-cmd --list-ports --zone=block``

4. On `web1` node add firewall rule in `internal` zone to enable `https` connection from Ansible controller machine and make sure that rule must persist even after system reboot. You can create a playbook `https.yml` under `~/playbooks/` directory. IP address of ansible controller is `172.20.1.2`.
```
- name:  enable custom IP over firewall
  hosts: web1
  gather_facts: no
  tasks:
    - firewalld:
        source: 172.20.1.1
        zone: internal
        permanent: yes
        state: enabled
        service: https

    - service:
        name: firewalld
        state: reloaded
```

5. We have a playbook `~/playbooks/web2-config.yml`, it has some existing code to change apache’s default port `80` to port `8082` as we want to run Apache on port `8082` on `web2` node. Make some changes as given below before running the playbook.
**A**. Add an entry in `~/playbooks/inventory` for `web2` node, IP address of `web2` node is `172.20.1.101` and ssh password and username are same as of `web1` (username = root and password = Passw0rd).

**B**. Update `web2-config.yml` to install `httpd` before updating its port in config, also `start/enable` its service.

**C**. Install `firewalld` package and `start/enable` its service.

**D**. As now Apache will listen on port `8082` so edit the playbook to add firewall rule in `public` zone so that Apache can allow all incoming traffic.

```
---
- hosts: web2
  tasks:
    - name: Install pkgs
      yum:
        name: httpd, firewalld
        state: present

    - name: Start/Enable services
      service:
        name: "{{ item }}"
        state: started
        enabled: yes
      with_items:
        - httpd
        - firewalld

    - name: Change Apache port
      replace:
        path: /etc/httpd/conf/httpd.conf
        regexp: "Listen 80"
        replace: "Listen 8082"

    - name: restart Apache
      service:
        name: httpd
        state: restarted

    - name: Add firewall rule for Apache
      firewalld:
        port: 8082/tcp
        zone: public
        permanent: yes
        state: enabled
        immediate: true
```

To verify firewall rules, SSH to `web2` server and run the following commands:-  ``firewall-cmd --list-ports --zone=public``



Ansible file Modules:

1. Create a playbook `~/playbooks/perm.yml` to create a blank file `/opt/data/perm.txt` with `0640` permissions on `web1` node.
   
```
--
- hosts: web1
  tasks:
  - name: Create a file
    file:
      path: /opt/data/perm.txt
      state: touch
      mode: '0640'
```

**Note:** If `file`, even with other options (such as `mode`), the file will be modified if it exists but will NOT be created if it does not exist. Set to `touch` or use the [ansible.builtin.copy](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/copy_module.html#ansible-collections-ansible-builtin-copy-module) or [ansible.builtin.template](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/template_module.html#ansible-collections-ansible-builtin-template-module) module if you want to create the file if it does not exist.


2. Using a playbook `~/playbooks/index1.yml` create `/var/www/html/index.html` file on `web1` node with content `This line was added using Ansible lineinfile module!`.
   
```
---
- hosts: web1
  tasks:
  - name: Add line to the file
    lineinfile:
      path: /var/www/html/index.html
      line: This line was added using Ansible lineinfile module! 
      create: yes
```


3. We have a playbook `~/playbooks/find.yml` that recursively finds files in `/opt/data` directory older than 2 minutes and equal or greater than 1 megabyte in size. It also copies those files under `/opt` directory. However it has some missing parameters so its not working as expected, take a look into it and make appropriate changes. 
```
---
- hosts: web1
  tasks:
    - name: Find files
      find:
        paths: /opt/data
        age: 2m
        size: 1m
        recurse: yes
      register: file

    - name: Copy files
      command: "cp {{ item.path }} /opt"
      with_items: "{{ file.files }}"
```



4. In `/var/www/html/index.html` file on `web1` node add some additional content using `blockinfile` module. Below is the content:
```
Welcome to KodeKloud!
This is Ansible Lab.
```
Make sure user owner and group owner of the file is `apache`, also make sure the block is added at beginning of the file. Create a new playbook for this `~/playbooks/index2.yml`

Solution 1: Using [file](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/file_module.html) and ``blockinfile`` module 
```
---
- hosts: web1
  tasks:
  - name : Set user owner and group owner of the file to apache
    file:   
      path: /var/www/html/index.html
      owner: apache
      group: apache
  - name: Add line to the file
    blockinfile:
      path: /var/www/html/index.html
      insertbefore: BOF
      block: |
        Welcome to KodeKloud!
        This is Ansible Lab. 
      
```

Solution 2: Using only [blockinfile](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/blockinfile_module.html)
```
--- 
- name: Add block to index.html 
  hosts: web1 
  tasks: 
    - blockinfile: 
      owner: apache 
      group: apache 
      insertbefore: BOF 
      path: /var/www/html/index.html 
      block: | 
         Welcome to KodeKloud! 
         This is Ansible Lab.
```


5. On `web1` node we want to run our `httpd` server on port `8080`. Create a playbook `~/playbooks/httpd.yml` to change port `80` to `8080` in `/etc/httpd/conf/httpd.conf` file using `replace` module. Also make sure Ansible restarts `httpd` service after making the change.
`Listen 80` is the parameter that need to be changed in `/etc/httpd/conf/httpd.conf`

**Solutions**: use the [replace ansible module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/replace_module.html)
```
---
- hosts: web1
  tasks:
  - name : change port 80 to 8080
    replace:
      path: /etc/httpd/conf/httpd.conf
      regexp: 'Listen 80'
      replace: 'Listen 8080'
  - service: name=httpd state=restarted
```


**Archiving Ansible Module:**

1. Create an `inventory` file under `~/playbooks` directory on Ansible controller host and add `web1` as managed node. IP address of the web1 node is `172.20.1.100`, SSH user is `root` and password is `Passw0rd`.
Create a playbook `~/playbooks/zip.yml` to make a zip archive `opt.zip` of `/opt` directory on `web1` node and save it under `/root` directory on `web1` node itself.

- 
```
[thor@ansible-controller playbooks]$ cat inventory 
web1 ansible_host=172.20.1.100 ansible_ssh_pass=Passw0rd ansible_user=root
[thor@ansible-controller playbooks]$ vi zip.yml
[thor@ansible-controller playbooks]$ cat zip.yml 
---
- hosts: web1
  tasks:
  - name : to make a zip archive opt.zip of /opt directory on web1 node
    archive:
      path: /opt
      dest: /root/opt.zip
      format: zip
```


2. On Ansible controller, we have a zip archive `local.zip`. We want to extract its contents on `web1` under `/tmp` directory. Create a playbook `local.yml` under `~/playbooks` directory to complete the task.
```
[thor@ansible-controller playbooks]$ cat local.yml 
---
- hosts: web1
  tasks:
  - name : extract its contents on web1 under /tmp directory
    unarchive:
      src: local.zip
      dest: /tmp
```


3. On `web1` node we have an archive `data.tar.gz` under `/root` directory, extract it under `/srv` directory by developing a playbook `~/playbooks/data.yml` and make sure `data.tar.gz` archive is removed after that.
- Use `unarchive` and `file` modules to complete the task.
```
---
- name: Extract data.tar.gz on web1
  hosts: web1
  tasks:
  - unarchive:
      src: /root/data.tar.gz
      dest: /srv
      remote_src: yes

  - file: path=/root/data.tar.gz state=absent
```


4. Create a playbook `download.yml` under `~/playbooks` directory to download and extract the `https://github.com/kodekloudhub/Hello-World/archive/master.zip` zip archive under `/root` directory on the `web1` node
```
---
- name: Extract data.tar.gz on web1
  hosts: web1
  tasks:
  - unarchive:
      src: https://github.com/kodekloudhub/Hello-World/archive/master.zip
      dest: /root
      remote_src: yes
```

