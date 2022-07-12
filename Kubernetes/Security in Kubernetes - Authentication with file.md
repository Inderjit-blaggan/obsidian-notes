[kode kloud Security PDF ]([Module 6 Kubernetes Security.pdf](file:///F:/Learning/Kubernetes/Module%206%20Kubernetes%20Security.pdf))

### Entities requiring access to cluster:

1. Users like administrators, developers
2. End users who access the applications
3. Bots: Third party applications accessing the cluster for integration purposes.

- kubernetes **does not manage user accounts natively**, it relies on an **external source** like a **file with user details** or **certificates or a third party identity service** like that to manage these users.
	- However, in case of **service accounts**, **K8 can manage them.** 
	- You can **create and manage service accounts** using the **kube API.**  
	- All **user access is managed by the API server**, whether you're accessing the cluster through a control tool or the API directly, all of these request go through the API server.
	- The Kube API server authenticates the request before processing it. 
		- Let's start with static password and token files as it is the easiest to understand.
			- Let's start with the simplest form of authentication, **you can create a list of users** and **their passwords** . in a case file and use that as the source for user information. 
			- The file has three columns `password`, `username` and `user I.D.` 
			- Within the file name has an option to the kube API server. We can pass the file as `--basic-auth-file=details.csv`
				- **Note**:  This is **not a recommended authentication mechanism** and Consider volume mount while providing the auth file in a kubeadm setup. Setup Role Based Authorization for the new users
			- ![[Pasted image 20220627121716.png]]
			- You must then **restart the kube API server** for these options to take effect.
			- If you set up your cluster using the KUBEADM to, then you must modify the kubeadm space over pod definition file. The kubeadm will automatically restart the Kube API server once you update this file. 
			- To authenticate using the basic credentials while accessing the API server, specify the user and password in the command like this. `curl -v -k [https://master-node-ip:6443/api/v1/pods](https://master-node-ip:6443/api/v1/pods) -u "user1:password123"`
			- Now to authenticate you can use the curl command with `-u "user-name:password"`![[Pasted image 20220627121851.png]]
			- In the File with the user details that we saw, we can optionally have a fourth column with the `group` details to **assign users to specific groups.** 
			- Similarly, instead of a **static password file**, you can have a **static token file** here instead of password, you specify a **token pass the token file as an option** token or file to the server while authenticating specified a token as an authorization bearer. 
			- ![[Pasted image 20220627122302.png]]
			- Token to your request like this: `curl -v -k https://master-node-ip:6443/api/v1/pods --header "Authorization: Bearer KpjCVbI7rCFAHYPkBzRb7gu1cUc4B"
			- That's it for this lecture.

- Remember that this **authentication mechanism that stores usernames, passwords and tokens** and clear text in a **static file is not a recommended approach** as it is insecure. But I thought this was the easiest way to understand the basics of authentication and communities going forward.


#### Setup basic authentication on Kubernetes (Deprecated in 1.19)

> Note: This is not recommended in a production environment. This is only for learning purposes. Also note that this approach is deprecated in Kubernetes version 1.19 and is no longer available in later releases

Follow the below instructions to configure basic authentication in a kubeadm setup.

1. Create a file with user details locally at `/tmp/users/user-details.csv`

```
1.  # User File Contents
2.  password123,user1,u0001
3.  password123,user2,u0002
4.  password123,user3,u0003
5.  password123,user4,u0004
6.  password123,user5,u0005

```

2. Edit the kube-apiserver static pod configured by kubeadm to pass in the user details. The file is located at `/etc/kubernetes/manifests/kube-apiserver.yaml`
```

1.  apiVersion: v1
2.  kind: Pod
3.  metadata:
4.    name: kube-apiserver
5.    namespace: kube-system
6.  spec:
7.    containers:
8.    - command:
9.      - kube-apiserver
10.        <content-hidden>
11.      image: k8s.gcr.io/kube-apiserver-amd64:v1.11.3
12.      name: kube-apiserver
13.      volumeMounts:
14.      - mountPath: /tmp/users
15.        name: usr-details
16.        readOnly: true
17.    volumes:
18.    - hostPath:
19.        path: /tmp/users
20.        type: DirectoryOrCreate
21.      name: usr-details
```
 
3. Modify the kube-apiserver startup options to include the basic-auth file
```
1.  apiVersion: v1
2.  kind: Pod
3.  metadata:
4.    creationTimestamp: null
5.    name: kube-apiserver
6.    namespace: kube-system
7.  spec:
8.    containers:
9.    - command:
10.      - kube-apiserver
11.      - --authorization-mode=Node,RBAC
12.        <content-hidden>
13.      - --basic-auth-file=/tmp/users/user-details.csv
```
4. Create the necessary roles and role bindings for these users:

```
1.  ---
2.  kind: Role
3.  apiVersion: rbac.authorization.k8s.io/v1
4.  metadata:
5.    namespace: default
6.    name: pod-reader
7.  rules:
8.  - apiGroups: [""] # "" indicates the core API group
9.    resources: ["pods"]
10.    verbs: ["get", "watch", "list"]

12.  ---
13.  # This role binding allows "jane" to read pods in the "default" namespace.
14.  kind: RoleBinding
15.  apiVersion: rbac.authorization.k8s.io/v1
16.  metadata:
17.    name: read-pods
18.    namespace: default
19.  subjects:
20.  - kind: User
21.    name: user1 # Name is case sensitive
22.    apiGroup: rbac.authorization.k8s.io
23.  roleRef:
24.    kind: Role #this must be Role or ClusterRole
25.    name: pod-reader # this must match the name of the Role or ClusterRole you wish to bind to
26.    apiGroup: rbac.authorization.k8s.io
```

4. Once created, you may authenticate into the kube-api server using the users credentials: `curl -v -k https://localhost:6443/api/v1/pods -u "user1:password123"`
