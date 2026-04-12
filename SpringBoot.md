**What is Spring Boot?**
Spring Boot is an extension of the Spring Framework that makes it easy to build production-ready applications quickly with minimal configuration.

**Important points**

* Auto-configuration (no need to write lots of XML/config)
* Embedded servers (Tomcat/Jetty → no external server setup)
* Opinionated defaults (pre-configured setup)
* Runs as standalone app (just `main()` method)

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

**Difference between Spring Boot and Spring Framework**

**What it is**

* Spring Framework → Core framework (DI, AOP, MVC, etc.)
* Spring Boot → Tool built on top of Spring to simplify development

**Important differences**

* Configuration

  * Spring: manual + XML/Java config
  * Boot: auto-configuration

* Setup

  * Spring: need to set up server, dependencies manually
  * Boot: embedded server + starter dependencies

* Development speed

  * Spring: slower setup
  * Boot: very fast

* Use case

  * Spring: flexible, low-level control
  * Boot: quick microservices / REST APIs

**Example difference**

* Spring: configure dispatcher servlet, view resolver manually
* Boot: just add dependency → it auto configures everything


**Spring Boot Features**

**What it is**
Key capabilities provided by Spring Boot to simplify development.

**Important points**

* Auto-configuration (based on classpath)
* Embedded server (Tomcat/Jetty)
* Starter dependencies (no version conflicts)
* Production-ready features (metrics, health)
* Minimal setup (no XML)

**Example**
Add dependency → REST API works without manual config.

---

**Spring Boot Starters**

**What it is**
Predefined dependency bundles in Spring Boot for specific use cases.

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
```

→ Includes Spring MVC + Jackson + Tomcat

---

**Actuator & Endpoints**

**What it is**
Module in Spring Boot to monitor and manage application.

**Important points**

* Provides health, metrics, info
* Exposes endpoints over HTTP/JMX
* Useful in production monitoring

**Example endpoints**

* `/actuator/health` → app status
* `/actuator/metrics` → performance data

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
