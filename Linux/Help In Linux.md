**Help in linux:**

- ``--help ``:  the option helps to find the help section for the command
- ``man command ``: to check the man page for the command
	- Incase we want to check a specific sectionof man page for a command:
	  ![[Pasted image 20220601153539.png]]
- ``apropos`` : is a command used to seach from the man pages. 
  - Eq: imagine we forgot the name of command which created directory
  - incase the command is not showing, usually in fresh installtion, use command ``sudo mandb`` to **update the db for man pages**
  - To get only the section required from man page use the ``-s`` **option to select the section** it can be a single section  like ``1`` or range of sections like ``1,8``
 ```
v@DESKTOP-JE0IJPU:~$ apropos -s 1 director
basename (1)         - strip directory and suffix from filenames
dbus-cleanup-sockets (1) - clean up leftover sockets in a directory
dir (1)              - list directory contents
docker-plugin-create (1) - Create a plugin from a rootfs and configuration. Plugin data directory must cont...
finalrd (1)          - final runtime directory generator for shutdown
find (1)             - search for files in a directory hierarchy
git-clone (1)        - Clone a repository into a new directory
git-mv (1)           - Move or rename a file, a directory, or a symlink
git-stash (1)        - Stash the changes in a dirty working directory away
grub-mknetdir (1)    - prepare a GRUB netboot directory.
mkdir (1)            - make directories
helpztags (1)        - generate the help tags file for directory
ls (1)               - list directory contents
mktemp (1)           - create a temporary file or directory
mountpoint (1)       - see if a directory or file is a mountpoint
pwd (1)              - print name of current/working directory
pwdx (1)             - report current working directory of a process
vdir (1)             - list directory contents
```

**Labs Example:**
1. Using `apropos` command, find out the the configuration file for NFS mounts and save its name in `/home/bob/nfs` file.
   - You can use a command like `apropos "NFS mounts"`
```
[bob@centos-host ~]$ apropos  -s 5 "NFS mounts"
nfsmount.conf (5)    - Configuration file for NFS mounts

[bob@centos-host ~]$ sudo find / -name nfsmount.conf
/etc/nfsmount.conf
``` 


