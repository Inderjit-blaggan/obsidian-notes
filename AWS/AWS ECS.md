
#ecsdoc
   ![[Pasted image 20220523110749.png]]
Amazon Elastic Container Service (Amazon ECS)  is a highly scalable, **high-performance container orchestration service** that supports Docker containers and allows you to easily run and scale containerized applications on AWS.

- Amazon ECS **eliminates the need for you to install and operate** your own container orchestration software, manage and scale a cluster of virtual machines, or schedule containers on those virtual machines.
- ECS is also deeply integrated into the rest of the AWS ecosystem.

**Task Defination:**

- To prepare your application to run on Amazon ECS, you create a **task definition**. 
- The **task definition is a text file, in JSON format, that describes one or more containers**, up to a maximum of ten, that form your application.
- It can be thought of as a **blueprint for your application**. Task definitions **specify various parameters** for your application. 
- Examples of task definition parameters are which containers to use, which launch type to use, which ports should be opened for your application, and what data volumes should be used with the containers in the task. 
- The specific parameters available for the task definition depend on which launch type you are using. 
- For more information about **creating task definitions,** see Amazon ECS Task Definitions 
  
The following is an **example of a task definition containing a single container that runs an NGINX web server** using the Fargate launch type. 
For a more [extended example](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/example_task_definitions.html) demonstrating the use of multiple containers in a task definition, see 

```
{
    "family": "webserver",
    "containerDefinitions": [
        {
            "name": "web",
            "image": "nginx",
            "memory": "100",
            "cpu": "99"
        }
    ],
    "requiresCompatibilities": [
        "FARGATE"
    ],
    "networkMode": "awsvpc",
    "memory": "512",
    "cpu": "256"
}
```

**Amazon ECS cluster**
- Ecs cluster is a **logical grouping of tasks or services.**
- If you are **running tasks or services that use the EC2 launch type**, a cluster is also a **grouping of container instances.**
- If you are using **capacity providers,** a cluster is also a **logical grouping of capacity providers**.
- A Cluster can be a **combination of Fargate and EC2 launch types**.
- When you first use Amazon ECS, a default cluster is created for you, but you can create multiple clusters in an account to keep your resources separate.

**Tasks and Scheduling**
- A **task is the instantiation** of a **task definition** within a cluster. After you have created a task definition for your application within Amazon ECS, you can specify the number of tasks that will run on your cluster. 
- Each task that uses the Fargate launch type has its own isolation boundary and does not share the underlying kernel, CPU resources, memory resources, or elastic network interface with another task. 
- The **Amazon ECS task scheduler is responsible for placing tasks within your cluster**. 
- There are several different scheduling options available. For example, you can **define a service that runs and maintains a specified number of tasks** simultaneously. For more information about the different scheduling options available, see [Scheduling Amazon ECS Tasks](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/scheduling_tasks.html)  .

**ECS Services**
- Amazon ECS allows you to **run and maintain a specified number of instances of a task definition** simultaneously in an Amazon ECS cluster. This is called a service. 
- If any of your tasks should fail or stop for any reason, the Amazon ECS service scheduler launches another instance of your task definition to replace it and maintain the desired count of tasks in the service depending on the scheduling strategy used. 
- In addition to **maintaining the desired count of tasks in your service**, you can **optionally run your service behind a load balancer**. The load balancer distributes traffic across the tasks that are associated with the service.

There are two **service scheduler strategies available:**

