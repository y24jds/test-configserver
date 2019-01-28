# test-configserver
Spring Cloud Config Server with Composite Repositories and monitor dependency

# Steps to reproduce issue of using composite repositories and spring-cloud-config-monitor dependency

1. Used spring Initalizr to create new project with Spring Boot 2.1.2, Config Server, and Cloud Bus.
https://start.spring.io/
Using the Greenwich.RELEASE version of spring-cloud
2. Also added the spring-cloud-config-monitor dependency to pom.xml
```xml<dependency>
<groupId>org.springframework.cloud</groupId>
<artifactId>spring-cloud-config-monitor</artifactId>
</dependency>
```
3. Also added the AMQP dependency to pom.xml
   <dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-bus-amqp</artifactId>
		</dependency>
    
4. Make sure that the main application class is annotated with: @SpringBootApplication, @EnableConfigServer, @Configuration

5. Add the following to the application.properties file (note, it doesnt really matter if the git repo's point to a valid repo or not, just the fact of using a composite repository definition and adding at least 2 repo's breaks spring-cloud-config-monitor):
spring.profiles.active=composite

spring.cloud.config.server.composite[0].type=git
spring.cloud.config.server.composite[0].uri=http://github.com/y24jds/common.git

spring.cloud.config.server.composite[1].type=git
spring.cloud.config.server.composite[1].uri=http://github.com/y24jds/{application}.git

6.  Start the application

# Issue:
Observe that the following is logged:
***************************
APPLICATION FAILED TO START
***************************

Description:

Field scmRepository in org.springframework.cloud.config.monitor.FileMonitorConfiguration required a single bean, but 2 were found:
	- git-env-repo0: defined by method 'build' in null
	- git-env-repo1: defined by method 'build' in null


Action:

Consider marking one of the beans as @Primary, updating the consumer to accept multiple beans, or using @Qualifier to identify the bean that should be consumed

