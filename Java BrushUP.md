# ✅ Object-Oriented Programming (OOP) — Brush-Up

OOP is a programming paradigm that organizes code into objects — real-world entities that contain data (fields) and behavior (methods).

OOP helps create software that is:

- **Modular**
- **Reusable**
- **Maintainable**
- **Extensible**

OOP has 4 main pillars:

⸻

### 1️⃣ Abstraction — “Show only what is needed”

You expose only the necessary details and hide the internal working.

 Example:
```java
List<Integer> list = new ArrayList<>();
list.add(10);
```
You know add() adds an element — you don’t know how the array grows internally → that is abstraction.

 Interview line:

Abstraction reduces complexity by exposing only essential features.

⸻

### 2️⃣ Encapsulation — “Bind data + methods and protect them”

Keep variables private and expose functionality through getters/setters.

 Example:
```java
class BankAccount {
    private double balance;

    public void deposit(double amount) { balance += amount; }
    public double getBalance() { return balance; }
}
```
balance is protected — clients cannot change it illegally.

 Interview line:

Encapsulation improves security and avoids accidental modification of data.

⸻

### 3️⃣ Inheritance — “Reuse code from parent”

A child class derives features from a parent class.

 Example:
```java
class Vehicle { void start() {} }
class Car extends Vehicle { void playMusic() {} }
```
 Interview line:

Inheritance provides reusability and establishes an IS-A relationship.

⸻

### 4️⃣ Polymorphism — “Many forms”

Same method behaves differently depending on the object.

 Two types:

1. Compile-time (Method Overloading)
```java
void print(int a) {}
void print(String s) {}
```
2. Runtime (Method Overriding)
```java
class Animal { void sound() { System.out.println("Animal sound"); } }
class Dog extends Animal { void sound() { System.out.println("Bark"); } }
```
Calling sound() chooses method at runtime based on object type.

 Interview line:

Polymorphism increases flexibility and enables dynamic behavior.

⸻

Here is a clean, crisp, interview-ready brush-up on Abstract Class vs Interface — the exact way to explain it in interviews.

⸻

# 🔥 ABSTRACT CLASS vs INTERFACE

1️⃣ Purpose

### Abstract Class:

Used when classes share a common base with some default behavior.

### Interface:

Defines a contract → what the class must do, not how.

⸻

2️⃣ Methods

 Abstract Class
 
	•	Can have abstract methods (no body)
	•	Can have concrete methods (with body)
	•	Can have constructor

 Interface
 
	•	Until Java 8 → only abstract methods
	•	After Java 8 → can have:
	•	default methods (with body)
	•	static methods
	•	private methods (Java 9+)
	•	Cannot have constructors

⸻

3️⃣ Fields

 Abstract Class
 
	•	Can have instance variables
	•	Can have different access modifiers (private, protected, etc.)

 Interface
 
	•	All fields are public static final implicitly
(i.e., constants only)

⸻

4️⃣ Inheritance Rules

 Abstract Class
 
	•	A class can extend only ONE abstract class (single inheritance)
	•	Abstract class can extend another class (abstract or concrete)

 Interface
 
	•	A class can implement multiple interfaces
	•	Interface can extend multiple interfaces

⸻

5️⃣ When to Use What? (Interview Gold Answer)

 Use Abstract Class when:
 
	•	You want to provide partial implementation
	•	You want shared variables or methods
	•	Classes are closely related
	•	You need non-final fields

Real Example:

Animal abstract class → all animals have eat(), sleep(), but sound differs.

⸻

 Use Interface when:
 
	•	You want loose coupling
	•	You want to define a behavior/capability
(e.g., Runnable, Serializable, Comparable)
	•	A class needs to implement multiple behaviors

Real Example:
A class can be both Runnable and Comparable at the same time.

⸻

# Constants vs Enums

Constants are simple variable values with no enforcement or behavior.

Enums are powerful, type-safe, self-contained classes that represent a fixed set of related values. 

```java
public static final int PENDING = 0;
public static final int SUCCESS = 1;
public static final int FAILED = 2;

void process(int status) {
    if (status == SUCCESS) {
        ...
    }
}
```
If someone passes 5, code still compiles → Not safe.
```java
enum PaymentStatus {
    PENDING, SUCCESS, FAILED
}

void process(PaymentStatus status) {
    if (status == PaymentStatus.SUCCESS) {
        ...
    }
}
```
If someone passes anything else, the compiler rejects → Safe.

Enums with Behavior

Enums are actually classes, so you can add logic.
```java
enum Direction {
    NORTH(0), SOUTH(180), EAST(90), WEST(270);

    private int angle;

    Direction(int angle) {
        this.angle = angle;
    }

    public int getAngle() {
        return angle;
    }
}
```

# Marker interface

A Marker Interface is an interface that has no methods and no fields.

It is used only to mark a class with some metadata so that JVM or frameworks treat that class differently.

•	Serializable
•	Cloneable
•	Remote
•	RandomAccess


# 🔥 Java 8 Features 

Java 8 introduced functional programming, streams, default methods, and new APIs.
These features made Java more concise, expressive, and parallel-friendly.

⸻

### 1️⃣ Lambda Expressions

Anonymous functions written in a compact form.

Example:
```java
(nums) -> nums * 2
```
Why important?

	•	Enables functional programming
	•	Removes boilerplate code (anonymous classes)

Interview line:

Lambda expressions allow passing behavior as arguments.

⸻

### 2️⃣ Functional Interfaces

An interface with exactly one abstract method.

Examples:

	•	Runnable
	•	Callable
	•	Comparator
	•	Function, Predicate, Supplier, Consumer

Annotation:

@FunctionalInterface

Example:
```java
@FunctionalInterface
interface Calculator {
    int add(int a, int b);
}
```

