**Create the docker image and push to the ECR Repository:**
- [Youtube video: ](https://www.youtube.com/watch?v=fPkO3644kDU)

1. **Clone the repository** from the git:
   - To Change the version edit the ``res.send('CICD App V2!')`` inside the **index.js** file
```
ubuntu@ip-172-31-10-78:~$ git clone https://github.com/sd031/aws_codebuild_codedeploy_nodeJs_demo.git

Cloning into 'aws_codebuild_codedeploy_nodeJs_demo'...
remote: Enumerating objects: 842, done.
remote: Counting objects: 100% (96/96), done.
remote: Compressing objects: 100% (17/17), done.
remote: Total 842 (delta 91), reused 79 (delta 79), pack-reused 746
Receiving objects: 100% (842/842), 645.14 KiB | 3.54 MiB/s, done.
Resolving deltas: 100% (319/319), done.
ubuntu@ip-172-31-10-78:~$
ubuntu@ip-172-31-10-78:~$
ubuntu@ip-172-31-10-78:~$

ubuntu@ip-172-31-10-78:~$ ls
aws  aws_codebuild_codedeploy_nodeJs_demo  awscliv2.zip

ubuntu@ip-172-31-10-78:~$ cd aws_codebuild_codedeploy_nodeJs_demo/

ubuntu@ip-172-31-10-78:~/aws_codebuild_codedeploy_nodeJs_demo$ ls
Appspec_blue_green_ecs.yml                buildspec_build_automation.yml  docs
Dockerfile                                buildspec_cache_docker.yml      eks_cicd
Jenkinsfile                               buildspec_cache_folder.yml      index.js
Jenkinsfile_Docker                        buildspec_eks.yml               k8s
Jenkinsfile_with_docker_file_integration  buildspec_normal.yml            k8s_reskill
README.md                                 builspec_without_cache.yml      package-lock.json
appspec.yml                               codedeploy.code-workspace       package.json
aws-auth.yaml                             db_backup_and_upload.sh         sample.txt
bash_script_reference.md                  dbname.sql                      samplepr
build_scripts                             deployment_scripts
buildspec.yml                             docker_image_push.sh

ubuntu@ip-172-31-10-78:~/aws_codebuild_codedeploy_nodeJs_demo$ cat index.js
const express = require('express')
const app = express()
const port = process.env.PORT || 3000 ;
const config = require('config')
console.log(config);

app.get('/', (req, res) => {
  res.send('CICD App V2!')
})

app.get('/status', (req, res) => {
    res.send('ok')
  })


app.listen(port, () => {
  console.log(`Example app listening at http://localhost:${port}`)
```


2. Login to ECR: 
- First login to AWS using a Role and then use the ECr command to login.
- While logging in if you get the error ``Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/auth": dial unix /var/run/docker.sock: connect: permission denied`` update the permission and ownership for the docker socket files. [Doc for refrence](https://newbedev.com/got-permission-denied-while-trying-to-connect-to-the-docker-daemon-socket-at-unix-var-run-docker-sock-post-http-2fvar-2frun-2fdocker-sock-v1-24-auth-dial-unix-var-run-docker-sock-connect-permission-denied-code-example)
```

ubuntu@ip-172-31-10-78:~/aws_codebuild_codedeploy_nodeJs_demo$ sudo aws ecr get-login-password --region us-east-2 |
 docker login --username AWS --password-stdin 000699977340.dkr.ecr.us-east-2.amazonaws.com
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/auth": dial unix /var/run/docker.sock: connect: permission denied

ubuntu@ip-172-31-10-78:~/aws_codebuild_codedeploy_nodeJs_demo$ sudo ls -l /var/run/docker.sock
srw-rw---- 1 root docker 0 May 26 04:47 /var/run/docker.sock

ubuntu@ip-172-31-10-78:~/aws_codebuild_codedeploy_nodeJs_demo$ sudo chmod 666 /var/run/docker.sock

ubuntu@ip-172-31-10-78:~/aws_codebuild_codedeploy_nodeJs_demo$ whoami
ubuntu

ubuntu@ip-172-31-10-78:~/aws_codebuild_codedeploy_nodeJs_demo$ sudo usermod -aG docker ${USER}

ubuntu@ip-172-31-10-78:~/aws_codebuild_codedeploy_nodeJs_demo$ id ubuntu
uid=1000(ubuntu) gid=1000(ubuntu) groups=1000(ubuntu),4(adm),20(dialout),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),118(netdev),119(lxd),999(docker)

