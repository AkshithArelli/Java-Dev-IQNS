**SOLID principles**, 

which are five key design principles in object-oriented programming that make your code more maintainable, readable, and scalable. We’ll cover:

The SOLID principles are a set of five design guidelines in object-oriented programming that help developers create more maintainable, flexible, and scalable software. Each letter in "SOLID" stands for a different principle:

1. **S - Single Responsibility Principle (SRP):**
   - A class should have only one reason to change, meaning it should have only one job or responsibility.
   - This makes classes easier to understand, maintain, and modify.

2. **O - Open/Closed Principle (OCP):**
   - Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification.
   - You should be able to add new functionality without changing existing code, usually achieved via interfaces or abstract classes.

3. **L - Liskov Substitution Principle (LSP):**
   - Objects of a superclass should be able to be replaced with objects of a subclass without affecting the correctness of the program.
   - Subclasses should honor the contract of their parent class.

4. **I - Interface Segregation Principle (ISP):**
   - No client should be forced to depend on methods it does not use.
   - It’s better to have many small, specific interfaces than a single large, general-purpose one.

5. **D - Dependency Inversion Principle (DIP):**
   - High-level modules should not depend on low-level modules; both should depend on abstractions.
   - Abstractions should not depend on details; details should depend on abstractions.

These principles help in building systems that are easy to maintain, extend, and refactor.




---

### 1. SRP – Single Responsibility Principle

✅ Simple Definition:

> A class should have only one reason to change.
(Meaning: it should do only one job.)




---

❌ Bad Example:
```java
class Report {
    public String getReportData() {
        return "Report data";
    }

    public void saveToFile(String data) {
        System.out.println("Saving to file: " + data);
    }

    public void printReport(String data) {
        System.out.println("Printing: " + data);
    }
}
```
❌ Why is this bad?

The Report class has 3 responsibilities:

Creating the report

Saving it

Printing it
➡ So it has 3 reasons to change (if printing logic changes, if file saving changes, etc.)




---

✅ Good Example:
```java
class Report {
    public String getReportData() {
        return "Report data";
    }
}

class ReportPrinter {
    public void print(String data) {
        System.out.println("Printing: " + data);
    }
}

class ReportSaver {
    public void save(String data) {
        System.out.println("Saving to file: " + data);
    }
}
```
✅ Why is this good?

Each class does one job only.

Report changes if data generation changes.

Printer changes if printing format changes.

Saver changes if file format/location changes.



---

### 2. OCP – Open Closed Principle

✅ Simple Definition:

> Classes should be open for extension but closed for modification.
(Meaning: You should be able to add new behavior without changing existing code.)




---

❌ Bad Example:
```java
class PaymentProcessor {
    public void process(String paymentType) {
        if (paymentType.equals("credit")) {
            System.out.println("Processing credit card");
        } else if (paymentType.equals("paypal")) {
            System.out.println("Processing PayPal");
        }
    }
}
```
❌ Why is this bad?

If you add a new payment method (like UPI), you have to modify the class.

Every new feature breaks the existing class logic.



---

✅ Good Example:
```java
interface PaymentMethod {
    void pay();
}

class CreditCardPayment implements PaymentMethod {
    public void pay() {
        System.out.println("Processing credit card");
    }
}

class PayPalPayment implements PaymentMethod {
    public void pay() {
        System.out.println("Processing PayPal");
    }
}

class PaymentProcessor {
    public void process(PaymentMethod method) {
        method.pay();
    }
}
```
✅ Why is this good?

To add UPI or Bitcoin, just create a new class that implements PaymentMethod.

No need to touch existing code — just extend it!


---

### 3. LSP – Liskov Substitution Principle

Definition:

> A child class should be able to replace its parent class without breaking the application.

 It ensures that a child class can be used in place of its parent class without altering the correctness of the program.

❌ Bad Example:
```java
class Bird {
    void fly() {
        System.out.println("Bird is flying");
    }
}

class Ostrich extends Bird {
    @Override
    void fly() {
        throw new UnsupportedOperationException("Ostrich can't fly");
    }
}
```
❌ Why is this bad?



You can’t substitute Ostrich for Bird — calling fly() breaks things, even though Ostrich is a Bird.

