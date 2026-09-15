# Interfaces — Python and Java

## Video Goal

In this lesson, we introduce **interfaces** and explain why Java has an explicit `interface` construct while Python generally does not.

The key ideas are:

* What an interface is
* An interface as a **contract**
* Why Java has explicit interfaces
* Why Python generally does not need a traditional interface construct
* Implementing an interface in Java
* The Python equivalent using **duck typing**
* Using interfaces with composition
* Using interfaces with inheritance
* Why interfaces make programs more flexible
* When an interface is actually useful

> **Key idea:**
> An interface describes **what an object can do** without necessarily specifying **how it does it**.

---

# 1. The Problem Interfaces Solve

Suppose we have a program that needs to send notifications.

We might have:

```text
Email
SMS
Push Notification
```

All of these can perform the same basic operation:

```text
send()
```

But the implementations are completely different.

An email might use an SMTP server.

An SMS might use a telecommunications API.

A push notification might use Apple's or Google's notification service.

Conceptually:

```text
             send()
                │
       ┌────────┼────────┐
       │        │        │
      Email     SMS     Push
```

We want our program to be able to say:

> "I need something that can send a notification."

without having to care exactly what kind of notification system it is.

That is where an interface is useful.

---

# 2. What Is an Interface?

An interface defines a **contract**.

For example:

```text
Any NotificationSender must provide:

    send(message)
```

The interface does not necessarily care how `send()` works.

It establishes what the object promises to provide.

Think of an interface as:

```text
             CONTRACT
                 │
                 ▼
          must provide:
              send()
                 │
       ┌─────────┼─────────┐
       ▼         ▼         ▼
     Email      SMS       Push
```

Each implementation fulfills the contract differently.

---

# 3. Why Does Java Have Interfaces?

Java is a **statically typed** language.

The compiler needs to know what operations are guaranteed to be available on a particular type.

Java therefore provides an explicit language construct:

```java
interface
```

For example:

```java
interface NotificationSender {
    void send(String message);
}
```

This says:

> Any class implementing `NotificationSender` must provide a `send(String)` method.

The interface specifies the contract.

It does not need to specify how the message is actually sent.

---

# 4. Implementing an Interface in Java

A class uses the `implements` keyword.

```java
class EmailSender implements NotificationSender {

    @Override
    public void send(String message) {
        System.out.println("Sending email: " + message);
    }
}
```

Another class can implement the same interface:

```java
class SmsSender implements NotificationSender {

    @Override
    public void send(String message) {
        System.out.println("Sending SMS: " + message);
    }
}
```

Now both classes satisfy the same contract:

```text
NotificationSender
       │
       │
   ┌───┴────┐
   ▼        ▼
 Email     SMS
```

But they implement `send()` differently.

---

# 5. Using the Interface

We can now write code that works with the interface rather than a specific implementation.

```java
NotificationSender sender;

sender = new EmailSender();
sender.send("Hello!");

sender = new SmsSender();
sender.send("Hello!");
```

The important part is that the calling code only needs to know:

```text
NotificationSender
```

It does not need to know:

```text
EmailSender
SmsSender
```

This is a major benefit of interfaces.

---

# 6. Interfaces and Composition

This is where interfaces become especially powerful.

Remember composition:

```text
Car HAS-A Engine
```

Instead of making `Car` depend on one specific engine implementation, we can make it depend on an interface.

For example:

```java
interface Engine {
    void start();
}
```

Now we can have:

```java
class GasEngine implements Engine {

    @Override
    public void start() {
        System.out.println("Gas engine starting.");
    }
}
```

and:

```java
class ElectricEngine implements Engine {

    @Override
    public void start() {
        System.out.println("Electric motor starting.");
    }
}
```

The `Car` uses the interface:

```java
class Car {

    private Engine engine;

    Car(Engine engine) {
        this.engine = engine;
    }

    void start() {
        engine.start();
    }
}
```

Now we can create:

```java
Car gasCar =
    new Car(new GasEngine());

Car electricCar =
    new Car(new ElectricEngine());
```

Both cars use the same `Car` class.

The difference is which object was composed into the car.