⸻

### 3️⃣ Stream API

Processes collections in a functional, declarative way.

Key operations:

	•	Intermediate: map, filter, sorted, distinct
	•	Terminal: collect, forEach, reduce, count

Example:
```java
List<String> list = Arrays.asList("a","bb","ccc");

list.stream()
    .filter(s -> s.length() > 1)
    .forEach(System.out::println);
```
Benefits:

	•	Cleaner code
	•	Lazy evaluation
	•	Supports parallel execution via .parallelStream()

⸻

### 4️⃣ Default & Static Methods in Interfaces

Default method:

Allows interfaces to have method bodies.
```java
interface A {
    default void show() {
        System.out.println("Default");
    }
}
```
Static method:
```java
interface A {
    static void log() { }
}
```
Why?

	•	Backward compatibility
	•	Avoid breaking existing implementations

⸻

### 5️⃣ Optional Class

Avoids NullPointerException by modeling optional values.

Example:
```java
Optional<String> name = Optional.ofNullable("Akshith");
name.ifPresent(System.out::println);
```
Methods:

	•	isPresent()
	•	ifPresent()
	•	orElse()
	•	orElseThrow()

⸻

### 6️⃣ Method & Constructor References

Examples:
```java
System.out::println      // method reference
String::toUpperCase      // instance method reference
ArrayList::new           // constructor reference
```
Equivalent to lambda expressions.

Benefit:

Cleaner and more readable.

⸻

### 7️⃣ New Date & Time API (java.time package)

Replaces old Date and Calendar which were mutable and inconsistent.

Classes:

	•	LocalDate
	•	LocalTime
	•	LocalDateTime
	•	Instant
	•	Period, Duration

Example:
```java
LocalDate today = LocalDate.now();
LocalDate next = today.plusDays(5);
```

⸻

### 8️⃣ Collectors API

Used with streams for grouping, partitioning, etc.

Example:
```java
Map<Integer, List<String>> grouped =
    list.stream()
        .collect(Collectors.groupingBy(String::length));
```

⸻

### 9️⃣ Parallel Streams

For parallel execution of stream operations:
```java
list.parallelStream().forEach(System.out::println);
```
 Can speed up CPU-intensive tasks

❌ Not recommended for shared mutable data

⸻

🔟 Nashorn JavaScript Engine

A JavaScript engine added in Java 8 (deprecated later).

⸻

# Consumer, Supplier and Predicate

### 🔥 1. Consumer — Takes input, returns nothing

 Definition:

A Consumer represents an operation that accepts a single input and returns no result.
```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
}
```
 Example:
```java
Consumer<String> print = s -> System.out.println(s);
print.accept("Hello");  // prints: Hello

list.forEach(s -> System.out.println(s));
```

⸻

### 🔥 2. Supplier — Provides output, takes nothing

 Definition:

A Supplier takes no input and returns a value.
```java
@FunctionalInterface
public interface Supplier<T> {
    T get();
}
```
 Example:
```java
Supplier<Double> randomSupplier = () -> Math.random();
System.out.println(randomSupplier.get());

Supplier<String> getName = () -> "Akshith";
```

⸻

### 🔥 3. Predicate — Takes input, returns boolean

 Definition:

A Predicate tests a condition and returns true/false.
```java
@FunctionalInterface
public interface Predicate<T> {
    boolean test(T t);
}
```
 Example:
```java
Predicate<Integer> isEven = n -> n % 2 == 0;
System.out.println(isEven.test(10)); // true

```
⸻

# Default & Static Methods in Interfaces

🔥 Default & Static Methods in Interfaces — Brush-Up

Before Java 8, interfaces could only contain abstract methods (and constants).

Java 8 introduced:

	•	Default methods → instance-level behavior
	•	Static methods → class-level behavior inside interfaces

These were added mainly for backward compatibility with the Collections and Stream APIs.

### 1️⃣ Default Methods in Interfaces

 What is a Default Method?

A method with a body inside an interface.
```java
default void show() {
    System.out.println("Showing...");
}
```
 Why Needed?

	•	To add new methods to interfaces without breaking existing implementations
	•	To provide common reusable behavior

 Usage Example:
```java
interface Vehicle {
    default void start() {
        System.out.println("Vehicle starting...");
    }
}

class Car implements Vehicle { }

new Car().start();  // Vehicle starting...
```
 Inside default methods you can:

	•	Use this
	•	Override them in implementing classes
	•	Provide reusable logic

⸻

## ⭐ Default Method Conflict (Important Interview Point)

If a class implements two interfaces with same default method → conflict occurs.
```java
interface A { default void show() { System.out.println("A"); } }
interface B { default void show() { System.out.println("B"); } }

class C implements A, B {
    @Override
    public void show() {
        A.super.show();  // or B.super.show()
    }
}
```
Interview line:

When two interfaces provide the same default method, the class must override it to resolve ambiguity.

⸻

### 2️⃣ Static Methods in Interfaces

 What are Static Methods?

A static method inside an interface is just like a static method in a class, but it belongs to the interface only, not to implementing classes.
```java
interface Utils {
    static void log(String msg) {
        System.out.println("LOG: " + msg);
    }
}
```
 How to call?
```java
Utils.log("Hello");
```
❌ You cannot call a static interface method on an object:
```java
Utils obj = new UtilsImpl();
obj.log("Hi"); // ❌ Not allowed
```
 Why Static Methods?

	•	Utility methods belonging logically to the interface
	•	Cleaner design (e.g., Collectors.toList() in Streams API)


# Exception Hierarchy

Exceptions are unexpected events that occur during runtime and can disrupt normal program flow.

Java uses exceptions to handle errors gracefully instead of crashing.

⸻

