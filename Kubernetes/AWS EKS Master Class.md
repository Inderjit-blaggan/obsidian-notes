- [**AWS EKS Kubernetes - Masterclass Github** ](https://github.com/stacksimplify/aws-eks-kubernetes-masterclass](https://github.com/stacksimplify/aws-eks-kubernetes-masterclass)
- [**Docker Fundamentals GitHub** ](https://github.com/stacksimplify/docker-fundamentals](https://github.com/stacksimplify/docker-fundamentals)
- [**Kubernetes Fundamentals GitHub**]([https://github.com/stacksimplify/kubernetes-fundamentals](https://github.com/stacksimplify/kubernetes-fundamentals)
- [Install kubectl from AWS Doument:](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html)
- [Install EKS Ctl ](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html)

**GitHub Repository Links**

-   **Course Main Repository:** [**terraform-on-aws-eks**](https://github.com/stacksimplify/terraform-on-aws-eks)
    
-   **Course Presentation:** [**Presentation Slides PPT**](https://github.com/stacksimplify/terraform-on-aws-eks/tree/main/course-presentation)
    
-   **Kubernetes Fundamentals GitHub Repository:** [**kubernetes-fundamentals**](https://github.com/stacksimplify/kubernetes-fundamentals)
    
-   **Kubernetes Fundamentals Presentation:** [**Presentation Slides**](https://github.com/stacksimplify/kubernetes-fundamentals/tree/master/presentation)
    

**Direct Links**

-   https://github.com/stacksimplify/terraform-on-aws-eks
    
-   https://github.com/stacksimplify/terraform-on-aws-eks/tree/main/course-presentation
    
-   https://github.com/stacksimplify/kubernetes-fundamentals
    
-   https://github.com/stacksimplify/kubernetes-fundamentals/tree/master/presentation

### Creating a EKS cluster using eksctl  
- [githib link to readme page](https://github.com/stacksimplify/aws-eks-kubernetes-masterclass/tree/master/01-EKS-Create-Cluster-using-eksctl/01-02-Create-EKSCluster-and-NodeGroups)

1. Create the **Cluster control plane**. 
```
eksctl create cluster --name=eksdemo1 \
                      --region=us-east-2 \
                      --zones=us-east-2a,us-east-2b \
                      --without-nodegroup 
```

**Note:** This will not create the node groups as we have used the `--without-nodegroup`  Flag which prevent the node group creation. If incase we dont add the Flag it will create 2 nodes and setup production level nodes.

2. List the cluster using command: 
```
v@DESKTOP-JE0IJPU: eksctl get clusters --region us-east-2
NAME            REGION          EKSCTL CREATED
eksdemo1        us-east-2       True
```

3. Now to enable use of the IAM rules with the EKS cluster, we must create and associate the **IAM OIDC Provider**
```
# Template
eksctl utils associate-iam-oidc-provider \
    --region region-code \
    --cluster <cluter-name> \
    --approve

# Replace with region & cluster name
eksctl utils associate-iam-oidc-provider \
    --region us-east-2 \
    --cluster eksdemo1 \
    --approve
```

4. Now **create the ec2 Key pair**, so as we can associate them to the Ec2 nodes, which will work as the worker node to the EKS cluster
5. Create Node Group with additional Add-Ons in Public Subnets: 
	-  These add-ons will create the respective IAM policies for us automatically within our Node Group role.
 ```
# Create Public Node Group   
eksctl create nodegroup --cluster=eksdemo1 \
                       --region=us-east-2 \
                       --name=eksdemo1-ng-public1 \
                       --node-type=t3.medium \
                       --nodes=1 \
                       --nodes-min=1 \
                       --nodes-max=4 \
                       --node-volume-size=20 \
                       --ssh-access \
                       --ssh-public-key=sre-test-cask \
                       --managed \
                       --asg-access \
                       --external-dns-access \
                       --full-ecr-access \
                       --appmesh-access \
                       --alb-ingress-access 
```
- Add the values for the flags ` --ssh-public-key=<key-pair-name>`

6. Now check the nodes created using command:
```
v@DESKTOP-JE0IJPU:$ eksctl get nodegroup --cluster=eksdemo1 --region us-east-2
CLUSTER         NODEGROUP               STATUS  CREATED                 MIN SIZE        MAX SIZE        DESIRED CAPACITY        INSTANCE TYPE   IMAGE ID        ASG NAME                               TYPE
eksdemo1        eksdemo1-ng-public1     ACTIVE  2022-06-28T19:28:47Z    1               4               1                       t3.medium       AL2_x86_64      eks-eksdemo1-ng-public1-96c0d5e4-cee4-6d5c-2ca5-70588f2ac4f1    managed
```

7. Update the kubeconfig file for the cluster using command:
```
v@DESKTOP-JE0IJPU:$ aws eks update-kubeconfig --region us-east-2  --name eksdemo1
Added new context arn:aws:eks:us-east-2:000699977340:cluster/eksdemo1 to /home/v/.kube/config

v@DESKTOP-JE0IJPU:$ kubectl get svc
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.100.0.1   <none>        443/TCP   59m

v@DESKTOP-JE0IJPU:$ kubectl get nodes -o wide
NAME                                           STATUS   ROLES    AGE     VERSION               INTERNAL-IP      EXTERNAL-IP     OS-IMAGE         KERNEL-VERSION                 CONTAINER-RUNTIME
ip-192-168-61-231.us-east-2.compute.internal   Ready    <none>   5m53s   v1.22.9-eks-810597c   192.168.61.231   18.220.50.196   Amazon Linux 2   5.4.196-108.356.amzn2.x86_64   docker://20.10.13
```

8. Our kubectl context should be automatically changed to new cluster: `kubectl config view --minify`



### Delete the EKS cluster:

- [Github README.md file for the Deletion instructions](https://github.com/stacksimplify/aws-eks-kubernetes-masterclass/tree/master/01-EKS-Create-Cluster-using-eksctl/01-04-Delete-EKSCluster-and-NodeGroups)

1.  Delete Node Group
-   We can delete a nodegroup separately using below `eksctl delete nodegroup`

```
# List EKS Clusters
eksctl get clusters

# Capture Node Group name
eksctl get nodegroup --cluster=<clusterName>
eksctl get nodegroup --cluster=eksdemo1

# Delete Node Group
eksctl delete nodegroup --cluster=<clusterName> --name=<nodegroupName>
eksctl delete nodegroup --cluster=eksdemo1 --name=eksdemo1-ng-public1
```

2. Delete Cluster
-   We can delete cluster using `eksctl delete cluster`
```
# Delete Cluster
eksctl delete cluster <clusterName>
eksctl delete cluster eksdemo1
```



# EKS - Create EKS Node Group in Private Subnets
- [Github doc and instructions](https://github.com/stacksimplify/aws-eks-kubernetes-masterclass/tree/master/07-ELB-Classic-and-Network-LoadBalancers/07-01-Create-EKS-Private-NodeGroup)

**Step-01:** Introduction

-   We are going to create a node group in VPC Private Subnets
-   We are going to deploy workloads on the private node group wherein workloads will be running private subnets and load balancer gets created in public subnet and accessible via internet.

**Step-02:** **Delete existing Public Node Group** in EKS Cluster

```
# Get NodeGroups in a EKS Cluster
eksctl get nodegroup --cluster=<Cluster-Name>
eksctl get nodegroup --cluster=eksdemo1

# Delete Node Group - Replace nodegroup name and cluster name
eksctl delete nodegroup <NodeGroup-Name> --cluster <Cluster-Name>
eksctl delete nodegroup eksdemo1-ng-public1 --cluster eksdemo1
```

**Step-03:** **Create EKS Node Group** in **Private Subnets**

-   Create Private Node Group in a Cluster
-   Key option for the command is `--node-private-networking`

```
eksctl create nodegroup --cluster=eksdemo1 \
                        --region=us-east-2 \
                        --name=eksdemo1-ng-private1 \
                        --node-type=t3.medium \
                        --nodes-min=1 \
                        --nodes-max=4 \
                        --node-volume-size=20 \
                        --ssh-access \
                        --ssh-public-key=sre-test-cask \
                        --managed \
                        --asg-access \
                        --external-dns-access \
                        --full-ecr-access \
                        --appmesh-access \
                        --alb-ingress-access \
                        --node-private-networking                       
```
- The Flag `--node-private-networking` makes the nodegroup private and also change the `--ssh-public-key=sre-test-cask`  Key pair value


**Step-04:** Verify if **Node Group created in Private Subnets**

- Verify External IP Address for Worker Nodes
-   External IP Address should be none if our Worker Nodes created in Private Subnets

```
kubectl get nodes -o wide
```

- Subnet Route Table Verification - Outbound Traffic goes via NAT Gateway

-   Verify the node group subnet routes to ensure it created in private subnets
    -   Go to Services -> EKS -> eksdemo -> eksdemo1-ng1-private
    -   Click on Associated subnet in **Details** tab
    -   Click on **Route Table** Tab.
    -   We should see that internet route via NAT Gateway (0.0.0.0/0 -> nat-xxxxxxxx)



# AWS - Classic Load Balancer - CLB

**Step-01:** Create **AWS Classic Load Balancer** Kubernetes Manifest & Deploy

-   **04-ClassicLoadBalancer.yml** : [local laptop path inside learning folder](/mnt/f/Learning/Kubernetes/aws-eks/aws-eks-kubernetes-masterclass/07-ELB-Classic-and-Network-LoadBalancers/07-02-Classic-LoadBalancer-CLB)
```
apiVersion: v1
kind: Service
metadata:
  name: clb-usermgmt-restapp
  labels:
    app: usermgmt-restapp
spec:
  type: LoadBalancer  # Regular k8s Service manifest with type as LoadBalancer
  selector:
    app: usermgmt-restapp     
  ports:
  - port: 80
    targetPort: 8095

```

-   **Deploy all Manifest**

```
# Deploy all manifests
kubectl apply -f kube-manifests/

# List Services (Verify newly created CLB Service)
kubectl get svc

# Verify Pods
kubectl get pods
```

## [](https://github.com/stacksimplify/aws-eks-kubernetes-masterclass/tree/master/07-ELB-Classic-and-Network-LoadBalancers/07-02-Classic-LoadBalancer-CLB#step-02-verify-the-deployment)Step-02: Verify the deployment

-   Verify if new CLB got created
    -   Go to Services -> EC2 -> Load Balancing -> Load Balancers
        -   CLB should be created
        -   Copy DNS Name (Example: a85ae6e4030aa4513bd200f08f1eb9cc-7f13b3acc1bcaaa2.elb.us-east-1.amazonaws.com)
    -   Go to Services -> EC2 -> Load Balancing -> Target Groups
        -   Verify the health status, we should see active.
-   **Access Application**

```
# Access Application
http://<CLB-DNS-NAME>/usermgmt/health-status
```

## [](https://github.com/stacksimplify/aws-eks-kubernetes-masterclass/tree/master/07-ELB-Classic-and-Network-LoadBalancers/07-02-Classic-LoadBalancer-CLB#step-03-clean-up)Step-03: Clean Up

```
# Delete all Objects created
kubectl delete -f kube-manifests/

# Verify current Kubernetes Objects
kubectl get all
```



# AWS - Network Load Balancer - NLB
[NLB GitHub Instructions](https://github.com/stacksimplify/aws-eks-kubernetes-masterclass/tree/master/07-ELB-Classic-and-Network-LoadBalancers/07-03-Network-LoadBalancer-NLB)

**Step-01:** Create AWS Network Load Balancer Kubernetes Manifest & Deploy
-   **04-NetworkLoadBalancer.yml**
```
apiVersion: v1
kind: Service
metadata:
  name: nlb-usermgmt-restapp
  labels:
    app: usermgmt-restapp
  annotations:
    service.beta.kubernetes.io/aws-load-balancer-type: nlb    # To create Network Load Balancer
spec:
  type: LoadBalancer # Regular k8s Service manifest with type as LoadBalancer
  selector:
    app: usermgmt-restapp     
  ports:
  - port: 80
    targetPort: 8095

```

-   **Deploy all Manifest**

```
# Deploy all manifests
kubectl apply -f kube-manifests/

# List Services (Verify newly created NLB Service)
kubectl get svc

# Verify Pods
kubectl get pods
```

**Step-02:** Verify the deployment

-   Verify if new CLB got created
    -   Go to Services -> EC2 -> Load Balancing -> Load Balancers
        -   CLB should be created
        -   Copy DNS Name (Example: a85ae6e4030aa4513bd200f08f1eb9cc-7f13b3acc1bcaaa2.elb.us-east-1.amazonaws.com)
    -   Go to Services -> EC2 -> Load Balancing -> Target Groups
        -   Verify the health status, we should see active.
-   **Access Application**

```
# Access Application
http://<NLB-DNS-NAME>/usermgmt/health-status
```

**Step-03:** Clean Up

```
# Delete all Objects created
kubectl delete -f kube-manifests/

# Verify current Kubernetes Objects
kubectl get all
```



## Managed Node group Vs worker nodes Vs Fargate in EKS
- [Youtube Link for video](https://www.youtube.com/watch?v=52lpMpHqLD4)

![[Pasted image 20220701021405.png]]![[Pasted image 20220701021705.png]]


what kind of   nodes   that we can use with the eks cluster?
- There are three   types of node which are available in the   eks:
1. Managed node   groups
2. Self-managed node groups
3. fargate 

The name itself   indicates   managed right   what does that mean aws partially   manages your nodes group right and they   offer some sort of capability   which looks like it is managed and   it eases some of your   management load right so that you can   focus on some better problems   
- let's look at this diagram i have taken   this diagram from the eks best practices   website by aws   now on this website if you see on the   left hand side which is this   is the control plane whereas this is the   data plane right and the   orange color indicates that's aws   responsibility and the blue color   showcases your responsibility as a   customer right we'll see similar   diagrams in the self-managed and the  fargate as well 
- Right now in this   particular case if you see   along with control plane   os kubelet crm cri and emi configuration   is also aws responsibility   
- whereas node scaling vpc and all other   workloads is uh user responsibility   right   
- so here worker node scaling means a   cluster auto scaler or karpenter or   similar solution   now when we say os cube let's hear an   ami configuration what does that mean is   let's see   now operating system or ami right as one   of the piece in that aws responsibility   was operating system   now with managed node group   aws offers   eks optimized ami uh based on either   amazon linux or a portal rocket now   bottle rocket is a purpose made uh   operating system for containers   uh it's kind of very similar to a flat   car linux or core os   for that matter right   now these optimized ami which are built   by aws it already has   required tools to run a particular   node such as cubelet with all the   configuration aws arm authenticator and   stuff like that it's all uh in this ami   right   you can optimize as you can   customize we'll see later how we can do   that for for that   for this operating system thing you can   only use ami based on these two you   cannot use any other linux based ami   now there   there can be some partners with aws   which may offer but in a very basic   thing uh simply you cannot you have to   use the cmi   because we have to use these amis you   cannot run   containers which requires windows on   managed node group for that you may need   to define some other node group which we   will see   from storage side you can use there is   no restriction you can either use efb   ebs   efs or like a s3 or   there is no restriction basically when   it comes to storage in a managed node   group   and if you for some reason if you want   to access these nodes   you can either use ssh or ssm   ssm is a facility uh by aws itself uh   using these two you can access the nodes   right   demon's head now you must be wondering   demonstrate is a demon set is a common   terminology in kubernetes why   this is mentioned here in upcoming   slides you will see why this was   important to mention   but thing to mention is you can use   demon set with the managed node group   um so there must be some workloads which   may need arm processor so aws does offer   uh in some instant type which has arm   processor in it such as graviton   so if your workload needs   arm with managed node group you can   select the   uh arm based graviton instance type   along with the managed node group and   you can use it right   provided that the operating system you   can use is either amazon linux or bottle   rocket   for aiml and deep learning workload you   can   use instant type with the gpus and with   the inferential chip inferential chip is   aws developed chip specifically for deep   learning workload right but one catch   here is you can use all this but only on   amazon linux because it's the only thing   which is supported in   a managed node group   along with all these major things   the when you spin up a node group the   underlying auto scaling group is managed   by aws itself   it automatically tags every   ec2 resources for auto discovery which   is being used for   cluster auto scaling   if you want to customize   uh your ami   sorry if you want to customize your node   and add few other extra things or you   want to add some bootstrap argument to   the cubelet   you can do that uh a lot you can just   take   eks optimized ami and with the help of   blocks launch template you can add like   a cloud   config and stuff to add few more things   in it right so that's quite possible   now one very important thing when it   comes to node is the billing   so though   few things are partially managed by aws   you don't pay for that management you   will only pay for the ec2 resources that   you are using right   now that was more about the   managed notes uh   most of the things are managed but if   you want you can do some sort of   customization in it   and   the you can use gpu aml workloads   everything that you want provided that   it is based on amazon linux or bottle   rocket right   now let's move on to the self-managed   node uh now   we know that manage node group so when   we will go on to the self-managed we'll   we'll be able to see the difference   right   now similar to that diagram there is the   self-managed worker diagram now if you   see carefully   there is no orange section in the left   side of the diagram right   now   with that saying   as the name also indicates the   self-manage so the whole data plane is   your responsibility as a user   responsibility right   so   next   operating system as i said in previous   diagram everything on the data plane is   user responsibility   that means it starts with operating   system or the ami   now in this case you can use amazon   linux bottle rocket flat car windows   any other linux which is ubuntu you can   use any of these   on addition to this   you can also use the eks optimized ami   which are used in a managed node group   with that you get all the configuration   by your   pre-installed and stuff so you can use   those ami's as well   with the storage pretty similar to   managed one you can use ebs efs there   are no restrictions on storage   again demon sets these are also yet   another node it's just that the   management is on the user side aws don't   manage these things but demonstrate does   run on it   similar to managed node group   you can use ssm or ssh to access these   nodes as well   now   again   aml deep learning   workloads you can use uh gpu based   instances or inferential chip   uh you can use all this for your ai ml   and deep learning workloads but again   the same condition is you have to use   only amazon linux even if you are using   self-managed node group you have to use   only amazon linux for it   right so maybe you can define some node   group   which is specific to this based on   amazon linux and few node groups   may be based on your own operating   system or something   similar to manual node group it does   support arm architecture   with the help of these graviton   processors so if you have arm workloads   you can run in self-managed as well   now along with this there are few things   which are do it yourself way   so for example the auto scaling group in   manage node group it was managing the   asg for you as well here you have to   manage your own auto scaling group you   have to manage all sorts of tagging   floor cluster auto scaler   you have to manage the launch   configuration   you have to manage your own kmi and the   cubelet configuration as well   right   one   thing here is if you don't want to   manage the cubelet on your own and all   the configuration you can simply use e   case optimize ami as well   right   and similar to what we discussed in the   manage node group   billing which is very important aspect   since everything is self-managed right   you will only pay for the ec2 resources   that you use and because you are already   paying for the control plane separately   as well right   we'll go   uh we'll keep one episode   dedicated to the billing how we can see   that how much we are spending on control   plane and other stuff right   now that was more about the self-managed   node group where everything is managed   by you   now in majority of cases where   you can run managed node groups and let   aws takes care of few things   but there can be some cases where you   want to run your own stuff right so   self-manage notebook there is option for   that   now third very interesting bit is   fargate right   you must have heard forget with ecs   right   [Music]   forget with ecs   and in the similar diagram if we see   now again on the left hand side os   cubelet ami is orange that means that's   aws responsibility even the scaling is   aws responsibility right   now   in very short one-liner what is fargate   is on-demand right size compute capacity   for a container right that means   if you spin up a container it will give   you compute for that particular   requirement right   and what does that mean   is there is no machine there is no ec2   right   in upcoming slide we'll see how that   will work out   as i said it is not an ec2 it spin up   its own micro vm for that particular   container right   now   there are many   downsides in forget for example   you cannot run container which run   requires windows so that means only   basic containers which require linux you   can run these containers only   you cannot use aml more gpu workloads   no arm no graviton based stuff   you cannot use public subnets as well   right   and not just these   you cannot choose or configure ami you   cannot do ssh or ssm on these nodes you   cannot use ebs   you cannot use daemon set   now you now this is the point where   we mentioned the daemon set in the   previous node groups right   you cannot use demon set here   you cannot modify cubelet configuration   as well you cannot use privilege pod   because   it's a micro vm   and   that micro vm that environment is   specific for that pod right   so you cannot do anything on the host   level   so is it any good right like if there   are so many restrictions what does it   offer right   now   if we look on other side of it   in this case you can use container which   require linux   in this case you can   you use just basic uh   processor you can use private subnets   uh   these things are no but on other side   you don't need to manage them right   and there was no ebs on on that side   there is efs which you can use   and   the very best thing here is you only pay   for memory and cpu usage   right   now what does these all indicates is   you don't need to care about the actual   node which is a ec2 machine it's just a   micro vm specific for your container   right so you don't need to worry about   any vulnerability on the operating   system you don't need to worry about any   auto scaling group   you don't need to worry about anything   you simply define your body ammo and it   will spin up a   far gate   instance for it and that's it right   and the auto scaling and stuff has been   taken care already for these type of   instances   now because these   instances these forget parts are little   bit different   networking is little bit different than   the self-managed and managed node group   you can only use network load balancer   and application load balancer with the   forget with ip targets only right and   what that means is because because it is   a   micro vm running only one pawn   and you cannot run demon set that means   you cannot run vpc cnn demonstrate pod   or if you are using celium or any other   you cannot run these so it is not   technical technically utilizing the cni   networking capability   it is using   the underlying vpc networking capability   right so that's why   it gets the   ip from the vpc itself   that's why the networking is little bit   different right   now   these were the   three different types of node group   they are special   in their own cases like   with managed node group   you have to manage very few things so   you can focus on bet other problems   there can be a use case where you   want to manage your own amis and stuff   maybe for security reasons or maybe some   company policies or something you can do   that there is no major difference as   such provided that with self-managed you   do everything yourself whereas in manage   aws takes care of   few things partially   but with far gate as we discussed it's   little bit different right isn't it so   you   spin up a pod a container and it spin up   a micro vm for you there's nothing else   in it you cannot modify anything   you don't worry about anything you pay   only for the cpu and memory and not the   whole instance   which is fairly good but it does come   with a cost but yeah there can be a use   case where you can use these kind of   things to minimize the cost minimize the   attack surface   and stuff like that so   these were the comparison between the   node groups that we had   in upcoming episodes we'll cover each of   them one by one   in more depth right   till then   if you haven't subscribed to our channel   do subscribe and share this video and   let us know in the comment section if   you have any questions or if you want me   to cover anything else right till then   bye bye