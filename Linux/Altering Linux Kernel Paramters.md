[Doc link techmint](https://www.tecmint.com/change-modify-linux-kernel-runtime-parameters/)

The latest specification of the [Filesystem Hierarchy Standard](https://www.tecmint.com/linux-directory-structure-and-important-files-paths-explained/) indicates that `/proc` represents the default method for handling process and system information as well as other kernel and memory information. Particularly, `/proc/sys` is where you can find all the information about devices, drivers, and some kernel features.

The actual internal structure of `/proc/sys` depends heavily on the kernel being used, but you are likely to find the following directories inside. In turn, each of them will contain other subdirectories where the values for each parameter category are maintained:

1.  `dev`: parameters for specific devices connected to the machine.
2.  `fs`: filesystem configuration (quotas and inodes, for example).
3.  kernel: kernel-specific configuration.
4.  `net`: network configuration.
5.  `vm`: use of the kernel’s virtual memory.

	- To modify the kernel runtime parameters we will use the `sysctl` command. The exact number of parameters that can be modified can be viewed with:  `sysctl -a | wc -l
	- If you want to view the complete list of Kernel parameters, just do: `sysctl -a 
	- As the the output of the above command will consist of A LOT of lines, we can use a pipeline followed by less to inspect it more carefully: `sysctl -a | less
	- Let’s take a look at the first few lines. Please note that the first characters in each line match the names of the directories inside `/proc/sys`:![[Pasted image 20220619040854.png]]
	- The above highlighted line indicates that `sr0` is an alias for the optical drive. In other words, that is how the kernel “**sees**” that drive and uses that name to refer to it.

#### Set or Modify Linux Kernel Parameters

- To set the value for a kernel parameter we can also use `sysctl`, but using the `-w` option and followed by the parameter’s name, the equal sign, and the desired value.
- Another method consists of using `echo` to overwrite the file associated with the parameter. In other words, the following methods are equivalent to disable the packet forwarding functionality in our system (which, by the way, should be the default value when a box is not supposed to pass traffic between networks):
```
echo 0 > /proc/sys/net/ipv4/ip_forward

sysctl -w net.ipv4.ip_forward=0

```

**Note:** **kernel parameters that are set** using `sysctl` will only be **enforced during the current session and will disappear when the system is rebooted**.
	- To set these values **permanently,** edit `/etc/sysctl.conf` with the desired values. For example, to disable packet forwarding in **/etc/sysctl.conf** make sure this line appears in the file:
```
net.ipv4.ip_forward=0
```

- Then run following command to apply the changes to the running configuration. `sysctl -p

**Other examples of important kernel runtime parameters are:**

- `fs.file-max` specifies the **maximum number of file handles the kernel can allocate for the system.** Depending on the intended use of your system (web / database / file server, to name a few examples), you may want to change this value to meet the system’s needs.
	- Otherwise, you will receive a “**Too many open files**” error message at best, and may prevent the operating system to boot at the worst.
	- If due to an innocent mistake you find yourself in this last situation, boot in single user mode (as explained in [Part 13 – Configure and Troubleshoot Linux Grub Boot Loader](https://www.tecmint.com/configure-and-troubleshoot-grub-boot-loader-linux/)) and edit **/etc/sysctl.conf** as instructed earlier. To set the same restriction on a per-user basis, refer to [Part 14 – Monitor and Set Linux Process Limit Usage](https://www.tecmint.com/monitor-linux-processes-and-set-process-limits-per-user/) of this series.

- `kernel.sysrq` is used to enable the **SysRq** key in your keyboard (also known as the print screen key) so as to allow certain key combinations to invoke emergency actions when the system has become unresponsive.
	- The default value **(16)** indicates that the system will honor the `Alt+SysRq+key` combination and perform the actions listed in the **sysrq.c** documentation found in [kernel.org](https://www.kernel.org/doc/Documentation/sysrq.txt) (where key is one letter in the b-z range). For example, `Alt+SysRq+b` will reboot the system forcefully (use this as a last resort if your server is unresponsive).

**Warning!** Do not attempt to press this key combination on a virtual machine because it may force your host system to reboot!


##### Block Ping Requests in Linux

When set to `1, net.ipv4.icmp_echo_ignore_all` will ignore ping requests and drop them at the kernel level. This is shown in the below image – note how ping requests are lost after setting this kernel parameter:

[![Block Ping Requests in Linux](https://www.tecmint.com/wp-content/uploads/2016/04/Block-Ping-Requests-in-Linux.png)](https://www.tecmint.com/wp-content/uploads/2016/04/Block-Ping-Requests-in-Linux.png)

A better and easier way to set individual runtime parameters is using **.conf** files inside `/etc/sysctl.d`, grouping them by categories.

For example, instead of setting **net.ipv4.ip_forward=0** and **net.ipv4.icmp_echo_ignore_all=1** in **/etc/sysctl.conf**, we can create a new file named `net.conf` inside **/etc/sysctl.d**:

```
echo "net.ipv4.ip_forward=0" > /etc/sysctl.d/net.conf
echo "net.ipv4.icmp_echo_ignore_all=1" >> /etc/sysctl.d/net.conf
```

If you choose to use this approach, do not forget to remove those same lines from `/etc/sysctl.conf`.

### Summary

In this article we have explained how to modify kernel runtime parameters, both persistent and non persistently, using `sysctl**, **/etc/sysctl.conf`, and files inside `/etc/sysctl.d`.