### 1️⃣ Exception Hierarchy (Very Important)
```
              java.lang.Object
                   ↓

                Throwable
              /           \
         Error           Exception
                         /        \
               Checked Exceptions  RuntimeException
```
##  Error

	•	Serious issues → Not recoverable
	•	Examples: OutOfMemoryError, StackOverflowError
	•	You should not catch them normally.

##  Exception

Recoverable problems. Two types:

⸻

### 2️⃣ Checked vs Unchecked Exceptions

##  Checked Exceptions

	•	Checked at compile time
	•	Must be handled using:
	•	try-catch OR
	•	throws keyword

Examples:

	•	IOException
	•	SQLException
	•	ClassNotFoundException

Example:
```java
try {
    FileReader fr = new FileReader("test.txt");
} catch (IOException e) {
    e.printStackTrace();
}
```

⸻

##  Unchecked Exceptions (RuntimeExceptions)

	•	Occur at runtime
	•	Not required to catch or declare
	•	Usually programming mistakes

Examples:

	•	NullPointerException
	•	ArrayIndexOutOfBoundsException
	•	ArithmeticException
	•	IllegalArgumentException

Example:
```java
int x = 10 / 0; // ArithmeticException
```

⸻

## 3️⃣ Common Built-in Exceptions

Runtime:

	•	NullPointerException
	•	NumberFormatException
	•	IllegalStateException
	•	ConcurrentModificationException
	•	IndexOutOfBoundsException

Checked:

	•	IOException
	•	FileNotFoundException
	•	SQLException
	•	ParseException

⸻

## 4️⃣ Exception Handling Blocks
 ```java
//try-catch

try {
    int x = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Cannot divide by zero");
}

// try-catch-finally

try {
    connection.open();
} finally {
    connection.close(); // always runs
}

// try-with-resources (Java 7+)

Automatically closes resources.

try (FileReader fr = new FileReader("file.txt")) {
    // use file
}
```

⸻

### 5️⃣ throws vs throw

 throw

Used to manually throw an exception.
```java
throw new IllegalArgumentException("Invalid age");
```
 throws

Used in method signature to indicate the method may throw exceptions.
```java
void read() throws IOException { }
```

⸻

### 6️⃣ Custom Exceptions

Use custom exceptions for business rules

Custom Checked Exception:
```java
class InvalidAgeException extends Exception {
    public InvalidAgeException(String msg) {
        super(msg);
    }
}
```
Custom Unchecked Exception:
```java
class InvalidInputException extends RuntimeException {
    public InvalidInputException(String msg) {
        super(msg);
    }
}

```
⸻

### 7️⃣ Exception Propagation

Runtime exceptions automatically propagate up the call stack until caught.
```
methodA() -> methodB() -> methodC()  
Exception occurs in C → goes to B → A → main → JVM
```
⸻

# Controller Advice, Rest Controller Advice, Exception Handler

@ExceptionHandler handles exceptions inside a single controller.

@ControllerAdvice applies cross-cutting exception handling to all controllers globally.

@RestControllerAdvice does the same but automatically returns JSON, making it ideal for REST APIs.
```java
@RestController
public class UserController {

    @GetMapping("/user/{id}")
    public String getUser(@PathVariable int id) {
        if(id <= 0) {
            throw new IllegalArgumentException("Invalid user id");
        }
        return "User found";
    }
}

@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity<String> handleIllegal(IllegalArgumentException ex) {
        return ResponseEntity.badRequest().body(ex.getMessage());
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handleOther(Exception ex) {
        return ResponseEntity.status(500).body("Something went wrong");
    }
}
```

# Sealed Classes in Java

Sealed classes restrict which classes can extend them.

Each permitted subclass must be final, sealed, or non-sealed, giving complete control over inheritance and enabling exhaustive pattern matching.
```java
public sealed class Payment permits CardPayment, UpiPayment, WalletPayment { }

public final class CardPayment extends Payment { }
public final class UpiPayment extends Payment { }
public non-sealed class WalletPayment extends Payment { }
```

# Serialization vs Deserialization
```
Concept				Meaning										Direction
Serialization		Converting a Java object → byte stream		Object ➝ Bytes
Deserialization		Converting a byte stream → Java object		Bytes ➝ Object
```

# sealed classes

1️⃣ Definition

transient keyword prevents a field from being serialized—those fields are skipped when an object is converted to bytes.

⸻

2️⃣ Why Needed?

	•	To avoid serializing sensitive data (passwords, tokens)
	•	To skip temporary or derived values

⸻

3️⃣ Key Features

	•	transient fields → not written during serialization
	•	When deserialized → restored with default values
	•	Works only with fields
	•	static fields aren’t serialized anyway

⸻

4️⃣ Example
```java
class User implements Serializable {
    private String name;
    private transient String password; // will not be serialized
}

User u = new User("Akshith", "secret123");
```
After deserialization:

	•	name = “Akshith”
	•	password = null

⸻

# Shallow Copy vs Deep Copy

### Shallow Copy

A shallow copy copies only the top-level object, but does not copy nested objects. Both objects share the same references inside.

### Deep Copy

A deep copy creates a fully independent clone by copying all nested objects recursively.

Shallow Copy example
```java
class Student implements Cloneable {
    String name;
    Address address; // mutable reference type

    public Student clone() throws CloneNotSupportedException {
        return (Student) super.clone(); // shallow copy
    }
}

class Address {
    String city;
}
```

Deep Copy example
```java
class Student implements Cloneable {
    String name;
    Address address;

    public Student clone() throws CloneNotSupportedException {
        Student copy = (Student) super.clone();
        copy.address = new Address(address.city); // deep copy
        return copy;
    }
}

class Address {
    String city;
    Address(String city) { this.city = city; }
}
```

# Multithreading

### ✅ 1️⃣ Thread — Definition

