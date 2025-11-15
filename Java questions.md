# 1. OOPs

✅ OOPS Concepts (Object-Oriented Programming) – Interview Explanation

There are four main pillars of OOPS:

	1.	Encapsulation
	2.	Abstraction
	3.	Inheritance
	4.	Polymorphism

I’ll explain each the way you should speak in an interview + small Java examples.

⸻

⭐ 1. Encapsulation

Interview Answer:

Encapsulation means wrapping data and methods into a single unit (a class) and restricting direct access to the data using access modifiers.

It protects data from unintended modifications.

Simple Example
```java
class BankAccount {
    private double balance; // hidden data

    public void deposit(double amount) {
        balance += amount;
    }

    public double getBalance() {
        return balance;
    }
}
```
👉 balance is private → cannot access directly → controlled using getter/setter.

🎯 Key point to mention: “Encapsulation ensures data protection.”

	•	Data lives inside the class
	•	Only methods inside the class can modify it

This is literally the meaning of encapsulation:

“Wrapping data + methods together as one unit”

⸻

⭐ 2. Abstraction

Interview Answer:

Abstraction means showing only the essential details and hiding unnecessary internal complexity.

Example
```java
abstract class Payment {
    abstract void pay();
}

class CreditCardPayment extends Payment {
    void pay() {
        System.out.println("Payment done using credit card");
    }
}
```

```java
Payment payment = new CreditCardSystem();
payment.pay();
```
You don’t need to know how the credit card works internally.

🎯 Key point: “Abstraction reduces complexity and focuses on what an object does, not how it does it.”

Think of it as:
👉 “I give you the function, but I hide the internal mechanics.”

You only see the idea of paying, not the details

⸻

⭐ 3. Inheritance

Interview Answer:

Inheritance allows one class (child/subclass) to acquire properties and methods of another class (parent/superclass).

It promotes code reusability.

Example
```java
class Animal {
    void eat() {
        System.out.println("Animal eats");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Dog barks");
    }
}
```
👉 Dog gets eat() method without writing it again.
🎯 Key point: “Inheritance helps with reusability and hierarchical relationships.”

⸻

⭐ 4. Polymorphism

Interview Answer:

Polymorphism means same method name, different behavior.

There are two types:

	•	Compile-time polymorphism → Method Overloading
	•	Runtime polymorphism → Method Overriding

⸻

Compile-time Polymorphism (Overloading)
```java
class Calculator {
    int add(int a, int b) { return a+b; }
    int add(int a, int b, int c) { return a+b+c; }
}
```

⸻

Runtime Polymorphism (Overriding)
```java
class Animal {
    void sound() { System.out.println("Animal makes sound"); }
}

class Dog extends Animal {
    @Override
    void sound() { System.out.println("Dog barks"); }
}
```
🎯 Key point: “Polymorphism provides flexibility to use one interface with multiple implementations.”

⸻

🔥 Bonus: Additional OOPS Concepts Interviewers Ask

5. Class vs Object
   
	•	Class → Blueprint
	•	Object → Instance of class

⸻

6. Interface
   
	•	100% abstraction (before Java 8)
	•	Can have default and static methods
```java
interface Vehicle {
    void start();
}
```

⸻

7. Abstract Class
   
	•	Cannot be instantiated
	•	Can contain abstract + concrete methods
	•	Used for partial abstraction

⸻

8. Constructor
   
	•	Special method used to initialize objects
	•	Same name as class
	•	No return type

⸻

9. Method Overloading vs Overriding
```
Feature	                 Overloading	    Overriding
Runtime/Compile time	   Compile-time	    Runtime
Parameters	             Different	      Same
Class relation	         Same class	      Parent-child
```

⸻

🎯 How to give a perfect OOPS interview answer

When interviewer asks “Explain OOPS”, say:

“OOPS has four major pillars — Encapsulation, Abstraction, Inheritance, and Polymorphism.”
Then explain each in one line, with example if required.


