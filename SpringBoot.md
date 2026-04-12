**What is Spring Boot?**
Spring Boot is an extension of the Spring Framework that makes it easy to build production-ready applications quickly with minimal configuration.

**Important points**

* Auto-configuration (based on classpath)
* Embedded server (Tomcat/Jetty)
* Starter dependencies (no version conflicts)
* Production-ready features (metrics, health)
* Minimal setup (no XML)

**Example**

```java
@SpringBootApplication
public class App {
    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }
}
```

---

**Spring Boot Starters**

**What it is**
Predefined dependency sets in Spring Boot that include everything needed for a feature.

**Important points**

* No need to manage multiple dependencies
* Ensures compatible versions
* Naming: `spring-boot-starter-*`

**Example**

```xml id="9jf3kw">
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>
//Includes Spring MVC + Jackson + Tomcat

<dependency>
  <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
//Health checks + Metrics endpoints

<dependency>
  <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>
//Spring Data MongoDB -> java objects to mongo documents, provides few built in queries
//MongoDB Java Driver -> connection

<dependency>
  <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
//Hibernate Validator + Bean Validation (JSR-380)


<dependency>
  <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
//Database + ORM

<dependency>
  <artifactId>spring-boot-starter-test</artifactId>
</dependency>
//JUnit + Mockito + Spring Test

<dependency>
  <artifactId>spring-boot-starter-security</artifactId>
</dependency>
//Authentication & authorization
```
---

**Actuator & Endpoints**

**What it is**
Module in Spring Boot to monitor and manage application.

**Important points**

* Provides health, metrics, info
* Exposes endpoints over HTTP/JMX
* Useful in production monitoring

**Example endpoints**

* `/actuator/health` → app status ("UP" or "DOWN" status)
* `/actuator/metrics` → performance data (such as CPU, memory, or HTTP requests)
* `/actuator/info` → (application name, version, and description)
---

**Spring IoC Container**

**What it is**
Core part of Spring Framework that manages objects (beans).

**Important points**

* IoC = control given to container (not developer)
* Creates, manages, injects dependencies
* Two types: BeanFactory, ApplicationContext

**Example**

```java id="qk3p9a">
@Component
class A {}

@Component
class B {
    @Autowired
    A a;
}
```

→ Container creates A and injects into B automatically

**Dependency Injection (DI)**

**What it is**
A concept in Spring Framework where dependencies are provided by the container instead of creating them using `new`.

**Important points**

* Reduces tight coupling
* Managed by IoC container
* Improves testability and flexibility
* Main types: Constructor, Setter, Field

---

**1. Constructor Injection**

**What it is**
Dependency is passed through constructor.

**Important points**

* Recommended way
* Ensures required dependency is always present
* Good for immutability

**Example**

```java id="c1">
@Component
class Engine {}

@Component
class Car {
    private final Engine engine;

    @Autowired
    public Car(Engine engine) {
        this.engine = engine;
    }
}
```

---

**2. Setter Injection**

**What it is**
Dependency is set using setter method.

**Important points**

* Used for optional dependencies
* Can change dependency later

**Example**

```java id="c2">
@Component
class Car {
    private Engine engine;

    @Autowired
    public void setEngine(Engine engine) {
        this.engine = engine;
    }
}
```

---

**3. Field Injection**

**What it is**
Dependency is injected directly into field.

**Important points**

* Less boilerplate
* Not recommended (hard to test, no immutability)

**Example**

```java id="c3">
@Component
class Car {
    @Autowired
    private Engine engine;
}
```

**Bean**

**What it is**
An object managed by the Spring Framework IoC container.

**Important points**

* Created, configured, and managed by Spring
* Defined using `@Component`, `@Bean`, etc.
* Used via Dependency Injection

**Example**

```java id="b1">
@Component
class MyService {}
```

---

**Bean Lifecycle**

**What it is**
Steps followed by Spring to create and destroy a bean.

**Important points**

* Instantiate → create object
* Dependency Injection → inject dependencies
* Initialize → `@PostConstruct`
* Ready to use
* Destroy → `@PreDestroy`

**Example**

```java id="b2">
@Component
class MyBean {

    @PostConstruct
    public void init() {
        System.out.println("Initialized");
    }

    @PreDestroy
    public void destroy() {
        System.out.println("Destroyed");
    }
}
```

---

**Bean Scopes**

**What it is**
Defines how many instances of a bean are created.

**Important points**

* Singleton (default) → one instance per container
* Prototype → new instance every time
* Request → one per HTTP request
* Session → one per user session

**Example**

```java id="b3">
@Component
@Scope("prototype")
class MyBean {}
```

**How a Spring Boot application starts**

**What it is**
Startup flow from `main()` method to a fully running application.

**Important points**

* Entry point → `SpringApplication.run()`
* Creates IoC container (ApplicationContext)
* Performs auto-configuration
* Scans and creates beans
* Starts embedded server
* Application becomes ready

**Flow (step-by-step)**

1. **Main method runs**

```java
@SpringBootApplication
public class App {
    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }
}
```

2. **SpringApplication starts**

* Bootstraps the app
* Detects configuration (`@SpringBootApplication`)

3. **Creates ApplicationContext**

* IoC container initialized

4. **Component Scan**

* Finds `@Component`, `@Service`, `@Repository`, etc.

5. **Auto-Configuration**

* Configures beans based on dependencies (e.g., web → Tomcat)

6. **Bean Creation & DI**

* Beans created and dependencies injected

7. **Embedded Server Starts**

* Tomcat/Jetty starts (for web apps)

8. **Application Ready**

