## Create a deployment in private subnet and expose it using network load balancer.


1. Create a deployment.
```
apiVersion: v1
kind: Service
metadata:
  name: myapp3-nlb-service
  labels:
    app: nginx
  annotations:
    service.beta.kubernetes.io/aws-load-balancer-type: nlb    # To create Network Load Balancer
spec:
  type: LoadBalancer # Default - CLB
  selector:
    app: myapp3
  ports:
    - port: 80
      targetPort: 80
  
```


2. Now create a service which will create the network load balancer
```
apiVersion: v1
kind: Service
metadata:
  name: myapp3-nlb-service
  labels:
    app: nginx
  annotations:
    service.beta.kubernetes.io/aws-load-balancer-type: nlb    # To create Network Load Balancer
spec:
  type: LoadBalancer # Default - CLB
  selector:
    app: myapp3
  ports:
    - port: 80
      targetPort: 80
```

3. Apply both the files
```
NLB/kube-custon$ kubectl apply -f nginx-deployment.yml
deployment.apps/myapp3-deployment created

NLB/kube-custon$ kubectl apply -f 04-NetworkLoadBalancer.yml 
service/myapp3-nlb-service created

v@DESKTOP-JE0IJPU:$ kubectl get svc
NAME                 TYPE           CLUSTER-IP       EXTERNAL-IP                                                                     PORT(S)        AGE
kubernetes           ClusterIP      172.20.0.1       <none>                                                                          443/TCP        3h29m
myapp3-nlb-service   LoadBalancer   172.20.196.137   a92db540bdb364e618d65228e2baccac-dda974aa3f32fb3c.elb.us-east-2.amazonaws.com   80:30743/TCP   7m1s
```

4. Use the DNS name for load balancer and access website over port 80.

![[Pasted image 20220701155034.png]]