
****Hashing , SSL, Encryption:

- Use [mkpasswd](https://rakeshjain-devops.medium.com/how-to-create-sha512-sha256-md5-password-hashes-on-command-line-2223db20c08c) to create secrure encryption code for your password: 
```
v@DESKTOP-JE0IJPU:~$ mkpasswd -m sha-512 Password@123
$6$iOCI6jLgjVDA$knEivV81.ImnZ3abY.y6kAybsMASG8rvmYm75lCo5RA8mZGrCGiSbiyDgiIHLWdt1LhhUZzXg8WU3HWs4GtU4/
v@DESKTOP-JE0IJPU:~$
```

 
 **Genrating SSH Keys:**

1. Generate the SSH key on the linux system.: ``ssh-keygen -t rsa -b 4096  -C "EMAIL@COM"``

3. Cat the pub file create in the above step for the user:  `cat  /root/.ssh/id_rsa.pub`