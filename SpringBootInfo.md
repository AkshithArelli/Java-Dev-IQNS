## Overview
Spring Boot simplifies the development of Spring-based applications by providing a convention-over-configuration approach. Understanding the lifecycle and key components is crucial for building robust applications. This document outlines the Spring Boot application lifecycle and its essential components.

## Application Lifecycle
The Spring Boot application lifecycle can be broken down into the following phases:

1. **Application Startup**
   - The `main` method is invoked, which calls `SpringApplication.run()`.
   - This initializes the Spring context and starts the embedded server (e.g., Tomcat).

2. **Bean Creation and Dependency Injection**
   - Spring scans for components (e.g., `@Component`, `@Service`, `@Repository`, `@Controller`).
   - Beans are created, and dependencies are injected based on annotations or XML configurations.

3. **Application Context Initialization**
   - The `ApplicationContext` is set up, loading all beans and configurations.
   - Event listeners are registered, and the context is refreshed.

4. **Application Running**
   - The application listens for requests on the configured port.
   - Background tasks, schedulers, and other services run as needed.

5. **Application Shutdown**
   - Triggered by signals (e.g., SIGTERM) or programmatic calls.
   - Beans are destroyed, resources are released, and the context is closed.

## Key Components

### 1. SpringApplication
- **Purpose**: The entry point for bootstrapping a Spring Boot application.
- **Key Features**:
  - Configures the application context.
  - Handles command-line arguments.
  - Sets up logging and error handling.
- **Example**:
  ```java
  @SpringBootApplication
  public class MyApplication {
      public static void main(String[] args) {
          SpringApplication.run(MyApplication.class, args);
      }
  }
  ```

### 2. ApplicationContext
- **Purpose**: The central interface for accessing application components.
- **Types**:
  - `AnnotationConfigApplicationContext`: For annotation-based configurations.
  - `ClassPathXmlApplicationContext`: For XML-based configurations.
- **Key Methods**:
  - `getBean()`: Retrieves beans by name or type.
  - `refresh()`: Refreshes the context.

### 3. Auto-Configuration
- **Purpose**: Automatically configures beans based on classpath dependencies.
- **How it Works**:
  - Spring Boot scans for JARs (e.g., `spring-boot-starter-web`) and applies default configurations.
  - Can be overridden using `@EnableAutoConfiguration` or properties.
- **Example**: If `spring-boot-starter-data-jpa` is present, JPA repositories are auto-configured.

### 4. Embedded Servers
- **Purpose**: Provides built-in web servers without external setup.
- **Supported Servers**:
  - Tomcat (default)
  - Jetty
  - Undertow
- **Configuration**: Customize via `application.properties` (e.g., `server.port=8081`).

### 5. Actuators
- **Purpose**: Provides production-ready features like health checks, metrics, and monitoring.
- **Endpoints**:
  - `/actuator/health`: Application health status.
  - `/actuator/info`: Application information.
  - `/actuator/metrics`: Performance metrics.
- **Enabling**: Add `spring-boot-starter-actuator` dependency.

### 6. Profiles
- **Purpose**: Allows configuration for different environments (e.g., dev, prod).
- **Usage**:
  - Define profiles in `application-{profile}.properties`.
  - Activate via `@Profile` annotation or `spring.profiles.active` property.
- **Example**:
  ```properties
  # application-dev.properties
  server.port=8080
  ```

### 7. Configuration Properties
- **Purpose**: Externalizes configuration using properties files or environment variables.
- **Binding**: Use `@ConfigurationProperties` to bind properties to POJOs.
- **Example**:
  ```java
  @ConfigurationProperties(prefix = "app")
  public class AppConfig {
      private String name;
      // getters and setters
  }
  ```

## Best Practices
- Use `@SpringBootApplication` to enable auto-configuration, component scanning, and configuration.
- Leverage profiles for environment-specific settings.
- Monitor applications using Actuators.
- Test components with `@SpringBootTest` for integration testing.
  
---


## 1. Spring Boot vs. Spring Framework

### Dependency Resolution / Avoiding Version Conflict
- **Spring Framework**:  
  - Manually add all required dependencies and define versions.  
  - Can lead to compatibility issues.
  - Example: All dependencies must be included in your `pom.xml` with explicit versions.

- **Spring Boot**:  
  - Uses `spring-boot-starter-parent` for managed dependencies.  
  - Automatically manages dependency versions to avoid conflicts.
  - Transitive dependencies’ versions are set according to the parent, simplifying configuration.

### Avoiding Additional Configuration
- **Spring Framework**:  
  - Requires manual configuration of properties and beans.

- **Spring Boot**:  
  - Provides sensible defaults, only custom configuration is needed.

### Embedded Servers
- **Spring Framework**:  
  - Needs to create a WAR file and deploy to an external server (e.g., Tomcat).

- **Spring Boot**:  
  - Creates executable JAR files with embedded servers (Tomcat/Jetty).
  - No need to deploy WAR files.

### Production-Ready Features
- Spring Boot provides **Actuator** for metrics, health checks, and monitoring.

---

## 2. How to Run a Spring Boot Application?