1. **REPLICA:**  The **replica scheduling strategy** places and **maintains the desired number of tasks across** your cluster. 
- By **default, the service scheduler spreads tasks across Availability Zones.** 
- You can use task placement strategies and constraints to customize task placement decisions. 
- For more information, see [Replica](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs_services.html#service_scheduler_replica) .


2. **DAEMON:**
 - The **daemon scheduling strategy deploys exactly one task on each active container instance** that meets all of the task placement constraints that you specify in your cluster. 
 - The **service scheduler evaluates the task placement constraints for running tasks** and **will stop tasks that do not meet the placement constraints**.
 - When using this strategy, there is no need to specify a desired number of tasks, a task placement strategy, or use Service Auto Scaling policies. 
 - For more information, see [Daemon](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs_services.html#service_scheduler_daemon) .

**AWS Fargate**
- [Fargate](https://aws.amazon.com/fargate/)  is a technology for Amazon ECS that allows you to run containers without having to manage servers or clusters. 
- With AWS Fargate, you **no longer have to provision, configure, and scale clusters of virtual machines to run containers.** 
- This removes the need to choose server types, decide when to scale your clusters, or optimize cluster packing.
- AWS Fargate removes the need for you to interact with or think about servers or clusters. Fargate lets you focus on designing and building your applications instead of managing the infrastructure that runs them.

**Service Discovery**
![[Pasted image 20220523112917.png]]
- Because **containers are immutable by nature,** they can churn regularly and be replaced with newer versions of the service. 
- This means that there is a **need to register the new and deregister the old/unhealthy services.** To do this on your own is challenging, hence the **need for service discovery**.

**AWS Cloud Map** is a cloud **resource discovery service**. 
- With Cloud Map, you can **define custom names for your application resources**, and it **maintains the updated location of these dynamically changing resources**. This increases your application availability because your web service always discovers the most up-to-date locations of its resources.
- **Cloud Map natively integrates with ECS**, and as we build services in the workshop, will see this firsthand. For more information on service discovery with ECS, please see [here](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/service-discovery.html) .


**Task placements and constraints** 
![[Pasted image 20220522010914.png]]
- So let's look at these as task placement process so that we have full idea on how this works.  
- So when a **task of type EC2 is launched**, ECS must determine where to add it, where to place it, where  to put it with constraints of CPU memory and available port.  
- What I mean by that is if we have, for instance, **a bunch of ec2 instances working and they have  a bunch of tasks and the ECS Service wants to start a new container**, it needs to figure out, okay,  where I'm going to put this new container.  
- This is when we can use a task placement and constraints to hint, say, okay, this is where this container  should be placed, right?  
- Now similarly, when the service **needs to scale down, it needs to determine which task to delete,  which task terminate as well.**  
- So for that, we can look at defining task placement strategy and task placement constraints.  
- It is important to remember that this only works for easy to launch types not forget because remember,  forget is serverless, right?  So we don't have two instances in the back end and each task will get its own app.  
- So task placement is based on best effort, which means if we can respect or accommodate this strategy,  then ECS will then find a place to run the containers.  


So when **Amazon places a task, it uses the following process to select container instances.**
![[Pasted image 20220522010934.png]]  
1. It will **find instances that will satisfy the CPU and memory and port requirements  in that task definition**.  Right.  
2. And then we'll **look at the constraints that were defined** to see if it can allow follow those constraints.  
3. And finally, it will **try to satisfy the task placement strategy**, which is best effort.  And once it has it, it will select one of the instances for task placement.  

Now, **let's look at some of the task placement strategies**.  

1. **Binpack**:  is based on the **least available amount of CPU or memory.**  So as you can see here, in this definition, you define the a placement strategy of the bin pack  for memory and it will try to maximize RAM on EC2 instance before adding task on to the next easy to  exist and also being packing it.  
- So essentially you can see we can have 2 ec2 instances running.  
- It will **fill up the first instance of EC2** and only once the ec2 on the **first instance is full**,  right?  That's when the tasks are going to be **added over to the next ec2 instance,** and 
![[Pasted image 20220522011733.png]]

2. **Random placement strategy**.  
- So in this case here, **tasks are placed randomly**.  
- So you have two AC two instances and the task is just placed randomly.  So it's a really easy one to grasp.  
![[Pasted image 20220522011751.png]]

3. **Spread strategy**:  So spread is really good when you **want to have high availability**.  
- So it will place the **task evenly based on the specified value** like so the specified value could be instance  ID to spread across many different availability zones.  
- Or we can spread the task across ECS availability zones, which means we will have an even amount of tasks  in A-Z.  
- So in this case it will be spread by A-Z.  So the task will be run across all the ec2 instances by, in this case, A-Z.  
  ![[Pasted image 20220522011834.png]]
  Now the great news is that all of **these task placement strategy can be mixed together.** 
  ![[Pasted image 20220522011904.png]] 
  - So we can have, in this case, a spread placement, followed by a strategy, followed by a spread inside  to really spread it out.  
    
  - Or we can have spread based on A-Z and have bin pack memory.  So it's really possible for you to have a task placement strategy that makes sense to you and it satisfies  your needs and requirements for the project that you're working on.  
  
  
  **Task placement constraints**.  
  ![[Pasted image 20220522011922.png]]
  - So these help us to define in this case, for example, **distance instance.**  Right.  
  - So this one here will place a task on distance, etc., which **means once a task is in one ec2 instance,  it cannot be placed anywhere else**.  
  
  
  - Another one would be the **member off.**  So this is to **satisfy an expression** that it will use a very special query language called CQ Language.  
  - And this is a little bit advanced where, for example, with this symbol here we.  Constrain or can create a constraint that will say that, okay, the easiest task that needs to be placed  on a certain instance type is required to be T-2, right?  So it could be T2, micro, T2, large and so on.  Right.  
  - As you see here, in summary, using all these tasks, placements and constraints, you can do a lot  of things.  
  - So you can define how you want your tasks to be run on your ECS clusters, which are backed by Ec2 instances.  And that really gives you a lot of flexibility to run easier services at a huge scale.  All right.  So I hope you liked these lectures and I'll see you in the next video.  
    
    
    **ECS Networking modes:**
    ![[Pasted image 20220522013743.png]]
    1. **None,** as the name implies, there will be **no external connectivity**, there  will be no port mapping.  And as you see, that probably shouldn't be your first choice.  
    - In fact, you probably will never use this mode in ECS 
    
    
2. **Bridge.**  So bridge is the **default mode**.  So what happens here is that inside of your ec2 instance, you have a docker virtual network.  
- Meaning what?  You create a task inside.  You will **get a task port and you can bind that port onto a host port** so that then the outer world can  connect to your task.  Right.  And in this case, the host port can be static or dynamic if you don't specify it.  
- The idea is that we can launch multiple tasks in the same instance, right?  And there will be dynamic port mapping.  
- And then we can combine this with an application balancer to have your networking figure out from a  client perspective.  
- So this is a default mode and it works fairly well as we have seen.  
  
  
  3. **Host mode** : Now, if you don't want to have a Docker virtual network, you want in this case to have maximum network  performance.  Then you need to have EC2 instance to run your task in host mode.  
     - In this case, you see that there is no Docker virtual network, which means the task itself is just  running on its own and it gets a task port in this case 80.  
     - So **there's no port binding in host mode,** networking mode.  So you would want to use this mode for extreme network performance.  
     - So when you know exactly the task you are going to run on it, then you will use this mode.  
     - So this is not something you would have by default.  
     - Of course, by default you would have a bridge or something else, which in this case would be a table  as VPC.  
       
       
       3. **VPC** is **required for fargate and optional for Ec2**.  
          ![[Pasted image 20220522013733.png]]
    - So in this case here you would have a **task within ec2 instance** and **then your task would get a dedicated  Eni** stands for Elastic Network Interface.  \
    - Now this interface will **provide a task with a unique IP**.  In this case we will have Port eight, 
    - for example.  So the combination of the two is the way to reach the task.  So in this case, we will have 172.16.4.5 and then append 80.  That would be how we would actually reach this task.  
    - So for example, if we were to launch another task within of course the same instance, that task would  get a new eni with a different IP.  
    - As you see in that example.  Right.  As you see in the diagram.  So here **there's no port mapping because each task will get one IP and one port and it will be a unique  IP per task.**  
    - Now, this obviously will give you the highest network performance because each task that is created  will have its own in ENI.  
    - And of course, as I said, there will be no port mapping because each task will have a unique IP.  
    - So unique IP per task. 
    - In **ec2 instances**, there is a **limitation**.  So if you are using **ec2 instance and the VPC mode,** then you are **limited by a limit of two instances  that have a maximum amount of network interfaces that can be attachable to your** ec2 instances so as  to make sure you're in good shape.  
    - Anytime you want to use this mode, I recommend that you go to the documentations to check, for example,  that **t2 small may have four eni** onto it. That means you can only **attach 4 tasks** like this in a class VPC mode.  
    - Okay, so if you're running with fargate or as we know, there's no ec2 instances, so each task will  get its own eni and own port, which means you need to make sure that your VPC have lots of available  IP addresses because you may run out of those as one task will be using one IP address.  
    
    
    
    
    
    
    
    
    
    
    
    #ecsarchitecture
    ![[Pasted image 20220522014035.png]]
    
**ECS architecture set up** 
- So in this infrastructure or in this architecture, rather, will make sure that we strictly separate  between public and private subnets, meaning there will be no direct access from the public interact  to the containers running in private subnets, which is very important, as we've explained before,  that we don't want to have direct access to our containers because that is not secure for obvious reasons.  
- So now we're going to set up a structure that will not allow that unsecure way of accessing our containers.  
- To demonstrate this infrastructure, we will create a dedicated VPC, expand it across two phases to  availability zones, and we will create two public subnets and two private subnets as well.  
- One in Az1 and another one in Az2.  
- So for **incoming traffic,** we will use an **application load balancer.**  
- And for an **outgoing traffic from the container** to the outer world, we will set up a **nat gateway.**  
- So at end our containers will run on fargate and this is within our private subnets.  
- So in **summary,** incoming traffic, as you see here, it will go through the Internet gateway and through  our load balancer and then to our private subnets and reach our containers and all the outgoing traffic  from our containers will go through in that gateway, then go to the Internet gateway and then to the  public Internet, as you see with the arrows that we have.  
- Now, the reason why we don't use this set up throughout this course is because there is additional  cost associated with this setup, because creating in that gateways can be very costly.  
  
  
  **ECS load balancing** 
  ![[Pasted image 20220522120201.png]]
  - **EC2** will be using **bridge network mode** and we  **get a dynamic port mapping.**  So we've seen this before.  
  - Now this **dynamic port mapping allows us to run multiple of the same task** in the same Ec2 instance.  Okay.  
  - So in this case here we can have an ec2 instance and as you can see there, the application balancer,  what it will do is it will find the right port for ec2 instances and ECS tasks.  
  - All of this is done through the dynamic port mapping capability.  
  - Now, from a **security perspective**, we must allow on the ec2 instances take a group any port from  the ALB.  
  - Now let's take a look at a diagram so this makes more sense.  
    
	- So we will have our ec2 instance and in this case will run a few containers.  And you can see these containers are Node.js containers and all of them have ports and you can see these  ports range from 30,000 onwards and these ports are all different depending on the Node.js container  we have here.  
	- So now what we want to do is we want to load balancer these for these will have a load balancer which  will expose port 80 or port two, four, four, three.  
	- Then thanks to load balancer dynamic porting feature, ALB can find in this case the correct port  to connect to our containers.  
	- So we've seen this again before, but now we'll put all of this in practice in the next lecture.  
	  
	**Load balancing in Fargate** 
	- Things are a little bit different because now in  fargate we can only use a as **VPC mode,** which **means each task will get a unique IP and same port.**  
	- So in this case here we can look at a diagram again.  
![[Pasted image 20220522120223.png]]

- You can see that each task that we have here has the same port. So the alb of the load balancer this time will have to redirect to the right IP in the right port automatically.  
  Thanks again to the task being registered automatically to the ALB.  
  - As far as security group is concerned, you must allow the new security group the task port from the  ec2 carrier group, which means you must allow the task port coming from the alb, the load balancer  to allow the traffic from the load balancer onto your Fargate task, which means you must allow the  task port coming from the Elb to allow the traffic from the Elb onto your fargate task.  
  - Okay, so that's it for application load balancer.  And next what we do is we will put all of this into practice
  
  
  
  
  
  **ECS Anywhere** 
  - is an extension of Amazon ECS that will allow **customers to deploy native Amazon ECS tasks in any environment.** 
  - This will include the existing model on AWS managed infrastructure, as well as customer-managed infrastructure. All this without compromising on the value of leveraging a fully AWS managed, easy to use, control plane that’s running in the cloud, and always up to date.

**ECS- Anywhere can solve the below use-cases:**

-   Manage legacy **monolithic applications in customer data centers**
-   Run containers in the **AWS Cloud and on-premises** in a consistent manner
-   Run applications on-premises for technical or compliance reasons
-   Manage applications at edge locations consistently, efficiently, and reliably

**ECS Anywhere Use cases:**
-   **_(Modernization)** Customers moving to containers and migrating their workloads to AWS._ Customers can now **containerize their workloads on-premises first**, make them portable, address on-premises dependencies, get familiar with AWS tools and get AWS-ready, followed by just updating the ECS services’ configuration from on-premises hardware to either Fargate or EC2.
  
-   **_(Hybrid)** Customers who need to **run containerized workloads on both AWS and on-premises.**_ Customers continue to run some workloads on-premises or other cloud environments for reasons such as data gravity, compliance, and latency requirements. Such customers need a single container orchestration platform for consistent tooling and deployment experience across all environments. 
  
-   _(Hybrid) **Customers who want to continue to utilize their on-premises infrastructure until their investments have fully amortized**._ Such customers are looking to use **their on-premises infrastructure as base capacity** while **bursting into AWS during peaks or as their business grows**. Over time, as they retire their on-premises hardware, they would continue to move the dial to use more compute on AWS until they have fully migrated.
-   _(IoT) **Customers wanting to run container workloads at multiple edge locations._** Such customers are looking to use ECS Anywhere to orchestrate containers at multiple edge locations. For example, these workloads could be **gathering raw data from machines, or raw images from drones** and processing them before sending to cloud.
  
**Supported platforms**
ECS-Anywhere can run on the below platforms:

- **Customer on-premise infrastructure** - Customers have their own on-premise infrastructure where they want to try the container the ECS anywhere is the right fit. Up to customer migrate to cloud and utilize their on premise investment.

- **AWS Outposts** - Customer is using AWS Outposts to have **virtual datacenter, co-location space, or on-premises facility** for a truly consistent hybrid experience infrastructure. ECS-Anywhere can fit in the AWS Outposts to support the container orchestration and management.

- **AWS Wavelength** - Customer is using mobile edge computing applications using the AWS Wavelength. ECS-Anywhere can be used to support the container orchestration and management on the AWS Wavelength.

- **AWS Local Zones** - Customer is using AWS Local Zones to **run their applications close to large population, industry, and IT centers**. 
	  - With AWS Local Zones, you **can easily run applications that need single-digit millisecond latency closer to end-users in a specific geography**. 
	  - ECS-Anywhere can fit in the AWS Local Zones to support the container orchestration and management.

- **AWS Region** - Customer is using AWS Region where they can **run their latency-sensitive applications using AWS services with in geographic proximity** to end-users. 
	 - ECS-Anywhere can be used to support the container orchestration and management on the customer AWS region infrastructure.



**Customer FAQs**

1. **Which platforms and operating systems does ECS Anywhere support?**
- You can use ECS Anywhere **with any VM (e.g. running on VMware, Microsoft Hyper-V, or OpenStack) or bare metal server running a supported Operating System (OS)**. 
	- The **ECS Agent** – software that allows a **host to connect with the ECS control plane** – is supported and tested for the LTS releases of: **Amazon Linux 2, Bottlerocket, Ubuntu, RHEL, SUSE, Debian, CentOS and Fedora.**

2. **Can I use ECS Anywhere with VMware Cloud on AWS (VMC)?**
- Yes, you can use **ECS Anywhere with VMC.** You can use the ECS-optimized Amazon Linux variants of VMware Virtual Machine Disk (VMDK) to launch instances. 
	  - These ECS-optimized VMDKs are pre-configured with the latest version of the ECS agent, Docker daemon, and Docker runtime dependencies.

3. **How do I ensure the link between my on-premises compute and AWS cloud is secure?**
- The **link between your on-premises compute and AWS cloud is secure** by default and hence you do not need to take any additional actions. 
	  - The ECS control plane running in the AWS region **orchestrates containers** by **sending instructions to the ECS agent installed on each registered** server over a secure link, which is authenticated using the instance IAM role credentials passed at the time of registering the server. 
	  - Additionally, the ECS agent uses the **AWS Systems Manager Agent** to automatically and securely **establish trust between the on-premises server and ECS control plane**; its connection to **AWS is encrypted with Transport Layer Security (TLS)**.

4. **What type of information flows from the on-premises compute back to the AWS region?**
- Only **information necessary for managing the containers is sent to the ECS control plane** running in the AWS region. 
  - As an example, **information about host health, container activity (launched, stopped), and container health checks (if configured) may be sent back to the AWS region.** 
  - This information **enables AWS to provide alerting on health and capacity**, and manage ECS tasks running on your on-premises compute. The contents of container memory, disk storage, or network traffic is not sent to the control plane.

5. **Can I have on-premises compute, EC2 instances, and Fargate in the same ECS cluster?**
- Yes, you can have on-premises compute, EC2 instances, and Fargate in the same cluster. This makes it easy for you to migrate your ECS workloads running on-premises to ECS in an AWS region on Fargate or EC2.

6. **Can I use the same ECS task definition for on-premises environments that I use to run ECS tasks on Fargate and/or EC2 instances?**
- Yes. An **ECS task definition is a specification for a group of containers that must run co-located.** 
	  - ECS task definitions can be **created so that they are compatible with on-premises compute, Fargate, and EC2, all in a single task definition**.

7. Can I use ECS Anywhere to run containers in air-gapped/disconnected environments?

No. ECS offers a cloud based and fully managed container orchestration solution that resides in an AWS region. Hence, it requires your on-premises compute to have a stable internet connection to communicate with the in-region ECS control plane.

Which other ECS integrations with AWS services can I use when using ECS Anywhere?

With ECS Anywhere, you can get CloudWatch Metrics for your clusters and services, use the CloudWatch log driver to get your containers’ logs, as well as access the ECS CloudWatch Event stream to monitor your clusters’ events. You can use Task IAM Role and Task Execution Role to give your containerized applications fine-grained access control to AWS resources and use Cloud Map for service discovery. Additionally, if you are using Direct Connect or AWS VPN, you can use services such as Application Load Balancer (ALB) and Network Load Balancer (NLB) for load balancing, and PrivateLink if you do not wish to communicate with ECS’s public endpoint over an internet or Network Address Translation (NAT) gateway.

Which third party solutions can I use when using ECS Anywhere?

ECS Anywhere works with the same tools that ECS in the cloud does, including Terraform, Consul, Datadog, Spinnaker, Jenkins, and many others.

Some of the services that will not be supported part of ECS Anywhere GA?

Application load balancing, autoscaling, cloudmap service discovery and volume attachments (EBS & EFS) to ECS Tasks as these services are running on-prem.

Can I run GPU-based workloads on ECS Anywhere?

Yes. You can enable GPU instances by adding the --enable-gpu flag to the Amazon ECS Anywhere installation script. Once the script is installed, you will be able to assign a number of GPUs to particular containers in the task definition. Amazon ECS uses this as a scheduling mechanism to pin physical GPUs to the desired containers for workload isolation and optimal performance. You can use Nvidia and CUDA drivers with Amazon ECS Anywhere by following the steps to install the drivers as provided here .

**Scaling in ECS:**
![[Pasted image 20220525091450.png]]
- So now that we have load balancing working, we want to be able to **scale our services to accommodate    more load over time** because that is the main idea I want to scale up.    
- Now the metrics such as CPU and RAM, all of these are actually tracked in cloud watch at the ECS level.    
- Now we can set up a **service auto scaling to have something like**, for example, 
	  - **target tracking** which    essentially targets a specific average cloud watch metric, 
	  - can then do that for steps scaling, which    essentially scales based on cloud watch 
	  - Alarms that you may have set up or **event scheduled scaling**,    which is based on predictable changes.    
- Like if you expect to have a lot of traffic coming to a website at 11 a.m., then you could set that    up so that it can accommodate the new amount of traffic coming in.    
- Now, you should know, though, that even though you are scaling at a task level, it doesn't mean    you are scaling those instances as well, because it's a bit more difficult when you have an ECS cluster    backed by EC2 instance to scale that because you need to both out scale the ECS services and the Ec2   instances.    Of course, we will see in the next slide how we can address that.    
- On the other hand, if you are running a fargate service, then all of this is really straightforward    because if you remember correctly, fargate is sort of less so out of scale is just a matter of scaling    your CPU and RAM respectively.    

 
  **Now what if you want to scale your EC2 clusters?**    
  ![[Pasted image 20220525091434.png]]
  - So in order to scale your ec2 clusters, you will use what's called a **capacity provider**.    
  - So a **capacity provider** is used in **association with a cluster** to determine the infrastructure that a    task runs on for ECS in front of it users, the target and the funded spot capacity providers are added    automatically, so you don't have to really do much to get this to work.    
  - Now for Amazon, it says on EC2, so backed by EC2 instances, you need to associate the **capacity provider    with an auto scaling group**.    
  - Now, when you **run a task or a service, you're refining capacity, provide strategy to prioritize** in    which provider to run on.    This allows, of course, the **capacity provider to automatically provision infrastructure for you so**    you don't have to do that itself.    
  - So the infrastructure itself is built so that it will create that infrastructure that you need to how    to scale.    
    ![[Pasted image 20220525091415.png]]
    - Now, let's say we have **two ec2instances** and as you can see, the **average CPU is less than 30%.**    But at the same time, you can see that these instances have no more capacity to run in your task.    
    - So **when a new task arrives,** if you had set up this way, if you had **set up a to scale based on CPU    percentage**, it would have **not scaled.** 
    - Right, because it's over capacity.    So with the capacity provider in this case, what would happen is that **it would kick in.**    Right.    And **you see two instance, as you can see there, where to place this new task in that auto scaling    group** and be able to run it.    
    - All of that will happen automatically with a class or a capacity provider because it will know that    **it needs to be triggered because it will see that the average CPU usage is 30%, but we have no more    capacity**.    
    - Therefore, there's a **new task coming than the cluster capacity provider** will kick in and create a new    instance and make sure that this new task can run.    Now, the great thing here, if you have the class provider of tape, forget then the new task will    be launched in the Fargate container thanks to that capacity provider.
   
So really you don't have to do much when it comes to fargate it.    
- So the bottom line really here is as follows If you want to scale service, you should use ECS service    auto scaling, which works amazingly for Fargate.    
- As you see, it is a bit more difficult for ECS cluster backed by ec2 instances and if you do    so you end up using a an ECS cluster backed by EC2 instances.    
- That means then you have to make sure this is very important.    
- You have to make sure to provide a cluster capacity provider to automatically launch the new ec2    instances when you have new tasks being placed on to new clusters when there's no more capacity, as    we saw in the beginning of this animation that I put together here.    


**How to dynamically commission and decommission Ec2 instances based on utilization.** 
![[Pasted image 20220525112920.png]]
- Now, the utilization is due to two factors.    
	  i. One is role scale out of tasks.    
	  ii. The next one is adding new services and tasks into our EC2.    
	  
What this means is we want to be able to react as flexibly as possible with our ec2  instances and    provide enough resources to run all of the requested tasks.    
- The important thing here is that we need to consider as well the task placement of the task definition,    right?    Because if a scale out is required, right, because of a certain strategy, placement strategy, the    placement strategy influences how these tasks are being distributed across EC2 places.    

**So with all of these, we get a few benefits.    So what do we get from this flexibility per say?**    
1. First of all, we're going to be able to get rid of the bottleneck of EC2 resources limits at provisioned    tasks.  and we're also going to be able to get a fully agile cluster when combined with task order scaling.    
- So we gain a lot from this flexibility that we add when we make sure that our instances can out of scale    as well.    And this is really awesome because now we don't have to manually intervene to speed up the bringing    up of new resources to the clusters when we need.    
  ![[Pasted image 20220525112950.png]]

**Well, let's go ahead and take a look how such infrastructure can be created or how will it be created.**
- So we'll have an ECS cluster with an auto scaling group attached to the cluster.    
- And in this case, you can see that we're running one ec2 instance in our auto scaling group.    
- Now, when we need to react to the resource request from the ECS scheduler, accordingly, we need a structure    that creates the link right between the ECS and the outer scaling group.    
- So for that, that's why we have what's called a **capacity provider**.    
- So what a **capacity provider does it glues the ECS and the ASG together** so that this commissioning and    decommissioning of tasks and so forth and services in our cluster happens smoothly and automatically.    

**Now, how does this work?    How does the capacity provider work?**
![[Pasted image 20220525113011.png]]
- The **scaling activity** is based on **observing the cloud watch alarm, which is created when we first created    the capacity provider**.    Right. All of that is created automatically.    The alarm it is.    
- And this cloud watch alarm keeps track of the **CPR capacity provider reservation.**  So all in all, the capacity provider reservation is calculated by the 
 
	**number required of instances / number of available instances x 100**
			  
- Now, it's a simple equation per say, but there's a lot that goes through it in the back end.    And it's a difficult thing to do because there are a lot of moving parts right internally.    
- But the capacity provider is able to orchestrate all of that.    
- But in all of this task, placement is also being considered so that the scaling is resolved accurately.    
- So **task scaling policies** allows us to say, well, I want to attach the being stack policy or any other    policy that we have seen before.    
- Right.    So it's a very important as well.    So it's part of the equation, policy, task placement.    It is.    
  
- So basically what happens is if this **cloud watch alarm is triggered**, then the **ECS will either add right    or remove etc**. instances from our auto scaling group depending on which alarm was triggered, the scale    in or the scale out.    
  ![[Pasted image 20220525113032.png]]
- Now let's go ahead and take a look at the configuration steps that we will take here.    
1. So first, we are going to **check the of scaling group configuration**, make sure that everything's okay,    and then we're going to create the actual capacity.    
	- The **actual capacity provider**, by attaching a auto scaling group 
	- will be able to **enable the manage scaling.**    
	- We're going to **specify the utilization percentage** when to trigger that scaling
	-  will **disable termination    protection** in this demo scenario, of course, just to make sure things go smoothly.    
2. And then we're going to **create a new service.**    
	- Once we create that new service, we're going to **link to the capacity provider** that we just created 
	- and then **add a task placement strategy.**   Which is going to be bin pack.    Okay.    
  
  Of course, we need to test things so we can test by increasing or decreasing the number of tasks in    the service configuration.    And we're going to go to cloud, watch alarm and see all of these two instances.    So that way we can see that this is indeed working.    All right.    
  
  
  **Capacity Provider in Deeper Dive:** [ecs-capacity-provider-dee-dive](https://aws.amazon.com/blogs/containers/deep-dive-on-amazon-ecs-cluster-auto-scaling/)
  
  ### Design goals
- Based on the feedback we had received from customers, we set out with three main design goals for **Cluster Auto Scaling**
	1. Design goal #1_: CAS should **scale the ASG out (adding more instances) whenever there is not enough capacity to run tasks** the customers is trying to run.
	
	2.  _Design goal #2_: CAS **should scale in (removing instances) only if it can be done without disrupting any tasks** (other than daemon tasks). 

	3. _Design goal #3_: **customers should maintain full control of their ASGs, including the ability to set the minimum and maximum size, use other scaling policies, configure instance** types, etc. 