A Thread is the smallest unit of execution in a program. Multiple threads allow parallelism.

⸻

🚀 Why Threads?

	•	Perform multiple tasks simultaneously
	•	Improve performance
	•	Background tasks (timers, async calls, schedulers)

⸻


### ✅ 2️⃣ Thread Creation (Two Ways)

Method 1: Extending Thread class
```java
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Thread running");
    }
}

new MyThread().start();
```
Method 2: Implementing Runnable (preferred)
```java
class MyTask implements Runnable {
    public void run() {
        System.out.println("Task executing");
    }
}

Thread t = new Thread(new MyTask());
t.start();
```
✔ Runnable is preferred because Java supports multiple interface inheritance, not multiple class inheritance.

⸻


### ✅ 3️⃣ start() vs run()
```
start()							run()
Creates a new OS thread			Does NOT create a new thread
Calls JVM to schedule thread	Runs like a normal method
Executes asynchronously			Executes synchronously
```
Example:
```java
Thread t = new Thread(() -> System.out.println("Running"));
t.start();  // New thread  
t.run();    // Same thread (main)
```

⸻

### ✅ 4️⃣ Thread Life Cycle

NEW → RUNNABLE → RUNNING → BLOCKED/WAITING → TERMINATED

States:
```
	1.	NEW – thread created but start() not called
	2.	RUNNABLE – eligible for CPU scheduling
	3.	RUNNING – executing instructions
	4.	BLOCKED / WAITING
		•	sleep()
		•	wait()
		•	I/O blocking
	5.	TERMINATED – completed or stopped
```
⸻

### ✅ 1️⃣ Synchronization

Definition

Synchronization ensures only one thread at a time can access shared resources, preventing race conditions.

⸻

Examples

✔ Synchronized Method
```java
public synchronized void increment() {
    count++;
}
```
✔ Synchronized Block (preferred)
```
synchronized (lock) {
    count++;
}
```
✔ Object-level Lock vs Class-level Lock
```
public synchronized void method() {} 
// locks 'this' object

public static synchronized void method() {}
// locks Class object
```

⸻

### ✅ 6️⃣ wait(), notify(), notifyAll()

Used for inter-thread communication, especially in producer–consumer.

✔ wait()

	•	Releases lock
	•	Moves thread to WAITING state

✔ notify()

	•	Wakes one waiting thread

✔ notifyAll()

	•	Wakes all waiting threads

Example:
```
synchronized (lock) {
    lock.wait();      // thread waits
    lock.notify();    // wake one
    lock.notifyAll(); // wake all
}
```
✔ Must be called inside synchronized block
✔ Used for coordination between threads

⸻

### ✅ 7️⃣ volatile — Definition

volatile ensures visibility of changes across threads.

What it does:

	•	Prevents thread caching
	•	Always reads from main memory
	•	Writes go directly to main memory

Example:
```java
volatile boolean flag = true;
```
Without volatile, one thread may not see updated values written by another thread.

volatile DOES NOT:

	•	Make operations atomic
	•	Replace synchronization

⸻

### ✅ 8️⃣ thread.join() — Definition

join() makes one thread wait until another thread completes execution.

Example:
```java
Thread t = new Thread(() -> {
    System.out.println("Task");
});

t.start();
t.join();  // main waits until t finishes
System.out.println("Main continues");
```
✔ Used when a task must finish before continuing
✔ Useful in multi-thread pipelines

⸻

### ✅ 9️⃣ Thread Priority

In Java:

	•	Priorities range 1 to 10
	•	Thread.MAX_PRIORITY = 10
	•	Thread.MIN_PRIORITY = 1
	•	Thread.NORM_PRIORITY = 5

Set priority:

t.setPriority(Thread.MAX_PRIORITY);

But…

Thread priority is only a hint to the scheduler.
JVM & OS may ignore it.

⸻

Here is a clean, crisp, interview-ready brush-up on Synchronization, Deadlocks, and ReentrantLock — in the structured format you prefer.

⸻

🚀 BRUSH-UP: SYNCHRONIZATION, DEADLOCK, REENTRANTLOCK

⸻

# 🔥 Deadlock

Definition

Deadlock is a situation where two or more threads are permanently blocked, each waiting for a resource held by the other.

⸻

### Four Conditions Required for Deadlock (VERY IMPORTANT)

All four must exist simultaneously:

1️⃣ Mutual Exclusion
Only one thread can access a resource at a time.

2️⃣ Hold and Wait
Thread holds one resource and waits for another.

3️⃣ No Preemption
Resources cannot be forcibly taken away.

4️⃣ Circular Wait
Thread A waits for Thread B’s resource,
Thread B waits for Thread C’s resource…
Thread N waits for Thread A’s resource.

If you break any one of these conditions → deadlock is prevented.

⸻

Simple Deadlock Example
```java
Thread t1 = new Thread(() -> {
            synchronized (lock1) {
                System.out.println("T1 locked lock1");

                try { Thread.sleep(100); } catch (Exception ignored) {}

                synchronized (lock2) {
                    System.out.println("T1 locked lock2");
                }
            }
        });

        Thread t2 = new Thread(() -> {
            synchronized (lock2) {
                System.out.println("T2 locked lock2");

                try { Thread.sleep(100); } catch (Exception ignored) {}

                synchronized (lock1) {
                    System.out.println("T2 locked lock1");
                }
            }
        });

        t1.start();
        t2.start();
```		
Two threads locking in opposite order → deadlock.

⸻

### 🛡 Deadlock Prevention Techniques

✔ 1. Lock Ordering (Most Common)

Always acquire locks in the same order everywhere.

synchronized(lock1) {
    synchronized(lock2) { }
}

⸻

✔ 2. Timeout using tryLock()

If lock cannot be acquired → avoid waiting forever.