```text
                    Car
                     │
                 has an
                     │
                     ▼
                   Engine
                  /      \
                 /        \
                ▼          ▼
          GasEngine    ElectricEngine
```

This is one of the most important practical uses of interfaces.

---

# 7. Why Is This Better?

Imagine the `Car` were written like this:

```java
class Car {

    private GasEngine engine;

    Car() {
        engine = new GasEngine();
    }
}
```

Now `Car` is tightly coupled to `GasEngine`.

If we want an electric car, we have to change the `Car` class.

With the interface:

```java
class Car {

    private Engine engine;

    Car(Engine engine) {
        this.engine = engine;
    }
}
```

the `Car` doesn't care what kind of engine it receives.

It only cares that the object satisfies:

```java
Engine
```

This makes the design more flexible.

---

# 8. Python Doesn't Traditionally Have Java-Style Interfaces

Python does not require a class to explicitly declare:

```text
implements Interface
```

Python is dynamically typed and commonly uses **duck typing**.

The idea is:

> If an object provides the operations I need, I can use it.

The famous informal explanation is:

> "If it walks like a duck and quacks like a duck, treat it like a duck."

For example:

```python
class EmailSender:

    def send(self, message):
        print("Sending email:", message)


class SmsSender:

    def send(self, message):
        print("Sending SMS:", message)
```

Neither class explicitly says:

```text
implements NotificationSender
```

But both provide:

```python
send(message)
```

Therefore:

```python
def notify(sender, message):
    sender.send(message)
```

works with either:

```python
notify(EmailSender(), "Hello!")
notify(SmsSender(), "Hello!")
```

Python determines whether the object supports the operation when the code runs.

---

# 9. Python's Duck Typing

The important Python concept is:

```python
def notify(sender, message):
    sender.send(message)
```

The function does not need to know the exact class of `sender`.

It only needs `sender` to provide:

```python
send()
```

Conceptually:

```text
                notify()
                   │
             needs send()
                   │
          ┌────────┴────────┐
          ▼                 ▼
    EmailSender         SmsSender
       send()              send()
```

Python cares primarily about what the object **can do**.

Java can also program this way through interfaces, but Java makes the contract explicit and compiler-checked.

---

# 10. Python Can Define an Explicit Interface-Like Contract

Modern Python also provides tools for expressing interfaces more explicitly.

One option is `typing.Protocol`.

```python
from typing import Protocol


class NotificationSender(Protocol):

    def send(self, message: str) -> None:
        ...
```

Now:

```python
class EmailSender:

    def send(self, message: str) -> None:
        print("Sending email:", message)
```

and:

```python
class SmsSender:

    def send(self, message: str) -> None:
        print("Sending SMS:", message)
```

can satisfy the protocol structurally.

The important distinction is that the classes do not need to explicitly inherit from `NotificationSender`.

Static type checkers can determine that they satisfy the protocol because they provide the required method.

For an introductory OOP course, the important takeaway is:

```text
Java
    explicit interface

Python
    duck typing
    optionally Protocol for static type checking
```

---

# 11. Interfaces and Inheritance Are Different

Don't confuse these concepts.

Inheritance:

```text
Dog IS-A Animal
```

```java
class Dog extends Animal
```

This creates a parent/child relationship.

An interface:

```text
Dog CAN perform Animal-like behavior
```

or:

```text
EmailSender CAN send notifications
```

does not necessarily mean that the implementing class is a specialized version of the interface.

For example:

```java
interface Flyable {
    void fly();
}
```

We could have:

```java
class Bird implements Flyable {
    public void fly() {
        System.out.println("Bird flying.");
    }
}
```

and:

```java
class Airplane implements Flyable {
    public void fly() {
        System.out.println("Airplane flying.");
    }
}
```

A bird and an airplane are obviously very different things.

But both can satisfy the same capability:

```text
CAN FLY
```

This is one reason interfaces are so useful.

---

# 12. Interfaces Can Be Used With Inheritance

Java allows a class to both:

* extend a superclass
* implement one or more interfaces

For example:

```java
class Animal {
    void eat() {
        System.out.println("Eating.");
    }
}
```

An interface:

```java
interface Flyable {
    void fly();
}
```

A subclass:

