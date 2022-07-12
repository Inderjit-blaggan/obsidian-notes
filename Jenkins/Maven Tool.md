
| Flags                    | Descriptions                                    |
| ------------------------ | ----------------------------------------------- |
| `mvn package`            | command used to build the package                                  |
| `mvn clean`              |                                                 |
| `java -jar package-name` | This will run the java application on port 8080 |
|  `mvn clean deploy`   | cleanly build and deploy artifacts into the shared repository.  |


- Hi.  In this lecture, I'm going to discuss a very important topic that is Maven Tool.  
- So basically, **Maven Tool is to build a package of Java** related application.  
- Suppose your developers, they have developed one project and the source code they have used is Java.  And finally, once they will build the package, they will use the **Maven or Gradle** tool because they  **have written the code in Java.**  
- **Apache maven is a software project management and comprehensive tool** and based on the concept of **project  object model** `POM`.  
- So that's a very important file.  So once I will be doing the lab session, I will explain you this file.  So basically, the developers are responsible for creating this file.  In this file, they define that group ID.  Your plugins, the dependencies, the version, the artifacts, the location.  
- So a lot of information is there defined in this file and your maven tool.  When they will try to **build the package, they refer to this file.**  
- So your developers are responsible for this file as a DevOps engineer.  You need not to worry about this file.  You just need to have a knowledge about this file.  What?  All the entries we have in this file.  Suppose if you want to integrate Maven with some other tool, then what are all the dependencies on?  What?  All the plugins you need to add in your `POM.xml` file that you need to know.  
- As a DevOps engineer.  Maven can manage a project, build reporting and documentation from a central piece of information  for the person building a project.  So here who is building the project?  Your DevOps team.  
- This means that it is only necessary to learn a small set of commands to build any Maven project  and the POM will ensure they will get the results they desire.  
- So as a DevOps engineer, you just need to learn only few commands of Maven to build a package  like ambient, clean install, ambient clean package, and ambient clean deploy.  
- So there are only few commands that you need to learn and your developers team, they will ensure that  your POM file is created correctly.  
- So let me show you in a diagram where this tools comes into the picture in DevOps lifecycle.  So here you can see this is the client and client has given one project to this company.  So this company is having the developers teams.  So developers teams first start gathering the information about the project and finally they will choose  one source code, the appropriate source codes which can be used to write that coding for this project.  
- Suppose they have chosen java language.  Now they will use the git or some other software as well, which is available in the market.  So this is a local distributed version control system.  
- With that git software, they will write the code.  And finally, they will push the code into the central location.  That is your web based Git repository, that is GitHub.  Now, all the developers, they have worked together and finally they have developed the code for this  project.  Now the next step is build the package.  
- So here you can see the source code is Java.  So they are going to use the Maven tool.  
- If it is Nodejs, it would be NPM.  So like this way, we have different, different tools for different applications.  So that's all.  In the next lecture I will discuss about the lifecycles of Maven.  So that's all, we'll see you in the next lecture.


## Build lifecycle of maven 
- Link to doc : https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html

- So there are 3 built-in, build lifecycles, that is your `default clean` and `site`.  So we need to understand each one in detail.  
	- So the first one is the **default lifecycle** which **handles your project deployment.**  
	- The second one is your the **clean lifecycle**, which **handers project cleaning.**  
	- And the third one, the **side lifecycle,** **handles the creation of your project's website.**  
	
#### A Build Lifecycle is Made Up of Phases[](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#a-build-lifecycle-is-made-up-of-phases)

Each of these build lifecycles is defined by a different list of build phases, wherein a build phase represents a stage in the lifecycle.

For example, the default lifecycle comprises of the following phases (for a complete list of the lifecycle phases, refer to the [Lifecycle Reference](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Lifecycle_Reference)):

-   `validate` - validate the project is correct and all necessary information is available
-   `compile` - compile the source code of the project
-   `test` - test the compiled source code using a suitable unit testing framework. These tests should not require the code be packaged or deployed
-   `package` - take the compiled code and package it in its distributable format, such as a JAR.
-   `verify` - run any checks on results of integration tests to ensure quality criteria are met
-   `install` - install the package into the local repository, for use as a dependency in other projects locally
-   `deploy` - done in the build environment, copies the final package to the remote repository for sharing with other developers and projects.

These lifecycle phases (plus the other lifecycle phases not shown here) are executed sequentially to complete the `default` lifecycle. Given the lifecycle phases above, this means that when the default lifecycle is used, Maven will first validate the project, then will try to compile the sources, run those against the tests, package the binaries (e.g. jar), run integration tests against that package, verify the integration tests, install the verified package to the local repository, then deploy the installed package to a remote repository.
  
