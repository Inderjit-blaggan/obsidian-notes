## Setting Up ssh Keys for github
- [Youtube Video](https://www.youtube.com/watch?v=zBssUO_5H_A)

**Step 1:** Generate the SSH key on the linux system.: `ssh-keygen -t rsa -b 4096  -C "EMAIL@COM"
![[Pasted image 20220701111600.png]]

**Step 2:** Cat the pub file create in the above step for the user:  `cat  /root/.ssh/id_rsa.pub`
![[Pasted image 20220701111646.png]]


**Step 3:** Add the details of the account in the config file `~/.ssh/config` and create seprate configs profile from them
```
# Key for usual repositories on github.com
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa
  IdentitiesOnly yes
#
 # Key for a particular repository on github.com
Host github.com-inderjitsingh.blaggan
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_git_personal
  IdentitiesOnly yes
```

**Step 4:** Now incase i have to use the second profile, clone the repo using command: `git clone git@<profile-name>:<repo-name`
```
v@DESKTOP-JE0IJPU:$ git clone git@github.com-inderjitsingh.blaggan:Inderjit-blaggan/aws-eks.git
Cloning into 'aws-eks'...
warning: You appear to have cloned an empty repository.
```