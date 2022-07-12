




**How to push code to code commit repository using SSO:**
- [aws Doc for refrence](https://aws.amazon.com/blogs/devops/federated-multi-account-access-for-aws-codecommit/)
```
v@DESKTOP-JE0IJPU:/mnt/c/Users/hp$ aws configure sso
SSO start URL [https://d-906769eaa2.awsapps.com/start]:
SSO Region [us-east-1]:
Attempting to automatically open the SSO authorization page in your default browser.
If the browser does not open or you wish to use a different device to authorize this request, open the following URL:

https://device.sso.us-east-1.amazonaws.com/

Then enter the code:


v@DESKTOP-JE0IJPU:$ git clone codecommit://SaasIO-SRE-000699977340@NodeTestAppRepo my-demo-repo

Cloning into 'my-demo-repo'...
warning: You appear to have cloned an empty repository.

v@DESKTOP-JE0IJPU:$ ls
'AWS-ECS-course-master-CODE and cloudformations'   aws_codebuild_codedeploy_nodeJs_demo   my-demo-repo   sample-nodejs-docker-app

v@DESKTOP-JE0IJPU:/mnt/f/Learning/AWS/AWS ECS$ cp -r  aws_codebuild_codedeploy_nodeJs_demo/* my-demo-repo/

v@DESKTOP-JE0IJPU:/mnt/f/Learning/AWS/AWS ECS$ cd my-demo-repo/

v@DESKTOP-JE0IJPU:/mnt/f/Learning/AWS/AWS ECS/my-demo-repo$ ls
Appspec_blue_green_ecs.yml                README.md                 buildspec.yml                   buildspec_normal.yml        deployment_scripts    k8s                samplepr
Dockerfile                                appspec.yml               buildspec_build_automation.yml  builspec_without_cache.yml  docker_image_push.sh  k8s_reskill
Jenkinsfile                               aws-auth.yaml             buildspec_cache_docker.yml      codedeploy.code-workspace   docs                  package-lock.json
Jenkinsfile_Docker                        bash_script_reference.md  buildspec_cache_folder.yml      db_backup_and_upload.sh     eks_cicd              package.json
Jenkinsfile_with_docker_file_integration  build_scripts             buildspec_eks.yml               dbname.sql                  index.js              sample.txt

v@DESKTOP-JE0IJPU:/mnt/f/Learning/AWS/AWS ECS/my-demo-repo$ git commit -m "branch master created"
[master (root-commit) 96cf38e] branch master created
 87 files changed, 8144 insertions(+)

v@DESKTOP-JE0IJPU:/mnt/f/Learning/AWS/AWS ECS/my-demo-repo$ git status
On branch master
Your branch is based on 'origin/master', but the upstream is gone.
  (use "git branch --unset-upstream" to fixup)

nothing to commit, working tree clean

v@DESKTOP-JE0IJPU:$ git push codecommit://SaasIO-SRE-000699977340@NodeTestAppRepo  master

Enumerating objects: 80, done.
Counting objects: 100% (80/80), done.
Delta compression using up to 8 threads
Compressing objects: 100% (76/76), done.
Writing objects: 100% (80/80), 47.66 KiB | 305.00 KiB/s, done.
Total 80 (delta 17), reused 0 (delta 0)
To codecommit://NodeTestAppRepo
 * [new branch]      master -> master
v@DESKTOP-JE0IJPU:/mnt/f/Learning/AWS/AWS ECS/my-demo-repo$ 
``` 
![[Pasted image 20220602141155.png]]