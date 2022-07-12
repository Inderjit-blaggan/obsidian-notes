## Compute Engine Service:
![[Pasted image 20220626004720.png]]
![[Pasted image 20220626004747.png]]

- IP Addressing in Compute Engine Service:

![[Pasted image 20220623164813.png]]


#### Static IP Address ( Elastic IP Address in AWS):

1. Go to `VPC` and search for `IP Addresses`


#### Userdata or startup script in GCP:
![[Pasted image 20220623165529.png]]
```
#!/bin/bash
apt update
apt -y install apache2
echo "Hello world from $(hostname) $(hostname -I)" > /var/www/html/index.html
```

1. Create Instance >> Management >> Automation 
   ![[Pasted image 20220623165731.png]]


#### Simplifying VM Creation in Google Cloud platform using **Instance Template** ( )
- It is similar to creating a instance, we need to pass all details and we can add `startup.script` to the template.
- Disadvantage: incase we have **more software to be installed on VM creation** it will **take more time**. thus we can think of using **Custom Images** instead.

#### Reducing launch TIme with Custom Image ( similar to AMI)
![[Pasted image 20220623170840.png]]

- To create Image, create a VM, install all packages. click on **Actions >> Create Image**
**Note:** It is always recommeded to stop the install and then take the image, while we can take the image on fly.

### Cost Saving in GCP instaces
- **Sustained Use Discounts:** we need not to do any configuration change for this, and discount increase on usage.
![[Pasted image 20220623172502.png]]
- **Commited Use Discounts:** (Reserved instance in AWS)
  ![[Pasted image 20220623172409.png]]
- **Preemptibel VM**( spot instances in AWS)
  ![[Pasted image 20220623172327.png]]


#### Creating a Dedicated tenancy in GCP compute Engine service using sole-tenant nodes
![[Pasted image 20220626004238.png]]
1. We can goto **Compute Engine Service** >> **Sole-tenant Nodes** >> Define the node template >> configure Autoscaling.
	   - It works on the node affinity Label.


#### Instance Groups
![[Pasted image 20220626005305.png]]
![[Pasted image 20220626005327.png]]

![[Pasted image 20220626005624.png]]

Demo Steps:

1. Create a **instance template** in the compute engine service.
2. Create **Instance Groups**. 
   - Choose the instance group.  Managed instance group are of 2 types, **stateless**(web application)  and  **statefull** (db kind of server) while we have 1 **unmanaged instance group**. 
   - We need to make choice for 1 of the instance group.
   - Now we need to choose the **instance Template**
   - then we need to choose the **location** (single zone or multiple zone)
   - **Configure autoscaling** : On, off or scale out (only add instnaces)
   - **Cool down period:**  after how long the metics should be checked after scaling the instances
   - **Scale-in controls:** setting limits for scaling out and scaling in.
   - **Auto healing:**  check based on health check and remove unhealthy vm and create new.
   - Create the group, it usually take 5-10 mins
3. 