### DevOps LifeCycle

![[Pasted image 20220707071402.png]]

I'm going to explain you the entire DevOps lifecycle.  You can easily understand what is CICD and what are the various tools that comes in each DevOps lifecycle.  So let's start.  
- So here you can see we have one company and this is the client.  The client has given one project to the company.  The project name is Project A.  So once the project has been aligned to the company, the company will start working on the project.  So in the initial phase, the company will start gathering the information about the project.  What is the requirement of the client?  What exactly the client is looking from the company for this project?  So here they will also collect the resources.  How many developers will be required to complete this project?  Also, they need the operation team because you're going to deploy the package into the production environment  using the DevOps.  
- So DevOps is the **combination of DevOps team plus operation team.**  So here you need both the team.  
1.  So initial phase, you need the developers.  **So developers is going to write the code for this project.**  Suppose you have chosen then 5 developers to complete this project.  Now developers will first collect the information of the project and they will decide on which language  they are going to write the code for this project.  So here we have the number of source code.  Suppose the developer has decided they are going to use the java language to write this code.  So here the developers, they will start working on their laptop.  So for source code management, the developers can use any tools for this example.  I'm just taking that git.  
	- So developers, **they will just install a git software in their laptops** and they will start writing  the code.  And **here in the GitHub that is a centralized location or we can say a web based git repository.**  
	- So here you have to **create a repository for this project** and all the five developers who is working  on this project, they will start writing on this project and they will coordinate each other.  And finally, once they will feel that they have written the code for this project, they will **push  the project or commit the code into the GitHub.**  
	- We have already created one repository.  So the code finally move into your GitHub repository.  This is what happened in the **continuous development**.  
 
2. Now, next is **continuous integration.**  Now we have the **code available in the GitHub.**  Now the **next step is you need to build the package.**  
	- So here you can see that the developers has written the code in java language.  So for **Java language, the tool is Maven.**  So with the help of Maven tool, you are going to **build the package**.  
	- Once the **package is built,** you are going to **check the code quality.**  So here the operations team is going to build the package.  Suppose the operation team, while they are trying to build the package, they are getting some errors.  
	- So they will coordinate with the developers.  They will pass on those errors to the development team.  Like while building the package, I am getting these errors.  So the developers teams further going to recreate the code for you.  And they will again push the code into the GitHub.  And then you can recreate the package using the Maven tool.  
	- So here you can see for different different language.  We have different tools.  Now, **once the package has been built, you can do the code quality check with the help of tool SonarQube**.  
	  
	  - **SonarQube,** so now **you now you have the package.**  So the **build package will be either in the `jar` format or `var` format.**  So there will be a single file for this build.  **We can say it is a build artifacts.**  
	  - Now you need to **save that file into the repository.**  So here you can **use the third party software like Nexus or Jfrog.**  
		  - So the question is here, **why don't we use the GitHub to save this build artifacts?  Or built.**  
			  - So the answer is the **GitHub is having the limitation of space.**  Suppose after building the package, the size of the package is of 3 GB.  GitHub is not providing you 3 GB space.  So in that case you can go with the third party software.  
	
	- So here you can see **building the package code quality test and**.  Deploy the package into the repository.  
	- So here, **if you want to work all the things automatically with the help of Jenkin,** then it is called  **continuous integration**.  
		- So as soon **as the code has been commit to the GitHub**, the **Jenkins will automatically pull that code  from the GitHub** and you **will integrate the git maven SonarQube, Nexus with the Jenkin**.  So Jenkins Automation Tool. 
		   
		- So as soon as the code will be **pushed or commit to the GitHub**, the **Jenkins will pull that code and with the help  of Maven, it is going to build the package**.  It is **also going to check the code quality and finally it will push the package into the repository.**  So this is called the **continuous integration.**  
	 
		 - So you can do manually as well as automatically with the help of Jenkins.  So it is up to you if you are doing each and everything automatically then it is called continuous integration.  