```java
class Bird extends Animal implements Flyable {

    @Override
    public void fly() {
        System.out.println("Bird flying.");
    }
}
```

Now:

```text
          Animal
             ▲
             │
           Bird
             │
             │ implements
             ▼
          Flyable
```

The relationships mean different things:

```text
Bird IS-A Animal

Bird CAN FLY
```

Inheritance describes the type relationship.

The interface describes a capability/contract.

---

# 13. Multiple Interfaces

Java allows a class to implement multiple interfaces.

For example:

```java
interface Flyable {
    void fly();
}

interface Swimmable {
    void swim();
}
```

A class can implement both:

```java
class Duck implements Flyable, Swimmable {

    @Override
    public void fly() {
        System.out.println("Duck flying.");
    }

    @Override
    public void swim() {
        System.out.println("Duck swimming.");
    }
}
```

The Duck satisfies both contracts.

```text
             Duck
            /    \
           ▼      ▼
       Flyable  Swimmable
```

This is another important difference from ordinary class inheritance in Java:

> A Java class can extend one superclass but can implement multiple interfaces.

---

# 14. A Concrete Composition Example

Consider a `Report` object that needs to save its output.

We might have several possible storage systems:

```text
File
Database
Cloud Storage
```

All of them can provide:

```text
save()
```

Define the interface:

```java
interface Storage {
    void save(String data);
}
```

Implement it:

```java
class FileStorage implements Storage {

    @Override
    public void save(String data) {
        System.out.println("Saving to file.");
    }
}
```

Another implementation:

```java
class DatabaseStorage implements Storage {

    @Override
    public void save(String data) {
        System.out.println("Saving to database.");
    }
}
```

Now the `Report` uses composition:

```java
class Report {

    private Storage storage;

    Report(Storage storage) {
        this.storage = storage;
    }

    void save() {
        storage.save("Report contents");
    }
}
```

We can choose the implementation:

```java
Report fileReport =
    new Report(new FileStorage());

Report databaseReport =
    new Report(new DatabaseStorage());
```

The `Report` doesn't need to know how storage works.

It only knows:

```text
Storage
    ↓
save()
```

This is a very common real-world use of interfaces.

---

# 15. The Same Idea in Python

Python doesn't need a Java-style interface for this.

We can simply define classes that provide the expected method.

```python
class FileStorage:

    def save(self, data):
        print("Saving to file.")


class DatabaseStorage:

    def save(self, data):
        print("Saving to database.")
```

Then:

```python
class Report:

    def __init__(self, storage):
        self.storage = storage

    def save(self):
        self.storage.save("Report contents")
```

Use either implementation:

```python
file_report = Report(FileStorage())
database_report = Report(DatabaseStorage())
```

The composition relationship is:

```text
             Report
                │
              HAS-A
                │
             Storage
            /       \
           ▼         ▼
     FileStorage  DatabaseStorage
```

Python relies on duck typing:

> If the object provides `save()`, `Report` can use it.

---

# 16. Why Interfaces Are Valuable

Interfaces provide **decoupling**.

Without an interface:

```text
Report ─────────► DatabaseStorage
```

The `Report` is tied to one implementation.

With an interface:

```text
                    Storage
                       ▲
                       │
             ┌─────────┴─────────┐
             │                   │
             ▼                   ▼
       FileStorage        DatabaseStorage
             ▲                   ▲
             └─────────┬─────────┘
                       │
                     Report
```

`Report` depends on the contract rather than a specific implementation.

That means we can replace:

```text
FileStorage
```

with:

```text
DatabaseStorage
```

without changing the basic `Report` design.

---

# 17. The Big Difference Between Java and Python

This is the key comparison for Python programmers.

### Java

Java can explicitly declare:

```java
interface Storage {
    void save(String data);
}
```

and:

```java
class DatabaseStorage implements Storage
```

The compiler knows that `DatabaseStorage` promises to satisfy the `Storage` contract.

---

### Python

Python normally just does:

```python
class DatabaseStorage:

    def save(self, data):
        ...
```

There is no requirement to explicitly declare that the class implements an interface.

If an object supports the required operation:

```python
storage.save(...)
```

Python can use it.

---

# 18. When Should You Use an Interface?

