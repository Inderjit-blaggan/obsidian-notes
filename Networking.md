**#ProxyServers:**
- [Using Squid in Linux As A Proxy Cache Server | Complete Project | Eduonix](https://www.youtube.com/watch?v=W2pqO3l-Uck)
- [Setting up ssl bump intercept on squid](https://medium.com/@steensply/installing-and-configuring-squid-proxy-for-ssl-bumping-or-peek-n-splice-34afd3f69522)


A proxy server is in a previous section in this class we have seen the reverse proxy server and we used ngnix for  that and we mentioned that: 
- A reverse proxy server or a service that  stands in front of your web application to receive requests from clients, routes  those requests to a back-end server and receives the reply from that back-end server and routes it back to the client  and we also mentioned that this provides a layer of security and a layer of performance because your application  servers are not directly exposed to the Internet. 
- Well a proxy server can also  work on the front-end part of the communication, you can have as a client you can have a proxy server and perhaps  this is the more well-known usage of a proxy server when the term proxy server is used you immediately think of the  proxy server that you might be using in your corporate LAN or in your organization it is that server that is  responsible for receiving your requests to visit a remote website or a remote resource or a remote web resource,  handles this request it makes this request on your behalf and it receives  the response from the remote web server and routes this response back to you as  a client.
- So why is it also called a caching server that is because part of  the proxy server work is that it caches the response that it receives from the  web server just the same as the reverse proxy works when it caches static content if you remember from the reverse  proxy section we'll be said that nginx caches the requests that it receives and also have a cache of the  static files too serve them to the clients quickly without having to resort back to the  application server. 
- The proxy server on the front and side also does the same thing but of course in the reverse  manner it caches the requests that you frequently make so that when you request  the same resource again from the proxy server it does not go and make the  request once more again, it just serves the already cached competent data tasks.

So in the nutshell there are numerous reasons for using a proxy server on the  client side let's go through them quickly:
1. first it provides internet connection sharing to multiple users so  if you have multiple users in the same LAN or didn't say network they can all have internet connection without having  to provide Braille IPS for them they just have to configure their browsers to point to the proxy server that you have  installed and they are already set 
2. It enhances performance by caching responses you have just mentioned that  point 
3. It controls access to the Internet by denying specific URLs or domains and  that is again one of the advantages of using a proxy server which is that you can censor malicious sites or  inappropriate sites so that users are denied their access and that is most commonly used in work environments  schools and universities and so on so if you are in a bank for example you  ought not be opening social media for example Facebook or Twitter for example so a proxy server may be used to deny  access to those social networking sites while you are at work 
4. The next feature is that it adds a layer of  security by hiding client machines and IP addresses behind it so think of it  now you're browsing the Internet your IP address is visible to your to the remote web server that you visit and also to  anybody who is intercepting this connection so you might be vulnerable to  an attack but a proxy server places itself in front of you as client and it  connects to the remote web server using its own IP address so your private IP address or your client IP address is  hidden it is not shown to the remote web server or to anybody who is intercepting  the connection and if an attack happens then the furthest point that which is the proxy server 
5. Finally it  can circumvent regional internet access restrictions that is if we are talking about public proxy servers I bet you  I've heard that term before a public proxy server is a proxy server that is available for free on the internet some  of them are free and some of them are paid all what we have to do is just point your browser to that proxy and  using this you can pass through any regional restrictions that are in place where you are living so for example can  pass a website that is banned by your government by using a proxy server.

The server that we are going to use and as an example for this lab is known as  squid squid is one of the most well known proxy and actually it can work as  a reverse proxy as well for Linux 
- Installing squid can be done using the  native OS package manager or it can be compiled from source 
sudo apt-get install squid3

- Start squid and enable it and check using: ps -ef| grep squid


**Configure Squid Proxy Server in Redhat Enterprise Linux 7/8:**  #squidproxy
- [Yourtube Video link:](https://www.youtube.com/watch?v=Yq3INojVZcY)

1. Login in server as the root user and configure local yum repository if not configured earlier. `` yum repolist``
- (if you don't know how to configure it, you can watch our videos by clicking on links provided below.) 
	- [Yum configure in RHEL 7:](https://www.youtube.com/watch?v=-N09WSCNcso)
	- [Yum configure in RHEL 8:](https://www.youtube.com/watch?v=cXqL5lLHM_o)

2. Now **install squid packages** from yum repository in the server: ``yum install -y squid*``

3. Start  the **squid service and enable** it. : 
- ``systemctl start squid``
- ``systemctl enable squid``

4. Add the **squid port in firewall** and **restart it**.
- ``firewall-cmd --permanent --add-port=3128/tcp``
- ``firewall-cmd --reload``

5. Note down your **server local  ip** address: ``ifconfig`` or ``ip addr``

6. Now **make chages in the configuration file** of squid as shown below: ``vim /etc/squid/squid.conf``
```
acl localnet src 192.168.1.10/24
http_access allow localnet
# comment: localnet is just a name, and can be any name in both  the lines
```

7. If you **want to block some of the specific website** in your LAN then **make the entry of ``blocksites`` file** in squid configuration file: ``vim /etc/squid/squid.conf``
```
acl blocksites url_regex "/etc/squid/blocksites"
http_access deny blocksites 
# comment: blocksites is just a name, and can be any name in both  the lines
```
- Make the **entry of block sites** in ``/etc/squid/blocksites`` to block the sites.
```
# vim /etc/squid/blocksites

.facebook.com
.twitter.com
.flipkart.com

```

8. **Restart the squid service.**: ``systemctl restart squid``

9. Now open firefox and make required changes: 
``Firefox -) Settings -) Preferences -) Advance -) Network -) Connections -) Select Manual Proxy Conf -) Provide the IP of your server which you have noted down & Port Number (3128).
``
10. Open firefox and test whether caching imporves your surffing experience & whether you can still access the blocked sites or not?


11. You can also **block files with the help of squid servers**. Make rules for the same in configuration file: ``vim /etc/squid/squid.conf``
```
acl blockfiles urlpath_regex "/etc/squid/blockfiles.acl"
http_access deny blockfiles
```
- Now open **/etc/squid/blockfiles.acl** in vi editor and make an entry for the files to be blocked.
```
\.torrent$
\.mp3.*$
\.3gp.*$
\.mp4.*$
```

12. **Set work hours.** ``vim /etc/squid/squid.conf``
```
acl work_hours time 10:00-19:00
http_access deny work_hours
```

13. **Set download speed :** vim /etc/squid/squid.conf
```
acl speedcontrol src 192.168.1.109/24
delay_pools 1
delay_class 1 2
delay_parameters 1 524288/524288 52428/52428
delay_access 1 allow speedcontrol
```

