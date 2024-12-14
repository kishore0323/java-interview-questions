Java
OOPS principles, Strings uses and its methods, Immutable Object, Singleton Class, Stack and Heap Memory, Garbage collection, Interfaces, Java 8 features, steams examples, comparator and comparable interface, Serializable, Design Patterns, functional interfaces and examples
Collections- Map methods and internal working, changes in java 8, , differences, internal algorithms used and uses of each collection, 
concurrent modification exception, types of thread safe collections, collection hirachcy,
Types of exception and errors, differnt ways handling exception and rules 
Concurrency API- Executors, CountdownLatch, Callable, Completable Future, Inter thread communication, Countdown latch, Cyclic Barrier, Semaphore, thread life cycle, thread and runnable, thread creation using lambda, Deadlock, Race Condition, thread safe variable and collections, Threadpool, Thread Schduler, ExectutorsThreadpool, Java Reflection API, Different ways of creating java objects, Clone, Class laoders, Aggregation and Composition, Access modifiers, static, final, finalise, synchronize, Method overriding rules and overloading, abtract class and interface difference, fail safe and fail fast, string buffer and builder, classpath, classnotfound, classdef exception 

SpringBoot
IOC, DI, Application Context, bean context, Bean Scope, Bean Factory, Rest API best practices, Custom Annotation and its types, Custom Exception Handler, Controller and RestController, MVC Flow, How to create singleton and prototype bean,
Qualifier, Primary, Asyc, Profile, Value Anntoations, Bean life cycle and its annotation, AOP, Advice, Http Status Codes, ComponentsScan
Spring Security, multiple database Configuration, How to add libraries if spring version has conflict & doesnot support 
OAuth and OAuth2 Difference, JSON and XML APIs parsing, Swagger Config, idempotent and non idempotent methods,
Filter and Authentication Interceptor, Spring framework and spring boot difference
Junit and Mockito Annotations, Test Driven (Rest Assured) and Behavorial Driven

Spring Data JPA and Hibernate
Pagination, Hibernate query types- Criteria, Native, HQL, CRUD & JPA Repository
L1 and L2 Caching, save, update, persist, load, get
Entity Object Anotations - Primary Key, Foregin Key, Relational Mappings, Embbeded Id
Transactional annotation and its strategy 

# Spring Quartz vs Spring Batch vs Spring Scheduler

Spring provides several ways to schedule and manage tasks within Java applications, with the most popular solutions being **Spring Quartz**, **Spring Batch**, and the built-in **Spring Scheduler**. Each has specific use cases and benefits. Here's a detailed breakdown of when to use each and how they differ:

### **1. Spring Quartz Scheduler**

**Quartz** is a powerful and feature-rich library for scheduling background jobs. It's suitable for advanced scheduling needs with complex scenarios and requirements.

#### **Key Features**

-   **Persistent Job Store**: Jobs can be persisted in a database, allowing the scheduling information to survive a server restart.
-   **Clustering**: Quartz supports clustering, making it possible to run jobs across multiple nodes (e.g., multiple servers).
-   **Advanced Scheduling**: Supports CRON expressions, calendar scheduling, and other advanced scheduling scenarios.
-   **Job Priorities**: You can define priorities for jobs, allowing more critical jobs to be executed first.
-   **Job Chaining**: Allows setting up jobs to run one after another based on the success or failure of previous jobs.
-   **Listeners**: Provides listeners for monitoring the execution of jobs, triggers, and the scheduler lifecycle.

#### **When to Use Quartz**

-   **Complex Scheduling Needs**: When you need advanced scheduling with complex recurrence patterns.
-   **Persistent Jobs**: If you need jobs to be persistent (e.g., stored in a database) and survive server restarts.
-   **Distributed Applications**: For applications running in clusters where the same job may need to be executed on different nodes.
-   **Job Management**: If you need job monitoring, pausing, resuming, or controlling jobs programmatically.

#### **Example Usage**
```
import org.quartz.*;
import org.quartz.impl.StdSchedulerFactory;

// A simple Quartz job example
public class QuartzExampleJob implements Job {
    @Override
    public void execute(JobExecutionContext context) throws JobExecutionException {
        System.out.println("Executing Quartz Job!");
    }

    public static void main(String[] args) throws SchedulerException {
        JobDetail job = JobBuilder.newJob(QuartzExampleJob.class)
                                  .withIdentity("exampleJob", "group1")
                                  .build();

        Trigger trigger = TriggerBuilder.newTrigger()
                                        .withIdentity("exampleTrigger", "group1")
                                        .startNow()
                                        .withSchedule(SimpleScheduleBuilder.simpleSchedule()
                                                                            .withIntervalInSeconds(10)
                                                                            .repeatForever())
                                        .build();

        Scheduler scheduler = new StdSchedulerFactory().getScheduler();
        scheduler.start();
        scheduler.scheduleJob(job, trigger);
    }
}
``` 

### **2. Spring Batch**

**Spring Batch** is a lightweight framework that enables the development of robust batch applications. It is not a general-purpose scheduling framework but a specialized tool for batch processing large datasets and managing complex job workflows.

#### **Key Features**

-   **Chunk-Oriented Processing**: Data can be processed in chunks (e.g., read 1000 rows, process them, then write).
-   **Declarative Configuration**: Jobs, steps, and workflows can be declared and configured in XML or using annotations.
-   **Job Parameters**: You can pass parameters to jobs for better configuration and flexibility.
-   **Retry, Skip, and Restart**: Provides built-in mechanisms to handle failures, retries, skipping problematic records, and restarting jobs.
-   **Integration with Spring**: Leverages Spring's dependency injection, transaction management, and integration support.

#### **When to Use Spring Batch**

-   **ETL (Extract, Transform, Load) Processes**: When you need to handle complex ETL operations involving large data sets.
-   **Data Migration**: Suitable for data migration tasks where data needs to be moved, transformed, and stored.
-   **Bulk Processing**: Ideal for processing massive amounts of data (e.g., reading from a database and writing to a file).
-   **Complex Workflow**: When you need multiple steps in a job with conditions, retries, restarts, or skippable records.

#### **Example Usage**

```
import org.springframework.batch.core.Job;
import org.springframework.batch.core.JobExecution;
import org.springframework.batch.core.JobParametersBuilder;
import org.springframework.batch.core.launch.JobLauncher;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Configuration;
import org.springframework.batch.core.configuration.annotation.EnableBatchProcessing;
import org.springframework.batch.core.configuration.annotation.JobBuilderFactory;
import org.springframework.batch.core.configuration.annotation.StepBuilderFactory;
import org.springframework.batch.core.step.tasklet.Tasklet;
import org.springframework.batch.repeat.RepeatStatus;

@Configuration
@EnableBatchProcessing
public class SpringBatchExample {

    @Autowired
    private JobBuilderFactory jobBuilderFactory;

    @Autowired
    private StepBuilderFactory stepBuilderFactory;

    @Bean
    public Job exampleJob() {
        return jobBuilderFactory.get("exampleJob")
                .start(stepBuilderFactory.get("step1")
                        .tasklet((contribution, chunkContext) -> {
                            System.out.println("Processing batch job step...");
                            return RepeatStatus.FINISHED;
                        }).build())
                .build();
    }

    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(SpringBatchExample.class);
        JobLauncher jobLauncher = context.getBean(JobLauncher.class);
        Job job = context.getBean(Job.class);

        try {
            JobExecution execution = jobLauncher.run(job, new JobParametersBuilder().toJobParameters());
            System.out.println("Job Status: " + execution.getStatus());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
``` 

