# Deployments with YAML

**Step-01:** Copy templates from ReplicaSet
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp3-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp3
  template:
    metadata: # Dictionary
      name: myapp3-pod
      labels: # Dictionary
        app: myapp3
    spec:
      containers: # List
        - name: myapp3-container
          image: stacksimplify/kubenginx:3.0.0
          ports:
            - containerPort: 80


---
apiVersion: v1
kind: Service
metadata:
  name: deployment-nodeport-service
spec:
  type: NodePort
  selector:
    app: myapp3
  ports:
    - name: http
      port: 80
      targetPort: 80
      nodePort: 31233
```
-   Copy templates from ReplicaSet and change the `kind: Deployment`
-   Update Container Image version to `3.0.0`
-   Update NodePort service `nodePort: 31233`
-   Change all names to Deployment
-   Change all labels and selectors to `myapp3`

```
# Create Deployment
kubectl apply -f 02-deployment-definition.yml
kubectl get deploy
kubectl get rs
kubectl get po

# Create NodePort Service
kubectl apply -f 03-deployment-nodeport-service.yml

# List Service
kubectl get svc

# Get Public IP
kubectl get nodes -o wide

# Access Application
http://<Worker-Node-Public-IP>:31233
```

**Step-02: Make sure the port is open on the node security group to allow the http traffic. 
- Get the node public IP using command:
![[Pasted image 20220629022519.png]]

**Step-03**: Deletion of the deployment using command:
```
v@DESKTOP-JE0IJPU:$ kubectl get deployment
NAME                READY   UP-TO-DATE   AVAILABLE   AGE
myapp3-deployment   3/3     3            3           9m11s

v@DESKTOP-JE0IJPU:$ kubectl delete deployment myapp3-deployment
deployment.apps "myapp3-deployment" deleted
```
## [](https://github.com/stacksimplify/kubernetes-fundamentals/tree/master/09-Deployments-with-YAML#api-references)API References

-   **Deployment:** [https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.18/#deployment-v1-apps](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.18/#deployment-v1-apps)