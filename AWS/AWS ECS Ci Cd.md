**CI/ CD**
![[Pasted image 20220526121009.png]]
- So now we're going to **talk about continuous integration, continuous delivery**.    
- So the idea behind CI/CD is that we want to **push our code case in a repository and have it deployed onto   ECS**.    But all of that has to happen automatically the right way. 
- Making sure it's tested before it's deployed is very important.    But also with possibility to go into different states.    
- So, for instance, we could have one state where it is for dev, for test pre-production, production    and so forth, but also with manual approval where needed.    
- So really, if you want to be a proper CI/CD developer, you need to learn about code.    And in this case,CICD, bless the CI/CD because it's a very important tool to speed up your development    and testing as well as deployment and so forth.    

Let's look at the first part of the CI/CD, which is , **continuous integration**.
  
   - So usually developers have the job of building or writing code.    When they write a code, they push the code to a repo often, right?    Maybe every other day or weekly so they can push their code into various different repositories such    as GitHub code, commit Bitbucket and so many others.    
   - So once the **code is committed**, of course it **goes through the testing build server where is checked    thoroughly to make sure that the code is okay.**    So that happens as soon as the **code is pushed**.    Could use code, build Jenkins CI and so forth.   
   - And then of course, the developers in this case will **get the feedback about the tests and check if    you have a pass or fail**.    
   - So we have here a loop where developer is able to get, of course, the feedback as soon as possible    when things are happening.    
   - So that way, if there are bugs, they can find those bugs and fix them quickly.    So in that sense, that means then the continuous integration part of code allows for delivery, allows    for a faster delivery because code is tested right away.    And we also deploy often that means then the developer and the whole team are happy because most of    the developers are unblocked to keep moving on onto the project.    
   - So essentially we have this diagram that exemplifies that simplifies what we just talked about.    
   
   1. **So a developer goes ahead and push code** and this code goes to the **repository** and it gets sent to a **build    server** where **it's tested.**    Right.    And then the developer gets the build results.    In this case, if it's failed or if this has bugs and so forth.    So this is pretty awesome.    
   
   
   Now when we look at the CD part, which is the **continuous delivery**, continuous delivery, what he    does is, is that **ensures that the software can be released reliably whenever needed.**    It also makes sure that deployments happen very often and quickly.    
   - Now, instead of waiting long periods for a release, so developers now are turning in code faster to    be tested, poisoned, provisioned and so forth, way faster than the older model where it would release    every three months, four months and so forth.    Right.    
   - So now developers are releasing code faster because there's this feedback loop as opposed of waiting    two or three months to release.    Now developers can release really fast, for instance, three or five releases a day.    What it means is that we are also using some sort of automated deployment, such as code deployed,    Jenkins, CDs, Spinnaker, etc..    And so again, is the same process, right?    
   
   
   1. So a **developer pushes the code into a repository** 
   2. then it goes **through a build server where the code    is built** and everything is checked and then it goes to a deployment server.    
   3. So deployment server is going to **deploy the application to different servers** and push those new versions    and provisioning everything in the back end.    
   
   This is the overall workflow for continuous delivery.    
   
   **So now let's look at the technology stack for the CICD.**    
   ![[Pasted image 20220526121031.png]]
   - So of course, we have the workflow where we code, we build, test, deploy and provision times.    So in a test for pushing the code, we have code commit, but he can also use GitHub or other third    party repos for building the code.    
   - We use ADA as code built, but first we can use Jenkins CI or third party CIA servers.    There's so many out there and for deployment or provisioning we use Elastic Beanstalk or we can also    use managed easy to access is fleet that we created using cloud formation as well as ADA was code deploy.    Okay.  
![[Pasted image 20220526121114.png]]  
   - Now as you can see, there are a lot of moving parts here.    So the great thing is that we can orchestrate all of these using a base code pipeline to make all of    this automatic orchestrated.    That way we don't have to do much.    We just let the ADA code pipeline deal with all of these steps.    Now, because we are talking about eight blast, so we're going to use the ICD services in eight.    Yes.    So what we're going to be using is code commit which store your code in private repository called build,    which builds your code and run command such as Docker push code pipeline, which as we saw, allows    us to orchestrate the entire workflow for CI CD and code deploy for deploying new version of an application    using in this case, ex blue green deployments.    So these are the services that we are going to be using in eight.    So next, let's look at an overview of how all of these will work together from code to X.    Okay.    So imagine that we have to update the X service.    First thing we're going to do is we're going to commit code using git commit cielo.    We're going to commit code to code commit when the code commit receives the code.    What will happen is that we're going to set up a code pipeline that will orchestrate this entire process.    This is where the beauty happens.    So Code Commit will go ahead and check for any changes that happen in the code.    Commit to repository.    Right.    And then we'll pass it on, on to code build.    After that, code build will push that image onto ACR, Amazon, ACR.    Once the image is in the ACR with an updated tag, for example, the code build will pass on that information    through the pipeline.    Right.    We'll pass that information to code deploy, which in turn will create a new task definition and update    the easiest service for us.    We will update the service for us with a pointer to a new image we have pushed to ACR.    Now X will be smart enough, will know to pull that new image from ACR and upgrade.    Of course, the easiest service that we have, the great thing here is that all of these will be fully    automated and then we don't have to do much.    All we have to do, the only thing we have to do as developers is to push code, to code commit.    And code pipeline will orchestrate all of this workflow for us.    So this is what we're going to put into practice in the next lectures.    I'll see you next.
     
     
