
# Spring Boot Features

	•	Dependency resolution
	•	Auto-configuration
	•	Embedded Tomcat
	•	Health metrics using Actuator

⸻

# Spring Boot Starters

Definition:

By adding just one starter, you get all the required modules with compatible versions, reducing manual configuration and dependency management.

### Example: spring-boot-starter-data-mongodb

Included dependencies:

	•	spring-data-mongodb
	•	MongoTemplate
	•	Mapping framework (@Document, @Id)
	•	mongodb-driver-core
	•	Connection to MongoDB server
	•	BSON encoding/decoding

⸻

# Actuator & Endpoints
```
Endpoint	      What It Shows
/health	        App + dependency health (up/down)
/info	          App metadata (version, name, build info)
/metrics	      Performance metrics (JVM, CPU, HTTP stats)
/mappings	      All URL → controller mappings
```

⸻

# Application Context & BeanFactory

## 1. BeanFactory (Core Container)

What it is: The basic Spring container that manages beans.

Responsibilities:

	•	Creates beans
	•	Provides DI (Dependency Injection)
	•	Manages bean lifecycle

Loading Type: Lazy (beans created only when first requested)

When used:

	•	Very light-weight applications
	•	Memory-constrained situations
	•	Rarely used directly in modern Spring apps

⸻

## 2. ApplicationContext (Advanced Container)

What it is: Superset of BeanFactory, includes enterprise features.

Responsibilities:

	•	Bean creation & DI
	•	Event handling
	•	AOP integration
	•	Internationalization
	•	Environment & profiles
	•	Resource loading
	•	Application-level events

Loading Type: Eager (beans created at startup, except lazy beans)

Usage: Almost all modern Spring/Spring Boot applications

BeanFactory lazily creates and manages beans. ApplicationContext eagerly loads beans and adds features like events, AOP, profiles, resource loading, and i18n. Spring Boot always uses ApplicationContext.

⸻

# Dependency Injection (DI)

### Definition:

Spring creates the required objects (dependencies) and injects them into your class instead of your class creating them.

Benefits:

	•	Reduces boilerplate
	•	Reduces coupling
	•	Avoids hardcoding
	•	Simplifies testing

Example: If OrderService needs PaymentService, Spring injects it automatically.

⸻

Types of DI

## 1. Constructor Injection (Recommended)

How it works: Dependencies are provided through the constructor.
```java
@Service
public class OrderService {
    private final PaymentService paymentService;

    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

Advantages:

	•	Recommended by Spring
	•	Class is immutable
	•	Dependency is mandatory
	•	Easy for unit testing
	•	Works with Lombok @RequiredArgsConstructor
	•	Avoids NullPointerException

When to use: Must-have dependencies, large apps, Spring Boot projects

⸻

## m2. Setter Injection

How it works: Dependencies are set via setter methods.
```java
@Service
public class OrderService {
    private PaymentService paymentService;

    @Autowired
    public void setPaymentService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

Advantages:

	•	Optional dependencies
	•	Can change at runtime

Disadvantages:

	•	Object can exist without dependency
	•	Risk of inconsistent state

When to use: Optional dependencies or runtime-configurable properties

⸻

## 3. Field Injection (Not Recommended)

How it works: Dependencies are injected directly into fields.
```java
@Service
public class OrderService {
    @Autowired
    private PaymentService paymentService;
}
```
Advantages: Short and quick

Disadvantages:

	•	Hard to test (cannot mock easily)
	•	Breaks immutability
	•	Hidden dependencies
	•	Poor maintainability

When to use: Temporary/demo code only

⸻

# CommandLineRunner vs ApplicationRunner

Purpose: Run code after Spring Boot startup, useful for initializing data or startup logic.

⸻

## 1. CommandLineRunner
   
	•	Provides raw command-line arguments (String[] args)
	•	Method: void run(String... args)

Example:
```java
@Component
public class MyCommandLineRunner implements CommandLineRunner {
    @Override
    public void run(String... args) {
        System.out.println("CommandLineRunner executed!");
        System.out.println(Arrays.toString(args));
    }
}
```
Output:
```
CommandLineRunner executed!
["--port=8080", "--env=dev"]
```

⸻

## 2. ApplicationRunner