- **Local Environment**:  
  - Run the main class directly from your IDE (loads from target folder).

- **Production/Server**:
  - Build with Maven/Gradle; generates a JAR in the target folder.
  - Run with: `java -jar target/your-app.jar`

---

## 3. Purpose of `@SpringBootApplication`

- Combines:
  - `@EnableAutoConfiguration`: Auto-configures Spring Beans based on dependencies in your `pom.xml`.
  - `@ComponentScan`: Scans and loads main class and sub-packages (by default, same package as main class).
  - `@Configuration`: Marks the class as a source of bean definitions (loads config classes).

**Tip**:  
To load packages outside the root, use explicit `scanBasePackages`.

---

## 4. Alternative to `@SpringBootApplication`

- You can replace `@SpringBootApplication` with explicit use of:
  ```java
  @Configuration
  @EnableAutoConfiguration
  @ComponentScan
  ```
- It will work, but you must define all annotations manually.

---

## 5. What Is Auto-Configuration?

- Auto-configuration loads classes into the classpath **only** if specific conditions are met (conditional annotations).
- E.g., adding `spring-boot-starter-data-jpa`:
  - Checks for `DataSourceAutoConfiguration.class` conditional properties.
- Enable debug mode (`debug=true`) to see positive/negative matches in log output.

---

## 6. Disabling Specific Auto-Configuration Classes

- Exclude auto-configuration classes using:
  - In Main Class:
    ```java
    @SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})
    ```
  - In `application.properties`:
    ```
    spring.autoconfigure.exclude=org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration, ...
    ```

---

## 7. Customizing Default Configurations

- **Override Defaults** in:
  - `application.properties`
  - `application.yml`

  Examples:
  - Change server port:
    ```
    # application.properties
    server.port=8081
    ```
    ```yaml
    # application.yml
    server:
      port: 8081
    ```

---

## 8. How Does Spring Boot `run()` Work Internally?

- Prepares the **environment** and loads configuration.
- Creates the **application context**.
- Loads properties from `application.properties` & `application.yml`.
- Displays **banner** on startup.
- Chooses the appropriate Application Context type (Web/Reactive/Servlet).
- Registers and loads beans.
- Starts the embedded server on the default or configured port.

---

## 9. What Is `CommandLineRunner`?

- An interface with the `run()` method.
- Used to execute code **just after** the Spring Boot application has started.
- Popular for tasks such as DB initialization, custom logic after all beans are loaded.
- Implement in main class or any `@Component`:
  ```java
  @Component
  public class MyRunner implements CommandLineRunner {
      @Override
      public void run(String... args) throws Exception {
         // Logic to run after context initialization
      }
  }
  ```
- **Order of Execution**:
  - Main class's run method executes first, followed by `CommandLineRunner`.

---

## 10. Purpose of `postProcessEnvironment` in `EnvironmentPostProcessor`

- Used to customize or modify the application's environment **before ApplicationContext is refreshed**.
- Allows you to:
  - Dynamically load properties from external sources (DB, APIs, Files).
  - Add custom property sources.
  - Modify/override properties depending on active profile.
  - Run pre-initialization logic with access to the environment only.

---

## 11. Difference Between `run()` in CommandLineRunner and `postProcessEnvironment()`

| Aspect             | `CommandLineRunner.run()`                        | `EnvironmentPostProcessor.postProcessEnvironment()`         |
|--------------------|--------------------------------------------------|------------------------------------------------------------|
| **Execution Timing** | After application context initialization         | Before application context is created                      |
| **Purpose**        | Run custom logic after startup                    | Modify environment during startup                          |
| **Use Case**       | Startup tasks, DB initialization, etc.            | Dynamic property loading, env customization                |
| **Dependency Access** | Full access to beans                           | No access to beans; only environment properties             |

**Example - EnvironmentPostProcessor:**
```java
public class MyEnvironmentPostProcessor implements EnvironmentPostProcessor {
    @Override
    public void postProcessEnvironment(ConfigurableEnvironment env, SpringApplication app) {
        System.out.println("Customizing environment before ApplicationContext initialization...");
    }
}
```

---

## 12. Purpose of Refreshing the Context

- **Reload Configuration**: Picks up new property values.
- **Reinitialize Beans**: Beans are destroyed and recreated with updated state.
- **Dynamic Updates**: Used for refreshing DB connections, caches, settings dynamically.
- **Event Publishing**: Triggers `ContextRefreshedEvent`, runs custom refresh logic.

---

## Glossary & Key Terms

- **Actuator**: Provides endpoints for health, metrics, info, etc.
- **@SpringBootApplication**: Shortcut annotation for most common Spring Boot setup.
- **AutoConfiguration**: Automatically configures beans based on dependencies and conditions.
- **CommandLineRunner**: Interface for executing code after context is initialized.
- **EnvironmentPostProcessor**: Interface for environment customization before context creation.
- **ApplicationContext**: Core container for bean management in Spring.
- **ContextRefreshedEvent**: Event published when the context is refreshed.

---

## 1. Stereotype Annotations in Spring

**Purpose:**  
Stereotype annotations define and classify components managed by the Spring container, guiding component scanning, bean registration, and architectural roles.

