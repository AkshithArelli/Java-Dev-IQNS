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

**spring-boot-starter-parent**
A parent POM provided by Spring Boot that manages dependency versions and build configuration for your project.

**Example**

```xml id="9jf3kw">
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.x.x</version>
</parent>
//After this, you can add dependencies like:

<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>
//(no version needed)
//Includes Spring MVC + Jackson (java obj to json & json -> java obj(pojo)) + Tomcat

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

---

**Dependency Injection (DI)**

**What it is**
A concept in Spring Framework where dependencies are provided by the container instead of creating them using `new`.

**Important points**

* Reduces tight coupling
* Managed by IoC container
* Improves testability and flexibility
* Main types: Constructor, Setter, Field


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

---

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

---

**@SpringBootApplication**

**What it is**
Main annotation in Spring Boot that bootstraps the application by combining 3 core configurations.

**Important points**

* Used on main class
* Triggers auto-configuration + component scanning + config setup
* Equivalent to 3 annotations together

**Example**

```java
@SpringBootApplication
class App {
    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }
}
```

**1. @Configuration**

**What it is**
Marks class as a configuration class (like XML config replacement).

**Important points**

* Used to define beans using `@Bean`
* Managed by Spring container

**Example**

```java
@Configuration
class AppConfig {
    @Bean
    MyService myService() {
        return new MyService();
    }
}
```

**2. @EnableAutoConfiguration**

**What it is**
Enables automatic configuration based on dependencies.

**Important points**

* Checks classpath (what libraries are present)
* Uses conditions (`@ConditionalOnClass`, etc.)
* Automatically creates required beans

**Example**

* Add `spring-boot-starter-web`
  → Spring auto-configures:

  * DispatcherServlet
  * Tomcat
  * JSON converter

**3. @ComponentScan**

**What it is**
Scans packages to find Spring components.

**Important points**

* Looks for `@Component`, `@Service`, `@Repository`, `@Controller`
* Default: scans current package + sub-packages

**Example**

```java
@Component
class MyService {}
```

→ Automatically detected and registered as bean


---

**Spring Boot Application Lifecycle**

**What it is**
The sequence of steps from starting a Spring Boot app to shutdown.

Startup flow from `main()` method to a fully running application.

**Important points (flow)**

1. **Main method**

```java
SpringApplication.run(App.class, args);
```

2. **SpringApplication initialization**

* Prepares environment
* Reads configs (`application.properties`)

3. **Environment prepared**

* Profiles, properties loaded

4. **ApplicationContext creation**

* IoC container created

5. **Component Scan**

* Finds beans (`@Component`, `@Service`, etc.)

6. **Auto-Configuration**

* Configures beans based on dependencies

7. **Bean creation & DI**

* Beans instantiated and dependencies injected

8. **Bean lifecycle callbacks**

* `@PostConstruct` executed

9. **Embedded server start**

* Tomcat/Jetty starts (for web apps)

10. **Application ready**

* `CommandLineRunner` / `ApplicationRunner` executed

11. **Application running**

* Ready to serve requests

12. **Shutdown**

* `@PreDestroy` called
* Context closed

**Example**

```java
@Component
class Runner implements CommandLineRunner {
    public void run(String... args) {
        System.out.println("App started");
    }
}
```

👉 Key idea:
Main → Context → Beans → Server → Ready → Shutdown

---

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
Annotations in Spring Framework used to map HTTP requests to controller methods.

**Important points**

* `@RequestMapping` → generic (can handle any HTTP method)
* Others → shortcuts for specific HTTP methods:

  * `@GetMapping` → GET (read data)
  * `@PostMapping` → POST (create data)
  * `@PutMapping` → PUT (update full resource)
  * `@PatchMapping` → PATCH (partial update)
  * `@DeleteMapping` → DELETE (remove data)
* Can be used at class level + method level

**Example**

```java
@RestController
@RequestMapping("/users")
class UserController {

    @GetMapping
    public List<User> getAll() {}

    @PostMapping
    public void create(@RequestBody User user) {}

    @PutMapping("/{id}")
    public void update(@PathVariable int id, @RequestBody User user) {}

    @PatchMapping("/{id}")
    public void partialUpdate(@PathVariable int id) {}

    @DeleteMapping("/{id}")
    public void delete(@PathVariable int id) {}
}
```

👉 Key idea:
These annotations map **HTTP methods → Java methods** for building REST APIs


---
**@RequestParam vs @PathVariable vs @RequestBody**

**What it is**
Different ways in Spring Framework to get data from an HTTP request.

---

**@RequestParam**

**Important points**

* Gets data from **query parameters** (`?id=1`)
* Mostly used for filtering, optional values

**Example**

```java
@GetMapping("/users")
public User getUser(@RequestParam int id) {}
```

→ `/users?id=1`

---

**@PathVariable**

**Important points**

* Gets data from **URL path**
* Used for identifying a specific resource

**Example**

```java
@GetMapping("/users/{id}")
public User getUser(@PathVariable int id) {}
```

→ `/users/1`

---

**@RequestBody**

**Important points**

* Gets data from **request body (JSON)**
* Used in POST/PUT APIs

**Example**

```java
@PostMapping("/users")
public void createUser(@RequestBody User user) {}
```

→ Body:

```json
{
  "name": "John"
}
```

---

👉 Key idea:

* `@RequestParam` → query
* `@PathVariable` → URL
* `@RequestBody` → JSON body

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