✅ Good Example:
```java
interface Bird {
    void layEggs();
}

interface FlyingBird extends Bird {
    void fly();
}

class Sparrow implements FlyingBird {
    public void fly() {
        System.out.println("Sparrow flies");
    }

    public void layEggs() {
        System.out.println("Sparrow lays eggs");
    }
}

class Ostrich implements Bird {
    public void layEggs() {
        System.out.println("Ostrich lays eggs");
    }
}
```
✅ Why is this good?



Now, Sparrow and Ostrich follow their correct capabilities — no broken behavior.


---


### 4. ISP – Interface Segregation Principle

✅ Simple Definition:

> Don't force a class to implement methods it doesn’t use. Split big interfaces into smaller, specific ones.



❌ Bad Example:
```java
interface Worker {
    void work();
    void eat();
}

class Robot implements Worker {
    public void work() {
        System.out.println("Robot working");
    }

    public void eat() {
        // Robot doesn't eat!
        throw new UnsupportedOperationException();
    }
}
```
❌ Why is this bad?

Robot is forced to implement eat() even though it doesn’t need it.

✅ Good Example:
```java
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

class Human implements Workable, Eatable {
    public void work() {
        System.out.println("Human working");
    }

    public void eat() {
        System.out.println("Human eating");
    }
}

class Robot implements Workable {
    public void work() {
        System.out.println("Robot working");
    }
}
```
✅ Why is this good?

Each class only implements what it actually needs.


---

🔁 Summary:

Principle	Simple Meaning	One-liner

LSP	Subclasses should behave like their parent	Don’t surprise the code with wrong child behavior
ISP	Don’t make classes implement things they don’t need	Split large interfaces into small ones

---

### 5. Dependency Inversion Principle (DIP)

✅ Simple Definition:

> High-level classes should not depend on low-level classes. Both should depend on abstractions (interfaces).



Also:

> Abstractions should not depend on details. Details should depend on abstractions.




---

🧠 Think of it like:

> A remote (high-level) shouldn't care which TV brand (low-level) it is turning on — as long as that TV follows a common interface.




---

❌ Bad Example (Tightly Coupled):
```java
class LightBulb {
    public void turnOn() {
        System.out.println("Light is on");
    }
}

class Switch {
    private LightBulb bulb = new LightBulb(); // tightly depends on concrete class

    public void operate() {
        bulb.turnOn();
    }
}
```
❌ Why is this bad?

Switch depends on a specific LightBulb class.

If you change the bulb type, you must modify the Switch class.



---

✅ Good Example (Using Abstraction):
```java
interface Switchable {
    void turnOn();
}

class LightBulb implements Switchable {
    public void turnOn() {
        System.out.println("Light is on");
    }
}

class Fan implements Switchable {
    public void turnOn() {
        System.out.println("Fan is on");
    }
}

class Switch {
    private Switchable device;

    public Switch(Switchable device) {
        this.device = device;
    }

    public void operate() {
        device.turnOn();
    }
}




public class Main {
    public static void main(String[] args) {
        // Create a LightBulb and a Switch for it
        Switchable bulb = new LightBulb();
        Switch lightSwitch = new Switch(bulb);
        lightSwitch.operate(); // Output: Light is on

        // Create a Fan and a Switch for it
        Switchable fan = new Fan();
        Switch fanSwitch = new Switch(fan);
        fanSwitch.operate(); // Output: Fan is on
    }
}
```
✅ Why is this good?

Switch depends on the Switchable interface, not the concrete class.

You can plug in LightBulb, Fan, or anything else without changing Switch.



---
. High-level: Abstract, policy, orchestration, business logic.
. Low-level: Concrete, implementation, mechanism, utility.
- How to Tell the Difference
### High-level class:

- Focuses on what needs to be done (the "what"), not how it's done.
- Uses or coordinates other classes to achieve a goal.
- Example: A class called OrderService that processes orders by using other components like payment gateways or inventory managers.
### Low-level class:

- Focuses on how something is done (the "how").
- Handles specific, detailed tasks, often reusable and generic.
- Example: A class called DatabaseConnection that knows how to connect to a specific database.

🔁 Summary Table:

Term	Meaning

High-level module	Logic controller (like Switch)
Low-level module	Concrete worker (like LightBulb, Fan)
DIP goal	Connect them using interfaces, not directly



---

🔌 Real-life Analogy:

Charger (Switch) connects to a phone or laptop (LightBulb/Fan) using a universal USB port (interface).

If the charger only worked with iPhones, it would violate DIP.

Supporting USB = following DIP.