### Common Stereotype Annotations:
| Annotation        | Role/Purpose                        | Example                                             |
|-------------------|-------------------------------------|-----------------------------------------------------|
| `@Component`      | Generic Spring-managed bean         | `@Component public class MyComponent {}`
| `@Service`        | Service/business logic layer        | `@Service public class MyService {}`
| `@Repository`     | Data access/persistence layer       | `@Repository public class MyRepository {}`
| `@Controller`     | Spring MVC controller (web requests)| `@Controller public class MyController {}`
| `@RestController` | REST API controller (JSON/XML)      | `@RestController public class MyRestController {}`  |

**Notes:**
- Specialized annotations (`@Service`, `@Repository`, `@Controller`, `@RestController`) are ultimately recognized as `@Component` for bean registration, but using the specific annotation clarifies the class’s role.

**Benefits:**
- **Component Scanning**: Automatic registration of beans.
- **Role Identification**: Architecture clarity.
- **Simplifies Configuration**: Reduces manual bean definitions.
- **Separation of Concerns**: Clear responsibilities per layer.

---

## 2. Defining Beans in Spring Boot

### 1. Using `@Bean` Annotation (Java Config)
```java
@Configuration
public class AppConfig {
    @Bean
    public MyService myService() {
        return new MyService();
    }
}
```

### 2. Using Stereotype Annotations (Component Scanning)
```java
@Component // or @Service, @Repository, @Controller, etc.
public class MyService {
    // Bean logic
}
```

---

## 3. Dependency Injection in Spring Boot

**Definition:**  
Dependency Injection (DI) is a design pattern where the Spring IoC container manages bean dependencies, injecting them where needed.

**Types of DI:**
1. **Constructor Injection** _(Preferred for mandatory dependencies & immutability)_
    ```java
    @Component
    public class MyService {
        private final MyRepository repo;
        @Autowired
        public MyService(MyRepository repo) { this.repo = repo; }
    }
    ```
2. **Setter Injection** _(Good for optional or mutable dependencies)_
    ```java
    @Component
    public class MyService {
        private MyRepository repo;
        @Autowired
        public void setRepo(MyRepository repo) { this.repo = repo; }
    }
    ```
3. **Field Injection** _(Quick, but less testable & flexible)_
    ```java
    @Component
    public class MyService {
        @Autowired
        private MyRepository repo;
    }
    ```

---

## 4. Constructor vs Setter Injection

**Constructor Injection**
- _Mandatory, immutable dependencies. Easier to test._
- Fails with circular dependencies (unless using `@Lazy`).

**Setter Injection**
- _Optional, mutable dependencies. Can resolve circular dependencies._
- Allows runtime reconfiguration, but risks partially constructed beans.

| Feature                 | Constructor Injection           | Setter Injection                |
|-------------------------|----------------------------------|---------------------------------|
| Mandatory dependencies  | Best suited                     | Not ideal                       |
| Optional dependencies   | Not suited                      | Best suited                     |
| Immutability            | Supported                       | Not supported                   |
| Circular dependencies   | Not supported (unless @Lazy)    | Supported                       |
| Testability             | Excellent                       | Good                            |
| Runtime reconfiguration | Difficult                       | Easy                            |

**Best Practice**  
Use constructor injection for required dependencies and setter injection for optional or circular dependencies.

---

## 5. Real-World Case for `@PostConstruct`

**Purpose:**  
Method annotated with `@PostConstruct` runs once dependency injection/completion is finished.

**Typical Use Case:**  
Loading configuration after required dependencies are injected, like fetching config from a database.

```java
@Service
public class AppConfigService {
    private final ConfigRepository repo;
    private Map<String, String> appConfig;

    public AppConfigService(ConfigRepository repo) { this.repo = repo; }

    @PostConstruct
    public void loadAppConfig() {
        appConfig = repo.findAllConfigs();
        System.out.println("Loaded: " + appConfig);
    }
}
```

**Order of Execution:**
1. Spring Boot application's main run method
2. `@PostConstruct` methods
3. `CommandLineRunner.run()` methods

---

## 6. Dynamically Loading Values

**Common Methods:**
1. **`@Value` Annotation**
   ```java
   @Component
   public class ConfigLoader {
       @Value("${app.dynamic.value}")
       private String dynamicValue;
   }
   ```
2. **Environment Variables**
   ```java
   @Autowired
   private Environment env;
   // Usage: env.getProperty("app.dynamic.value")
   ```
3. **VM Options/Command-Line**
   ```
   -Dapp.dynamic.value=xxx
   ```

---

## 7. YAML vs Properties File

| Feature                  | application.yml (YAML)  | application.properties      |
|--------------------------|------------------------|----------------------------|
| Structure                | Hierarchical, readable | Flat: key-value pairs      |
| Nested values            | Easy                   | Dot notation required      |
| Lists/arrays             | Native support         | Indirect via [] or comma   |
| Data types               | Native (str, num, bool)| All as strings             |
| Example                  | See below              | See below                  |

**YAML Example:**
```yaml
app:
  name: MyApp
  config:
    servers:
      - server1
      - server2
```