### **3. Spring Scheduler (`@Scheduled` Annotations)**

Spring Scheduler is a simple scheduling mechanism that comes out-of-the-box with the Spring Framework. It uses annotations for simple scheduled tasks and is suitable for lightweight scheduling needs.

#### **Key Features**

-   **Simple Configuration**: Minimal setup using annotations like `@Scheduled`.
-   **CRON Expressions**: Supports scheduling with CRON expressions.
-   **Fixed Rate/Delay**: Offers simple scheduling with fixed-rate or fixed-delay options.
-   **Thread Pool Configuration**: Supports task execution using a configurable thread pool.

#### **When to Use Spring Scheduler**

-   **Lightweight Scheduling**: When you have simple scheduling needs, like sending emails every hour or cleaning up temporary files daily.
-   **Non-Persistent Jobs**: If jobs don't need to survive server restarts (no need to store job state in a database).
-   **Application-Level Scheduling**: When you need to trigger periodic actions directly within the application.

#### **Example Usage**

```
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.scheduling.annotation.EnableScheduling;
import org.springframework.context.annotation.Configuration;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
@EnableScheduling
public class SpringSchedulerExample {

    public static void main(String[] args) {
        SpringApplication.run(SpringSchedulerExample.class, args);
    }

    @Scheduled(fixedRate = 5000) // Execute every 5 seconds
    public void scheduleTask() {
        System.out.println("Scheduled task executed - " + System.currentTimeMillis());
    }

    @Scheduled(cron = "0 0 12 * * ?") // Execute every day at 12 PM
    public void cronTask() {
        System.out.println("CRON task executed at 12 PM daily");
    }
}
``` 

### **Comparison: Spring Quartz vs Spring Batch vs Spring Scheduler**

