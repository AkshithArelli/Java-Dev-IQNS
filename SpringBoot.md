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

 Key idea:
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

 Key idea:
These annotations map **HTTP methods → Java methods** for building REST APIs


---
**@RequestParam vs @PathVariable vs @RequestBody**

**What it is**
Different ways in Spring Framework to get data from an HTTP request.

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

 Key idea:

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

---

**Profiles in Spring Boot**

**What it is**
Way to run application with different configurations (dev, test, prod).

**Important points**

* Use `@Profile` to load beans conditionally
* Activate using `spring.profiles.active`
* Separate config files: `application-dev.properties`, etc.

**Example**

```java
@Component
@Profile("dev")
class DevService {}
```

```properties
spring.profiles.active=dev
```

---

**@Valid vs @Validated**

**What it is**
Used for validation in Spring Framework.

**Important points**

* `@Valid` → standard Java (JSR-380), basic validation
* `@Validated` → Spring-specific, supports **validation groups (different rules for different operations)**


`@Valid` / `@Validated` only **trigger validation**. Actual rules must be defined inside the class.

**Important points**

* Without validation annotations → nothing happens
* You must use annotations like:

  * `@NotNull`
  * `@NotBlank`
  * `@Size`
  * `@Email`
* These come from validation API used by Spring Framework

**@Valid Example**

```java
class User {

    @NotNull
    private String name;

    @Email
    private String email;
}
```

```java
@PostMapping
public void create(@Valid @RequestBody User user) {}
```

 If request has:

```json
{ "name": null, "email": "abc" }
```

→ validation fails automatically

**@Validated Example**

**Important points**

* Define **groups (interfaces)**
* Apply validation conditionally
* Use `@Validated(Group.class)`


**Example**

```java
// 1. Define groups
interface Create {}
interface Update {}
```

```java
// 2. Apply validations with groups
class User {

    @NotNull(groups = Update.class)   // required only for update
    private Long id;

    @NotBlank(groups = {Create.class}) // required only for create
    private String name;
}
```

```java
// 3. Use in controller/service
@RestController
class UserController {

    @PostMapping("/users")
    public void create(@RequestBody @Validated(Create.class) User user) {}

    @PutMapping("/users")
    public void update(@RequestBody @Validated(Update.class) User user) {}
}
```

 Behavior:

* **Create API** → `name` required, `id` not required
* **Update API** → `id` required, `name` optional

 Key idea:
`@Validated` lets you apply **different validation rules for different scenarios**


---

**REST API Principles & Best Practices**

**What it is**
Guidelines to design clean, scalable APIs using HTTP in Spring Boot.

**Important points**

* Use proper HTTP methods

  * GET → read, POST → create, PUT → update, DELETE → remove
* Resource-based URLs (nouns, not verbs)

  * `/users` not `/getUsers`
* Stateless → no server-side session
* Use proper status codes

  * 200, 201, 400, 404, 500
* Versioning

  * `/api/v1/users`
* Consistent response format (JSON)
* Pagination & filtering for large data

**Example**

```http
GET /api/v1/users/1 → 200 OK
POST /api/v1/users → 201 Created
```

---

**Security in API Endpoints**

**What it is**
Protecting APIs from unauthorized access and attacks using Spring Security.

**Important points**

* Authentication → who are you? (JWT, OAuth2)
* Authorization → what can you access? (roles/permissions)
* Use HTTPS (encrypt data)
* Input validation (prevent injection attacks)
* Rate limiting (prevent abuse)
* Use tokens instead of sessions (stateless APIs)

**Example**

```java id="g8o8fw"
@PreAuthorize("hasRole('ADMIN')")
@GetMapping("/admin")
public String admin() {
    return "Only admin";
}
```

 Key idea:

* REST → clean design
* Security → protect access + data


---

**Stateless vs Stateful**

**What it is**
How server handles client state in Spring Boot apps.

**Important points**

* Stateless

  * No client data stored on server
  * Each request is independent
  * Scalable (used in REST APIs)

* Stateful

  * Server stores client session/data
  * Requests depend on previous state
  * Less scalable