**Properties Example:**
```
app.name=MyApp
app.config.servers[0]=server1
app.config.servers[1]=server2
# Or: app.config.servers=server1,server2
```

---

## 8. Difference Between `.yml` & `.yaml`
There’s **no difference**; both are extensions for YAML files with identical functionality. Choice is stylistic only.

---

## 9. Precedence: Properties vs YAML

If same value is set in both files:
- **`application.properties`** value is loaded first and overrides `.yml`.
- Default order (highest priority first):  
  1. `application.properties`
  2. `application.yml`

---

## 10. Loading External Properties

- Place the properties file outside the project, then specify the external file path.
- Example:
   ```
   --spring.config.location=/external/path/yourfile.properties
   ```
- Access values:
```java
@Component
public class EnvConfig {
    @Value("${discount.offer.price}")
    private String dynamicValue;
}
```

---

## 11. Binding Config Properties to Java Objects

**Use `@ConfigurationProperties` for type-safe binding:**

1. **Config Class**
   ```java
   @Component
   @ConfigurationProperties(prefix = "app")
   public class AppConfig {
       private String name;
       private String version;
       private List<String> servers;
       // getters/setters
   }
   ```
2. **YAML/Properties**
   ```yaml
   app:
     name: MyApp
     version: 1.0.0
     servers:
       - server1.example.com
       - server2.example.com
   ```

3. **Inject and Use**
   ```java
   @Service
   public class AppService {
       private final AppConfig appConfig;
       public AppService(AppConfig appConfig) { this.appConfig = appConfig; }
   }
   ```

---

## 12. Spring Bean Life Cycle

1. **Instantiation:** Spring creates bean instance (constructor).
2. **Dependency Injection:** Dependencies set by Spring (fields, setters, constructor).
3. **`@PostConstruct`:** Method runs after dependencies injection.
4. **Custom Initialization:** via `InitializingBean.afterPropertiesSet()` or `@Bean(initMethod="...")`.
5. **Bean Usage:** Used within your Spring app.
6. **`@PreDestroy`:** Method called before destruction.
7. **Custom Destruction:** via `DisposableBean.destroy()` or `@Bean(destroyMethod="...")`.

**Example**
```java
@Component
public class MyBean {
    public MyBean() { System.out.println("Bean Instantiated"); }

    @PostConstruct
    public void postConstruct() { System.out.println("PostConstruct called"); }

    public void customInit() { System.out.println("Custom Init called"); }

    @PreDestroy
    public void preDestroy() { System.out.println("PreDestroy called"); }

    public void customDestroy() { System.out.println("Custom Destroy called"); }
}

@Configuration
public class AppConfig {
    @Bean(initMethod = "customInit", destroyMethod = "customDestroy")
    public MyBean myBean() { return new MyBean(); }
}
```

**Execution Order:**  
1. Constructor  
2. Dependency Injection  
3. `@PostConstruct`  
4. Custom Init  
5. Bean Usage  
6. `@PreDestroy`  
7. Custom Destroy  

---

## 1. Resolving Bean Dependency Ambiguity

When Spring finds multiple beans of the same type for a dependency (e.g., `@Autowired Vehicle vehicle` with both `Car` and `Bike` implementing `Vehicle`), it throws a `NoUniqueBeanDefinitionException`.

**How to resolve?**
- **@Primary**: Marks a bean as the default when multiple candidates match.
  ```java
  @Component
  @Primary
  public class Car implements Vehicle {...}
  ```
- **@Qualifier**: Explicitly specifies which bean to inject.
  ```java
  @Autowired
  @Qualifier("bike")
  private Vehicle vehicle;
  ```
  - Bean name is usually the class name in lowercase (e.g., "bike" for class Bike).
  - Can also use a custom name: `@Component("sportsBike")` and `@Qualifier("sportsBike")`.

**Summary Table**
| Approach   | Use When                                                |
|------------|--------------------------------------------------------|
| @Primary   | One bean is preferred as default                       |
| @Qualifier | Choosing a specific bean ("named" injection)           |

_Note:_  
Injecting the actual class (e.g. `Car`) instead of the interface avoids ambiguity, but it’s not generally recommended for good design.

---

## 2. Dependency Ambiguity Alternatives

- Use Spring-provided `@Qualifier`
- Use Java-provided `@Resource`

---

## 3. Bean Scope in Spring

Bean scope determines bean lifecycle and visibility (how many instances, where/how shared).

| Scope        | Description                                | Where Used      |
|--------------|--------------------------------------------|-----------------|
| singleton    | Single instance per Spring container _(default)_ | General, stateless components
| prototype    | New instance each time requested           | Stateful, short-lived beans  
| request      | One per HTTP request (web only)            | Web apps, per-request data  
| session      | One per HTTP session (web only)            | Web apps, user/session data  
| application  | One per ServletContext (web only)          | Web apps, app-wide data  

### Examples

#### Singleton (Default)
```java
@Component
@Scope("singleton") // optional
public class MyService {}
// All @Autowired MyService are the same object
```

