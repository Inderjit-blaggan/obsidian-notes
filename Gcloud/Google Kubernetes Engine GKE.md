![[Pasted image 20220626024553.png]]

![[Pasted image 20220626024623.png]]

#### Setting Up a GKE cluster:

![[Pasted image 20220626033421.png]]
![[Pasted image 20220626033447.png]]
![[Pasted image 20220626033506.png]]
1. Go to the **Kubernetes Engine** and create the **Cluster**
2. Now either click on the **:** parallel to cluster name in UI and select **connet button**. It will give a command : ``gcloud container clusters get-credentials blaggan-cluster --zone us-central1-c --project blaggan-project``
3. To curl on fly we can use command: `watch curl url`

```
gcloud config set project my-kubernetes-project-304910
gcloud container clusters get-credentials my-cluster --zone us-central1-c --project my-kubernetes-project-304910
kubectl create deployment hello-world-rest-api --image=in28min/hello-world-rest-api:0.0.1.RELEASE
kubectl get deployment
kubectl expose deployment hello-world-rest-api --type=LoadBalancer --port=8080
kubectl get services
kubectl get services --watch
curl 35.184.204.214:8080/hello-world
kubectl scale deployment hello-world-rest-api --replicas=3
gcloud container clusters resize my-cluster --node-pool default-pool --num-nodes=2 --zone=us-central1-c
kubectl autoscale deployment hello-world-rest-api --max=4 --cpu-percent=70
kubectl get hpa (horizontal pod autoscaling)
```

```
inderjitsinghblaggan@cloudshell:~/default-service (blaggan-project)$ gcloud container clusters get-credentials blaggan-cluster --zone us-central1-c --project blaggan-project
Fetching cluster endpoint and auth data.
kubeconfig entry generated for blaggan-cluster.

inderjitsinghblaggan@cloudshell:~/default-service (blaggan-project)$ kubectl get nodes
NAME                                             STATUS   ROLES    AGE     VERSION
gke-blaggan-cluster-default-pool-391e41e4-789c   Ready    <none>   5m46s   v1.22.8-gke.202
gke-blaggan-cluster-default-pool-391e41e4-d0lk   Ready    <none>   5m46s   v1.22.8-gke.202
gke-blaggan-cluster-default-pool-391e41e4-hz6t   Ready    <none>   5m46s   v1.22.8-gke.202

inderjitsinghblaggan@cloudshell:~/default-service (blaggan-project)$ kubectl create deployment  hello-world-rest-api --image=in28min/hello-world-rest-api:0.0.1.RELEASE
deployment.apps/hello-world-rest-api created

inderjitsinghblaggan@cloudshell:~/default-service (blaggan-project)$ kubectl get services
NAME                   TYPE           CLUSTER-IP      EXTERNAL-IP      PORT(S)          AGE
hello-world-rest-api   LoadBalancer   10.124.14.198   35.225.227.190   8080:31899/TCP   11m
kubernetes             ClusterIP      10.124.0.1      <none>           443/TCP          21m

inderjitsinghblaggan@cloudshell:~/default-service (blaggan-project)$ curl 35.225.227.190:8080
{healthy:true}inderjitsinghblaggan@cloudshell:~/default-service

inderjitsinghblaggan@cloudshell:~/default-service (blaggan-project)$ kubectl get deployment
NAME                   READY   UP-TO-DATE   AVAILABLE   AGE
hello-world-rest-api   1/1     1            1           10s

inderjitsinghblaggan@cloudshell:~/default-service (blaggan-project)$ kubectl expose deployment hello-world-rest-api --type=LoadBalancer --port=8080
service/hello-world-rest-api exposed

inderjitsinghblaggan@cloudshell:~/default-service (blaggan-project)$ kubectl scale deployment hello-world-rest-api --replicas=3
deployment.apps/hello-world-rest-api scaled


inderjitsinghblaggan@cloudshell:~/default-service (blaggan-project)$ gcloud container clusters resize blaggan-cluster --node-pool default-pool --num-nodes=1 --zone=us-central1-c
Pool [default-pool] for [blaggan-cluster] will be resized to 1.
Do you want to continue (Y/n)?  y
Resizing blaggan-cluster...done.

inderjitsinghblaggan@cloudshell:~/default-service (blaggan-project)$ gcloud container clusters describe  blaggan-cluster  --zone=us-central1-c | grep 'currentNodeCount'
currentNodeCount: 1

inderjitsinghblaggan@cloudshell:~/default-service (blaggan-project)$ kubectl autoscale deployment hello-world-rest-api --max=4 --cpu-percent=70
horizontalpodautoscaler.autoscaling/hello-world-rest-api autoscaled

inderjitsinghblaggan@cloudshell:~/default-service (blaggan-project)$ kubectl get hpa
NAME                   REFERENCE                         TARGETS         MINPODS   MAXPODS   REPLICAS   AGE
hello-world-rest-api   Deployment/hello-world-rest-api   <unknown>/70%   1         4         0          12s

inderjitsinghblaggan@cloudshell:~/default-service (blaggan-project)$ gcloud container clusters update blaggan-cluster --enable-autoscaling --min-nodes=1 --max-nodes=10  --zone=us-central1-c
Updating blaggan-cluster...done.     
Updated [https://container.googleapis.com/v1/projects/blaggan-project/zones/us-central1-c/clusters/blaggan-cluster].
To inspect the contents of your cluster, go to: https://console.cloud.google.com/kubernetes/workload_/gcloud/us-central1-c/blaggan-cluster?project=blaggan-project

inderjitsinghblaggan@cloudshell:~/default-service (blaggan-project)$ kubectl delete service hello-world-rest-api
service "hello-world-rest-api" deleted

inderjitsinghblaggan@cloudshell:~/default-service (blaggan-project)$ kubectl get service 
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.124.0.1   <none>        443/TCP   39m

inderjitsinghblaggan@cloudshell:~/default-service (blaggan-project)$ kubectl get deployments
NAME                   READY   UP-TO-DATE   AVAILABLE   AGE
hello-world-rest-api   3/3     3            3           30m

inderjitsinghblaggan@cloudshell:~/default-service (blaggan-project)$ kubectl delete deployment hello-world-rest-api
deployment.apps "hello-world-rest-api" deleted

inderjitsinghblaggan@cloudshell:~/default-service (blaggan-project)$ kubectl get pods
No resources found in default namespace.

inderjitsinghblaggan@cloudshell:~/default-service (blaggan-project)$ gcloud container clusters delete blaggan-cluster  --zone=us-central1-c
The following clusters will be deleted.
 - [blaggan-cluster] in [us-central1-c]

Do you want to continue (Y/n)?  y
```