* Runs `CommandLineRunner` / `ApplicationRunner` if present

**Example**

```java
@Component
class MyRunner implements CommandLineRunner {
    public void run(String... args) {
        System.out.println("App started");
    }
}
```

**@SpringBootApplication**

**What it is**
A main annotation in Spring Boot that combines multiple configurations to bootstrap the application.

**Important points**

* It is a combination of 3 annotations:

  * `@Configuration` → marks class as configuration (bean definitions)
  * `@EnableAutoConfiguration` → enables auto setup based on dependencies
  * `@ComponentScan` → scans for components in package

* Placed on main class

* Triggers full Spring Boot startup

**Example**

```java id="sb1">
@SpringBootApplication
public class App {
    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }
}
```

→ This single annotation replaces manual configuration, component scanning, and auto-configuration setup

**Auto-Configuration in Spring Boot**

**What it is**
Feature that automatically configures beans based on dependencies, classpath, and properties—so you don’t write manual config.

**Important points**

* Triggered by `@EnableAutoConfiguration` (inside `@SpringBootApplication`)
* Uses **classpath detection** → checks which libraries are present
* Uses **conditional annotations** → loads config only if conditions match
* Reads configs from `META-INF/spring.factories` (or newer `AutoConfiguration.imports`)
* Can be customized via `application.properties`

**How it works (flow)**

1. Spring Boot starts
2. Checks dependencies (e.g., web, JPA, security)
3. Loads matching auto-config classes
4. Applies conditions like:

   * `@ConditionalOnClass` → class exists
   * `@ConditionalOnMissingBean` → bean not already defined
5. Creates and registers beans

**Example**

* Add dependency:

```xml id="ac1">
spring-boot-starter-web
```

* What happens automatically:

  * DispatcherServlet configured
  * Embedded Tomcat started
  * JSON converter (Jackson) configured

👉 You didn’t write any config, but everything works

**Override example**

```java id="ac2">
@Bean
public ObjectMapper customMapper() {
    return new ObjectMapper();
}
```

→ Spring Boot backs off because `@ConditionalOnMissingBean` fails

**@Primary vs @Qualifier**

**What it is**
Used to resolve multiple bean conflicts in Spring Framework.

**Important points**

* `@Primary` → default bean when multiple exist
* `@Qualifier` → specify exact bean

**Example**

```java
@Component
@Primary
class A implements Service {}

@Component
class B implements Service {}

@Autowired
Service s; // A injected

@Autowired
@Qualifier("b")
Service s2; // B injected
```

---

**@SpringBootApplication ( @Configuration, @EnableAutoConfiguration, @ComponentScan )**

**What it is**
Main annotation of Spring Boot combining 3.

**Important points**

* `@Configuration` → define beans
* `@EnableAutoConfiguration` → auto setup
* `@ComponentScan` → scan components

**Example**

```java
@SpringBootApplication
class App {}
```

---

**@Configuration, @Component, @Service, @Repository**

**What it is**
Annotations to define beans in Spring Framework.

**Important points**

* `@Configuration` → config class with `@Bean` methods
* `@Component` → generic bean
* `@Service` → business logic (semantic)
* `@Repository` → DAO + exception translation

**Example**

```java
@Configuration
class AppConfig {
    @Bean
    MyBean bean() { return new MyBean(); }
}
```

---

**@Controller vs @RestController**

**What it is**
Used in web layer.

**Important points**

* `@Controller` → returns view (HTML)
* `@RestController` → returns JSON (API)
* `@RestController = @Controller + @ResponseBody`

**Example**

```java
@RestController
class A {
    @GetMapping("/hello")
    String hi() { return "Hello"; }
}
```

---

**@RequestMapping, @GetMapping, @PostMapping, @PutMapping, @PatchMapping, @DeleteMapping**

**What it is**
Map HTTP requests to methods.

**Important points**

* `@RequestMapping` → generic
* Others → specific HTTP methods

**Example**

```java
@GetMapping("/users")
public List<User> getUsers() {}
```

---

**@RequestParam vs @PathVariable vs @RequestBody**

**What it is**
Ways to get data from request.

**Important points**

* `@RequestParam` → query params (`?id=1`)
* `@PathVariable` → URL path (`/users/1`)
* `@RequestBody` → request JSON

**Example**

```java
@GetMapping("/users/{id}")
public User get(@PathVariable int id) {}
```

---

**@Entity, @Table, @Data**

**What it is**
Used in persistence.

**Important points**

* `@Entity` → marks JPA entity
* `@Table` → map to DB table
* `@Data` (Lombok) → getters/setters, etc.

**Example**

```java
@Entity
@Table(name="users")
@Data
class User {
    int id;
    String name;
}
```

---

**@Autowired vs @Inject**

**What it is**
Used for dependency injection.

**Important points**

* `@Autowired` → Spring-specific
* `@Inject` → Java standard (JSR-330)
* Both work same in Spring

**Example**

```java
@Autowired
Service s;
```

---

**@Target & @Retention**

**What it is**
Meta-annotations to define custom annotations.

**Important points**

* `@Target` → where annotation can be used (class, method)
* `@Retention` → lifecycle (SOURCE, CLASS, RUNTIME)

**Example**

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@interface MyAnno {}
```

---

**@Transactional**

**What it is**
Manages database transactions in Spring Framework.

**Important points**

* Starts transaction before method
* Commits if success
* Rolls back on runtime exception
* Works via AOP (proxy)

**Example**

```java
@Transactional
public void transfer() {
    debit();
    credit();
}
```

→ If any step fails, everything rolls back