Interfaces are particularly useful when:

### Multiple unrelated classes need to provide the same capability.

For example:

```text
Flyable
    ├── Bird
    └── Airplane
```

The classes have little reason to share a parent class, but both can fly.

---

### You want composition to be replaceable.

For example:

```text
Report
   │
   └── Storage
        ├── FileStorage
        ├── DatabaseStorage
        └── CloudStorage
```

The `Report` doesn't care which storage implementation it receives.

---

### You want to define a contract between parts of a program.

For example:

```text
PaymentProcessor
    processPayment()
```

Different implementations could provide:

```text
CreditCardProcessor
PayPalProcessor
BankTransferProcessor
```

The rest of the program can depend on the contract rather than a particular implementation.

---

# 19. When Do You NOT Need an Interface?

Do not create interfaces simply because you can.

If you only have one implementation and there is no meaningful need for substitution, an interface may add unnecessary complexity.

For example:

```java
class Calculator {
    int add(int a, int b) {
        return a + b;
    }
}
```

Creating:

```java
interface CalculatorInterface {
    int add(int a, int b);
}
```

may accomplish very little if there is no alternative implementation or meaningful contract.

A good rule is:

> **Use an interface when you have a meaningful capability or contract that multiple implementations can satisfy.**

---

# 20. Inheritance vs. Interface vs. Composition

These three concepts work together.

### Inheritance

Answers:

> **What is this object?**

```text
Dog IS-A Animal
```

### Interface

Answers:

> **What can this object do?**

```text
Bird CAN Fly
Airplane CAN Fly
```

### Composition

Answers:

> **What does this object have or use?**

```text
Car HAS-A Engine
Report HAS-A Storage
```

They can be combined.

For example:

```text
                   Vehicle
                      ▲
                      │
                     Car
                      │
                    HAS-A
                      │
                      ▼
                    Engine
                      │
                implements
                      │
                      ▼
                  Startable
```

A class can have a type relationship, contain other objects, and implement capabilities.

---

# 21. Vocabulary

| Term             | Meaning                                                                                          |
| ---------------- | ------------------------------------------------------------------------------------------------ |
| **Interface**    | A contract describing operations a class must provide                                            |
| **Contract**     | A promise that certain operations are available                                                  |
| **Implement**    | A Java class fulfills an interface's contract                                                    |
| **Duck typing**  | Python's approach of using an object's behavior rather than requiring a particular declared type |
| **Protocol**     | Python's type-checking mechanism for describing a structural interface                           |
| **Decoupling**   | Reducing dependencies between parts of a program                                                 |
| **Capability**   | Something an object can do, such as `fly()` or `send()`                                          |
| **Polymorphism** | Treating different implementations through a common contract                                     |

---

# 22. The Big Mental Model

Think of an interface as a **plug specification**.

A device doesn't care who manufactured the plug.

It cares that the plug follows the required specification.

Similarly:

```text
                Storage
              INTERFACE
                  │
           must provide save()
                  │
        ┌─────────┼─────────┐
        ▼         ▼         ▼
       File     Database    Cloud
```

The code using `Storage` doesn't need to know which implementation it received.

It only needs the contract.

---

# 23. Final Takeaway

You should be able to explain:

> **An interface defines a contract that describes what an object can do without requiring a particular implementation.**

In Java:

```java
interface Storage {
    void save(String data);
}
```

Classes explicitly implement the contract:

```java
class FileStorage implements Storage
```

Python does not traditionally require explicit interfaces.

Instead, Python commonly uses **duck typing**:

```python
storage.save(data)
```

If the object provides the required behavior, it can be used.

Python can also use `typing.Protocol` when an explicit, statically checkable contract is useful.

The three major OOP relationships are:

```text
Inheritance
    IS-A

Interface
    CAN-DO

Composition
    HAS-A / USES-A
```

And a particularly powerful combination is:

```text
             Report
                │
              HAS-A
                │
             Storage
                │
        ┌───────┼────────┐
        ▼       ▼        ▼
       File   Database  Cloud
```

The `Report` depends on **what Storage can do**, not on how a particular storage system does it.

That is the central reason interfaces are valuable:

> **They let us program against a contract rather than a specific implementation.**