-------------

# 2. Exception Hierarchy
```
java.lang.Object
     ↓
  Throwable
   ├── Error (unchecked)
   │      ├── OutOfMemoryError
   │      ├── StackOverflowError
   │      └── etc...
   └── Exception
          ├── RuntimeException (unchecked)
          │       ├── NullPointerException
          │       ├── ArithmeticException
          │       ├── IllegalArgumentException
          │       └── ArrayIndexOutOfBoundsException
          └── Checked Exceptions
                  ├── IOException
                  ├── SQLException
                  ├── ParseException
                  └── ClassNotFoundException
```

- Exception hierarchy starts from Throwable class.

- Throwable has two children: Error and Exception.

- Errors are unrecoverable and represent issues outside application control.

- Exceptions are recoverable and divided into 

1. Checked exceptions (must be handled) 

These are exceptions that the compiler forces you to handle.

You MUST either:

	•	use try-catch
	•	or throws in method signature

Examples:

	•	IOException
	•	SQLException
	•	FileNotFoundException
	•	ClassNotFoundException


2. Unchecked exceptions (RuntimeException).

These happen during execution due to programming errors. Compiler does NOT force you to catch them.

Examples:

	•	NullPointerException
	•	ArithmeticException
	•	ArrayIndexOutOfBoundsException
	•	IllegalArgumentException
	•	ClassCastException

## Custom Exceptions

A custom exception is a user-defined exception that helps represent business-specific errors more clearly.
We create custom checked exceptions by extending Exception and custom unchecked exceptions by extending RuntimeException.”


A custom exception is an exception you define yourself when Java’s built-in exceptions are not enough for your business logic.

For example:

	•	“InsufficientBalanceException”
	•	“InvalidAgeException”
	•	“UnauthorizedUserException”
	•	“OrderNotFoundException”

They make your code more meaningful, readable, and domain-specific.

⸻

🟦 Types of Custom Exceptions

You can create:

1️⃣ Custom Checked Exceptions

Extend Exception.

2️⃣ Custom Unchecked Exceptions

Extend RuntimeException.

⸻

🟦 1. Custom Checked Exception

👉 Use when caller must handle the exception

(either try-catch or throws)

Example: InvalidAgeException
```java
class InvalidAgeException extends Exception {
    public InvalidAgeException(String message) {
        super(message);
    }
}

//Use it:

public void register(int age) throws InvalidAgeException {
    if (age < 18) {
        throw new InvalidAgeException("Age must be 18 or above");
    }
}
```

⸻

🟩 2. Custom Unchecked Exception

👉 Use when the exception is caused by programming errors

(no need to force try-catch)

Extend RuntimeException.
```java
class InsufficientBalanceException extends RuntimeException {
    public InsufficientBalanceException(String message) {
        super(message);
    }
}

//Use it:

public void withdraw(double amount) {
    if (amount > balance) {
        throw new InsufficientBalanceException("Not enough balance");
    }
}
```

⸻

🟧 Checked vs Unchecked Custom Exceptions
```
Type Extend			Must be caught?		When to use
Checked	Exception	Yes (compile-time)	Expected business rule failures
Unchecked			RuntimeException	No	Code bugs, invalid input, illegal states
```

⸻

## Why do we need to use super(message); in custom exceptions?


Because Exception, RuntimeException, and Throwable (the parent classes) already have a constructor that accepts an error message.

Example from Java source:
```java
public Throwable(String message) {
    this.detailMessage = message;
}
```
So when you write:

super(message);

You are sending your custom message up to the parent class, so that:

✔ The exception stores the message

✔ The message appears in the logs

✔ getMessage() returns your message

✔ Stack trace shows helpful information

⸻

🟦 Without super(message) — the exception message becomes NULL