- So when you **build a package,** you **always go with the default lifecycle.**  
- Suppose today I have build the package, right?  And next day, your developers, they have done some changes in the code.  Now they want to **recreate the or we can say they want to rebuild the package.**  But one day before I had already created the package.  So in that case, **I can go with the clean**.  
	
	- ### What will happen when we use the clean lifecycle?  
	- It will **delete the previous day package** and **then it will recreate your new package**.  
	- So this is the clean lifecycle handles the project cleaning?  

- The **last one is the site lifecycle** handles the **creation of your project's website**.  So let me go to the official website of the Maven.  So here you can see this is the official website.  It is the introduction to the build lifecycle.  And here you can see the lifecycle is made up of phases.  
  
	- ## Default lifecycle comprises of the following phases.  
		1. `validated`  So it validated the project is correct and all the necessary information is available.  So here the first stage is your validate.  So once the developers they have written the code, then the Maven comes into the picture.  So once you will fire this command `nvm validate` it will validate your coding, it will check all  the necessary dependencies.  Plug ins are already placed in the coding part or not.  
		- So here it will look for the POM file in the POM file.  It will look for all the dependencies.  Suppose at this is state.  If it is found.  If it is found some error.  It will show you those errors.  And then as a DevOps engineer, you will ask to the developers, teams that I'm getting these errors.  You just need to fix those errors.  
		  
	2. `compile`  It will **compile the source code of the project**.  So here we know that.  Your computer understand the coding in the binary like 0 and 1.  So here at this stage, it will compile the source code of the project.  
	3.  `test` and `package` is next.  It will **test the compile source code using a suitable unit testing framework.**  These tests should not require the code be packaged or deployed.  So here it will just do the testing so they have certain parameters.  So it will look for all those parameters and will do the testing of your testing of your coding.  Finally, if everything seems fine, it is going to build your packet.  
		- Suppose if will simple fire `mvn clean package`.  What will happen? 
		- It **will validate.  It will compile.  It will test.**  And it will **create the package**.  
		- So here it will create the **file in jar**.  Extension would be JAR.  So basically that information, what would be the extension that will be defined in your pom file,  you on JAR extension or VAR.  
		  
	5.  `verify` run any **checks on test of intigration test to ensure the quality criteria are met**.  So in this phase it will verify and check for the quality criteria.  
	6. `install` and then `deploy` so you have the package.  If you want to install it, you can go with that install the package into the local repository for  use as a dependency into the projects locally.  
		- Suppose your package that you have created and you want that the package should be used by any  other project.  Then in that case you can go with that option, you can install it.  Deploy done in the build environment.  
		- Suppose you have **created the package**.  Now you want to **deploy your package into some different.**  System or local system.  Or you wanted to **deploy to some third party software like a Jfrog, Nexus**.  
		- So here you can go with that, done in the develop environment copy is the final package to the remote  repository for sharing with other developers and projects.  
		- So these are the 7 phases of the default lifecycle.  So that's all we will see you in the next lecture.

## Repositories in Maven

Hi.  So let's continue and understand about the repositories that we have in the Maven.  
- So there are **3 types of repositories.**  
	1. The first one is the local.  
	2. The second one is the remote and private.  
	3. And the third one is the central and public.  
![[Pasted image 20220708120006.png]]
- So here, what is the meaning of repository?  Suppose I need to build a package.  My developers they have created the coding for the project.  I'm going to use the Maven tool.  
- So the Maven tool, **once it will try to build the package, it requires so many dependencies**.  So **those dependencies, if it is available on the local system, it is going to download from there.**  
- Suppose the dependencies are not available on the local machine.  
- Then it will go for the private location or the remote location.  S
- suppose those dependencies or those plug ins are also not available there.  Then it will go to the public.  It will try to download from the internet.  So here it will go for the Maven website.  And from there it will try to download the required dependencies to build the package.  
	- So this is the first time exercise.  Suppose I have build the package and first time it has downloaded so many files from the different different  websites that you have defined in your  pom.xml file 
	- The second time.  When you recreate the package that time, it is not again going to the public or center location because  it will downloaded those dependencies into the local machine.  
	- And here we have this location, this $ home inside of this, we have .m2 file.  It is going to create .m2 file.  And then we have the repository directory in this location.  It will have all the required dependencies.  
	- So this is for the project on which the developers are working.  Suppose there is different project.  So obviously for different projects there would be different dependencies, but for the same project  it will look for the same location.  Right.  And one more thing.  
	- Suppose you have created the package.  Your package will always created inside the target directory and the extension would be your jar or var.  
	- So whatever you have defined in the pom file, accordingly it is going to create your artifacts file.  Right?  I will show you practically.  
	- So the next lecture would be your practically.  So whatever we have discussed, theoretically.  I'm going to show you each and everything practically so that you will have clear picture about the  Maven tool.  So that's all will see you in the next lecture.  

