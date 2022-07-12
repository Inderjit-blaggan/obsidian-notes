![[Pasted image 20220708134239.png]]
SonarQube works on the port number `9000`
- We cant run sonaqube as root, as it used elastic seach on backend and Elastic seach cannot run as root
	- To integrate sonarqube with the maven we need to used the sonar maen scanner, and to configure it we need to add the plugins files to the `pom.xml`  file.
	- Link to sonarscanner for maven https://docs.sonarqube.org/latest/analysis/scan/sonarscanner-for-maven/ 
	
	- ### How to Fix Version of Maven Plugin

		- It is recommended to lock down versions of Maven plugins:
			
			```
			<build>
			  <pluginManagement>
			    <plugins>
			    #add the plugins in the pom.xml
			      <plugin>
			        <groupId>org.sonarsource.scanner.maven</groupId>
			        <artifactId>sonar-maven-plugin</artifactId>
			        <version>3.7.0.1746</version>
			      </plugin>
			      
			    </plugins>
			  </pluginManagement>
			</build>
			```
![[Pasted image 20220708134349.png]]
	## Connecting Sonarqube to maven
	1. We can login to sonarqube, Click on my account  >> Security >> Generate Tokens >> Copt the token and store it
	2. Run the following command to do the sonarqube analysis from the maven server from the directory having pom.xml: `mvn sonar:sonar -Dsonar.host.url<url> -Dsonar.login=<add the genrated token>`
 ![[Pasted image 20220708134447.png]]