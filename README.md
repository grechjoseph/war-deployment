Servlet containers expect the applications to meet some contracts to be deployed. For Tomcat the contract is the Servlet API 3.0.

To have our application meeting this contract, we have to perform some small modifications in the source code.

First, we need to package a WAR application instead of a JAR. For this, we change pom.xml with the following content:

1. <packaging>war</packaging>

Now, let's modify the final WAR file name to avoid including version numbers:

1. <build>
2.     <finalName>${artifactId}</finalName>
3.     ... 
4. </build>

Then, we're going to add the Tomcat dependency:

1. <dependency>
2.    <groupId>org.springframework.boot</groupId>
3.    <artifactId>spring-boot-starter-tomcat</artifactId>
4.    <scope>provided</scope>
5. </dependency>

Finally, we initialize the Servlet context required by Tomcat by implementing the SpringBootServletInitializer interface:

1. @SpringBootApplication
2. public class SpringBootTomcatApplication extends SpringBootServletInitializer {
3. }

To build our Tomcat-deployable WAR application, we execute the mvn clean package. After that, our WAR file is generated at target/spring-boot-tomcat.war (assuming the Maven artifactId is “spring-boot-tomcat”).

We should consider that this new setup makes our Spring Boot application a non-standalone application (if you would like to have it working in standalone mode again, remove the provided scope from the tomcat dependency).