if (lock.tryLock(100, TimeUnit.MILLISECONDS)) {
    // do work
} else {
    // handle timeout
}


⸻

✔ 3. Avoid Nested Locks

Break complex locking structures.
Simplify critical sections.

⸻

✔ 4. Use Higher Level Concurrency Tools

	•	Executors
	•	Semaphores
	•	ConcurrentHashMap
	•	Atomic variables

⸻

✔ 5. Using volatile + immutable objects

Reduces need for locking.

⸻

# 🔐 ReentrantLock

Definition

A reentrant lock is an advanced locking mechanism from java.util.concurrent.locks that allows a thread to acquire the same lock multiple times without blocking itself.

⸻

Why Needed?

	•	More flexibility than synchronized
	•	Allows:
	•	timed lock attempts
	•	interruptible lock attempts
	•	fairness policies
	•	manual lock/unlock control

⸻

Key Features

✔ 1. Reentrancy

A thread holding a lock can acquire it again.

✔ 2. tryLock()

Avoids blocking forever; useful to prevent deadlocks.

if (lock.tryLock()) {
    // acquired
}

✔ 3. tryLock(timeout, unit)

Wait for limited time → timeout instead of deadlock.

✔ 4. Interruptible Locks

lock.lockInterruptibly();

Useful when a waiting thread should be interruptible.

✔ 5. Fairness Policy

ReentrantLock lock = new ReentrantLock(true); // fair mode

Maintains queue order of threads.

⸻
```java
import java.util.concurrent.locks.ReentrantLock;

public class ReentrantExample {

    private static final ReentrantLock lock = new ReentrantLock();

    public static void main(String[] args) {
        new ReentrantExample().methodA();
    }

    public void methodA() {
        lock.lock();   // Thread acquires lock 1st time
        try {
            System.out.println("Inside methodA - lock acquired once");

            methodB(); // Call another method that tries to acquire same lock
        } finally {
            lock.unlock();
        }
    }

    public void methodB() {
        lock.lock();   // Thread acquires SAME LOCK 2nd time
        try {
            System.out.println("Inside methodB - lock acquired second time by SAME thread");
        } finally {
            lock.unlock();
        }
    }
}
```

Here is your clean, crisp, interview-ready brush-up for:
	•	Runnable vs Callable
	•	Future vs CompletableFuture
	•	@Async vs ThreadPoolExecutor

Structured in the same format you prefer.

⸻

# Runnable vs Callable

⸻

✅ 1️⃣ Definition

Runnable

Represents a task that does not return a value and cannot throw checked exceptions.

Callable

Represents a task that returns a value and can throw checked exceptions.

⸻
Examples:
```java
Runnable task = () -> {
	System.out.println("Runnable task is running.");
};

Thread thread = new Thread(task);
thread.start(); // Starts the thread

Callable<Integer> task = () -> {
	System.out.println("Callable task is running.");
	return 123; // return result
};
```	
⸻


# 🚀 Future vs CompletableFuture

✔ 1️⃣ Future

Represents the result of an asynchronous computation, but is blocking and limited.

✔ 2️⃣ CompletableFuture — Definition

An advanced async API that supports non-blocking, chaining, callbacks, combining tasks, and fully asynchronous pipelines.

⸻

✔ Examples

Future (Blocking)
```java
Future<Integer> future = executor.submit(() -> 10);
int result = future.get(); // blocking
```

⸻

CompletableFuture (Non-Blocking)
```java
CompletableFuture.supplyAsync(() -> 10)
    .thenApply(n -> n * 2)
    .thenAccept(System.out::println);
```
Pipeline explained:

	•	supplyAsync → produce value
	•	thenApply → transform value
	•	thenAccept → consume value

✔ No blocking

✔ Runs asynchronously

⸻

✔ One-line Summary

Future is blocking and limited, while CompletableFuture supports async pipelines, chaining, combining tasks, and non-blocking programming.

⸻

# ExecutorService & Thread pools

ExecutorService manages a pool of threads.

Instead of creating threads manually, we submit tasks to the executor.

This improves performance, reduces memory usage, avoids too many threads, and provides clean task management.

A thread pool is a group of worker threads maintained by the ExecutorService.
```
           Submit Tasks
                |
                v
        +-----------------+
        | ExecutorService |
        +-----------------+
          /     |      \
         v      v       v
   Worker1   Worker2  Worker3   <-- Thread Pool
```

# ForkJoinPool for divide-and-conquer vs vs normal ExecutorService

It uses a technique called:

Fork → Divide task into smaller subtasks

Join → Combine results of subtasks

Think of it like splitting a big job into small parts, processing all parts in parallel, then merging results.

⸻

Why ForkJoinPool (vs normal ExecutorService)?

Normal ExecutorService works best when:

•	each task is independent
•	tasks are not recursively broken down

ForkJoinPool is designed for:

•	recursive tasks (divide and conquer)
•	tasks that can be split into smaller tasks
•	tasks that benefit from parallel computation (CPU-heavy)
```
                          MAIN TASK
                              |
                     -----------------
                     |               |
                  SubTask1       SubTask2
                   |   |           |   |
                T1    T2        T3     T4
```
All run in parallel → Combine results → Final answer

# @Async vs ThreadPoolExecutor

@Async is only a declarative annotation for executing a method asynchronously. It internally uses a TaskExecutor (usually ThreadPoolTaskExecutor).

If you need fine control over the thread pool (core size, max size, queue, rejection policy), configure a ThreadPoolTaskExecutor manually and point @Async to it.


Here is a clean, crisp, interview-ready brush-up on Atomic Classes vs volatile — in your preferred structured format.

⸻

# Atomic Classes vs volatile

⸻

volatile

volatile guarantees visibility of changes across threads but does NOT make operations atomic.

