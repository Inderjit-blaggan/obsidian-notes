- Interview Question youtube : https://www.youtube.com/watch?v=IbmFE-eGIpc 

1. Which directory in maven stores the dependencies files in the local directory?
- `$(Home)/.m2``
![[Pasted image 20220708120006.png]]


2. List out the build, source and test source directory for POM in Maven?
- Build = Target
- Source = src/main/java
- Test = src/main/test
![[Pasted image 20220708135459.png]]


3. What information does POM contains?
POM file contains the following information
- project dependencies : inside the ``<dependencies>`` tag
- plugins:  inside the ``<plugins>`` tag
- goals
- build profiles
- project version
- developers
- mailing list



4. Explain Maven lifecycle Phases? 
-   `validate` - validate the project is correct and all necessary information is available
-   `compile` - compile the source code of the project
-   `test` - test the compiled source code using a suitable unit testing framework. These tests should not require the code be packaged or deployed
-   `package` - take the compiled code and package it in its distributable format, such as a JAR.
-   `verify` - run any checks on results of integration tests to ensure quality criteria are met
-   `install` - install the package into the local repository, for use as a dependency in other projects locally
-   `deploy` - done in the build environment, copies the final package to the remote repository for sharing with other developers and projects.


5. What happens when we run `mvn install`
- The mvn install command run and install the plugins used in the install phase to add artificats to the local repository.
- The install plugin  used the information in the POM ( groupID , artifact id, version) to determine the proper location of the artifact within the local repository.
- Install the packages into the local repository, for use as a dependency in other project locally

Inshort: it will look for dependency block in the pom.xml file and download then for future use inside the `.m2 directory`
![[Pasted image 20220708141242.png]]



6. Why maven takes much time for the 1st execution and from 2nd executio it will take less time?
- In the first run maven need to download all the dependencies needed in the machine configured local repository and usually store in `User_folder/.m2/repository`
- As the next time it will be run the dependencies are already downloaded and can be used from the .m2 directory



![[Pasted image 20220708141922.png]]

![[Pasted image 20220708142130.png]]


![[Pasted image 20220708142153.png]]
![[Pasted image 20220708142430.png]]



![[Pasted image 20220708142531.png]]



![[Pasted image 20220708142717.png]]