ubuntu@ip-172-31-10-78:~/aws_codebuild_codedeploy_nodeJs_demo$ sudo aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin 000699977340.dkr.ecr.us-east-2.amazonaws.com
WARNING! Your password will be stored unencrypted in /home/ubuntu/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded

ubuntu@ip-172-31-10-78:~/aws_codebuild_codedeploy_nodeJs_demo$ docker build -t ec2-sre-cask-test .
Sending build context to Docker daemon  1.076MB
Step 1/7 : FROM node:14
 ---> d0c8d2556876
Step 2/7 : WORKDIR /usr/src/app
 ---> Using cache
 ---> eb97ba9ffe23
Step 3/7 : COPY package*.json ./
 ---> Using cache
 ---> 70019690d782
Step 4/7 : RUN npm install
 ---> Using cache
 ---> 800606fa3b90
Step 5/7 : COPY . .
 ---> Using cache
 ---> e83cc47ff373
Step 6/7 : EXPOSE 3000
 ---> Using cache
 ---> 4d746760c502
Step 7/7 : CMD [ "node", "index.js" ]
 ---> Using cache
 ---> bdc66f653f56
Successfully built bdc66f653f56
Successfully tagged ec2-sre-cask-test:latest

ubuntu@ip-172-31-10-78:~/aws_codebuild_codedeploy_nodeJs_demo$ docker images
REPOSITORY                                                       TAG       IMAGE ID       CREATED         SIZE
ec2-sre-cask-test                                                latest    bdc66f653f56   6 minutes ago   967MB
node                                                             14        d0c8d2556876   5 days ago      946MB
000699977340.dkr.ecr.us-east-2.amazonaws.com/ec2-sre-cask-test   latest    66cd53172aac   2 years ago     7.43MB
gkoenig/simplehttp                                               latest    66cd53172aac   2 years ago     7.43MB

ubuntu@ip-172-31-10-78:~/aws_codebuild_codedeploy_nodeJs_demo$ docker tag ec2-sre-cask-test:latest 000699977340.dkr.ecr.us-east-2.amazonaws.com/ec2-sre-cask-test:latest

ubuntu@ip-172-31-10-78:~/aws_codebuild_codedeploy_nodeJs_demo$ docker push 000699977340.dkr.ecr.us-east-2.amazonaws.com/ec2-sre-cask-test:latest
The push refers to repository [000699977340.dkr.ecr.us-east-2.amazonaws.com/ec2-sre-cask-test]
8f810f3bd53d: Pushed
a3ef2b11c7df: Pushed
09683a71186d: Pushed
0f62de1df557: Pushed
d63158b698f0: Pushed
4488a77c3129: Pushed
427fb2218abc: Pushed
86de77bb75cd: Pushed
d1301d7296e8: Pushed
a672acde7cce: Pushed
4010eb7bba90: Pushed
928f399cab6b: Pushed
f52e05ae36a3: Pushed
latest: digest: sha256:caed7eb4f6b5e41418e812e2e16a05520af99ccde0eb5f8b59f2ed1c27bafcbd size: 3052
```
- Now you can see in the AWS console, the task defination has been pushed
   ![[Pasted image 20220602112421.png]]


3. **Create the Task Definations:**



4. **Create the AWS Infrastructure:**
	- Readme File:
```
# Create core AWS infrastructure

* VPC
* 2 subnets in 2 different AZs
* Internet Gateway
* according routing tables

Command:  
aws cloudformation create-stack --capabilities CAPABILITY_IAM --stack-name ecs-core-infrastructure --template-body file://./core-infrastructure-setup.yml  --region us-east-2

```
- AWS cloudformation code file path: [file path](F:\Learning\AWS\AWS ECS\AWS-ECS-course-master-CODE and cloudformations\AWS-ECS-course-master-CODE\ECS_Cloudformation\Core_infra_VPC_subnet_igw_RT)