Atomic Classes (java.util.concurrent.atomic)

Atomic classes provide atomic (thread-safe) operations like increment, decrement, compare-and-set without using synchronization.

Example:

Even if volatile int count is visible to all threads,

count++;

is NOT atomic because it’s 3 operations internally:

	1.	read
	2.	increment
	3.	write

Multiple threads can interleave → inconsistent results.

⸻

🚀 Atomic Classes (Overview)

Popular classes:

	•	AtomicInteger
	•	AtomicLong
	•	AtomicBoolean
	•	AtomicReference
	•	AtomicLongArray, etc.

⸻

🔥 AtomicInteger Example

AtomicInteger count = new AtomicInteger(0);
```
count.incrementAndGet(); // atomic ++
count.getAndIncrement();
count.addAndGet(5);
```
These operations are atomic, no race conditions, no synchronized needed.

🚀 volatile Example (Visibility Guarantee)

```java
volatile boolean flag = true;

Thread t = new Thread(() -> {
    while (flag) { }
    System.out.println("Stopped");
});
t.start();

Thread.sleep(1000);
flag = false; // visible immediately to t
```

### ✔ Difference

volatile prevents stale reads; atomic classes prevent race conditions.

⸻

# How does java handle pass by values and pass by reference

Java always passes arguments by value.

For primitives, the actual value is copied.

For objects, the value copied is a reference, so modifying the object’s fields affects the original object, but reassigning the reference inside the method does not change the caller’s reference.
```java
void modify(int x) {
    x = 50;
}

int a = 10;
modify(a);

System.out.println(a); // 10
```
```java
class Student { 
int age; 
}

void change(Student s) {
    s.age = 25;
}

Student st = new Student();
st.age = 20;

change(st);
System.out.println(st.age); // 25

void change(Student s) {
    s = new Student();  // reassigning local copy
    s.age = 30;
}

Student st = new Student();
st.age = 20;

change(st);

System.out.println(st.age); // 20 ❗ not 30
```
Here is a clean, crisp, interview-ready quick brush-up on all four topics — in your preferred structured format.

⸻

#  == vs .equals()

✔ Definition

==

Compares references (memory addresses) for objects, and values for primitives.

.equals()

Compares content/logical equality (when overridden).

⸻

✔ Key Points
```
Comparison				==								.equals()
Primitives				Value comparison				Not used
Objects					Reference comparison			Content comparison (if overridden)
Default 				equals (Object)	Same as ==		Must override for meaningful comparison
```

⸻

✔ Example
```java
String s1 = new String("Java");
String s2 = new String("Java");

s1 == s2;        // false (different objects)
s1.equals(s2);   // true (same content)
```

⸻

⭐ One-line summary

== checks reference equality; .equals() checks logical equality.

⸻

# Internal Working of HashMap

✅ Definition

HashMap stores key–value pairs in buckets using the key’s hashCode() to compute bucket index.

Within the bucket, HashMap uses equals() to find the correct key.

Collisions are handled using LinkedList or Red-Black Tree (Java 8+).

⸻

### 1️⃣ How HashMap Stores a Key (put operation)

When you do:
```java
map.put("A", 1);
```
HashMap performs:

Step 1: Compute hash
```java
int hash = "A".hashCode();
```
Step 2: Determine bucket index
```java
index = hash & (capacity - 1);
```
Step 3: Go to that bucket

	•	If bucket empty → insert new Node → DONE
	•	If bucket not empty → collision → go to next step

Step 4: Compare keys using equals()

	•	If key.equals(existingKey) → replace value
	•	Else → add new node to bucket
	•	As LinkedList or
	•	As TreeNode if chain length > 8 (Java 8)

⸻

### 2️⃣ How HashMap Retrieves a Value (get operation)

When you do:
```java
map.get("A");
```
HashMap:

Step 1: Compute hash → find bucket

Step 2: Traverse nodes in bucket

Step 3: Compare keys via equals()

If match → return value

If not → keep searching

If none found → return null

⸻

### 3️⃣ Collision Handling (VERY IMPORTANT)

✔ Java 7: LinkedList → O(n) in worst case

✔ Java 8+:

	•	If bucket becomes too large (≥ 8 entries)
	•	Converts list → Red-Black Tree
	•	Lookup becomes O(log n)

This prevents performance degradation from hash collisions.

⸻

### 4️⃣ Resizing (Rehashing)

Occurs when:

size > capacity * loadFactor

Default load factor = 0.75

On resize:

	•	Capacity doubles (e.g., 16 → 32)
	•	All keys are rehashed
	•	Costly operation → avoid frequent resizing by setting initial capacity properly

⸻

### 5️⃣ Core Logic in One Sentence

HashMap uses hashCode() to locate the bucket, and equals() to locate the exact key; collisions are handled using LinkedList or Red-Black Tree.

⸻

🧠 6️⃣ Simple Example to Remember
```java
HashMap<String, Integer> map = new HashMap<>();
map.put("FB", 1);
map.put("Ea", 2);
```
Why interesting?

Both "FB" and "Ea" have the same hashCode(), so:

	•	They land in the same bucket
	•	HashMap uses equals() to differentiate them
	•	Stored as separate nodes in the bucket

⸻

# 🚀 equals() and hashCode()

✔ Definition

If two objects are equal using .equals(), they must have the same hashCode().

HashMap depends on this so equal keys go to the same bucket and retrieval works correctly.

⸻

Here is a clean, crisp, interview-ready brush-up on the difference between HashMap, Hashtable, SynchronizedMap, and ConcurrentHashMap — simplified and accurate.

⸻

# HashMap vs Hashtable vs SynchronizedMap vs ConcurrentHashMap

⸻

### ✅ 1️⃣ HashMap

✔ Definition