Example:
```
class MyException extends RuntimeException {
    public MyException() {
        // NO super(message)
    }
}
```
Using it:
```java
throw new MyException("Something went wrong");
```
Output:
```
MyException: null
```
❌ No message
❌ Hard to debug
❌ Useless in logs and monitoring

⸻

🟩 With super(message) — message is preserved
```java
class MyException extends RuntimeException {
    public MyException(String message) {
        super(message);
    }
}
```
Now:
```java
throw new MyException("Something went wrong");
```
Output:
```
MyException: Something went wrong
```
✔ Message is visible
✔ Debugging becomes easy
✔ Logs become meaningful

⸻

🧠 Interview-Friendly Explanation

We use super(message) to pass our custom error message to the parent Exception class. This allows the exception to store the message, display it in the stack trace, and retrieve it through getMessage(). Without it, the message will be lost.

⸻

🔥 Bonus: super(cause) and super(message, cause)

Exception classes allow:
```java
super(cause);           // chain exception
super(message, cause);  // message + root cause
```
These help track real underlying failures.

---------------

# 3. try-with-resources

try-with-resources is a Java feature that automatically closes resources (like files, DB connections, sockets, streams) after their usage — without requiring a finally block.

A resource is anything that implements the interface:

AutoCloseable


⸻

🚫 Old way (before Java 7): Manual closing

Using a file:
```java
BufferedReader br = null;
try {
    br = new BufferedReader(new FileReader("data.txt"));
    System.out.println(br.readLine());
} catch (IOException e) {
    e.printStackTrace();
} finally {
    try {
        if (br != null) br.close();   // must close manually
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```
Problems:

❌ Long code
❌ Easy to forget closing
❌ Possible memory/resource leaks

⸻

✅ New way (after Java 7): try-with-resources
```java
try (BufferedReader br = new BufferedReader(new FileReader("data.txt"))) {
    System.out.println(br.readLine());
} catch (IOException e) {
    e.printStackTrace();
}
```
Advantages:

✔ Resource automatically closed
✔ No need for finally block
✔ Cleaner code
✔ Prevents resource leaks
✔ More readable

⸻

🧠 How it works internally?

At the end of the try block, Java automatically calls:
```java
br.close();
``
* because BufferedReader implements:
```
public interface Closeable extends AutoCloseable
```
* So anything implementing AutoCloseable works with try-with-resources.

⸻

🎯 Interview-level Summary

“Try-with-resources is a Java feature that automatically closes resources at the end of a try block. Any class implementing AutoCloseable can be used. This avoids memory leaks and makes code cleaner compared to the old try-catch-finally approach.”

⸻

⭐ Extra Points for Interviews

1. Resources are closed in reverse order

Last opened → closed first.

2. Works with custom resources

You can create your own class:
```java
class MyResource implements AutoCloseable {
    @Override
    public void close() {
        System.out.println("Closed automatically");
    }
}
```
Then:
```java
try (MyResource r = new MyResource()) {
    // use resource
}
```
3. Finally block is not required

4. Less error-prone → prevents resource leaks


------------------

# 4. Constants vs Enums

Here is a clear and interview-friendly explanation of Enums vs Constants in Java:

⸻

✅ Enums vs Constants in Java

👉 Constants are simple variable values with no enforcement or behavior.

👉 Enums are powerful, type-safe, self-contained classes that represent a fixed set of related values.
⸻

🔍 Practical Example (Why Enums are Better)

❌ Using constants:
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

⸻

✅ Using enums:
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

⸻

🔥 Enums with Behavior

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

⸻

🧠 When to Use What?

✔ Use ENUM when:

	•	Values are fixed (states, directions, types)
	•	You want type safety
	•	You need to attach behavior
	•	You want clean and readable domain models

✔ Use CONSTANTS when:

	•	It’s a simple number/string used rarely
	•	Behavior is not required
	•	No need to enforce fixed acceptable values

⸻

Enums are preferred in modern Java for all domain-specific fixed categories.

----------

# 5. Fail-Fast vs Fail-Safe

1. Fail-Fast Iterator

A fail-fast iterator immediately throws an exception if the underlying collection is structurally modified while iterating.

🔥 Example collections (fail-fast)

	•	ArrayList
	•	HashMap
	•	LinkedList
	•	HashSet
	•	Vector (iterator)
	•	Most collections from java.util package

🔥 Behavior

If the collection is modified (add/remove) during iteration:

👉 ConcurrentModificationException is thrown.

✔ Why?

Fail-fast iterators use a variable called modCount.

Every structural modification changes the modCount.

Iterator compares expectedModCount with modCount. If mismatch → throw exception.

⸻

❌ Fail-Fast Example
```java
List<String> list = new ArrayList<>();
list.add("A");
list.add("B");
list.add("C");