**Example**

* Stateless → JWT token sent in every request
* Stateful → session stored on server after login

---

**Circular Dependency**

**What it is**
When two or more beans depend on each other in Spring Framework.

---

**Important points**

* A depends on B, and B depends on A
* Causes **startup failure** (especially with constructor injection)
* Hard to manage and indicates bad design
* Spring may allow it with field/setter injection (but not recommended)

---

**Example**

```java
@Component
class A {
    private final B b;

    public A(B b) {
        this.b = b;
    }
}

@Component
class B {
    private final A a;

    public B(A a) {
        this.a = a;
    }
}
```

→  Fails due to circular dependency

---

**How to fix**

* Redesign (best solution)
* Use interface/third class
* Use `@Lazy` (temporary workaround)

**Example fix**

```java
@Component
class B {
    public B(@Lazy A a) {}
}
```

 Key idea:
Circular dependency = design problem → avoid it

---

**Cohesion vs Coupling**

**What it is**
Design principles in software.

**Important points**

* Cohesion → how related responsibilities are within a class

  * High cohesion = good

* Coupling → dependency between classes

  * Low coupling = good

**Example**

* High cohesion → UserService handles only user logic
* Low coupling → UserService depends on interface, not implementation

 Key idea:

* High cohesion + low coupling = clean design

---

**Caching in Spring Boot**

**What it is**
Storing frequently used data in memory to avoid repeated computation or DB calls.

**Important points**

* Improves performance
* Reduces DB load
* Can use in-memory or distributed cache (like Redis)

**Example**

```java
@Cacheable("users")
public User getUser(int id) {
    return userRepository.findById(id);
}
```

→ First call → DB, next calls → cache

---

**Caching Annotations**

**What it is**
Annotations to control caching behavior.

**Important points**

* `@EnableCaching` → enables caching
* `@Cacheable` → store result in cache
* `@CacheEvict` → remove cache
* `@CachePut` → update cache without skipping method

**Example**

```java
@CacheEvict(value = "users", key = "#id")
public void deleteUser(int id) {}
```

---

**Cache Levels**

**What it is**
Where cache is stored.

**Important points**

* L1 (Local/In-memory)

  * Inside application (e.g., ConcurrentHashMap)
  * Fast but not shared

* L2 (Distributed)

  * External cache like Redis
  * Shared across services

* L3 (Database cache)

  * DB-level caching (less common in Spring usage)

**Example**

* L1 → simple Spring cache
* L2 → Spring + Redis

 Key idea:
Cache = faster reads, less DB load


---

**Trace ID & Span ID**

**What it is**
Used in distributed systems (microservices) to track a request across services using tools like Spring Cloud Sleuth.

---

**Important points**

* **Trace ID**

  * Unique ID for a **complete request flow**
  * Same across all services involved
  * Helps track end-to-end journey

* **Span ID**

  * Unique ID for a **single operation/service call**
  * Changes at each step
  * Represents a unit of work

---

**Example**

* User calls API → Service A → Service B → DB

```
TraceId: 12345 (same everywhere)

Service A → SpanId: A1  
Service B → SpanId: B1  
DB Call   → SpanId: C1
```

 Key idea:

* Trace ID → whole journey
* Span ID → individual steps inside journey


---

**Logging Levels**

**What it is**
Different levels to classify log messages in Spring Boot (uses logging frameworks like Logback).

**Important points**

* `TRACE` → very detailed (step-by-step, lowest level)
* `DEBUG` → debugging info (variables, flow)
* `INFO` → general app events (startup, requests)
* `WARN` → something unexpected but not breaking
* `ERROR` → failure or exception

 Order (low → high severity):
`TRACE < DEBUG < INFO < WARN < ERROR`

---

**Example**

```java id="lq7m3y"
private static final Logger log = LoggerFactory.getLogger(App.class);

log.info("Application started");
log.debug("User id: {}", id);
log.error("Exception occurred", e);
```

 Key idea:
Use lower levels for development, higher levels for production logs

---

**JUnit**

**What it is**
Testing framework for Java to write and run unit tests (commonly used with Spring Boot).

