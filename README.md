- # Microservice Design Patterns and Principles with example
Microservice architecture has become very popular due to it's aglity, scalability, resilience and maintainability. In this article,  have explained and demonstrated important design patterns and principles that are mostly being followed for microservice style application. 
 
- # Brief explanation of key design principles for microservices architecture used in the project:
  - <b>Database per Service ![#f03c15](https://via.placeholder.com/15/f03c15/f03c15.png) `Database Pattern`</b>
    <p>One important criteria of microservice is must be loosely coupled so that it can be built, maintained and deployed independently. Sometime, each service may have different data storage requirement and easy replication is also a client ask in order to scale. To mange these, database per service is a good approach where restriction of the database can be controlled by only specific service, can be scaled based on the demand, size of the database will be small as compare as database per application which turns easy maintenance.</p>
  - <b>API Gateway ![#f03c15](https://via.placeholder.com/15/f03c15/f03c15.png) `Integration Pattern`</b>
    <p>Most of the cases an application can contains more than thousand of microservices and each microservice have diffrent end points so managing all servicess a common access point is necessary. API Gateway creates an common entry gate for all microservices of the application or for group of microservices of the application and routes the traffic to concerned services as a proxy. Additionally, it provides features like aggregation of services, caching, authentication and rate limiting etc</p>
  - <b>Externalized Configuration ![#f03c15](https://via.placeholder.com/15/f03c15/f03c15.png) `Cross-Cutt Aling Concern Pattern`</b>
    <p>Often services need to be run in diffrent environment like DEV, ST, UAT etc and to manage those environments different configurations are required in various properties file. A change in any of those properties a new build and re-deployment is required to be reflected. Externalize configuration helps to overcome the problem of rebuilding and redeployment when configuration of the application are changed. </p>
  - <b>Circuit Breaker ![#f03c15](https://via.placeholder.com/15/f03c15/f03c15.png) `Cross-Cutting Concern Pattern`</b>
    <p>In a microservice based architecture, most of the services are inter dependent and lot of time they communicate internally to fulfill user's need. It is obvious that any time any downstearm service can move to non responding state or down state due to system issue. At that moment, requests are coming to a service can be sent to downsteam service that is in down state and unnecessarily uses system resources.If this process continues for longer period of time for lerge of request then there may be high chance that working service cane also be moved to down state due to to the unavailabilty of the system resouces. Circuit Breaker is used to detect the failure and prevent cascading effect </p>
  - <b>Service Discovery ![#f03c15](https://via.placeholder.com/15/f03c15/f03c15.png) `Cross-Cutting Concern Pattern`</b>
    <p> Now a days services are deployed in distributed system and sometime IP and Port are dynamically assigned to the services so it is very difficult to remeber the end point of services by consumer and most importantly every time address changes will break consumer service. So, services registry needs to be created to store all metadata of the producer services and everytime time new service added in network must be registered in service registry during start or de register during shutdown. All consumer service must query in service registry to pull correct producer service location before calling</p>
  - <b>Distributed Tracing ![#f03c15](https://via.placeholder.com/15/f03c15/f03c15.png) `Observability Patterns`</b>
    <p> In microservice architecture, request often span across different services to perform various operations. It is quite difficult to trace the request and response of service call for quick troubleshooting. Distributed Tracing patterns help to associate Trace Id and Span Id in each service call and Trace Id will be same for entire round trip of the request but Span Id will be diffrent for any intermediary service call. So using Trace Id entire journey/call stack can be idetified.</p>
  - <b>Health Check ![#f03c15](https://via.placeholder.com/15/f03c15/f03c15.png) `Observability Patterns`</b>
     <p> To detect correct status of the service is very important so that request will not be routed to any unhealth service. /health API is used to validate the status of the service and based on that instance can be declared as healthy or unhealthy. </p> 
  - <b>Authentication and Authorization ![#f03c15](https://via.placeholder.com/15/f03c15/f03c15.png) `Security Patterns`</b>
     <p> security is an important aspect of microservice architecture and we should follow standard Oauth2.0 process for securing our APIs to avoid any unauthorize access of data, function and system. This can be acheived by authenticating user and authorize their access of APIs based on defined role/authority.  </p> 
- # Use Case
<p> I have considered a simple use case to demonstrate the above said design patterns and principles. A educational society has decided to launch few model schools for child to provide better way of learning. They are looking for a "School Managment Software" to perform most operations though online processes. </p>

- Society can add new school in system and also can update various parameters of existing school like location, school governance details.
- Society has the the authority to delete school from the list.
- Society can add/update/delete student in a school.
- School Fees for student can be generated monthly basis.
- Studdent can view their details and correct in case necessary.
- Student can make payment of the fees.
 
- ## Technology stack
Here, I have used spring boot framework to implement "School Managment Software" by considering above design patterns. Below are the technology stacks involved:
  - java 17 (JDK 17)
  - Spring Boot 3.1.3
  - Spring Cloud 2022.0.4
  - Maven 3.9.2
  - H2 Database.

- ## Architecture
I also have provided an overall architecture diagram for clarity purpose. Definitely there is scope for improvement and I will be adding more features to make it robust further.
  ![Student-Management](![school-management-architecture-img](https://github.com/mail2mrcm/springboot-microservices/assets/118661926/03c3e807-886b-40bd-8d4f-101127f45da5)

- ## Project Structure & Components
As said above, spring boot framework is used for the development, various spring boot and spring cloud provided components/libraries are used to build this architecture. 
|Spring Libraries/Components|Purpose|Reference|
|---------------------------|-------|---------|
|Spring Cloud Config |Config server is used for externalize the configurations. Group Id `org.springframework.cloud` and Artifact Id `spring-cloud-config-server` is added for enabling config server and all client applications have  Group Id `spring-cloud-config-server` and Artifact Id `spring-cloud-starter-config`|[Config Server](https://docs.spring.io/spring-cloud-config/docs/current/reference/html/)|
|Spring Cloud Gateway|This is an API gateway is used to create common entry of all APIs involved. Group Id `org.springframework.cloud` and Artifact Id `spring-cloud-starter-gateway` is added in dependency|[Service Discovery](https://spring.io/guides/gs/service-registration-and-discovery/)|
|Spring Cloud Circuit Breaker| Resilience4j library used for fault tolerance and circuitbreaker.Group Id `org.springframework.cloud` and artifact ID `spring-cloud-starter-circuitbreaker-reactor-resilience4j` is added as dependency for circuitbreaker|[Circuit Breaker](https://docs.spring.io/spring-cloud-circuitbreaker/docs/current/reference/html/)|
|Spring Cloud Netflix Eureka |This is client side service discovery allows services to find and communicate with each other without hard-coding the hostname and port. Group Id `org.springframework.cloud` and Artifact Id `spring-cloud-starter-netflix-eureka-server` is added in discovery server application and Group Id `org.springframework.cloud` and Artifact Id `spring-cloud-starter-netflix-eureka-client` is added in client applications for registering in eureka|[Service Discovery](https://spring.io/guides/gs/service-registration-and-discovery/)|
|Spring Cloud Gateway |This is an API Gateway. Group Id `org.springframework.cloud` and artifact ID `spring-cloud-starter-gateway` is added in dependency for API Gateway feature|[Gateway](https://cloud.spring.io/spring-cloud-gateway/reference/html/)|
|Spring Boot Actuator| This is used for health check of the application. Group Id `org.springframework.boot` and Artifact Id `spring-boot-starter-actuator` is added for the same.|[Health Check](https://www.baeldung.com/spring-boot-actuators)
|H2 Database| Dedicated H2 database is used for every microservices involved in this project. this is being used for demo purpose but for real application it should be any RDBMS or NoSQL database. Group Id `com.h2database` and Artifact Id `h2` is added in POM file|
|Spring Authorization Server|This framework is used for authentication and authorization purpose. Group Id `org.springframework.boot` and Artifact Id `spring-boot-starter-oauth2-authorization-server` is added in dependency for authorization process.|[Auth Server](https://docs.spring.io/spring-authorization-server/reference/getting-started.html)
|Spring Resource Server|This is used for validating user's access and takes permit/deny actions while user tries to access APIs/resources. Group Id `org.springframework.boot` and Artifact Id `spring-boot-starter-oauth2-resource-server` is added in dependency. As this works on top of spring security so spring securtity dependency Group Id `org.springframework.boot` and Artifact Id `spring-boot-starter-security` is also added.|[Resource Server](https://docs.spring.io/spring-security/reference/servlet/oauth2/resource-server/index.html)

  ![image](https://github.com/mail2mrcm/springboot-microservices/assets/118661926/f68de201-2c2b-4c5c-bded-90c67821bd68")
  - `Spring Config server` - This is used to externalize application configurations. [Spring Cloud Config](https://docs.spring.io/spring-cloud-config/docs/current/reference/html/)
     I have loaded configuration from resources so used profile = native and all configurations are available under /config of resources location.
      
  - `Netflix Eureka as Service discvery` - This is for service registry. configuration of the service registry is available in config server i.e, ***service-discovery.yml***. Please 
     refer [service registry](https://spring.io/guides/gs/service-registration-and-discovery/) for more technical details.
    
  - `Spring CLoud API Gateway` - Gateway of all microservices.  All srevice to be accessed through this gateway. This application is also regstered in service discovery as per the 
     configuration available in ***gateway-service.yml***. [Spring api gateway](https://spring.io/guides/gs/gateway/) for more details
    
  - `Student Miroservice `- add/get/update student and also perform internal communication with Payment microservice to fetch payment details of student. This application is also 
     regstered in service discovery as per the configuration available in ***student-service.yml***.
    
  - `Payment Microservice` - add/make payment function. This application is regstered in service discovery as per the configuration available in ***payment-service.yml***.
    
- ## Buiild & Deployment  :
  1. Build Config server and Deploy config-server1.0.0.jar. Default port 9090 is used.
  2. Build Service discovery application and deploy the same. Default port 9091 is used.
  3. Build Student microservice and deploy it in port 9092.
  4. Build Payment microservice and deploy it in port 9094.
  5. finally build API Gateawy and deploy it in port 9093.
     
- ## Test :
Only 3 APIs are available currently and can be added further <br/> 
 `Add student` - [POST] http://localhost:9093/api/1.0/student <br/>
   ```diff
      curl -X POST -H "Content-Type: application/json" -d '{"orgId": 1, "firstName": "chandan", "lastName": "maity", "gender": "male"}' http://localhost:9093/api/1.0/student/
   ```
 `Add fees to be paid for the student` - [POST] http://localhost:9093/api/1.0/payment <br/>
   ```diff
      curl -X POST -H "Content-Type: application/json" -d '{"studentId": 1, "description": "Semister Fees", "amount": 5000.0, "status": "pending"}' http://localhost:9093/api/1.0/payment/
   ``` 
 `Get student with payments` - [GET] http://localhost:9093/api/1.0/student/{id}/with-payments <br/>
   ```diff
      curl -X GET -H "Content-Type: application/json" http://localhost:9093/api/1.0/student/{id}/with-payments
   ``` 
 `Make payment by student` - [PUT] http://localhost:9093/api/1.0/payment/{id} <br/>
   ```diff
      curl -X PUT -H "Content-Type: application/json" -d '{"studentId": 1, "description": "Semister Fees", "amount": 5000.0, "status": "paid"}' http://localhost:9093/api/1.0/payment/1/
   ```      