#### Prototype
```java
@Component
@Scope("prototype")
public class MyPrototypeBean {}
// Each injection/request gets a new instance
```
_Prototype beans are not managed by Spring after creation (e.g., destruction)._

#### Request Scope (Web)
```java
@Component
@Scope("request")
public class RequestScopedBean {}
```
_Each HTTP request gets its own bean instance._

#### Session/Application Scope (Web)
```java
@Component
@Scope("session")
public class SessionScopedBean {}

@Component
@Scope("application")
public class ApplicationScopedBean {}
```

#### XML-Based Scope
```xml
<bean id="myBean" class="com.example.MyBean" scope="prototype"/>
```

**Summary Table**

| Scope      | New Instance?      | Typical Use Case                     |
|------------|--------------------|--------------------------------------|
| singleton  | No                 | Shared/global/stateless logic        |
| prototype  | Yes (each request) | Short-lived/stateful, fresh objects  |
| request    | Yes (per request)  | Web UI/request-specific objects      |
| session    | Yes (per session)  | User session management              |
| application| Yes (per app ctx)  | Global app-wide shared data          |

#### Why Not Always Singleton?

Singleton beans are **shared**, so they are not suitable for:
- Stateful components (risk of corrupted/shared data)
- Multi-threaded scenarios (data races)
- Per-request/per-session data (web applications)

**Use-case examples:**
- Use request/session scope for user/request-specific data
- Use prototype for fresh object per use (e.g., report generator, tracker)
- Use singleton for stateless, shared, configuration, or resource-management beans

---

## 4. Defining Custom Bean Scope

**Motivation:**  
Needed when the standard scopes don't match your lifecycle needs, e.g., per-thread, per-task, etc.

**Steps:**
1. Implement `org.springframework.beans.factory.config.Scope`
2. Register scope with Spring (`CustomScopeConfigurer`)
3. Use bean with your custom scope annotation

**Example: Thread Scope**
```java
// 1. Implement custom scope
public class ThreadScope implements Scope {
    private final ThreadLocal<Map<String, Object>> threadScope = ThreadLocal.withInitial(HashMap::new);
    public Object get(String name, ObjectFactory<?> factory) {
        return threadScope.get().computeIfAbsent(name, k -> factory.getObject());
    }
    // ... other required methods
}

// 2. Register with Spring
@Configuration
public class ScopeConfig {
    @Bean
    public static CustomScopeConfigurer customScopeConfigurer() {
        CustomScopeConfigurer configurer = new CustomScopeConfigurer();
        configurer.setScopes(Map.of("thread", new ThreadScope()));
        return configurer;
    }
}

// 3. Use with custom @Scope
@Component
@Scope("thread")
public class MyBean {
    // now each thread gets its own bean instance
}
```
**Use Case:**  
Per-thread contextual objects (e.g., logging context, per-user data in threaded processing).

---

## 5. Singleton vs Prototype Scope: Real-World Use Cases

**Singleton Scope**
- Stateless, shared logic (`@Service`, `@Repository`)
- Shared resources (database pools, caches)
- Utility/configuration beans

**Prototype Scope**
- Stateful: shopping cart, user sessions
- Expensive creation (calculators, report generators)
- Request-scoped processing

---

## 6. Injecting Prototype Bean into Singleton Bean

**Default:**  
Prototype bean injected into singleton via `@Autowired` creates only **one instance** (at singleton bean init).

**To get a fresh instance every time:**
- Use `ObjectFactory<Task>.getObject()` or `Provider<Task>.get()` to manually request a new prototype instance.

**Example:**
```java
@Component
@Scope("prototype")
public class Task {}

@Component
public class TaskService {
    @Autowired
    private ObjectFactory<Task> taskFactory;
    public void execute() {
        Task task = taskFactory.getObject();
        // always new instance
    }
}
```

**How to Explain:**  
Injecting a prototype into a singleton only creates one instance; use a factory/provider for fresh beans.

---

## 7. Spring Singleton vs Plain Singleton

| Type             | How Defined                                 | Scope      | Managed By           |
|------------------|---------------------------------------------|------------|----------------------|
| Spring Singleton | One bean per Spring container/application    | App/Context| Spring Container     |
| Java Singleton   | One instance per JVM (static)                | JVM-wide   | JVM (not Spring)     |

---

## 8. BeanPostProcessor — Custom Bean Initialization/Destruction

**Purpose:**  
Modify or customize bean behavior during initialization/destruction via hooks:

| Method                             | Timing            | Usage Example                              |
|-------------------------------------|-------------------|---------------------------------------------|
| postProcessBeforeInitialization     | Before init       | Logging, validation, property setup         |
| postProcessAfterInitialization      | After init        | Wrapping beans, proxies, monitoring         |

**Example:**
```java
@Component
public class CustomBeanPostProcessor implements BeanPostProcessor {
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {...}
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {...}
}
```
- Used for proxies, cross-cutting concerns, advanced bean modification

---

## 9. Using `@PostConstruct` and `@PreDestroy`

| Annotation    | When Runs              | Common Use                   |
|---------------|------------------------|------------------------------|
| @PostConstruct| After bean initialization | Setup/init logic              |
| @PreDestroy   | Before bean destruction | Cleanup/resource release      |