A non-thread-safe key–value map that allows one null key and multiple null values.

✔ Key Points

	•	Not synchronized → not safe for multithreading
	•	Fastest in single-thread use
	•	Allows null key & null values
	•	Uses hashing + linked list / tree (Java 8)

⸻

### ✅ 2️⃣ Hashtable

✔ Definition

A thread-safe map where all methods are synchronized, but very slow.

✔ Key Points

	•	Entire table is locked → one thread at a time
	•	Does not allow null key or null value
	•	Legacy class (Java 1.0)
	•	Poor scalability under load

⸻

### ✅ 3️⃣ SynchronizedMap

Created using:
```java
Map m = Collections.synchronizedMap(new HashMap<>());
```
✔ Definition

A wrapper around HashMap where all methods are synchronized.

✔ Key Points
	•	Behaves similar to Hashtable
	•	Single lock for entire map
	•	Safer but slow in multi-threaded scenarios
	•	Null keys/values allowed (because underlying HashMap allows)

⸻

### ✅ 4️⃣ ConcurrentHashMap

✔ Definition

A high-performance thread-safe map using fine-grained locking and non-blocking operations (CAS).

✔ Key Points

	•	No global lock → multiple threads can access the map simultaneously
	•	Uses bucket-level locking (Java 7) or Node-level CAS + sparse locking (Java 8)
	•	Does NOT allow null key or null value
	•	Best choice for multi-threaded environments
	•	Extremely scalable

⸻

⭐ 5️⃣ One-line Interview Summary

HashMap is non-thread-safe, Hashtable & SynchronizedMap use full-locking (slow), while ConcurrentHashMap uses fine-grained locking/CAS for high-performance concurrent access.

⸻

# Comparable vs Comparator

⸻

✅ 1️⃣ Definitions

### Comparable

Used to define the natural/default sorting order of a class. Implemented inside the class via compareTo().

### Comparator

Used to define custom or multiple sorting orders. Written outside the class via compare().

⸻

✅ 2️⃣ Method Difference
```
Interface	Method	Used For
Comparable	int compareTo(T o)	Natural sorting
Comparator	int compare(T o1, T o2)	Custom sorting
```

⸻

🚀 3️⃣ When to Use Which?

✔ Use Comparable when:

	•	The class has one natural sorting (e.g., sorting students by rollNo).
	•	You want objects of the class to be sortable by default.
	•	Sorting logic belongs to the object itself.

Example:

String, Integer, Double → all implement Comparable.

⸻

✔ Use Comparator when:
```
	•	You want multiple sorting criteria.
Example: Sort Students by name, then age, then marks.
	•	You cannot modify the class (e.g., class from a library).
	•	Sorting logic should be external.
```
⸻

🚀 4️⃣ Comparable Example (natural sorting)
```java
class Student implements Comparable<Student> {
    int id;
    String name;

    @Override
    public int compareTo(Student other) {
        return this.id - other.id; // sorted by id
    }
}
```
Sorting:
```java
Collections.sort(list); // uses compareTo()
```

⸻

🚀 5️⃣ Comparator Example (custom sorting)
```java
Comparator<Student> byName =
    (s1, s2) -> s1.name.compareTo(s2.name);

Comparator<Student> byAge =
    (s1, s2) -> s1.age - s2.age;
```
Sorting:
```java
Collections.sort(list, byName);
```

⸻

### 🚀 6️⃣ Importance in Ordered Collections (TreeSet, TreeMap)

TreeSet and TreeMap are sorted collections.

They require ordering rules, which come from either:

	1.	Comparable → natural ordering
	2.	Comparator → custom ordering

✔ Why important?

Because ordering decides:

	•	Where to place elements in a tree
	•	How to maintain BST structure
	•	Whether two elements are considered equal

✔ EXAMPLE (SUPER IMPORTANT)

### In TreeSet, equality is determined by compareTo() or compare(), NOT equals():

Keep compareTo/compare consistent with equals when using sorted collections.

```
Consistency rule					If a.equals(b) is true, then a.compareTo(b) should return 0
Why important						Otherwise, sorted collections may lose or skip elements
```
if (compare(x, y) == 0)

    they are considered duplicate by TreeSet

So:

	•	CompareTo() or Comparator defines sorting
	•	AND defines uniqueness

⸻


🚨 VERY IMPORTANT INTERVIEW POINT

✔ HashSet uses equals() & hashCode() to detect duplicates

✔ TreeSet uses compareTo() or compare() to detect duplicates

Meaning:

In TreeSet:

compare(a, b) == 0  → duplicates!

Even if equals() returns false.


⸻

⭐ 8️⃣ One-Line Interview Summary

Use Comparable for natural ordering defined inside a class; use Comparator for custom or multiple sorting outside the class.
Ordered collections like TreeSet and TreeMap rely entirely on Comparable/Comparator for sorting and determining duplicates.

⸻

# Thread safety in java collections

Normal collections like ArrayList, HashMap, and HashSet are not thread-safe, so multiple threads modifying them can corrupt data.

Java provides synchronized versions (like Vector, Hashtable, and Collections.synchronizedList()),

which make them thread-safe by locking the entire data structure, but this is slow due to full-object locking.

To solve this, Java introduced concurrent collections (like ConcurrentHashMap, CopyOnWriteArrayList, ConcurrentLinkedQueue, and BlockingQueue),

which provide thread safety using fine-grained locks or lock-free algorithms, making them safe and highly efficient in multi-threaded environments.
```java
List<Integer> list = new CopyOnWriteArrayList<>();
Map<String, Integer> map = new ConcurrentHashMap<>()
```

# Fail fast vs Fail safe

⭐ Fail-Fast vs Fail-Safe

### ✅ Fail-Fast Iterator