![Screenshot 2024-11-19 at 6 06 05 PM](https://github.com/user-attachments/assets/97799bf7-f461-4c2a-9680-993af0305fe3)


### **Summary of When to Use Each**

-   **Use Spring Quartz** when you need:
    -   Complex scheduling requirements (e.g., multiple triggers, time zones).
    -   Job persistence and clustering.
    -   Job management features (pause, resume, etc.).
-   **Use Spring Batch** when you need:
    
    -   Batch processing of large datasets.
    -   ETL processes or complex workflows with retries, skips, and chunk processing.
    -   Job parameterization, restartable jobs, and transactional support.
-   **Use Spring Scheduler** when you need:
    
    -   Lightweight scheduling tasks that are simple.
    -   Periodic tasks like notifications, report generation, or cleanup jobs.
    -   Easy configuration with simple annotations.

By understanding the strengths and limitations of each framework, you can choose the right solution based on your application's needs for scheduling and batch processing.



# List of security measures for protecting REST APIs in Spring Boot

In Spring Boot, protecting a REST API is crucial for ensuring that only authorized and authenticated clients can access your resources. Below are some of the most common and effective ways to protect a REST API in Spring Boot:

### 1. **Authentication and Authorization with Spring Security**

Spring Security is the standard way to secure Spring applications. It provides a comprehensive set of tools to handle authentication and authorization.

#### a. **Basic Authentication**

-   **Description**: A simple way to protect APIs using a username and password in the HTTP headers. It's easy to set up but generally not recommended for production without HTTPS due to security vulnerabilities.
-   **Best for**: Internal or quick prototypes; not ideal for production without additional security.

**Configuration**:
```
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .csrf().disable() // Disable CSRF for stateless APIs
            .authorizeRequests()
            .anyRequest().authenticated()
            .and()
            .httpBasic(); // Use Basic Authentication
    }
}
```

#### b. **JWT (JSON Web Token) Authentication**

-   **Description**: JWT is a token-based authentication system. A user logs in with their credentials, receives a token, and uses that token for subsequent API calls. The token contains encoded user data and can be verified without accessing a database.
-   **Best for**: Stateless applications, microservices, and scalable architectures.

**Steps**:

1.  Implement user authentication.
2.  Generate a JWT token on successful login.
3.  Protect endpoints with JWT verification.

**Example**:

**Security Configuration**:
```
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .csrf().disable()
            .authorizeRequests()
            .antMatchers("/auth/**").permitAll() // Allow authentication endpoint
            .anyRequest().authenticated()
            .and()
            .addFilterBefore(new JwtTokenFilter(), UsernamePasswordAuthenticationFilter.class);
    }
}
```

#### c. **OAuth2 and OpenID Connect**

-   **Description**: OAuth2 is a framework that allows third-party services to exchange information securely without exposing credentials. OpenID Connect (OIDC) is an identity layer built on top of OAuth2.
-   **Best for**: Integrating third-party identity providers like Google, Facebook, or corporate Single Sign-On (SSO).

**Example** using OAuth2 with Spring Boot Starter:

**application.yml**:
```
spring:
  security:
    oauth2:
      client:
        registration:
          google:
            client-id: YOUR_GOOGLE_CLIENT_ID
            client-secret: YOUR_GOOGLE_CLIENT_SECRET
            redirect-uri: "{baseUrl}/login/oauth2/code/{registrationId}"
            scope: profile, email
        provider:
          google:
            authorization-uri: https://accounts.google.com/o/oauth2/auth
            token-uri: https://oauth2.googleapis.com/token
            user-info-uri: https://www.googleapis.com/oauth2/v3/userinfo 
```
**Security Configuration**:
```
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .csrf().disable()
            .authorizeRequests()
            .anyRequest().authenticated()
            .and()
            .oauth2Login(); // Use OAuth2 login
    }
}
```

### 2. **Using HTTPS (SSL/TLS)**

-   **Description**: Protect communication by using HTTPS instead of HTTP. HTTPS encrypts the communication channel, protecting sensitive data like tokens, credentials, and payloads.
-   **Best for**: All production APIs, securing data in transit.

**Steps**:
1.  Generate an SSL certificate (self-signed for testing, CA-signed for production).
2.  Configure Spring Boot to use the certificate.

**Example (`application.yml`)**:
```
server:
  port: 8443
  ssl:
    enabled: true
    key-store: classpath:keystore.jks
    key-store-password: password
    key-alias: my-alias
```
### 3. **API Rate Limiting (Throttling)**

-   **Description**: Implement rate limiting to prevent abuse by limiting the number of requests per user or client IP. This can be done using filters or third-party libraries.
-   **Best for**: Protecting against DDoS attacks, API abuse, and controlling access rates.

**Example using a Filter**:
```
import org.springframework.stereotype.Component;
import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import java.io.IOException;
import java.util.concurrent.ConcurrentHashMap;
import java.util.concurrent.atomic.AtomicInteger;

@Component
public class RateLimitingFilter implements Filter {

    private final ConcurrentHashMap<String, AtomicInteger> requestCountsPerIpAddress = new ConcurrentHashMap<>();

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        String clientIp = request.getRemoteAddr();
        requestCountsPerIpAddress.putIfAbsent(clientIp, new AtomicInteger(0));
        int requestCount = requestCountsPerIpAddress.get(clientIp).incrementAndGet();

        if (requestCount > 100) { // Limit to 100 requests per window (e.g., per minute)
            throw new ServletException("Too many requests");
        }

        chain.doFilter(request, response);
    }

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {}

    @Override
    public void destroy() {}
}
```

### 4. **CSRF (Cross-Site Request Forgery) Protection**

-   **Description**: CSRF attacks trick users into performing actions they didn't intend. To protect against CSRF, you can use the built-in Spring Security CSRF protection.
-   **Best for**: Protecting state-changing operations (like POST, PUT, DELETE) in web applications.

**Enable CSRF Protection**:
```
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .csrf().csrfTokenRepository(CookieCsrfTokenRepository.withHttpOnlyFalse()) // Enable CSRF with Cookie-based token
            .and()
            .authorizeRequests()
            .anyRequest().authenticated();
    }
}
``` 

### 5. **CORS (Cross-Origin Resource Sharing) Configuration**

-   **Description**: If your API needs to be accessed from web applications hosted on different domains, you'll need to configure CORS to allow specific origins.
-   **Best for**: Controlling which domains can access your API.

**Configuration using `@CrossOrigin` annotation**:
```
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@CrossOrigin(origins = "https://trusted-domain.com")
public class MyController {

    @GetMapping("/data")
    public String getData() {
        return "Hello, World!";
    }
}
``` 

**Global CORS Configuration**:
```
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig {

    @Bean
    public WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurer() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/**")
                        .allowedOrigins("https://trusted-domain.com")
                        .allowedMethods("GET", "POST", "PUT", "DELETE")
                        .allowCredentials(true);
            }
        };
    }
}
``` 

### 6. **Input Validation**

-   **Description**: Validate incoming data to ensure it's safe and in the expected format. This helps protect against attacks like SQL injection and malicious payloads.
-   **Best for**: Ensuring data integrity and preventing injections.

**Example using `@Valid` annotation**:
```
import javax.validation.constraints.NotNull;
import javax.validation.constraints.Size;

public class UserRequest {
    @NotNull
    private String username;

    @Size(min = 8, message = "Password must be at least 8 characters")
    private String password;

    // Getters and Setters
}
```
**Controller**:
```import org.springframework.web.bind.annotation.*;
import javax.validation.Valid;

@RestController
public class UserController {

    @PostMapping("/user")
    public String createUser(@Valid @RequestBody UserRequest userRequest) {
        return "User created!";
    }
}
``` 

### 7. **API Key Authentication**

**API Key Authentication** is another way to protect your REST API in Spring Boot by issuing and validating unique keys for clients. Below are the details:

-   **Description**: API Key Authentication involves generating a unique key for each client, which they must include in the request headers to access your API. The server validates the API key before processing the request.
-   **Best for**: Simple scenarios, such as allowing access to trusted services, internal APIs, or providing different access levels for various clients.

**Example** using an API Key Validator Filter:

**Filter for API Key Validation**:

```
import org.springframework.stereotype.Component;
import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;
import java.io.IOException;

@Component
public class ApiKeyFilter implements Filter {

    private static final String API_KEY = "123456789"; // Example API key, ideally store securely

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        HttpServletRequest httpRequest = (HttpServletRequest) request;
        String apiKey = httpRequest.getHeader("X-API-KEY");

        if (API_KEY.equals(apiKey)) {
            chain.doFilter(request, response);
        } else {
            throw new ServletException("Invalid API Key");
        }
    }

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {}

    @Override
    public void destroy() {}
}
```

**Configuration**:
```import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class ApiKeyConfig implements WebMvcConfigurer {
    // Add any additional API key configurations if needed.
}
```

### 8. **Token-Based Authentication (Custom Tokens)**

-   **Description**: Instead of using standard JWT, you can create a custom token-based mechanism. This involves issuing a custom token to the client upon authentication and validating it for each request.
-   **Best for**: Cases where you need a custom token format or when JWT does not meet specific requirements.

### 9. **HMAC (Hash-Based Message Authentication Code)**

-   **Description**: Use HMAC for authentication by signing API requests with a secret key. The server validates the signature to ensure that the data hasn’t been tampered with and that the client has the correct secret key.
-   **Best for**: Ensuring data integrity and authenticity, particularly in systems where data verification is critical.

### 10. **mTLS (Mutual TLS)**

-   **Description**: Mutual TLS is a way to authenticate both the client and the server by exchanging certificates. The client and the server both have certificates, and they verify each other's certificates during the handshake.
-   **Best for**: High-security environments, enterprise-level APIs, scenarios requiring client verification.

**Steps**:

1.  Obtain certificates for both server and clients.
2.  Configure Spring Boot to require client certificates.

**Example (`application.yml`)**:
```
server:
  port: 8443
  ssl:
    key-store: classpath:server-keystore.jks
    key-store-password: password
    trust-store: classpath:client-truststore.jks
    trust-store-password: password
    client-auth: need` 
```
### 11. **Role-Based Access Control (RBAC)**

-   **Description**: Implement roles to manage access permissions at different levels. Use `@PreAuthorize`, `@Secured`, or `@RolesAllowed` annotations to control who can access specific endpoints.
-   **Best for**: APIs with multiple roles (e.g., Admin, User, Guest) where access needs to be restricted based on roles.

**Example**:
```import org.springframework.security.access.prepost.PreAuthorize;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class AdminController {

    @GetMapping("/admin")
    @PreAuthorize("hasRole('ADMIN')") // Only users with ADMIN role can access
    public String adminEndpoint() {
        return "Hello, Admin!";
    }
}
``` 

### 12. **IP Whitelisting/Blacklisting**

-   **Description**: Restrict access based on the client's IP address by maintaining a whitelist (allowed IPs) or blacklist (blocked IPs).
-   **Best for**: Internal APIs or scenarios where certain IPs are known to be trustworthy or malicious.

**Example**:
```import org.springframework.stereotype.Component;
import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;
import java.io.IOException;
import java.util.HashSet;
import java.util.Set;

@Component
public class IpWhitelistFilter implements Filter {

    private static final Set<String> WHITELIST = new HashSet<>();

    static {
        WHITELIST.add("192.168.1.100"); // Add trusted IP addresses
    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        HttpServletRequest httpRequest = (HttpServletRequest) request;
        String clientIp = httpRequest.getRemoteAddr();

        if (WHITELIST.contains(clientIp)) {
            chain.doFilter(request, response);
        } else {
            throw new ServletException("Forbidden: Unauthorized IP address");
        }
    }

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {}

    @Override
    public void destroy() {}
}
``` 

### 13. **Content Security Policies**

-   **Description**: Implement security headers such as `Content-Security-Policy`, `X-Frame-Options`, `X-Content-Type-Options`, etc., to enhance the security of your API and mitigate attacks like Cross-Site Scripting (XSS) and Clickjacking.
-   **Best for**: Enhancing API security at the HTTP header level.

### 14. **Database Encryption and Data Masking**
-   **Description**: Encrypt sensitive data stored in the database and mask or redact sensitive information in API responses. Ensure data encryption both in transit and at rest.
-   **Best for**: Securing sensitive information like PII (Personally Identifiable Information) and financial data.

### 15. **Security Audits and Logging**
-   **Description**: Implement comprehensive logging and auditing to track API usage, failed authentication attempts, and suspicious activity. This helps in identifying potential security breaches and debugging issues.
-   **Best for**: Monitoring security-related events, compliance, and regulatory requirements.

**Example** using Spring Boot’s built-in logging:

```import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.web.bind.annotation.*;

@RestController
public class AuditController {

    private static final Logger logger = LoggerFactory.getLogger(AuditController.class);

    @GetMapping("/secure-data")
    public String getSecureData() {
        logger.info("Access to secure data was made at: {}", System.currentTimeMillis());
        return "Secure Data";
    }
}
```

### 16. **API Gateway for Centralized Security**

-   **Description**: Use an API Gateway (e.g., **Spring Cloud Gateway**, **Zuul**, **Kong**, **NGINX**) as a central point to handle security features such as rate limiting, SSL termination, token validation, IP filtering, etc.
-   **Best for**: Managing security and routing for multiple microservices in a centralized manner.

Each method has its strengths, and in many cases, combining multiple techniques (e.g., HTTPS + JWT + OAuth2) is the best approach to ensure robust API security in Spring Boot. Choose the methods that best fit your requirements, context, and threat model.


![Screenshot 2024-11-17 at 5 26 56 PM](https://github.com/user-attachments/assets/e088ba91-e855-478b-9a3c-26788d0dca05)






# Types of Auto-Scaling in Kubernetes

Auto-scaling in Kubernetes involves adjusting the number of running pods based on the resource demands of your application. Kubernetes provides several ways to automatically scale your application to handle varying loads. Below, I'll explain the different types of auto-scaling in Kubernetes, how they work, and how to configure them:

1.  **Horizontal Pod Autoscaler (HPA)**:
    
    -   **What it Does**: Automatically adjusts the number of replicas (pods) of a deployment, replication controller, or replica set based on observed CPU/memory utilization or custom metrics.
    -   **When to Use**: For scaling the number of pods horizontally (increasing or decreasing the number of replicas based on demand).
2.  **Vertical Pod Autoscaler (VPA)**:
    
    -   **What it Does**: Automatically adjusts the CPU and memory requests/limits for containers in a pod to optimize resource usage.
    -   **When to Use**: When you want to adjust the resources allocated to pods instead of the number of pods.
3.  **Cluster Autoscaler**:
    
    -   **What it Does**: Automatically adjusts the number of nodes in a Kubernetes cluster. It adds nodes if there are pending pods that can't be scheduled due to resource constraints or removes nodes if they are underutilized.
    -   **When to Use**: For auto-scaling the infrastructure to support the load when new pods are scheduled or to scale down when demand decreases.

### **Horizontal Pod Autoscaler (HPA)**

#### **How HPA Works**

-   HPA monitors metrics like **CPU usage**, **memory usage**, or **custom metrics**.
-   It uses these metrics to determine when to scale up (add pods) or scale down (remove pods) based on a target threshold.
-   For example, if CPU usage goes above 70%, HPA might scale up the number of replicas, and if it goes below 30%, it might scale down.

#### **Configuring HPA**

1.  **Prerequisites**:
    
    -   **Metrics Server**: HPA requires a metrics server to collect resource metrics (CPU/Memory). Make sure you have the `metrics-server` running in your cluster:
        
        `kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml` 
        
2.  **Creating an HPA**:
    
    -   Use the following `kubectl` command to create an HPA based on CPU usage:
        
        `kubectl autoscale deployment <deployment-name> --cpu-percent=70 --min=2 --max=10` 
        
    -   Here:
        -   `<deployment-name>` is the name of the deployment you want to scale.
        -   `--cpu-percent=70` means HPA will trigger scaling if the CPU usage goes above 70%.
        -   `--min=2` is the minimum number of pods to maintain.
        -   `--max=10` is the maximum number of pods allowed.
3.  **YAML Example for HPA**:
    
    -   You can define HPA using a YAML configuration file:
        
        ```
        apiVersion: autoscaling/v2
        kind: HorizontalPodAutoscaler
        metadata:
          name: example-hpa
        spec:
          scaleTargetRef:
            apiVersion: apps/v1
            kind: Deployment
            name: my-deployment
          minReplicas: 2
          maxReplicas: 10
          metrics:
            - type: Resource
              resource:
                name: cpu
                target:
                  type: Utilization
                  averageUtilization: 70
                  ``` 
        
    -   To apply the HPA, save the file (e.g., `hpa.yaml`) and run:
        `kubectl apply -f hpa.yaml` 
        
4.  **Check the HPA Status**:
    
    -   Use the following command to monitor the HPA:
   
        `kubectl get hpa` 
        
#### **Triggering of HPA**

-   HPA is triggered based on the metrics collected by the metrics server. If the metrics show that the target utilization exceeds the defined threshold (like CPU > 70%), it will scale up.
-   HPA periodically checks the metrics and adjusts the number of replicas accordingly.

### **Vertical Pod Autoscaler (VPA)**

#### **How VPA Works**

-   VPA automatically adjusts the **CPU and memory requests/limits** for your containers to match the resource needs.
-   It can recommend changes, automatically apply them, or both.
-   Unlike HPA, VPA doesn't change the number of replicas; instead, it modifies the resources assigned to each pod.

#### **Configuring VPA**

1.  **Install VPA**:
    
    -   VPA is not a built-in feature, so you need to install it using:
        
        `kubectl apply -f https://github.com/kubernetes/autoscaler/releases/latest/download/vertical-pod-autoscaler.yaml` 
        
2.  **YAML Example for VPA**:
    
    -   Here’s an example YAML file for VPA:
        
        ```
        apiVersion: autoscaling.k8s.io/v1
        kind: VerticalPodAutoscaler
        metadata:
          name: example-vpa
        spec:
          targetRef:
            apiVersion: "apps/v1"
            kind: Deployment
            name: my-deployment
          updatePolicy:
            updateMode: "Auto"
				         ```  
        
    -   To apply the VPA, save the file (e.g., `vpa.yaml`) and run:
        
        `kubectl apply -f vpa.yaml` 
        
3.  **VPA Modes**:
    
    -   `Auto`: Automatically updates resource requests/limits.
    -   `Initial`: Sets the initial requests/limits for new pods.
    -   `Off`: VPA only gives recommendations and doesn’t change the resources automatically.

### **Cluster Autoscaler**

#### **How Cluster Autoscaler Works**

-   Cluster Autoscaler adjusts the number of nodes in the cluster based on pod requirements.
-   It works with cloud providers (like AWS, GCP, Azure) to add/remove nodes as needed.
-   If there are unschedulable pods due to insufficient resources, Cluster Autoscaler will increase the cluster size.
-   If there are underutilized nodes, it will remove them.

#### **Configuring Cluster Autoscaler**

-   **Install Cluster Autoscaler** with the appropriate configuration for your cloud provider.
-   **Example for AWS**:
    -   You need to specify node groups and configure IAM roles to allow scaling.
    -   Deploy using a configuration file or Helm chart.

#### **YAML Example for Cluster Autoscaler**

-   Example configuration for `cluster-autoscaler` in AWS EKS:
    
    ```apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: cluster-autoscaler
      namespace: kube-system
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: cluster-autoscaler
      template:
        metadata:
          labels:
            app: cluster-autoscaler
        spec:
          containers:
          - name: cluster-autoscaler
            image: k8s.gcr.io/autoscaling/cluster-autoscaler:v1.23.0
            command:
              - ./cluster-autoscaler
              - --v=4
              - --cloud-provider=aws
              - --nodes=1:10:<node-group-name>
              - --scale-down-unneeded-time=10m
              - --scale-down-utilization-threshold=0.5
            resources:
              limits:
                cpu: 100m
                memory: 300Mi
              requests:
                cpu: 100m
                memory: 300Mi
            env:
            - name: AWS_REGION
              value: <your-region>
    ```

### **Summary of Auto-Scaling in Kubernetes**

![Screenshot 2024-11-19 at 6 44 03 PM](https://github.com/user-attachments/assets/a559eef8-61d1-406a-85bd-4c9e0c9e34f0)

By using these auto-scaling mechanisms, Kubernetes can manage workloads efficiently, optimizing resource usage while ensuring application availability and performance.


# What is Podman?

**Podman** is a container management tool similar to Docker, which is used to manage and run OCI (Open Container Initiative) containers and container images. In the context of Kubernetes, Podman can be an alternative to Docker for building, running, and managing containers. Below is an overview of what Podman is, how it can be used in Kubernetes, and its key features:

-   **Podman** is a daemonless container engine, which means it doesn't require a background service (like Docker's `dockerd`) to run containers.
-   It can run containers and manage container images following the same OCI standards as Docker, making it compatible with Kubernetes.
-   It supports **rootless containers**, allowing non-privileged users to run containers without root access, enhancing security.
-   Podman has built-in support for creating and managing pods (collections of one or more containers sharing the same network namespace), which aligns with Kubernetes concepts.

### **How Podman is Used in Kubernetes**

While Kubernetes itself traditionally relies on a **container runtime** to run containers (e.g., Docker, containerd, CRI-O), Podman can be a part of a Kubernetes ecosystem in the following ways:

1.  **Building and Managing Images**:
    
    -   Podman can be used to build container images locally. These images can be pushed to a container registry (like Docker Hub, Quay, or a private registry) for use in Kubernetes clusters.
    -   Developers often use Podman instead of Docker to build and manage container images before deploying them to Kubernetes.
  
    `# Building an image using Podman
    podman build -t my-application:latest`
    
    `# Tagging the image for a registry
    podman tag my-application:latest myregistry.com/my-application:latest`
    
    `# Pushing the image to a container registry
    podman push myregistry.com/my-application:latest` 
    
2.  **Running Kubernetes Pods Locally with Podman**:
    
    -   Podman can simulate running Kubernetes pods locally using `podman play kube`. This allows developers to deploy and test Kubernetes YAML definitions (like Deployment, Pod, or Service) locally.
    -   This feature is useful for testing Kubernetes configurations without having a full Kubernetes cluster.
    
    `# Running a Kubernetes YAML file locally using Podman
    podman play kube my-kubernetes.yaml` 
    
3.  **Rootless Containers for Kubernetes**:
    
    -   Podman supports running containers without root privileges (`rootless mode`), which increases security by reducing the risks associated with privileged containers.
    -   In a Kubernetes environment, **CRI-O**, a lightweight container runtime, can be configured to use Podman for managing containers, leveraging Podman’s rootless features.
4.  **Pod Management**:
    
    -   Podman can create and manage "pods" locally, which are similar to Kubernetes pods (a group of containers sharing the same network namespace). This can be useful for local development or testing environments.
    
    `# Creating a Pod using Podman
    podman pod create --name my-pod`
    
    `# Running containers inside the Pod
    podman run -dt --pod my-pod my-container-image` 
    

### **Why Use Podman with Kubernetes?**

1.  **Daemonless Operation**:
    
    -   Unlike Docker, Podman does not rely on a background service (`dockerd`) to run. Each Podman command operates independently, making it potentially more lightweight and secure for certain environments.
2.  **Rootless Containers**:
    
    -   Podman supports running containers as a non-root user (`rootless mode`). This reduces security risks and makes it easier to run containers in environments with strict security requirements.
    -   Kubernetes is increasingly adopting rootless containers, making Podman a suitable option for environments where root access is restricted.
3.  **Kubernetes YAML Compatibility**:
    
    -   Podman can natively read Kubernetes YAML files (`podman play kube`) to create pods and containers locally, which is useful for developers testing Kubernetes workloads on their local machines without a full Kubernetes setup.
4.  **Compatibility with Docker CLI**:
    
    -   Podman provides a `docker`-compatible command line interface (CLI). Commands like `podman run`, `podman build`, and `podman push` are similar to Docker's equivalents.
    -   This makes transitioning from Docker to Podman relatively easy for teams already familiar with Docker commands.
5.  **OCI Compliance**:
    
    -   Podman is OCI-compliant, meaning it works with container images that adhere to the OCI specifications. This makes it compatible with other OCI-compliant tools like Kubernetes, CRI-O, and containerd.

### **Integration of Podman with Kubernetes**

Although Podman is not a direct runtime for Kubernetes, it integrates with the ecosystem in the following ways:

1.  **Podman and CRI-O**:
    
    -   Kubernetes uses the **Container Runtime Interface (CRI)** to communicate with container runtimes.
    -   **CRI-O** is a lightweight container runtime interface that supports Kubernetes and is fully compatible with OCI standards.
    -   Podman shares much of its codebase with CRI-O and is often used alongside CRI-O in environments where lightweight container management is preferred.
2.  **Podman in Development Workflows**:
    
    -   Developers can use Podman locally to build, test, and manage containers, then push images to a container registry. These images can be pulled and deployed in a Kubernetes cluster using standard Kubernetes configurations.
3.  **Podman Desktop**:
    
    -   Tools like **Podman Desktop** provide a user interface for managing containers, pods, and Kubernetes clusters, offering a local environment for developers working with Kubernetes resources.

### **Sample Workflow: Using Podman in a Kubernetes Environment**

1.  **Build the Container Image with Podman**:
    
    `# Build the image
    podman build -t my-app:latest`
    
   `# Tag and push to a container registry
    podman tag my-app:latest myregistry.com/my-app:latest
    podman push myregistry.com/my-app:latest` 
    
2.  **Deploy to Kubernetes Cluster**:
    
    -   Create a Kubernetes Deployment YAML file (`deployment.yaml`):
        
        ```
        apiVersion: apps/v1
        kind: Deployment
        metadata:
          name: my-app
        spec:
          replicas: 3
          selector:
            matchLabels:
              app: my-app
          template:
            metadata:
              labels:
                app: my-app
            spec:
              containers:
                - name: my-app
                  image: myregistry.com/my-app:latest
                  ports:
                    - containerPort: 8080
        ```
    -   Deploy the application using `kubectl`:
        
        `kubectl apply -f deployment.yaml` 
        
3.  **Test Kubernetes Configuration Locally with Podman**:
    
    `# Use the Kubernetes YAML to create resources locally in Podman
    podman play kube deployment.yaml` 
    

### **Comparison of Docker and Podman in Kubernetes Context**

![Screenshot 2024-11-19 at 6 53 11 PM](https://github.com/user-attachments/assets/db8a1ef1-66ea-4bea-93b5-2abacd420f9a)

### **Summary: When to Use Podman in Kubernetes**

-   **Local Development**: Use Podman for local development and testing of Kubernetes pods and configurations. It allows for container and pod management without needing a full Kubernetes cluster.
-   **Rootless Security**: Use Podman in environments where running as a non-root user is required for enhanced security.
-   **Docker Alternative**: Use Podman as a Docker replacement for managing containers with similar commands and functionality.
-   **Kubernetes Production Environments**: While Podman can be used in development, production Kubernetes clusters usually rely on dedicated container runtimes like **containerd** or **CRI-O** for managing containers.

Podman is a versatile tool for container management, particularly for local development, security-conscious environments, and OCI-compliant workflows that align with Kubernetes operations.



# No ways of hiding elements in Angular


### **Using `*ngIf` Directive**

The `*ngIf` directive is the most common way to conditionally hide or show elements in Angular. It removes the element from the DOM when the condition is `false`.

#### **Usage Example**:

```
<div *ngIf="isVisible">
  This content is visible when `isVisible` is true.
</div>
``` 

#### **Explanation**:

-   The element is added to the DOM only if the `isVisible` variable is `true`.
-   If `isVisible` is `false`, the element is completely removed from the DOM.

#### **When to Use**:

-   Use `*ngIf` when you want to conditionally add or remove an element from the DOM.

### 2. **Using `hidden` Attribute**

You can use the native `hidden` attribute to hide elements. Unlike `*ngIf`, the `hidden` attribute does not remove the element from the DOM; it simply hides it using CSS (`display: none`).

#### **Usage Example**:
```
<div [hidden]="!isVisible">
  This content is visible when `isVisible` is true.
</div>
``` 

#### **Explanation**:

-   The `hidden` attribute is bound to the `isVisible` variable. If `isVisible` is `false`, the element is hidden (but still exists in the DOM).

#### **When to Use**:

-   Use `[hidden]` when you want to hide elements but still keep them in the DOM for layout purposes or data binding.

### 3. **Using Angular `ngClass` Directive**

The `ngClass` directive allows you to conditionally apply or remove CSS classes. You can use this to hide elements by applying CSS styles.

#### **Usage Example**:

```
<div [ngClass]="{'hidden-class': !isVisible}">
  This content is visible when `isVisible` is true.
</div>
```

```
/* styles.css */
.hidden-class {
  display: none;
}``` 

#### **Explanation**:

-   In the example above, the `hidden-class` will be applied if `isVisible` is `false`, hiding the element.

#### **When to Use**:

-   Use `ngClass` when you want more control over how to hide the element using CSS classes.

### 4. **Using Angular `ngStyle` Directive**

The `ngStyle` directive allows you to conditionally set inline styles, including `display`, `visibility`, etc.

#### **Usage Example**:

```
<div [ngStyle]="{ 'display': isVisible ? 'block' : 'none' }">
  This content is visible when `isVisible` is true.
</div>
``` 

#### **Explanation**:

-   The `display` style is set to `none` when `isVisible` is `false`, hiding the element.

#### **When to Use**:

-   Use `ngStyle` for quick styling changes, especially when you need to hide elements based on multiple conditions.

### 5. **CSS-Based Hiding (Conditional Classes with Component Logic)**

You can hide elements using CSS classes directly within the component logic by applying specific classes conditionally.

#### **Usage Example**:

```
<div class="content" [class.hidden]="!isVisible">
  This content is visible when `isVisible` is true.
</div>
``` 

```
/* styles.css */
.hidden {
  display: none;
}
``` 

#### **Explanation**:

-   `[class.hidden]="!isVisible"` conditionally applies the `hidden` class, hiding the element.

#### **When to Use**:

-   Use conditional classes when you have multiple CSS classes that you want to apply conditionally.

### 6. **Using Structural Directive (`*ngIf` with Template Reference)**

Another advanced technique is to use `*ngIf` with a `<ng-template>` to control the visibility.

#### **Usage Example**:

```
<ng-container *ngIf="isVisible; else hiddenTemplate">
  <div>
    This content is visible when `isVisible` is true.
  </div>
</ng-container>

<ng-template #hiddenTemplate>
  <div>
    This content is visible when `isVisible` is false.
  </div>
</ng-template>
``` 

#### **Explanation**:

-   The `<ng-template>` allows you to have a fallback element/template when `*ngIf` is `false`.
-   It enables you to manage complex conditions and layouts.

#### **When to Use**:

-   Use `*ngIf` with `<ng-template>` when you need to manage complex conditional rendering with multiple templates.

### 7. **Using `visibility` CSS Property via `ngStyle` or `ngClass`**

Instead of removing or hiding elements with `display: none`, you can use the `visibility` property, which hides the element but still occupies space in the layout.

#### **Usage Example**:

```
<div [ngStyle]="{ 'visibility': isVisible ? 'visible' : 'hidden' }">
  This content is visible when `isVisible` is true.
</div>
``` 

#### **Explanation**:

-   `visibility: hidden` hides the element without removing it from the document layout.

#### **When to Use**:

-   Use `visibility` when you want to hide elements but still want them to occupy space in the DOM layout.

#### Summary Table of Methods

![Screenshot 2024-11-19 at 7 38 41 PM](https://github.com/user-attachments/assets/e41e5ddb-6d90-4927-998e-c02fb7f311e5)


# Ways of Angular Components Communication

### 1. **@Input() and @Output() Decorators**

These decorators are the primary way to communicate between a parent and child component. They allow data to be passed down from a parent to a child and for the child to emit events back up to the parent.

#### **Parent to Child Communication (`@Input()`)**

The `@Input()` decorator allows a parent component to pass data to its child component via property binding.

##### **Example**:
```
// Child Component (child.component.ts)
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `<h3>{{ childData }}</h3>`
})
export class ChildComponent {
  @Input() childData: string = ''; // Parent will pass data to this variable
}
``` 

`<!-- Parent Component Template (parent.component.html) -->
<app-child [childData]="'Hello from Parent'"></app-child>` 

#### **Child to Parent Communication (`@Output()`)**

The `@Output()` decorator, combined with `EventEmitter`, allows a child component to emit an event back to the parent.

##### **Example**:

```
// Child Component (child.component.ts)
import { Component, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `<button (click)="sendData()">Send Data to Parent</button>`
})
export class ChildComponent {
  @Output() dataEmitter = new EventEmitter<string>();

  sendData() {
    this.dataEmitter.emit('Data from Child');
  }
}
``` 

`<!-- Parent Component Template (parent.component.html) -->
<app-child (dataEmitter)="receiveData($event)"></app-child>` 

```
// Parent Component (parent.component.ts)
receiveData(data: string) {
  console.log(data); // Output: "Data from Child"
}
``` 

### 2. **Service for Communication (Shared Service)**

Services are a common way to enable communication between unrelated components (i.e., sibling components). By creating a shared service, you can share data or events across components.

#### **Example**:

```
// Shared Service (shared.service.ts)
import { Injectable } from '@angular/core';
import { BehaviorSubject } from 'rxjs';

@Injectable({ providedIn: 'root' })
export class SharedService {
  private messageSource = new BehaviorSubject<string>('default message');
  currentMessage = this.messageSource.asObservable();

  changeMessage(message: string) {
    this.messageSource.next(message);
  }
}
``` 

```
// Component A (component-a.component.ts)
import { Component } from '@angular/core';
import { SharedService } from './shared.service';

@Component({
  selector: 'app-component-a',
  template: `<button (click)="sendMessage()">Send Message</button>`
})
export class ComponentA {
  constructor(private sharedService: SharedService) {}

  sendMessage() {
    this.sharedService.changeMessage('Hello from Component A');
  }
}
``` 

```
// Component B (component-b.component.ts)
import { Component, OnInit } from '@angular/core';
import { SharedService } from './shared.service';

@Component({
  selector: 'app-component-b',
  template: `<h3>{{ message }}</h3>`
})
export class ComponentB implements OnInit {
  message: string = '';

  constructor(private sharedService: SharedService) {}

  ngOnInit() {
    this.sharedService.currentMessage.subscribe(msg => this.message = msg);
  }
}
``` 

#### **When to Use**:

-   When components are **not directly related** (like sibling components).
-   When you need to **share complex state** or **data across multiple components**.

### 3. **ViewChild and ContentChild**

`@ViewChild` and `@ContentChild` allow a parent component to directly access a child component’s properties and methods.

#### **@ViewChild**

Used to get a reference to a child component or DOM element within the parent’s template.

##### **Example**:

```
// Child Component (child.component.ts)
import { Component } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `<p>Child Component</p>`
})
export class ChildComponent {
  public childMethod() {
    console.log('Child method called!');
  }
}
``` 

```
// Parent Component (parent.component.ts)
import { Component, ViewChild, AfterViewInit } from '@angular/core';
import { ChildComponent } from './child.component';

@Component({
  selector: 'app-parent',
  template: `<app-child></app-child><button (click)="callChildMethod()">Call Child Method</button>`
})
export class ParentComponent implements AfterViewInit {
  @ViewChild(ChildComponent) childComponent!: ChildComponent;

  ngAfterViewInit() {
    // Access child component methods or properties
  }

  callChildMethod() {
    this.childComponent.childMethod(); // Calls the method in the child component
  }
}
``` 

#### **@ContentChild**

Used when you want to access a child component that is projected into a parent component using `<ng-content>`.

### 4. **Input Setter**

Instead of using `@Input()` directly, you can use a setter method to handle changes when the parent passes new data to the child.

#### **Example**:
```
// Child Component (child.component.ts)
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `<p>{{ data }}</p>`
})
export class ChildComponent {
  private _data: string = '';

  @Input()
  set data(value: string) {
    this._data = value.toUpperCase(); // Custom processing
  }

  get data(): string {
    return this._data;
  }
}
``` 

### 5. **EventEmitter (Sibling Components)**

Using a shared service with `EventEmitter` can allow sibling components to communicate through a common mediator (service).

### 6. **BehaviorSubject and Observables in Services**

A `BehaviorSubject` is an RxJS Subject that maintains a current value. It’s useful for sharing state across components.

#### **Example**:

`// Same as the Shared Service example above.` 

### 7. **Local Template Variables**

Local template variables provide a simple way to access child component instances directly in the parent’s template.

``` 
Parent Component Template (parent.component.html) 
<app-child #childComponent></app-child>
<button (click)="childComponent.childMethod()">Call Child Method</button>
``` 

### 8. **Output with Custom EventEmitter in Service**

You can also create a custom `EventEmitter` in a shared service to handle communication.

### 9. **State Management Libraries (NGRX, NGXS, Akita)**

If your application requires more complex state management, you can use state management libraries like **NGRX**, **NGXS**, or **Akita**.

-   **NGRX**: Inspired by Redux, NGRX manages the state using a global store.
-   **NGXS**: A state management pattern using observables.
-   **Akita**: A more flexible state management solution.

### **Summary Table of Communication Methods**

![Screenshot 2024-11-19 at 7 33 37 PM](https://github.com/user-attachments/assets/4b3f08f2-94de-4a01-a04a-40f511ecadfa)


# What Are Structural Directives?

Structural directives in Angular are directives that change the structure of the DOM by adding or removing elements. They usually use the `*` (asterisk) syntax to indicate that they modify the structure. Here's a detailed overview of structural directives available in Angular:

Structural directives are responsible for altering the layout by adding or removing DOM elements. They do not merely manipulate styles or attributes but change the structure of the document itself. Examples include `*ngIf`, `*ngFor`, and `*ngSwitch`.

### **Built-in Structural Directives**

1.  **`*ngIf` Directive**
    
    -   Used to conditionally include or exclude an element from the DOM.
    -   If the expression evaluates to `true`, the element is included; if `false`, it is removed.
    
    ```
    <div *ngIf="isLoggedIn">
      Welcome, user!
    </div>
    ```
    
    #### **With `else` Clause**:
    
    ```
    <div *ngIf="isLoggedIn; else loggedOut">
      Welcome, user!
    </div>
    
    <ng-template #loggedOut>
      <p>Please log in.</p>
    </ng-template>
    ``` 
    
    -   The `*ngIf` removes or adds the element to the DOM based on the `isLoggedIn` value.
    -   You can also use an `else` clause to handle the `false` case.
2.  **`*ngFor` Directive**
    
    -   Used to iterate over a list of items and render them.
    -   Creates a template for each item in a collection.
    ```
    <ul>
      <li *ngFor="let item of items">{{ item }}</li>
    </ul>
    ```
```
<ul>
  <li *ngFor="let item of items; let i = index">
        {{ i + 1 }}. {{ item }}
      </li>
    </ul>
```
  
    -  The `*ngFor` directive repeats the `<li>` element for each item in the `items` array.
    -  You can also access the index of the item using `let i = index.
 
 
```
    <ul>
      <li *ngFor="let item of items; trackBy: trackByFn">{{ item.name }}</li>
    </ul>
    
    trackByFn(index: number, item: any): number {
      return item.id; // unique id for each item
    }
    ``` 
    
    -   `trackBy` improves performance by identifying items uniquely, avoiding unnecessary re-renders.
    
    
3.  **`*ngSwitch` Directive**
    
    -   Used to conditionally display one of multiple elements based on a switch condition.
    -   It consists of `ngSwitch`, `*ngSwitchCase`, and `*ngSwitchDefault`.
    
    ```
    <div [ngSwitch]="userRole">
      <p *ngSwitchCase="'admin'">Admin Panel</p>
      <p *ngSwitchCase="'user'">User Dashboard</p>
      <p *ngSwitchCase="'guest'">Guest View</p>
      <p *ngSwitchDefault>Unknown Role</p>
    </div>
    ```
    
    #### **Explanation**:
    
    -   The `[ngSwitch]` directive binds to a value (`userRole`), and `*ngSwitchCase` handles different cases.
    -   The `*ngSwitchDefault` provides a fallback if no case matches.

### **Custom Structural Directives**

You can also create your own structural directives to manipulate the DOM based on your application's needs.

#### **Example** - Creating a Custom Structural Directive (`*appUnless`)

The following example creates a custom structural directive similar to `*ngIf`, but it shows an element if a condition is `false`.

##### **Step 1: Create the Directive**

```
// unless.directive.ts
import { Directive, Input, TemplateRef, ViewContainerRef } from '@angular/core';

@Directive({
  selector: '[appUnless]'
})
export class UnlessDirective {
  private hasView = false;

  constructor(private templateRef: TemplateRef<any>, private viewContainer: ViewContainerRef) {}

  @Input() set appUnless(condition: boolean) {
    if (!condition && !this.hasView) {
      this.viewContainer.createEmbeddedView(this.templateRef);
      this.hasView = true;
    } else if (condition && this.hasView) {
      this.viewContainer.clear();
      this.hasView = false;
    }
  }
}
``` 

##### **Step 2: Use the Custom Directive**
```
<div *appUnless="isLoggedIn">
  You are not logged in.
</div>
``` 

#### **Explanation**:

-   `TemplateRef` represents the template to render.
-   `ViewContainerRef` is the container where the template is added or removed.
-   `appUnless` is the custom directive that acts opposite to `*ngIf`.

### **Key Differences Between Structural Directives and Attribute Directives**

![Screenshot 2024-11-19 at 7 20 02 PM](https://github.com/user-attachments/assets/2bd17eca-366d-4fd0-922c-842d2f97798d)

### **Best Practices for Using Structural Directives**

1.  **Use `*ngIf` over `[hidden]`** when you need to remove an element from the DOM completely to optimize performance.
2.  **Use `trackBy` with `*ngFor`** when dealing with large lists to improve rendering performance.
3.  **Avoid nesting multiple `*ngIf` and `*ngFor`** within the same element. Consider using `<ng-container>` for complex conditions.
4.  **Use `*ngSwitch` for multiple conditions** to make the template more readable than using multiple `*ngIf`.
5.  **Use `ng-template` for reusable templates** and complex condition handling.

### **Advanced Structural Directive Techniques**

#### **Using `<ng-template>`**

`<ng-template>` is used as a placeholder for structural directives. It does not render any content on its own but can be used to define templates.

##### **Example**:

```<ng-template #noData>
  <p>No data available.</p>
</ng-template>

<div *ngIf="data.length > 0; else noData">
  <ul>
    <li *ngFor="let item of data">{{ item }}</li>
  </ul>
</div>
```

-   `<ng-template>` acts as a container for a template that will be rendered conditionally.
-   The `#noData` template is used when there is no data available.

### **Summary of Common Structural Directives**

![Screenshot 2024-11-19 at 7 15 40 PM](https://github.com/user-attachments/assets/2c1b70d2-210b-49c4-9b05-f35f1edf6eef)




# Behavioral Directives

Behavioral directives in Angular, also known as **attribute directives**, are directives that are used to change the behavior or appearance of an element, component, or another directive. Unlike **structural directives** that manipulate the DOM structure (adding or removing elements), **behavioral directives** are responsible for manipulating the properties, styles, classes, or other attributes of elements without altering the DOM structure.

### **Key Concepts of Behavioral Directives**

-   Behavioral directives are primarily used to **change the appearance** or **behavior** of elements.
-   They are applied like **attributes** to elements in the template, hence the name **attribute directives**.
-   Examples include modifying styles, adding/removing CSS classes, managing event listeners, or any other visual effect that does not require a change in the DOM structure.

### **Built-in Behavioral Directives in Angular**

1.  **`ngClass`**
    -   Dynamically adds or removes CSS classes on an HTML element based on conditions.
    -   It accepts a string, an object, or an array of class names.
    
    ```
    <div [ngClass]="{'active': isActive, 'disabled': !isActive}">
      Conditional Classes Example
    </div>
    ```
    
    #### **Explanation**:
    
    -   In the above example, the `active` class is applied if `isActive` is `true`, and the `disabled` class is applied if `isActive` is `false`.
    ```
    <div [ngClass]="['class1', 'class2', isActive ? 'active' : 'inactive']">
      Using an Array with ngClass
    </div>
    ``` 
    
2.  **`ngStyle`**
    
    -   Dynamically sets inline styles for an HTML element based on conditions.
    -   It accepts an object where the key is the style property, and the value is the style value.
    
    ```
    <div [ngStyle]="{'color': textColor, 'font-size': fontSize}">
      Dynamic Styles Example
    </div>
    ``` 
    -   The color and font-size are set dynamically based on the `textColor` and `fontSize` variables.
    
3.  **`ngModel`**
    
    -   Two-way data binding directive used for binding form inputs.
    -   It synchronizes the value in the component and the view, making it ideal for forms.
    
    ```
    <input [(ngModel)]="userInput" placeholder="Type something">
    <p>{{ userInput }}</p>
    ``` 
    
    -   The `[(ngModel)]` binds the input field to the `userInput` variable, allowing changes in the input to reflect immediately in the view.

### **Creating a Custom Behavioral Directive**

Behavioral directives are not limited to built-in ones; you can also create your own custom attribute directives to encapsulate reusable behavior.

#### **Example - Creating a Custom Directive (`appHighlight`)**

This example creates a custom directive to change the background color of an element when the mouse hovers over it.

##### **Step 1: Create the Directive**

```
// highlight.directive.ts
import { Directive, ElementRef, HostListener, Input } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  @Input('appHighlight') highlightColor: string = 'yellow'; // Default color

  constructor(private el: ElementRef) {}

  @HostListener('mouseenter') onMouseEnter() {
    this.highlight(this.highlightColor);
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.highlight('');
  }

  private highlight(color: string) {
    this.el.nativeElement.style.backgroundColor = color;
  }
}
``` 

##### **Step 2: Use the Custom Directive**

```
<p appHighlight="lightblue">Hover over this text to highlight it in light blue.</p>
<p appHighlight>Hover over this text to highlight it with the default color.</p>
``` 

#### **Explanation**:

-   The `appHighlight` directive changes the background color of the element when the mouse enters or leaves.
-   `ElementRef` is used to access the DOM element and manipulate its styles.
-   `@HostListener` is used to listen to DOM events like `mouseenter` and `mouseleave`.

### **Common Angular Built-in Behavioral Directives**

![Screenshot 2024-11-19 at 7 08 11 PM](https://github.com/user-attachments/assets/90974636-2dfa-4e94-ba07-373906fba830)

### **HostBinding and HostListener for Custom Directives**

In custom behavioral directives, you often use `@HostBinding` and `@HostListener` to manipulate the behavior of the host element.

#### **`@HostBinding` Example**

`@HostBinding` is used to bind a property of the host element directly.

```
@Directive({
  selector: '[appBorderHighlight]'
})
export class BorderHighlightDirective {
  @HostBinding('style.border') border: string = '';

  @HostListener('mouseenter') onMouseEnter() {
    this.border = '2px solid green';
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.border = '';
  }
}
``` 

#### **Explanation**:

-   `@HostBinding('style.border')` binds the `style.border` property of the host element.
-   The border changes when the user hovers over the element.

### **Additional Concepts Related to Behavioral Directives**

1.  **Directive Life Cycle Hooks**
    
    -   Like components, directives also have lifecycle hooks like `ngOnInit`, `ngOnChanges`, `ngAfterViewInit`, etc., to control initialization and change detection.
    
    ```
    import { Directive, OnInit } from '@angular/core';
    
    @Directive({
      selector: '[appSample]'
    })
    export class SampleDirective implements OnInit {
      ngOnInit() {
        console.log('Directive Initialized');
      }
    }
    ``` 
    
2.  **Using Dependency Injection in Directives**
    
    -   You can inject services into a directive to access data or perform additional tasks.
    
    ```
    @Directive({
      selector: '[appTooltip]'
    })
    export class TooltipDirective {
      constructor(private tooltipService: TooltipService) {}
    
      @HostListener('mouseenter') showTooltip() {
        this.tooltipService.show();
      }
    
      @HostListener('mouseleave') hideTooltip() {
        this.tooltipService.hide();
      }
    }
    ```
3.  **Passing Data to a Directive via Input**
    
    -   Just like components, you can pass data to directives using `@Input`.
        
    ```
    @Directive({
      selector: '[appChangeColor]'
    })
    export class ChangeColorDirective {
      @Input('appChangeColor') color: string = 'blue';
    
      constructor(private el: ElementRef) {
        this.el.nativeElement.style.color = this.color;
      }
    }
    ```
   
### **Best Practices for Using Behavioral Directives**

1.  **Keep Directives Focused**: A directive should do one thing well. Avoid making directives that do multiple tasks.
2.  **Prefer Built-in Directives**: Whenever possible, use built-in directives like `ngClass` and `ngStyle` for common use cases.
3.  **Avoid DOM Manipulation**: Avoid manipulating the DOM directly. Use Angular's APIs like `Renderer2` to ensure cross-browser compatibility.
4.  **Use `@HostListener` for Event Binding**: Instead of directly attaching event listeners to the DOM, use `@HostListener` to make it declarative.
5.  **Keep Logic Simple**: Try to keep the logic within directives simple. Complex logic should be handled in services or components.

### Summary of Behavioral Directives in Angular
![Screenshot 2024-11-19 at 7 00 23 PM](https://github.com/user-attachments/assets/d786d1fd-530d-4f6e-b98c-a6cc67eb5419)
