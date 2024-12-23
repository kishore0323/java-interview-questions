System Design:
CAP theorem
How to improve API performance?
REST API vs. GraphQL
Load Balancer and its types
API Gateway and its uses
Different HTTP versions and its features, http status codes
OSI 7 layer model
API design, do's and dont's
Design a system for 1 million request
Design a system for rapido, uber, ola
Design a system for bookmyshow app
Desing a system for ecommerce app
Desing a system for streaming app like prime, netflix
API gateway, Saga pattern (choreography based and orchestration based), request and replay pattern, circuit breaker  
An asynchronous lock free ring buffer for logging - https://steven-giesel.com/blogPost/11f0ded8-7119-4cfc-b7cf-317ff73fb671
Oauth1 vs Oauth2
http and https working

Angular Most important Concepts:
4 ways of Data Binding, Component Communication, Component Lifecycle, Directives, Pipes
Observables, Ivy, Lazy Loading, SSR, Templates, Services, Injectable,
Routing, Authentication, List of some most used Decorators, Type script basic syntax
Modules, Shallow Copy and Deep Copy, Web Components

Java OOPS principles, Strings uses and its methods, Immutable Object, Singleton Class, Stack and Heap Memory, Garbage collection, Interfaces, Java 8 features, steams examples, comparator and comparable interface, Serializable, Design Patterns, functional interfaces and examples Collections- Map methods and internal working, changes in java 8, , differences, internal algorithms used and uses of each collection, concurrent modification exception, types of thread safe collections, collection hirachcy, Memory leak, Out of memory error and how to handle and use Profiler, Types of exception and errors, differnt ways handling exception and rules Concurrency API- Executors, CountdownLatch, Callable, Completable Future, Inter thread communication, Countdown latch, Cyclic Barrier, Semaphore, thread life cycle, thread and runnable, thread creation using lambda, Deadlock, Race Condition, thread safe variable and collections, Threadpool, Thread Schduler, ExectutorsThreadpool, Java Reflection API, Different ways of creating java objects, Clone, Class laoders, Aggregation and Composition, Access modifiers, static, final, finalise, synchronize, Method overriding rules and overloading, abtract class and interface difference, fail safe and fail fast, string buffer and builder, classpath, classnotfound, classdef exception

SpringBoot IOC, DI, Application Context, bean context, Bean Scope, Bean Factory, Rest API best practices, how to create Custom Annotation and its types, Custom Exception Handler, Controller and RestController, MVC Flow, How to create singleton and prototype bean,Headers, Cookie, Session Qualifier, Primary, Asyc, Profile, Value Anntoations, Bean life cycle and its annotation, AOP, Advice, Http Status Codes, ComponentsScan Spring Security, multiple database Configuration, How to add libraries if spring version has conflict & doesnot support OAuth and OAuth2 Difference, JSON and XML APIs parsing, Swagger Config, idempotent and non idempotent methods, Proxy, Factory, Builder, Decorator, Facade Design Pattern used in Spring boot Filter and Authentication Interceptor, Spring framework and spring boot difference Junit and Mockito Annotations, Test Driven (Rest Assured) and Behavorial Driven

Spring Data JPA and Hibernate Pagination, Hibernate query types- Criteria, Native, HQL, CRUD & JPA Repository L1 and L2 Caching, save, update, persist, load, get Entity Object Anotations - Primary Key, Foregin Key, Relational Mappings, Embbeded Id Transactional annotation and its strategy

Microservices Monolithic and Microservices Differences, Bounded Context, 12 factors, design principles, disadvantages, sharing data base with MS, Why Stateless, Asysnchronous Communciation in MS, Inter MS communication, Saga Pattern, Circuit Design Pattern, API Gateway, Master Slave DP for database in MS, CQRS DP, Distributed tracing, Design patterns used in MS, Design a systems to serve 1 Million Request per second, Types of Saga patterns. Fault tolerance in MS, No of ways to protect REST APIs, Load Balancer, How to Maintain Transaction across MS. Kafka, RabbitmQ, Redis, Elastic Saearch Usage, Concepts and working.

Spring Boot Microservices Coding Question

Scenario:
You're building a microservices-based application for an e-commerce platform. One microservice is responsible for managing product information (Product Service), another manages orders (Order Service).

Tasks:
Create a Spring Boot application for the Product Service:
The service should expose a REST API endpoint /products that returns a list of all available products.
Each product should have properties like id, name, description, price, and imageUrl.
Add a /products/{id} endpoint to retrieve a specific product by its ID. Include proper error handling and documentation for the API.

Create Order Service:
The service should expose a REST API endpoint /orders that allows creating new orders.
Each order should have properties like id, customerName, products (list of product IDs), and orderDate.
Implement a mechanism to call the Product Service /products/{id} endpoint to retrieve product details when creating an order.

Include proper error handling and logging for failed calls to the Product Service."
Add the repository to read from database tables
Write Junit test cases for the above services

AWS
For your ever running production, you need a bare minimum of these --

1. Multiple machines (EC2)
2. Containerise your app (docker)
3. Container orchestration to manage these multiple containers on multiple machines (Kubernetes)
4. Load balancers - To expose your cluster to the world
5. SSL Certificates -- for HTTPS
6. Cloudfront + S3 -- if deploying front-end websites
7. Connect your domains to these above (Route 53)

Developer Productivtiy Tools
Diagram as code
JSON Crack - To visiualize json tree - https://jsoncrack.com/
Product testing, management, monitoring, and devops tools