•	Found in normal collections like ArrayList, HashMap, HashSet.
•	If the collection is structurally modified while iterating

(add/remove outside iterator), it throws:

ConcurrentModificationException (CME)

•	Works on the original collection directly.
•	Uses a variable called modCount to detect changes.
Example (Fail-Fast):
```java
List<Integer> list = new ArrayList<>();
for (Integer i : list) {
    list.add(10);   // ❌ mod → CME
}
```
⸻

### ⭐ Fail-Safe Iterator

•	Found in concurrent collections like:
•	CopyOnWriteArrayList
•	ConcurrentHashMap
•	ConcurrentLinkedQueue
•	Does NOT throw CME.
•	Works on a separate cloned copy of the collection while iterating.
•	Structural changes do not affect iteration.

Example (Fail-Safe):
```java
CopyOnWriteArrayList<Integer> list = new CopyOnWriteArrayList<>();
for (Integer i : list) {
    list.add(10);   // ✔ No CME
}
```

⭐ Summary

Fail-Fast iterators throw ConcurrentModificationException if the collection is modified during iteration because they work on the original structure. Fail-Safe iterators do not throw exceptions because they work on a copy of the collection (like in ConcurrentHashMap or CopyOnWriteArrayList).


Here is a clean, crisp, interview-ready brush-up on object creation for different types of classes — exactly the essentials with simple examples.

⸻

# Object Creation for Different Types of Classes

We cover:

1️⃣ Singleton Class
2️⃣ Immutable Class
3️⃣ Anonymous Class
4️⃣ Inner Class
5️⃣ Static Inner Class
6️⃣ Nested Class
7️⃣ Final Class

⸻

✅ 1️⃣ Singleton Class

A Singleton class ensures only one object is ever created.

Steps to Create Singleton

1️⃣ Make constructor private

2️⃣ Create a static instance inside the class

3️⃣ Provide a public static method to return that instance

⸻

✅ 1️⃣ Normal Singleton (NOT Thread Safe)

✔ Definition

A simple singleton that ensures only one instance but breaks in multi-threading.

✔ Code

public class Singleton {
    private static Singleton instance;

    private Singleton() { }

    public static Singleton getInstance() {
        if (instance == null) {            // ❌ Not thread safe
            instance = new Singleton();
        }
        return instance;
    }
}

✔ Issue

If two threads call getInstance() at the same time → two instances can be created.

⸻

✅ 2️⃣ Thread-Safe Singleton (Synchronized Method)

✔ Definition

Synchronize getInstance() so only one thread enters at a time.

✔ Code

public class Singleton {
    private static Singleton instance;

    private Singleton() { }

    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}

✔ Pros
	•	100% thread-safe

❌ Cons
	•	Slow — every call to getInstance() acquires a lock
	•	Unnecessary locking after object is created

⸻

✅ 3️⃣ Thread-Safe AND Fast (Double-Checked Locking + volatile)

(Most common interview answer)

✔ Definition

Avoid locking once the instance is created → fast & thread-safe.

✔ Code

public class Singleton {
    private static volatile Singleton instance; // volatile required!

    private Singleton() { }

    public static Singleton getInstance() {
        if (instance == null) {                // First check (no lock)
            synchronized (Singleton.class) {
                if (instance == null) {        // Second check (with lock)
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}

✔ Why volatile?

Prevents instruction reordering — ensures the object is fully constructed before assignment.

✔ Pros

	•	Thread-safe
	•	Fast after first initialization
	•	Industry-standard answer

⸻

✅ 4️⃣ Best & Simplest: Enum Singleton (Recommended by Joshua Bloch)

✔ Definition

Enum guarantees one instance, thread safety, and protects from serialization attacks.

✔ Code

public enum Singleton {
    INSTANCE;
}

✔ Usage

Singleton obj = Singleton.INSTANCE;

✔ Pros

	•	Thread-safe automatically
	•	Serialization-safe
	•	Reflection-safe
	•	Cleanest solution

⸻

✅ 2️⃣ Immutable Class

✔ Definition

A class whose state cannot change after creation.

✔ How object is created?

Simply using new or static factory method.

✔ Example

final class Employee {
    private final String name;
    Employee(String name) { this.name = name; }
    public String getName() { return name; }
}

Employee e = new Employee("Akshith");

✔ No setters
✔ Fields are final
✔ Object created normally

⸻

✅ 3️⃣ Anonymous Class

✔ Definition

A class without a name created on the spot.

✔ Object Creation

Created using new + interface/class.

✔ Example

Runnable r = new Runnable() {
    @Override
    public void run() {
        System.out.println("Running...");
    }
};

Object is created immediately — no class name needed.

⸻

✅ 4️⃣ Inner Class (Non-static Inner Class)

✔ Definition

A class defined inside another class, requiring an instance of outer class.

✔ Object Creation

Outer outer = new Outer();
Outer.Inner inner = outer.new Inner();

✔ Important

Cannot create inner class object without the outer class object.

⸻

✅ 5️⃣ Static Inner Class

✔ Definition

A static nested class inside another class.
Does not require outer class object.

✔ Object Creation

Outer.StaticInner obj = new Outer.StaticInner();

Looks like a nested top-level class.

⸻

✅ 6️⃣ Nested Class (General Term)

A nested class means any class inside another class:
	•	Static inner class
	•	Non-static inner class
	•	Anonymous class
	•	Local class

✔ Example of a Local Nested Class

void method() {
    class LocalClass { }
    LocalClass obj = new LocalClass();
}


⸻

✅ 7️⃣ Final Class

✔ Definition

A class that cannot be extended.

✔ Object Creation

Same as normal class — use new.

✔ Example

final class Vehicle { }

Vehicle v = new Vehicle();   // valid

Final only prevents subclassing — not object creation.

⸻

