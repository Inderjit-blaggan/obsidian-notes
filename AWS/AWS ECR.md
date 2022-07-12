**Elastic Container Registry**
![[Pasted image 20220526085627.png]]
- Amazon ECR is a way in which we **can store, manage and deploy containers on AWS**.    
- The great thing about Amazon ECR is that you pay for what you use.    So if you need more space to add more images to the ECR, then you will pay for the space per say that    you're using.    
- And we think also is that **ECR is fully integrated with ECS in IAM** for security.    
- So what that means is that it will **need and I am role in order for us to be able to pull images** into    an easy to from ECR repository.    
- Now the great thing also is that within this fully within this full integration with ECS is that all    of these resources, **all of these images in ECR, they are also backed internally by Amazon S3 So your    entire infrastructure that is native** to attack glass, which is really cool.    
- Also, there's a feature to **scan those images that we put in our ECR to look for vulnerabilities for    security reasons.**    For instance, if you have a bad or compromised image that will be dealt with appropriately internally    using vulnerability scanning.
- You can also **version tag** and do a **lifecycle on your images.**    For instance, if you have old images, you want to recycle through them, that is also available.    
- So here's an example how this would look.    So we have our Amazon ECR, our repository, right?    So in this case here, you can add images into this repository.    And because this images are there, you want to be able to pull them out for you to run them into easy    to instance instances as tasks.    Well, for that data set you need an I am role which will allow you to give access to pull those images    indeed.    And so that they can be run into any C2 instance.    Okay.    
- Now, to publish these images, we are going to do all of that manually in this section.    But there's a way to use code build with sicced pipeline, which makes this whole process really easy    and automatic.    And we're going to see that later in this course.    So we talked about images.    So let's look at how they look like, right?    The images we're talking about Docker images.    
 - Well, in easy in Amazon, etc., the images will look something like this.    So we have image, we have this number there and then we have some letter and then we have a C or region,    then full bar and then later.    So what does it all mean?    So.    So first you can see there we have a **registry number** and the U.S. West to in this case, depending on    your **region.**    
- So you are **allowed to have one registry per account, per region combination** that you see here in the    registry and then the region next you have a repository.    
- So repository essentially is the place where you're going to be adding those images, right?    So it contains images and is inside or within a registry.    
- So as you see there, full bar would be a **repository** and it could give that repository any name you    like.    
- And then of course, we have to have authorization which defines access to repository for your policy    as we saw.    So you need, for instance, an IAM role to be able to be authorize and have access to our images.    
- Of course, that goes with **authentication**, right, to get the credentials.    And we use Docker Command to retrieve US credentials, as I will show you in a second.    
- And we have the **images** you can see in the blue at the top, right.    So the image is, of course, a tag will be stored in repository.    
   ![[Pasted image 20220526085908.png]]
- So Amazon ECR, it's its own thing within AWS and it's a new service.    But we need to use Docker Command to push and pull images.    We do that using aws cli or log in command.    So all of that of course will go through a registry.    Remember registry.    
- You allowed one registry per account or region combination.    So in this case here, if you want to log in, you would use that MLS, ACR, and you will get the first    part of you getting you log in and password and passing the region for instance.    
  
  ![[Pasted image 20220526085512.png]]
- Otherwise to and then second part of you using Docker in this case to pull in that username and password    and then we pass in where we want to log in entering this case in our ECR repository and once you are    logged in, then you're going to be able to docker build and tag your images.    
- In this case I'm going to say Docker, build passenger name demo and then build it.    And then I'm going to be able to tag that image so that it confirms to what ACR expects here that bless    Amazon, ECR expects gains for important.    
- And then we finally going to be able to use Docker, Push and Pull.    
- In this case, we're going to say Docker, push and pass in the registry.    In passing the required argument, as you see here, which was saying we're pushing that our repository    created and we can pull also passing in the exact same address per say.    
  
  
  
  Handson:
1. Create a repository inside the account:
   ```
v@DESKTOP-JE0IJPU:~$ aws ecr create-repository --repository-name ec2-sre-cask-test --image-scanning-configuration scanOnPush=true --region us-east-2
{
    "repository": {
        "repositoryArn": "arn:aws:ecr:us-east-2:000699977340:repository/ec2-sre-cask-test",
        "registryId": "000699977340",
        "repositoryName": "ec2-sre-cask-test",
        "repositoryUri": "000699977340.dkr.ecr.us-east-2.amazonaws.com/ec2-sre-cask-test",
        "createdAt": "2022-05-26T09:51:32+05:30",
        "imageTagMutability": "MUTABLE",
        "imageScanningConfiguration": {
            "scanOnPush": true
        },
        "encryptionConfiguration": {
            "encryptionType": "AES256"
        }
    }
}

![[Pasted image 20220526095352.png]]


2. Add the images to the remote ECR repo:
   
   ```
   root@ip-172-31-10-78:~# docker pull gkoenig/simplehttp
Using default tag: latest
latest: Pulling from gkoenig/simplehttp
Digest: sha256:1f0c62b20a6718a42d5e6f06c6f4df37c0b52d0fc3e64b431889bb5d492b08ee
Status: Image is up to date for gkoenig/simplehttp:latest
docker.io/gkoenig/simplehttp:latest

root@ip-172-31-10-78:~# docker images
REPOSITORY           TAG       IMAGE ID       CREATED       SIZE
gkoenig/simplehttp   latest    66cd53172aac   2 years ago   7.43MB
root@ip-172-31-10-78:~# aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin 000699977340.dkr.ecr.us-east-2.amazonaws.com
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
root@ip-172-31-10-78:~# docker tag gkoenig/simplehttp 000699977340.dkr.ecr.us-east-2.amazonaws.com/ec2-sre-cask-test:latest


root@ip-172-31-10-78:~# docker images
REPOSITORY                                                       TAG       IMAGE ID       CREATED       SIZE
gkoenig/simplehttp                                               latest    66cd53172aac   2 years ago   7.43MB
000699977340.dkr.ecr.us-east-2.amazonaws.com/ec2-sre-cask-test   latest    66cd53172aac   2 years ago   7.43MB


root@ip-172-31-10-78:~# docker push 000699977340.dkr.ecr.us-east-2.amazonaws.com/ec2-sre-cask-test:latest
The push refers to repository [000699977340.dkr.ecr.us-east-2.amazonaws.com/ec2-sre-cask-test]
22cfb2031743: Pushed
latest: digest: sha256:1f0c62b20a6718a42d5e6f06c6f4df37c0b52d0fc3e64b431889bb5d492b08ee size: 528
root@ip-172-31-10-78:~#

```