	•	Provides parsed arguments via ApplicationArguments
	•	Method: void run(ApplicationArguments args)

Example:
```java
@Component
public class MyAppRunner implements ApplicationRunner {
    @Override
    public void run(ApplicationArguments args) {
        System.out.println("ApplicationRunner executed!");
        System.out.println("Option names: " + args.getOptionNames());
    }
}
```
Output:
```
Option names: [port, env]
```
Difference:

	•	CommandLineRunner → raw strings, manual parsing
	•	ApplicationRunner → parsed, easier to use

⸻

# Spring Boot Profiles

Definition:

Profiles allow environment-specific configurations (dev, test, prod) so the same application can run differently in each environment.

Property Files:

application-dev.properties
application-test.properties
application-prod.properties

Example:

application-dev.properties
```
server.port=8081
spring.datasource.url=jdbc:mysql://localhost:3306/dev_db
```
application-prod.properties
```
server.port=8080
spring.datasource.url=jdbc:mysql://prod-db-server:3306/prod_db
```

⸻

Activate Profile:

	•	application.properties: spring.profiles.active=dev
	•	Command-line: java -jar myapp.jar --spring.profiles.active=prod
	•	Environment variable: SPRING_PROFILES_ACTIVE=prod

⸻

@Profile Annotation on Beans
```java
@Service
@Profile("dev")
public class DevEmailService implements EmailService { ... }

@Service
@Profile("prod")
public class ProdEmailService implements EmailService { ... }
```
	•	Only beans of active profile are created
	•	Supports multiple active profiles: spring.profiles.active=dev,featureX

Profiles isolate environment-specific configurations. Beans with @Profile are loaded only if the profile is active.

⸻


Spring Boot Profiles allow easy switching between dev, test, and prod environments without changing code.

⸻

# 🌱 What is a Bean?

A Bean in Spring is:

An object that is instantiated, assembled, and managed by the Spring IoC container.

	•	Typically, beans are your service classes, repositories, controllers, etc.
	•	Spring manages their lifecycle, dependencies, and configuration.
	•	Created using:
	•	XML (<bean> tag)
	•	Annotations (@Component, @Service, @Repository, @Controller)

Example:
```java
@Service
public class UserService {
    public void createUser() { ... }
}
```
Here, UserService is a Spring Bean.

⸻

⚡ Bean Life Cycle

Spring manages beans in 6 main steps:

	1.	Instantiation: Spring creates the bean instance.
	
	2.	Populate Properties: Spring injects dependencies (DI).
	
	3.	BeanNameAware / BeanFactoryAware / ApplicationContextAware:
	
		Spring passes contextual information to the bean (optional).
		
	4.	Pre-initialization (BeanPostProcessor – before init):
	
		Spring allows custom modification of bean before initialization.
	
	5.	Initialization (@PostConstruct / afterPropertiesSet / init-method):
		
		Bean is fully initialized and ready to use.
	
	6.	Post-initialization (BeanPostProcessor – after init):

		Spring can modify the fully initialized bean.
	
	7.	Destruction (@PreDestroy / destroy-method):
		
		Called when the container shuts down, e.g., for cleanup.

Diagram (simplified):
```
Instantiate → Populate Properties → Pre-initialize → Initialize → Post-initialize → Ready → Destroy
```
Example with Annotations:
```java
@Component
public class UserService {

    @PostConstruct
    public void init() {
        System.out.println("Bean is initialized");
    }

    @PreDestroy
    public void cleanup() {
        System.out.println("Bean is destroyed");
    }
}
```

⸻

🌐 Bean Scopes

Bean Scope defines how many instances of a bean Spring will create and when.
```
Scope			Description								Default?
Singleton		Single instance per Spring container	✅ Yes
Prototype		New instance every time requested		❌
Request			One instance per HTTP request			❌
Session			One instance per HTTP session			❌
Application		One instance per ServletContext			❌
Websocket		One instance per WebSocket				❌
```
Example: Singleton vs Prototype
```java
@Component
@Scope("prototype")
public class MyBean { ... }
```
	•	Singleton → Spring creates 1 bean and shares it.
	•	Prototype → Spring creates a new bean every time getBean() is called.

⸻

🎯 Notes

	•	Bean: Managed object in Spring IoC container
	•	Life Cycle: Instantiation → Dependency Injection → Initialization → Ready → Destruction
	•	Scopes: Control how many instances are created and their lifetime (singleton is default)

⸻