3. The next phase of your DevOps lifecycle is **continuous testing.**  
	- Now you have the **package which has been built and also the code quality has been checked.**  And finally your **package is on the nexus.  Repository.**  
	- Now you want to do the **continuous testing of your package,** but where you are going to do the testing  **first, you need to deploy your package so you can deploy your package into the test environment.**  
		- You can create any VM and on that VM, you can deploy your package.  
		- So first you will do the **testing of your package on test environment, on a staging environment, pre-production  environment, or UET environment**.  
		- So before to **move your package into the production environment**, you have to do the continuous testing  of your package.  So here **deploy to the test environment, run in integration test, load test and any other test so deployed  software or deployed package can be tested continuously for bugs using the tools like selenium and test.**  
		- And so here you can see so here you can see a few DevOps tools that comes under the continuous testing  Suppose at this phase, continuous testing your packages got fail.  
		- So in that case, again, the operation team the IT team will talk to the DevOps teams and the DevOps.  team is going to recreate the package according to the errors.  
		- Suppose your **package has been passed from the continuous testing.**  

4. The next phase is your **continuous feedback.**  So hear the **customer feedback to improve the working of the software product and release new versions,**  based on the response.  
	- Suppose your package has been tested properly from the continuous testing.  It has been passed from the continuous testing.  Now you will show your product to the customer.  
	- At that point, your customer may provide you some feedback.  Maybe they are not happy with the outcomes.  They want some modification in the product.  
	- So again, you will talk to the development teams and provide the feedback of the customer or the client,  so that the developers team again work on that.  So here this comes under the continuous feedback.  
	- Suppose the customer is also happy with the outcomes of the product. 
	  

 
4. The next stage is your **continuous deployment.**  So here you can see in the continuous deployment there are 2 phases staging.  We have already understood.  
	- During the staging you will do the continuous testing.  Now your product has been passed from the continuous testing and also from the Continuous feedback.  
	- Now you are good to move your product or your software or your package into the production environment.  So finally you will move your package into the production environment suppose still here.  
	- The deployment of the package to the production environment is completely automated with the help of  Jenkin then it is called CICD.  
	- So in CI we have the continuous integration.  Once the package is deployed on the GitHub, automatically it will build the package and also it will  do the code quality check.  And then finally it will move your package into the Nexus repository.  And once the package is in the Nexus repository, it will automatically build the image using the Docker  container and it will deploy to the VM.  So here the Docker and ansible comes into the picture.  Now you have deployed your package into the staging test environment.  So everything is doing with the help of Jenkins, Ansible and Docker and the deployed package will be  tested using the various tools like selenium and test.  And so this also you can integrate with the Jenkins.  And finally you will have the feedback from the customer suppose.  Everything is doing automatically with the help of full Jenkin and the various tools.  Here you can see it is integrated with a jenkin and finally you have moved your package into the production  environment.  So this is called **continuous deployment**.  
	  
	  - Suppose your package, your **software is in the test environment and before to move your package into  the production environment,** you **need some approval from the customer**.  If you have such type of environment, then it is called **continuous delivery.**  
		    - So in **continuous delivery you need some approval from the client.**  
		    - If it is **completely automated, you are not supposed to do anything manually** then it is called **continuous  integration and continuous deployment.**  

5. So finally, once you deploy your software or package into the production environment, it requires  monitoring.  So you can monitor the server, you can monitor the package, you can monitor the you can monitor the  subsystems like CPU, memory disk utilization.  
	- And a lot of things you can monitor on the servers using various softwares like ELK, Grafana, Prometheus.  and Nagios.  
	- So these are the important tools that comes under the continuous monitoring.  So this is all about the DevOps lifecycle.  
	- So hope with this diagram you can easily correlate between each DevOps lifecycle and understood what  are the various tools comes under each phase of DevOps lifecycle.  So that's all we'll see you in the next lecture.