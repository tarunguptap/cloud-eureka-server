# cloud-eureka-server
spring cloud eureka server

## Lab 4 - Create a Spring Cloud Eureka Server

**Part 1, create server**

1. Create a new Spring Boot application.
  - Name the project "cloud-eureka-server”, and use this value for the Artifact.  
  - Use JAR packaging and the latest versions of Java.  
  - Boot version 2.0.1 is the most recent at the time of this writing, but you can generally use the latest stable version available.  
  - Select Eureka Server dependency.

3. Add a dependency for group "org.springframework.cloud" and artifact "spring-cloud-starter-netflix-eureka-server".  You do not need to specify a version -- this is already defined in the parent project.  

4. Save an application.yml (or properties) file in the root of your classpath (src/main/resources recommended).  Add the following key / values (use correct YAML formatting):
  - server.port: 8011
  - eureka.client.register-with-eureka: false
  - eureka.client.fetch-registry: false

5. (optional) Save a bootstrap.yml (or properties) file in the root of your classpath.  Add the following key / values (use correct YAML formatting):
  - spring.application.name=cloud-eureka-server

6. Add @EnableEurekaServer to the Application class.  Save.  Start the server.  Temporarily ignore the warnings about running a single instance (i.e. connection refused, unable to refresh cache, backup registry not implemented, etc.).  Open a browser to [http://localhost:8011](http://localhost:8011) to see the server running.

**Other properties of application.yml
- eureka.client.service-url.defaultZone: http://localhost:8012/eureka/,http://localhost:8013/eureka/
This property is used to define the other instances of the Eureka server.

**Refer below link for configuring Peers
- https://projects.spring.io/spring-cloud/spring-cloud.html#_peer_awareness


  **BONUS - Refactor to use multiple Eureka Servers**  
    
  To make the application more fault tolerant, we can run multiple Eureka servers.  Ordinarily we would run copies on different racks / data centers, but to simulate this locally do the following:

1.  Stop all of the running applications.

2.  Edit your computer's /etc/hosts file (c:\WINDOWS\system32\drivers\etc\hosts on Windows).  Add the following lines and save your work:

  ```
  # START section for Microservices with Spring Course
  127.0.0.1       eureka-primary
  127.0.0.1       eureka-secondary
  127.0.0.1       eureka-tertiary
  # END section for Microservices with Spring Course
  ```

3.  Within the cloud-eureka-server project, add application.yml with multiple profiles:
primary, secondary, tertiary.  The server.port value should be 8011, 8012, and 8013 respectively.  The eureka.client.serviceUrl.defaultZone for each profile should point to the "eureka-*" URLs of the other two; for example the primary value should be: http://eureka-secondary:8012/eureka/,http://eureka-tertiary:8013/eureka/

4.  Run the application 3 times, using -Dspring.profiles.active=primary (and secondary, and tertiary) to activate the relevant profile.  The result should be 3 Eureka servers which communicate with each other.

5.  In your GitHub project, modify the application.properties eureka.client.serviceUrl.defaultZone to include the URIs of all three Eureka servers (comma-separated, no spaces).

6.  Start all clients.  Open [http://localhost:8020/sentence](http://localhost:8020/sentence) to see the completed sentence.

7.  To test Eureka’s fault tolerance, stop 1 or 2 of the Eureka instances.  Restart 1 or 2 of the clients to ensure they have no difficulty finding Eureka.  Note that it may take several seconds for the clients and servers to become fully aware of which services are up / down.  Make sure the sentence still displays.