**Example:**
```java
@Component
public class MyBean {
    @PostConstruct
    public void init() {...}

    @PreDestroy
    public void destroy() {...}
}
```

**Key Points:**
- @PostConstruct after dependency injection/init
- @PreDestroy before bean removal/shutdown
- Common for singleton beans; not called for prototype beans (Spring doesn’t manage their destruction)
- Alternatives: `InitializingBean.afterPropertiesSet` and `DisposableBean.destroy`

---

## 1. Common HTTP Methods Used in Spring REST APIs

- **GET**: Retrieve resources/data.
- **POST**: Create new resources.
- **PUT**: Update or replace existing resources.
- **DELETE**: Remove resources.
- **PATCH**: Partially update resources.

---

## 2. Specifying HTTP Methods for REST Endpoints

Use mapping annotations in Spring:

| Annotation         | HTTP Method |
|--------------------|------------|
| `@GetMapping`      | GET        |
| `@PostMapping`     | POST       |
| `@PutMapping`      | PUT        |
| `@DeleteMapping`   | DELETE     |
| `@PatchMapping`    | PATCH      |
| `@RequestMapping(method = ...)` | Any        |

---

## 3 & 5. Designing Filterable REST Endpoints (e.g., Filter by Product Type)

Given a product database, define a REST endpoint that filters products by `productType`:

```java
@RestController
@RequestMapping("/products")
public class ProductController {
    @GetMapping
    public List<Product> getProducts(@RequestParam(required = false) String productType) {
        if (productType != null) {
            return productService.findByProductType(productType);
        } else {
            return productService.findAll();
        }
    }
}
```
- Use `@RequestParam` for optional query parameters.
- Default value for `@RequestParam`:  
  ```java
  @RequestParam(defaultValue = "Electronics") String productType
  ```

---

## 6. `@PathVariable` vs `@RequestParam`

| Feature                 | `@PathVariable`             | `@RequestParam`                       |
|-------------------------|-----------------------------|---------------------------------------|
| Source                  | URI path                    | Query parameter                       |
| Use case                | Identifying resources       | Filtering/optional/additional data    |
| Required by default     | Yes (404 if missing)        | No (can be optional)                  |
| Example                 | `/users/{id}`               | `/users?name=John`                    |

**Both can be used in the same controller method.**

---

## 7. `@RestController` vs `@Controller`

| Annotation      | Purpose                           | Return Type   | Typical Use                |
|-----------------|-----------------------------------|--------------|----------------------------|
| `@Controller`   | MVC controller (renders views)    | String (view name) | HTML, JSP, Thymeleaf     |
| `@RestController`| REST controller (returns data)   | JSON/XML/Obj | RESTful APIs, web services |

- `@RestController = @Controller + @ResponseBody`
- Use `@RestController` to return data directly, not views.
- For `@Controller` to return data, use `@ResponseBody` on methods.

---

## 9. Deserializing JSON Request Payloads (`@RequestBody`)

- Create a DTO class matching the JSON structure.
- Use `@RequestBody` in your controller method:

```java
@PostMapping("/products")
public ResponseEntity<String> addProduct(@RequestBody ProductDTO product) {
    // fields in ProductDTO must match JSON keys
}
```

---

## 10. POST vs PUT for Update Operations

| Method | Purpose                  | Idempotent | Typical Use           |
|--------|--------------------------|------------|-----------------------|
| POST   | Create resource/action   | No         | Create, non-idempotent updates |
| PUT    | Update/replace resource  | Yes        | Update (recommended)  |

- Prefer **PUT** for updates (aligns with RESTful and idempotency standards).
- **Idempotent**: Multiple identical requests have the same server effect.
- GET, PUT, DELETE are idempotent; POST is NOT.

---

## 11. Request Body in GET Method

- **Avoid using request body** in GET.
- Pass data using query parameters or path variables.
- If a request body is needed, use POST/PUT instead.

```java
@GetMapping("/users")
public ResponseEntity<String> getUser(@RequestParam String name) {...}

@PostMapping("/users")
public ResponseEntity<String> getUser(@RequestBody UserRequest userRequest) {...}
```

---

## 11 (Content Negotiation): Returning XML/JSON from REST Endpoints

- Use `produces` in mapping annotation:
  ```java
  @GetMapping(value = "/worker", produces = {"application/json", "application/xml"})
  ```

- Model class for both formats:
  ```java
  @JacksonXmlRootElement(localName = "Worker")
  public class Worker {
      private String id;
      private String name;
      // getters/setters
  }
  ```

- Client sets the `Accept` header (e.g., `application/json`, `application/xml`) to receive different formats.

---

## 12. Common HTTP Status Codes in REST APIs

