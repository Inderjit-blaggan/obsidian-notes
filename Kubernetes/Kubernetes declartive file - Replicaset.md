# ReplicaSets with YAML

**Step-01:** Create ReplicaSet Definition
-   **replicaset-definition.yml**
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp2-rs
spec:
  replicas: 3 # 3 Pods should exist at all times.
  selector:  # Pods label should be defined in ReplicaSet label selector
    matchLabels:
      app: myapp2
  template:
    metadata:
      name: myapp2-pod
      labels:
        app: myapp2 # Atleast 1 Pod label should match with ReplicaSet Label Selector
    spec:
      containers:
      - name: myapp2
        image: stacksimplify/kubenginx:2.0.0
        ports:
          - containerPort: 80

```

Step-02: Create ReplicaSet
-   Create ReplicaSet with 3 Replicas

```
# Create ReplicaSet
kubectl apply -f 02-replicaset-definition.yml

# List Replicasets
kubectl get rs
```
-   Delete a pod
-   ReplicaSet immediately creates the pod.

```
# List Pods
kubectl get pods

# Delete Pod
kubectl delete pod <Pod-Name>
```

Step-03: Create NodePort Service for ReplicaSet
```
apiVersion: v1
kind: Service
metadata:
  name: replicaset-nodeport-service
spec:
  type: NodePort
  selector:
    app: myapp2
  ports:
    - name: http
      port: 80
      targetPort: 80
      nodePort: 31232  

```

-   Create NodePort Service for ReplicaSet & Test

```
# Create NodePort Service
kubectl apply -f 03-replicaset-nodeport-servie.yml

# List NodePort Service
kubectl get svc

# Get Public IP
kubectl get nodes -o wide

# Access Application
http://<Worker-Node-Public-IP>:<NodePort>
http://<Worker-Node-Public-IP>:31232

```

**Step-04:** Make sure the port is open on the node security group to allow the http traffic. 
- Get the node public IP using command:
```
v@DESKTOP-JE0IJPU:$ kubectl get node -o wide
NAME                                           STATUS   ROLES    AGE   VERSION               INTERNAL-IP      EXTERNAL-IP     OS-IMAGE         KERNEL-VERSION                 CONTAINER-RUNTIME
ip-192-168-61-231.us-east-2.compute.internal   Ready    <none>   78m   v1.22.9-eks-810597c   192.168.61.231   18.220.50.196   Amazon Linux 2   5.4.196-108.356.amzn2.x86_64   docker://20.10.13
```
![[Pasted image 20220629021642.png]]

## [](https://github.com/stacksimplify/kubernetes-fundamentals/tree/master/08-ReplicaSets-with-YAML#api-references)API References

-   **ReplicaSet:** [https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.18/#replicaset-v1-apps](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.18/#replicaset-v1-apps)