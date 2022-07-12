
![[Pasted image 20220626012530.png]]

#### Creating a Load Balancer

1. Choose b/w the different types like **HTTP Load Balancer** , **TCP Load Balancer** and **UDP Load Balancer**.
2. We need to Configure the 
	- **Backend**:
			- Add the Name
			- Protocols
			- Instance group
			- Balancing mode
			- We can have multiple backends configured, i.e multiple instance groups
			- Configure a **cloud CDN**
			- 
	- **Host and Path rules**
			- Add mutiple Paths for the routing.
	- **Frontlend Configuration**
			- Ports where you want the load balancer to recive the requests
	 