| Code | Meaning                  | Typical Usage                       |
|------|--------------------------|-------------------------------------|
| 200  | OK                       | Successful GET/PUT/DELETE           |
| 201  | Created                  | Resource created (POST)             |
| 202  | Accepted                 | Request accepted for processing     |
| 204  | No Content               | Successful, no response body        |
| 302  | Found (redirect)         | Redirect to another URL             |
| 400  | Bad Request              | Invalid input/parameters            |
| 401  | Unauthorized             | Auth required/invalid cred          |
| 403  | Forbidden                | Permission denied                   |
| 404  | Not Found                | Resource absent                     |
| 405  | Method Not Allowed       | HTTP verb not supported             |
| 415  | Unsupported Media Type   | Wrong content type                  |
| 500  | Server Error             | Unexpected server failure           |
| 502  | Bad Gateway              | Upstream error                      |
| 503  | Service Unavailable      | Service temporarily unavailable     |

---

## 13. Setting Custom Status Codes

- **ResponseEntity**:  
  ```java
  return ResponseEntity.status(HttpStatus.CREATED).body("Resource created");
  ```
- **@ResponseStatus**:  
  ```java
  @GetMapping("/path")
  @ResponseStatus(HttpStatus.ACCEPTED)
  public String getCustomStatus() {...}
  ```

---

## 14. Enabling Cross-Origin Resource Sharing (CORS)

1. **@CrossOrigin** (controller or method):
   ```java
   @CrossOrigin(origins = "http://localhost:3000")
   @RestController
   public class MyController { ... }
   ```
2. **Global CORS (`WebMvcConfigurer`)**:
   ```java
   @Configuration
   public class WebConfig implements WebMvcConfigurer {
       @Override
       public void addCorsMappings(CorsRegistry registry) {
           registry.addMapping("/**").allowedOrigins("http://localhost:3000");
       }
   }
   ```
3. **Spring Security CORS setup**:  
   Define `CorsConfigurationSource` and register with `HttpSecurity.cors(...)`.

---

## 15. File Upload in Spring Boot REST

```java
@PostMapping("/upload")
public String uploadFile(@RequestParam("file") MultipartFile file) {
    if (file.isEmpty()) { return "No file selected"; }
    byte[] bytes = file.getBytes();
    Path path = Paths.get("C:/some_directory/" + file.getOriginalFilename());
    Files.write(path, bytes);
    return "File uploaded";
}
```
- Use `MultipartFile` and `@RequestParam("file")` in controller.

---

## 16. API Versioning Strategies

| Strategy         | Sample Usage                                 |
|------------------|----------------------------------------------|
| URI versioning   | `/api/v1/resource` `/api/v2/resource`        |
| Request Param    | `/api/resource?version=1`                    |
| Header           | `X-API-Version: 1` in request headers        |
| Accept Header    | `Accept: application/vnd.example.v1+json`    |

- Choose based on backward compatibility, client needs.

---

## 17. Documenting REST APIs (Swagger/OpenAPI)

1. **Add Maven Dependency**:  
   `springdoc-openapi-ui`
2. **Auto-generates** interactive docs at `/swagger-ui.html`.
3. **Annotate Controllers**:  
   Use `@Operation`, `@ApiResponse`, etc. for detail.
4. **Custom Configurations**:  
   Define OpenAPI bean for descriptions, versions, etc.

---

## 18. Hiding REST Endpoints from Documentation

- Use `@Hidden` annotation on methods/classes.
- Restriction in documentation only; for security, use Spring Security.

---

## 19. Consuming RESTful APIs

**RestTemplate**:
```java
@Service
public class PostService {
    @Autowired
    private RestTemplate restTemplate;
    public Post getPostById(Long id) {
        return restTemplate.getForObject("https://.../posts/" + id, Post.class);
    }
}
```
**Feign Client**:
```java
@FeignClient(name="postClient", url="https://.../")
public interface PostClient {
    @GetMapping("/posts/{id}")
    Post getPostById(@PathVariable Long id);
}
```
- RestTemplate: Imperative, boilerplate, being deprecated.
- Feign: Declarative, less code, Spring Cloud integration.

---

## 1. Exception Handling with @RestControllerAdvice

**@RestControllerAdvice**
- Specialized for REST APIs, globally applies to all `@RestController`.
- Automatically adds `@ResponseBody` (responses as JSON/XML).
- Handles exceptions, binds global data, and initializes model attributes.

### Benefits
- **Centralized**: All exception logic in one place.
- **Consistency**: Consistent error response format.
- **Cleaner Controllers**: No clutter with try-catch blocks in controllers.

### Example Structure

```
com.example.demo
├── controller (UserController.java)
├── exception
│   ├── InvalidInputException.java
│   ├── ResourceNotFoundException.java
│   └── GlobalExceptionHandler.java
├── model (User.java)
├── service (UserService.java)
└── DemoApplication.java
```

### Custom Exception Example

```java
public class InvalidInputException extends RuntimeException { ... }
public class ResourceNotFoundException extends RuntimeException { ... }
```

### Controller Example

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    @Autowired
    private UserService userService;

    @GetMapping("/{id}")
    public User getUser(@PathVariable int id) {
        if (id < 0) throw new InvalidInputException("User ID must be positive");
        return userService.getUserById(id);
    }
}
```

### Global Exception Handler

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(InvalidInputException.class)
    public ResponseEntity<Map<String,Object>> handleInvalidInput(InvalidInputException ex) { ... }

    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<Map<String,Object>> handleNotFound(ResourceNotFoundException ex) { ... }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<Map<String,Object>> handleAll(Exception ex) { ... }
}
```

