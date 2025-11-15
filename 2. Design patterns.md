# Design Patterns

Design patterns are general, reusable solutions to commonly occurring problems in software design

Total: 21

Eg:

## Creational

These patterns deal with object creation mechanisms, trying to create objects in a manner suitable for the situation.

  - Singleton
  - Factory
  - Builder

## Structural 

These patterns focus on how classes and objects can be combined to form larger structures.

  - Proxy
  - Facade

## Behavioural

These patterns are concerned with algorithms and the assignment of responsibilities between objects, focusing on communication and interaction. 

  - Strategy
  - Template


### Singleton design pattern

Definition

Singleton ensures only one instance of a class exists in the entire application and provides a global point of access to it.

⸻

Why Use It?

	•	When you need one shared object, e.g.:
	•	Logger
	•	Cache manager
	•	Configuration reader
	•	Database connection factory

⸻

Key Features

	1.	Private constructor → prevents external object creation
	2.	Static instance → stores the single object
	3.	Public static method → returns the same instance every time

⸻

Eager Initialization (Simple but memory-heavy)

Object created at application startup (whether you use it or not).

```java
public class Singleton {

    private static final Singleton instance = new Singleton();

    private Singleton() {}

    public static Singleton getInstance() {
        return instance;
    }
}
```
Pros

	•	Simple
	•	Thread-safe by default

Cons

	•	Instance created even when not needed
  
⸻

Lazy Initialization (Not Thread-Safe)

Object created at application startup (whether you use it or not).

```java
  public class MySingleton {
      private static MySingleton instance;

      private MySingleton() {
          // Private constructor to prevent direct instantiation
      }

      public static MySingleton getInstance() {
          if (instance == null) {
              instance = new MySingleton();
          }
          return instance;
      }
  }
```
⸻

Double-Checked Locking (Best balanced approach)

```java
public class Singleton {

    private static volatile Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```
Why this is best?

	•	Lazy-loaded
	•	Thread-safe
	•	No synchronization overhead

⸻

Pros

	•	Saves memory
	•	Ensures consistency
	•	Good for shared resources

Cons

	•	Harder to test (global state)
	•	Can violate Single Responsibility

⸻

One-Line Summary
```
Singleton = one instance + global access + controlled creation.
```
⸻

### Factory Method Pattern

✅ Definition (One Sentence)

Factory Method is a pattern where you create objects using a method, instead of calling new directly.

This allows subclasses to decide which object to create.

⸻

🎯 Simple Real-Life Analogy

When you click “Open File”, you don’t create the PDF/Word/Excel viewer manually.

The system decides which viewer to create.

⸻

🔥 Simplest Possible Java Example

Step 1: Product
```java
interface Animal {
    void speak();
}
```
Step 2: Concrete Products
```java
class Dog implements Animal {
    public void speak() {
        System.out.println("Dog barks");
    }
}

class Cat implements Animal {
    public void speak() {
        System.out.println("Cat meows");
    }
}
```
Step 3: Factory Method
```java
class AnimalFactory {

    public static Animal getAnimal(String type) {
        if (type.equals("dog")) {
            return new Dog();
        } else if (type.equals("cat")) {
            return new Cat();
        }

        return null;
    }
}
```
Step 4: Client
```java
public class TestFactory {
    public static void main(String[] args) {
        Animal a1 = AnimalFactory.getAnimal("dog");
        a1.speak();  // Dog barks

        Animal a2 = AnimalFactory.getAnimal("cat");
        a2.speak();  // Cat meows
    }
}
```

⸻

🧠 Why Is This Factory Method?

Because:

❌ Without factory:
```java
Animal a = new Dog();   // tightly coupled
```
✔ With factory:
```java
Animal a = AnimalFactory.getAnimal("dog");  // loose coupling
```
Client doesn’t know which concrete class is being created.

⸻

⭐ When to Use This Pattern?

	•	When object creation logic changes frequently
	•	When you want to remove “new” from client code
	•	When you want to follow Open/Closed Principle