**ECS Rolling Update**
![[Pasted image 20220526121311.png]]
- So when we want to update a service from V1 to V2, we can also control the tasks.    
- In this case, how many tasks can be stopped and how many tasks can be started?    But most importantly, in which order is this happening?    
- So we've seen before in our AWS Dashboard, we've seen something like this where we see a **minimum health    percent**.  In this case, we have **100** and a **maximum percent of 200.**    
- Let's see what all of this means and how it all works.    
	  - Let's assume we have **9 tasks** and we are running at **100% actual capacity**.    So when we **update the ecs service to V2**, we need to **set and define a minimum health percent** and that    could be **between 0 to 100%.**    
	  - So if you set the **minimum percent to 100**, we **don't want anything to be terminated, anything less than    that.**    We do allow ecs to terminate tasks.    
	  - because we want to create V2 tasks and operate our service in a rolling fashion. 
	  - Now the maximum percent is actually doing the opposite.    It's saying, let's take the actual capacity, we are over 100%.    Then you are allowed to create the two tasks in over a capacity, keyword over capacity.    So the two settings really together will show you what you are able to terminate and create based on    percentages you have defined.    Right?    

So I have a **few diagrams to help us understand this even better.**    
![[Pasted image 20220526121330.png]]
- Let's say we have **minimum of 50% and maximum of 100**, and we're going to **start with four tasks** now because    our **minimum is 50%**.    
- That means we **allow 50%** of **our V1 tasks** to be done at any point in time.    So the **ecs service is going to terminate two of the V1 tasks** as you see here.    
- So in this case, we're going to be **running under a capacity at 50% capacity**, right?    But now we can **create 2 more v2 tasks to replace if we want tasks that were terminated,** which means    
- now **we're back at 100% capacity** and **again we terminated those v1 tasks.**    We go **under a 50% capacity** and then we're only left with 2 v2 tasks and we can create the extra missing    V2 tasks to replace it.    
- If we go on tasks that were removed and you can see we are back to a full capacity with all of the two    tasks updated.    
  So by this example **we can see that by setting max of 100, we can never go above the capacity of four    tasks, but by setting mean of 50%, we did allow half of the tasks to be gone and replaced by v2    tasks**.    
  
  
  
**Okay, so let's go ahead and look at the same example by this time** with **min 100 and max 150.**    
![[Pasted image 20220526121347.png]]
- So we're **going to also start with 4 tasks** as you see them.    
- So if **Min is 100%,** **ecs will not terminate** any of the **v1 tasks** because we are just at capacity.    Makes sense.    
- Therefore what will happen is **easiest will instead create 2 v2 tests** because **we allow max of 50** and min    capacities.    
- So that way **ecs will add to V2 tasks** on top of the **4 v1 task already** because in this **case 150%**    of four is two.    
- So we add those new **v2 tasks**.    So once those **V2 tasks are running, there were added over a capacity**.    
- **ecs will then terminate two tasks**, right?    So that we **go back to four**, which is our **main capacity** and then go **ahead and add again new V two tasks**,    which means then we're going to go ahead and **terminate those V1 task two again** and we're going to    end up with 4 v2 tasks, which is our capacity.    
**In this case, we will have upgraded our ecs service**.    
- So the **difference here from what we've had before we had to go under capacity to upgrade our service**, which can be a **bad situation in certain**    cases, as you can imagine.    
- But here **you notice is opposite.    We actually go over capacity to upgrade our service, which means ecs provisions, different tasks.**    So to make sure we'd never go under capacity.    
- Now, it really depends on what's your goal, how you want to update your services, and if you can    take risk of having more tasks or less in your services.    
![[Pasted image 20220526121405.png]]

**Now let's go ahead and look at another example** so that we can solidify this concept if we have **minimum    50% and max hundred and 50 starting at four**.    Right, with.    What tasks?    
- If the cluster has **capacity to create new task**, **then new tasks will be created**.    Wait, this is what are called the **favor availability.**    
- If the cluster doesn't have staples, it **doesn't have enough capacity,** then that means all **tasks will    be terminated before new ones are created.**    
- Now, if we have **minimum of 0%** and **max of 100**, **all the tasks can be taken down before new ones are    created**, which is really bad.    
- And if **Min is 100%** and **Max is 200%,** **all the task will be created**.    And then that's important.    It **creates first new task, and then only then old ones** will be terminated.    So you can see the whole picture of what we've been talking about about is this rolling update.    Okay.    
  
  
  
  1. AWS code commit:
     ![[Pasted image 20220526121910.png]]
  - First of all, we're going to put this simple HTP source files into Code Commit, V.C.S.    So, as you already know, Code Commit is the equivalent of GitHub for Amazon.    So Code Commit is a offering for managed git service.    All of these hands on.    We're going to have, of course, a few steps that we have to go through to set everything up.    So to put this simple HTP source files into code commit v c as we are going to follow a few steps.    So here are the steps that are going to be following.    First, we're going to create a repository in code commit, then we're going to prepare our environment    for that.    We then going to clone that budgetary to our local machine.    Then we're going to fetch source code from simple HTP, from GitHub right to the local machine.    We can add some source code to the clone to repository, write, commit and push the code to code commit.