#### Typical Error Response Example

```json
{
  "error": "Resource Not Found",
  "message": "User with ID 5 not found",
  "timestamp": "2025-05-21T11:06:42.789"
}
```

---

## 2. Best Practices for Handling Multiple Exceptions

- Avoid one handler per exception; use common patterns for scalability and cleaner code.
- **Wrap low-level exceptions in a high-level custom exception (e.g., BusinessException).**
- **Use a base exception (e.g., ApiException) with a `HttpStatus` property**, then handle all using one method in your global handler.

### Example Base Exception

```java
public class ApiException extends RuntimeException {
    private final HttpStatus status;
    public ApiException(String message, HttpStatus status) { ... }
    public HttpStatus getStatus() { ... }
}
```

### Example Global Handler

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(ApiException.class)
    public ResponseEntity<Map<String,Object>> handleApiExceptions(ApiException ex) { ... }
}
```

### Output Example

```json
{
  "error": "Bad Request",
  "message": "Order amount must be greater than 0",
  "timestamp": "2025-05-21T16:13:00.000"
}
```

**Summary Table**

| Technique                      | Purpose                          |
|---------------------------------|----------------------------------|
| Base custom exception           | Reduce duplication               |
| Centralized handler (`ApiException`) | Handle many cases at once       |
| Fallback for Exception.class    | Catch unknown errors             |
| @RestControllerAdvice           | Global processing                |

---

## 3. Validating Input Payloads & Returning Error Messages

**Why Validate?**  
Protects against bad data, security holes, and bugs.

### 1. Add Validation Dependency

```xml
<dependency>
    <groupId>jakarta.validation</groupId>
    <artifactId>jakarta.validation-api</artifactId>
</dependency>
<dependency>
    <groupId>org.hibernate.validator</groupId>
    <artifactId>hibernate-validator</artifactId>
</dependency>
```

### 2. Create a DTO with Validation Annotations

```java
public class UserRequest {
    @NotBlank(message = "Name is mandatory")
    private String name;

    @Email(message = "Invalid email format")
    private String email;

    @Min(value = 18, message = "Age must be at least 18")
    @Max(value = 60, message = "Age must be at most 60")
    private int age;

    @Size(min=10, max=10, message="Phone number must be 10 digits")
    private String phone;

    @Pattern(regexp="^(ADMIN|USER)$", message="Role must be either ADMIN or USER")
    private String role;
}
```

### 3. Use @Valid in Controller

```java
@PostMapping
public ResponseEntity<String> createUser(@Valid @RequestBody UserRequest req) { ... }
```

### 4. Handle Validation Errors Globally

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<Map<String,Object>> handleValidationErrors(MethodArgumentNotValidException ex) {
        // Map of field error -> message
    }
}
```

**Sample Error Output:**

```json
{
  "error": "Validation Failed",
  "details": {
    "name": "Name is mandatory",
    "email": "Invalid email format"
  },
  "timestamp": "2025-05-21T19:04:22"
}
```

---

## 4. Custom Bean Validation

When built-in annotations are insufficient, define custom validator annotations.

### Steps

**1. Create Custom Annotation**

```java
@Documented
@Constraint(validatedBy = UsernameValidator.class)
@Target({ ElementType.FIELD })
@Retention(RetentionPolicy.RUNTIME)
public @interface ValidUsername {
    String message() default "Invalid username: must start with a letter and be 5–15 characters long";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}
```

**2. Create Validator Class**

```java
public class UsernameValidator implements ConstraintValidator<ValidUsername, String> {
    private static final String USERNAME_PATTERN = "^[A-Za-z][A-Za-z0-9_]{4,14}$";
    public boolean isValid(String username, ConstraintValidatorContext ctx) {
        return username != null && username.matches(USERNAME_PATTERN);
    }
}
```

**3. Use It**

```java
public class UserRequest {
    @ValidUsername
    private String username;
}
```

**Typical Invalid Input & Error Response:**

```json
{
  "error": "Validation Failed",
  "details": {
    "username": "Invalid username: must start with a letter and be 5–15 characters long"
  },
  "timestamp": "2025-05-21T13:20:12"
}
```

### When to Use

- When built-in annotation logic isn't enough.
- For reusable custom rules (e.g., special username, password, or code formats).

---

## Summary Tables

### Exception Annotations

| Annotation           | Purpose                             |
|----------------------|-------------------------------------|
| @RestControllerAdvice| Global across all REST controllers  |
| @ExceptionHandler    | Handles specific exception types    |
| @ResponseBody        | Returns JSON/XML by default         |

### Validation Annotations

| Annotation      | Restriction/Rule                       |
|-----------------|----------------------------------------|
| @NotNull        | Value must not be null                 |
| @NotBlank       | Non-null, non-empty String             |
| @Email          | Valid email address                    |
| @Min/@Max       | Numeric range constraints              |
| @Size           | Length of String/list/array            |
| @Pattern        | Regex pattern                          |
| @ValidUsername  | Custom rule (example)                  |

---