(add new animals later without touching client code)

⸻

🎯 One-Line Interview Summary

“Factory Method hides the object creation logic and lets a method decide which object to create, improving flexibility and reducing coupling.”

⸻

### Builder Design Pattern

Builder Pattern is used when:

	•	You have a complex object with many fields.
	•	Some fields are optional.
	•	You want to create the object step-by-step in a clean readable way.

Instead of writing messy constructors like:
```java
User u = new User("Akshith", 25, "Hyderabad", null, null, "Software");
```
Builder pattern lets you write:
```java
User u = new User.Builder()
                .name("Akshith")
                .age(25)
                .city("Hyderabad")
                .build();
```
Much cleaner and readable.

⸻

🧩 Simple Example — User Object

❌ Problem Without Builder

You create a User class with many fields → constructor becomes huge → difficult to remember order → prone to mistakes.

⸻

✅ Builder Pattern Implementation

1. Create a class with static Builder
```java
class User {

    private final String name;
    private final int age;
    private final String email;
    private final String city;

    private User(Builder builder) {
        this.name = builder.name;
        this.age = builder.age;
        this.email = builder.email;
        this.city = builder.city;
    }

    // Builder Inner Class
    public static class Builder {
        private String name;
        private int age;
        private String email;
        private String city;

        public Builder name(String name) {
            this.name = name;
            return this;
        }
        public Builder age(int age) {
            this.age = age;
            return this;
        }
        public Builder email(String email) {
            this.email = email;
            return this;
        }
        public Builder city(String city) {
            this.city = city;
            return this;
        }

        public User build() {
            return new User(this);
        }
    }

    @Override
    public String toString() {
        return name + ", " + age + ", " + email + ", " + city;
    }
}
```

⸻

2. Creating the object (super simple)
```java
public class Main {
    public static void main(String[] args) {
        User user = new User.Builder()
                        .name("Akshith")
                        .age(25)
                        .email("a@gmail.com")
                        .city("Hyderabad")
                        .build();

        System.out.println(user);
    }
}
```

⸻

⭐ Interview-Friendly Summary

👍 When to use Builder Pattern?

	•	Object has many fields, especially optional ones.
	•	You want readable, flexible, immutable object creation.
	•	Avoid telescoping constructors (too many constructor arguments).

👍 Advantages

	•	Clean & readable object creation.
	•	Avoids constructor explosion.
	•	Supports immutability.
	•	You can validate fields before building.

👍 Real Life Examples

	•	StringBuilder in Java
	•	Lombok @Builder
	•	HTTP Request construction:
	
```java
new HttpRequest.Builder().header().body().build();
```
⸻


### Proxy Design Pattern

Proxy Pattern provides a substitute or placeholder object that controls access to another object.

You use it when you want to add:

	•	Logging
	•	Security checks
	•	Caching
	•	Lazy loading
	•	Access control
without changing the real object’s code.

⸻

🎯 Simple Real-Life Example

You cannot talk directly to a celebrity →

You talk to their agent (proxy) →

The agent:

	•	filters calls
	•	handles scheduling
	•	protects the celebrity

Celebrity = Real Object

Agent = Proxy

⸻

🎯 When to Use Proxy

	•	To add security before calling real service
	•	To cache results
	•	To lazy-load heavy objects
	•	When real object is remote (RPC, REST)
	•	To maintain control on access

⸻

🧠 Interview-Friendly UML (Simple)

Client → Proxy → RealSubject

Both Proxy and RealSubject implement the same interface.

⸻

✅ Simple Java Example (Best for interviews)