**Important points**

* Used to test methods independently
* Assertions to validate results
* Runs tests automatically

**Example**

```java
@Test
void testAdd() {
    assertEquals(4, 2 + 2);
}
```

---

**@Mock, @InjectMocks, @Spy, @Test, @ExtendWith, @AssertEquals, @AssertNotNull, @verify(...)**

**What it is**
Annotations & methods used in JUnit + Mockito.

**Important points**

* `@Test` → marks test method
* `@Mock` → creates fake object
* `@InjectMocks` → injects mocks into class under test
* `@Spy` → partial mock (real + mock)
* `@ExtendWith(MockitoExtension.class)` → enable Mockito
* `assertEquals`, `assertNotNull` → validate output
* `verify(mock)` → check method calls

**Example**

```java
@ExtendWith(MockitoExtension.class)
class TestClass {

    @Mock
    Repo repo;

    @InjectMocks
    Service service;

    @Test
    void test() {
        when(repo.get()).thenReturn("data");
        assertEquals("data", service.call());
        verify(repo).get();
    }
}
```

---

**@Mock vs @Spy**

**What it is**
Types of test doubles.

**Important points**

* `@Mock`

  * Fully fake object
  * No real logic executed

* `@Spy`

  * Real object + can mock some methods
  * Calls real methods unless mocked

**Example**

```java
@Spy
List<String> list = new ArrayList<>();
```

---

**when(...).thenReturn(...), doNothing(), doThrow()**

**What it is**
Mockito stubbing behavior.

**Important points**

* `when().thenReturn()` → return value
* `doNothing()` → for void methods
* `doThrow()` → simulate exception

**Example**

```java
when(repo.get()).thenReturn("data");

doNothing().when(repo).save();

doThrow(new RuntimeException())
    .when(repo).delete();
```

---

**How to test private methods**

**What it is**
Testing logic inside private methods (in Java / Spring Boot apps).

---

**Important points**

*  Don’t test private methods directly (best practice)
*  Test through **public methods** that use them
* If logic is complex → **move it to a separate class** and test that class
* Reflection can be used, but **not recommended** (breaks encapsulation)

---

**Example (recommended way)**

```java id="x8x0d3"
class Calculator {

    public int add(int a, int b) {
        return internalAdd(a, b); // private method
    }

    private int internalAdd(int a, int b) {
        return a + b;
    }
}
```

```java id="fntw9g"
@Test
void testAdd() {
    Calculator c = new Calculator();
    assertEquals(5, c.add(2, 3)); // indirectly tests private method
}
```

 Key idea:
Test **behavior via public methods**, not private implementation details

---

**Swagger Annotations (OpenAPI)**

**What it is**
Annotations used to document REST APIs in Spring Boot (via Swagger/OpenAPI tools like springdoc).

---

**@Tag**

**Important points**

* Groups related APIs
* Applied at controller level

**Example**

```java
@Tag(name = "User APIs")
@RestController
class UserController {}
```

---

**@Operation**

**Important points**

* Describes a specific API method
* Adds summary/description

**Example**

```java
@Operation(summary = "Get user by id")
@GetMapping("/users/{id}")
public User getUser() {}
```

---

**@ApiResponse**

**Important points**

* Defines possible responses
* Helps document status codes

**Example**

```java
@ApiResponse(responseCode = "200", description = "Success")
@ApiResponse(responseCode = "404", description = "User not found")
```

---

**@Parameter**

**Important points**

* Describes request parameters
* Used for query/path params

**Example**

```java
@GetMapping("/users")
public User getUser(
    @Parameter(description = "User ID") @RequestParam int id) {}
```

---

**@Schema**

**Important points**

* Describes model (DTO/entity fields)
* Adds field-level documentation

**Example**

```java
class User {
    @Schema(description = "User name")
    String name;
}
```

---

**@Deprecated**

**Important points**

* Marks API as outdated
* Indicates it should not be used

**Example**

```java
@Deprecated
@GetMapping("/old-api")
public void oldMethod() {}
```

 Key idea:
Swagger annotations = **API documentation + clarity for consumers**