for (String s : list) {          // iterator internally
    list.add("D");               // modification during iteration
}
```
👉 This throws ConcurrentModificationException.

⸻

2. Fail-Safe Iterator

Fail-safe iterators do NOT throw exceptions if the collection is modified during iteration.

🔥 Example collections (fail-safe)

Collections from java.util.concurrent package:

	•	CopyOnWriteArrayList
	•	ConcurrentHashMap
	•	ConcurrentSkipListMap
	•	ConcurrentSkipListSet

🔥 Behavior

Fail-safe iterators work on a clone (copy) of the collection.

👉 Modifying the original collection does not affect the iterator.
👉 No ConcurrentModificationException.

⸻

✔ Fail-Safe Example
```java
CopyOnWriteArrayList<String> list = new CopyOnWriteArrayList<>();
list.add("A");
list.add("B");
list.add("C");

for (String s : list) {
    list.add("D");   // allowed
}
```
👉 No exception.
👉 Iterator reads old snapshot; new values appear after iteration ends.

⸻

🔥 Key Differences (Interview Table)
```
Feature								Fail-Fast										Fail-Safe
Behavior on modification			Throws ConcurrentModificationException			No exception
Works on							Actual collection								Copy (snapshot)
Memory usage						Low												High (copy created)
Speed								Faster											Slower
Example collections					ArrayList, HashMap, HashSet						CopyOnWriteArrayList, ConcurrentHashMap
Thread-safety						Not thread-safe									Thread-safe
Iterator type						Fail-fast iterator								Snapshot iterator
```

⸻

🧠 Why do they exist?

Fail-Fast

	•	Detect inconsistent modifications quickly.
	•	Prevents unpredictable behavior.

Fail-Safe

	•	Useful for concurrent environments.
	•	Allows iteration while modifications happen.

⸻

🎯 Interview Summary (Best Answer)

Fail-fast iterators throw ConcurrentModificationException if the collection is structurally modified during iteration because they work on the actual collection. Fail-safe iterators do not throw exceptions because they work on a cloned snapshot of the collection. Fail-safe is used in concurrent collections, while fail-fast is used in regular collections.


⸻

### ✅ Abstract Class vs Interface (Concise + Example)

Abstract Class

	•	Can have abstract + concrete methods
	•	Can have instance variables
	•	Can have constructors
	•	Single inheritance
	•	Used when objects share common behavior + partial implementation

Example
```java
abstract class Animal {
    void eat() { System.out.println("Eating..."); }   // concrete method
    abstract void sound();                           // abstract method
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Bark");
    }
}
```

⸻

Interface

	•	Only abstract methods (until Java 8)
	•	Can have default & static methods
	•	Only public static final constants
	•	No constructors
	•	Supports multiple inheritance
	•	Used when classes just need to follow a contract

Example
```java
interface Vehicle {
    void start();                 // implicitly abstract
}

class Car implements Vehicle {
    @Override
    public void start() {
        System.out.println("Car starting...");
    }
}
```

⸻

🧠 One-line memory trick

	•	Interface = “What to do” (no implementation).
	•	Abstract class = “What + partial how to do” (some implementation).

⸻