1. Common interface
```java
interface Service {
    void request();
}
```
2. Real object
```java
class RealService implements Service {
    public void request() {
        System.out.println("Real service is doing the work...");
    }
}
```
3. Proxy object (adds access control + logging)
```java
class ServiceProxy implements Service {

    private RealService realService;
    private boolean isAuthenticated;

    public ServiceProxy(boolean isAuthenticated) {
        this.isAuthenticated = isAuthenticated;
    }

    @Override
    public void request() {
        if (!isAuthenticated) {
            System.out.println("Access denied! Authentication required.");
            return;
        }

        System.out.println("Proxy: Logging before calling real service...");

        if (realService == null) {
            realService = new RealService(); // lazy loading
        }

        realService.request();
    }
}
```
4. Client usage
```java
public class Main {
    public static void main(String[] args) {

        Service service = new ServiceProxy(false);
        service.request(); // Access denied

        Service service2 = new ServiceProxy(true);
        service2.request(); // Logs + calls real service
    }
}
```

⸻

📌 Output
```
Access denied! Authentication required.

Proxy: Logging before calling real service...
Real service is doing the work...
```

⸻

🎤 How to give the answer in interview (30-second version)

“Proxy pattern provides a substitute object that controls access to the real object.
It adds extra features like authentication, logging, caching, or lazy loading without modifying the real object’s code.
The client interacts with the proxy which decides whether to forward the request to the actual object.”

⸻

⭐ Real-time example in microservices

	•	Spring AOP uses proxy
	•	Spring Security uses proxy for authentication
	•	Hibernate uses proxy for lazy loading
	•	Feign clients act like proxies for REST services

⸻

### Template Design Pattern

The Template Method Design Pattern is a behavioral pattern that defines the skeleton of an algorithm in a superclass but lets subclasses override specific steps of the algorithm without changing its structure.

⸻

🧠 What it means in simple words:

You define a general structure of an operation, and let subclasses fill in the blanks.

⸻

✅ When to use it?

	•	You have an algorithm with fixed steps, but some steps can vary.
	•	You want to avoid code duplication for common steps.
	•	You want to ensure all variations follow the same process.

⸻

🍽️ Real-Life Example: Preparing a Beverage

Let’s say you are preparing Tea or Coffee, and the process is mostly the same:

	1.	Boil water
	2.	Brew
	3.	Pour in cup
	4.	Add condiments

⸻

🧱 Step-by-Step Java Example

Step 1: Create an abstract class with the template method
```java
public abstract class Beverage {
    // Template method
    public final void prepareRecipe() {
        boilWater();
        brew();
        pourInCup();
        addCondiments();
    }

    private void boilWater() {
        System.out.println("Boiling water");
    }

    private void pourInCup() {
        System.out.println("Pouring into cup");
    }

    // Steps that subclasses will override
    protected abstract void brew();
    protected abstract void addCondiments();
}
```

⸻

Step 2: Create subclasses
```java
public class Tea extends Beverage {
    @Override
    protected void brew() {
        System.out.println("Steeping the tea");
    }

    @Override
    protected void addCondiments() {
        System.out.println("Adding lemon");
    }
}

public class Coffee extends Beverage {
    @Override
    protected void brew() {
        System.out.println("Dripping coffee through filter");
    }

    @Override
    protected void addCondiments() {
        System.out.println("Adding sugar and milk");
    }
}
```

⸻

Step 3: Use it in main
```java
public class Main {
    public static void main(String[] args) {
        Beverage tea = new Tea();
        tea.prepareRecipe();

        System.out.println("-----");

        Beverage coffee = new Coffee();
        coffee.prepareRecipe();
    }
}
```

⸻

🖨️ Output
```
Boiling water
Steeping the tea
Pouring into cup
Adding lemon
-----
Boiling water
Dripping coffee through filter
Pouring into cup
Adding sugar and milk
```

⸻

✅ Benefits

	•	Promotes code reuse (shared logic lives in the abstract class).
	•	Supports Open/Closed Principle – allows extending behavior without modifying the base class.
	•	Ensures consistent execution order.

⸻

🚀 Where it’s used in real life?

	•	Spring Framework:
	•	JdbcTemplate, RestTemplate, KafkaTemplate all use this pattern.
	•	You write only the custom logic; the rest is handled for you.
	•	JUnit testing frameworks: common setup logic with customizable test methods.
	•	Frameworks and SDKs where the flow is fixed but hooks are provided.