---

**Spring Scheduler**

**What it is**
Feature in Spring Boot to run tasks automatically at fixed intervals or time.

---

**@EnableScheduling, @Scheduled(cron), @SchedulerLock**

**Important points**

* `@EnableScheduling` → enables scheduling in app
* `@Scheduled(cron = "...")` → runs method based on cron expression
* `@SchedulerLock` (from ShedLock) → ensures only one instance runs task in distributed systems

**Example**

```java id="n3jzv1"
@EnableScheduling
@SpringBootApplication
class App {}

@Scheduled(cron = "0 0/5 * * * ?") // every 5 mins
@SchedulerLock(name = "job1")
public void runJob() {
    System.out.println("Running job");
}
```

---

**ShedLock - lockAtMostFor, lockAtLeastFor**

**What it is**
Configuration in ShedLock to control locking duration.

**Important points**

* `lockAtMostFor`

  * Maximum time lock is held
  * Prevents deadlock if app crashes

* `lockAtLeastFor`

  * Minimum time lock is held
  * Prevents multiple executions too frequently

**Example**

```java id="iqizyo"
@SchedulerLock(
    name = "job1",
    lockAtMostFor = "10m",
    lockAtLeastFor = "1m"
)
```

 Key idea:

* Scheduler → run tasks automatically
* ShedLock → avoid duplicate execution in multi-instance apps


---

**Spring Batch**

**What it is**
Framework in Spring Framework to process large volumes of data (batch processing like ETL, reports).

---

**Job, JobBuilderFactory**

**What it is**

* **Job** → complete batch process
* **JobBuilderFactory** → used to create jobs

**Important points**

* Job = multiple steps
* Has start → end flow

**Example**

```java
Job job = jobBuilderFactory.get("job1")
    .start(step1)
    .build();
```

---

**Step, StepBuilderFactory, ItemReader, ItemProcessor, ItemWriter**

**What it is**

* **Step** → single unit of work in a job
* **StepBuilderFactory** → creates steps
* **ItemReader** → reads data
* **ItemProcessor** → processes/transforms
* **ItemWriter** → writes data

**Important points**

* Works in chunk (read → process → write)
* Core flow: Reader → Processor → Writer

**Example**

```java
stepBuilderFactory.get("step1")
    .<Input, Output>chunk(10)
    .reader(reader)
    .processor(processor)
    .writer(writer)
    .build();
```

---

**JdbcCursorItemReader, JdbcBatchItemWriter**

**What it is**
JDBC-based implementations for DB operations.

**Important points**

* `JdbcCursorItemReader`

  * Reads data row-by-row from DB
  * Uses SQL query

* `JdbcBatchItemWriter`

  * Writes data in batch to DB
  * Uses batch insert/update

**Example**

```java
JdbcCursorItemReader<User> reader = new JdbcCursorItemReader<>();
reader.setSql("SELECT * FROM users");

JdbcBatchItemWriter<User> writer = new JdbcBatchItemWriter<>();
```


 Key idea:
Job → Step → (Reader → Processor → Writer) → handle large data efficiently

---

**How to secure a Spring Boot application**

**What it is**
Protect APIs and data using authentication + authorization (commonly via Spring Security).

**Important points**

* **Authentication (who are you?)**

  * JWT, OAuth2, Basic Auth
* **Authorization (what can you access?)**

  * Roles/permissions (`ADMIN`, `USER`)
* **Use HTTPS**

  * Encrypt data in transit
* **Secure endpoints**

  * Restrict access using config or annotations
* **Input validation**

  * Prevent attacks (SQL injection, etc.)
* **CSRF protection**

  * Needed for stateful apps
* **Stateless APIs**

  * Prefer JWT instead of sessions

---

**Example**

```java id="9h3f7r"
@PreAuthorize("hasRole('ADMIN')")
@GetMapping("/admin")
public String admin() {
    return "secured";
}
```

```java id="lzlpv7"
@Bean
SecurityFilterChain security(HttpSecurity http) throws Exception {
    http
      .authorizeHttpRequests(auth -> auth
          .requestMatchers("/public/**").permitAll()
          .anyRequest().authenticated()
      )
      .httpBasic();
    return http.build();
}
```

 Key idea:
Authenticate user → authorize access → protect data & endpoints

---

**CommandLineRunner & ApplicationRunner**

**What it is**
Interfaces in Spring Boot used to run code **after application startup**.

---

**Important points**

* Both run after Spring Boot fully starts
* Used for initialization (load data, logs, setup)
* Can have multiple runners (ordered with `@Order`)

---

**CommandLineRunner**

**Important points**

* Takes arguments as `String... args`
* Simple and commonly used

**Example**

```java
@Component
class MyRunner implements CommandLineRunner {
    public void run(String... args) {
        System.out.println("App started with args");
    }
}
```

---

**ApplicationRunner**

**Important points**

* Takes `ApplicationArguments` (more structured)
* Can access option/non-option args

**Example**

```java
@Component
class MyAppRunner implements ApplicationRunner {
    public void run(ApplicationArguments args) {
        System.out.println(args.getOptionNames());
    }
}
```

 Key idea:

* Both run after startup
* `CommandLineRunner` → simple
* `ApplicationRunner` → advanced argument handling

---

**How to package a Spring Boot application**

**What it is**
Process of building your app into a runnable **JAR/WAR file** for deployment.

---

**Important points**

* Use build tools: **Maven / Gradle**
* Default packaging → **executable JAR** (preferred)
* Includes:

  * Application code
  * Dependencies
  * Embedded server (Tomcat)

---

**Maven (most common)**

**Steps**

1. Add plugin

```xml
<build>
  <plugins>
    <plugin>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-maven-plugin</artifactId>
    </plugin>
  </plugins>
</build>
```

2. Build command

```bash
mvn clean package
```

3. Run

```bash
java -jar target/app.jar
```

---

**WAR packaging (less used)**

**Important points**

* Used if deploying to external server (Tomcat)
* Change packaging to `war`

 Key idea:
Spring Boot → package as **fat JAR** → run anywhere with `java -jar`

---


**REST Controller implementation**
```java
package com.practise.demo.streams;

import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.*;

@RestController
@RequestMapping("/api/users")
public class UserController {

    private Map<Long, String> users = new HashMap<>();

    // 🔹 GET - Fetch all users
    @GetMapping
    public ResponseEntity<List<String>> getAllUsers() {
        return ResponseEntity.ok(new ArrayList<>(users.values()));
    }

    // 🔹 GET - Fetch user by ID
    @GetMapping("/{id}")
    public ResponseEntity<String> getUserById(@PathVariable Long id) {
        if (!users.containsKey(id)) {
            return ResponseEntity.notFound().build();
        }
        return ResponseEntity.ok(users.get(id));
    }

    // 🔹 POST - Create new user
    @PostMapping
    public ResponseEntity<String> createUser(@RequestBody String name) {
        Long id = (long) (users.size() + 1);
        users.put(id, name);
        return ResponseEntity.ok("User created with ID: " + id);
    }

    // 🔹 PUT - Full update (replace user)
    @PutMapping("/{id}")
    public ResponseEntity<String> updateUser(
            @PathVariable Long id,
            @RequestBody String name) {

        if (!users.containsKey(id)) {
            return ResponseEntity.notFound().build();
        }

        users.put(id, name);
        return ResponseEntity.ok("User updated successfully");
    }

    // 🔹 PATCH - Partial update
    @PatchMapping("/{id}")
    public ResponseEntity<String> patchUser(
            @PathVariable Long id,
            @RequestBody String name) {

        if (!users.containsKey(id)) {
            return ResponseEntity.notFound().build();
        }

        users.put(id, name); // simplified partial update
        return ResponseEntity.ok("User partially updated");
    }

    // 🔹 DELETE - Remove user
    @DeleteMapping("/{id}")
    public ResponseEntity<String> deleteUser(@PathVariable Long id) {
        if (!users.containsKey(id)) {
            return ResponseEntity.notFound().build();
        }

        users.remove(id);
        return ResponseEntity.ok("User deleted successfully");
    }
}
```
