# Object Composition — Python and Java

## Video Goal

In this lesson, we introduce **object composition**, another fundamental way of building relationships between objects.

The goal is to understand:

* What composition means
* The difference between **"is-a"** and **"has-a"**
* How objects can contain other objects
* How one object can delegate work to another object
* Composition in Python and Java
* How composition differs from inheritance
* Why composition can often produce more flexible designs
* When composition is a good choice

> **Key idea:**
> **Inheritance describes what an object *is*. Composition describes what an object *has* or *uses*.**

---

# 1. Start With the Problem

Suppose we are creating a program to model cars.

A car has:

```text
Engine
Wheels
Battery
Transmission
```

We could try to model everything directly inside `Car`:

```python
class Car:
    def start_engine(self):
        ...

    def stop_engine(self):
        ...

    def rotate_wheels(self):
        ...

    def charge_battery(self):
        ...
```

But this starts to create a very large class.

More importantly, an engine is its own concept.

A battery is its own concept.

A wheel is its own concept.

Instead of putting everything into one giant class, we can create separate objects and have the `Car` object contain them.

```text
                 Car
          ┌───────┼────────┐
          │       │        │
          ▼       ▼        ▼
       Engine   Battery   Wheels
```

This is **composition**.

---

# 2. The "Has-A" Relationship

Composition commonly represents a **has-a relationship**.

A car:

```text
has an Engine
has a Battery
has Wheels
```

A computer:

```text
has a CPU
has Memory
has Storage
```

A house:

```text
has a Kitchen
has Bedrooms
has a Garage
```

A library:

```text
has Books
has Members
```

Compare this with inheritance.

```text
Dog IS-A Animal
```

versus:

```text
Car HAS-A Engine
```

This distinction is one of the most important things you should remember.

---

# 3. A Simple Python Example

First create an `Engine` class:

```python
class Engine:

    def start(self):
        print("Engine started.")

    def stop(self):
        print("Engine stopped.")
```

Now create a `Car`:

```python
class Car:

    def __init__(self):
        self.engine = Engine()

    def start(self):
        self.engine.start()
        print("Car started.")
```

Create a car:

```python
car = Car()
```

The `Car` object contains an `Engine` object.

Conceptually:

```text
car
 │
 ▼
┌─────────────────┐
│ Car             │
│                 │
│ engine ─────────┼─────► Engine
│                 │
│ start()         │
└─────────────────┘
```

We can start the car:

```python
car.start()
```

Output:

```text
Engine started.
Car started.
```

The `Car` delegates the engine-related work to its `Engine` object.

---

# 4. Java Composition

The same idea in Java:

```java
class Engine {

    void start() {
        System.out.println("Engine started.");
    }

    void stop() {
        System.out.println("Engine stopped.");
    }
}
```

The `Car` contains an `Engine`:

```java
class Car {

    private Engine engine;

    Car() {
        engine = new Engine();
    }

    void start() {
        engine.start();
        System.out.println("Car started.");
    }
}
```

Create the car:

```java
Car car = new Car();
```

Then:

```java
car.start();
```

Output:

```text
Engine started.
Car started.
```

Again:

```text
Car
 │
 └── has an ──► Engine
```

---

# 5. The Important Part: One Object Uses Another Object

You should focus on this line.

Python:

```python
self.engine = Engine()
```

Java:

```java
engine = new Engine();
```

The `Car` has a reference to an `Engine` object.

The `Car` can then use that object:

```python
self.engine.start()
```

or:

```java
engine.start();
```

This is the basic mechanism behind composition:

> **An object contains references to other objects and uses them to perform its work.**

---

# 6. Composition Can Involve Multiple Objects

A realistic example can have several components.

## Python

```python
class Engine:
    def start(self):
        print("Engine started.")


class Battery:
    def provide_power(self):
        print("Battery providing power.")


class Car:
    def __init__(self):
        self.engine = Engine()
        self.battery = Battery()

    def start(self):
        self.battery.provide_power()
        self.engine.start()
        print("Car started.")
```

Now:

```python
car = Car()
car.start()
```

Conceptually:

```text
                Car
          ┌──────┴──────┐
          │             │
          ▼             ▼
       Battery        Engine
```

The `Car` doesn't need to know how a battery provides power or how an engine starts.

It simply uses those objects.

---

# 7. Java Version

```java
class Engine {

    void start() {
        System.out.println("Engine started.");
    }
}


class Battery {

    void providePower() {
        System.out.println("Battery providing power.");
    }
}


class Car {

    private Engine engine;
    private Battery battery;

    Car() {
        engine = new Engine();
        battery = new Battery();
    }

    void start() {
        battery.providePower();
        engine.start();
        System.out.println("Car started.");
    }
}
```

Then:

```java
Car car = new Car();
car.start();
```

The relationship is:

```text
Car
 ├── Engine
 └── Battery
```

---

# 8. Composition vs. Inheritance

This is the central comparison.

## Inheritance

Inheritance represents:

```text
IS-A
```

Example:

```text
Dog IS-A Animal
```

```python
class Dog(Animal):
    pass
```

The `Dog` becomes a specialized type of `Animal`.

---

## Composition

Composition represents:

```text
HAS-A
```

Example:

```text
Car HAS-A Engine
```

```python
class Car:
    def __init__(self):
        self.engine = Engine()
```

The `Car` is **not** a specialized kind of `Engine`.

It simply contains and uses one.

---

# 9. A Useful Comparison

```text
Inheritance:

             Animal
                ▲
                │
               Dog

Dog IS-A Animal
```

```text
Composition:

              Car
               │
               │ HAS-A
               ▼
             Engine

Car HAS-A Engine
```

A useful rule:

> **If you can naturally say "X is a Y," inheritance may make sense.**

> **If you naturally say "X has a Y," composition may make sense.**

---

# 10. Why Prefer Composition?

This is the most important design discussion in the video.

Suppose we create:

```python
class Car:
    ...
```

and make it inherit from `Engine`:

```python
class Car(Engine):
    ...
```

What does that claim?

It says:

```text
Car IS-A Engine
```

But that isn't true.

A car uses an engine.

It isn't an engine.

Composition models the real relationship more accurately:

```python
class Car:
    def __init__(self):
        self.engine = Engine()
```

Now:

```text
Car HAS-A Engine
```

The design makes conceptual sense.

---

# 11. Composition Provides Flexibility

Suppose we have different engines:

```python
class GasEngine:
    def start(self):
        print("Gas engine starting.")


class ElectricMotor:
    def start(self):
        print("Electric motor starting.")
```

A car can use either one.

```python
class Car:
    def __init__(self, engine):
        self.engine = engine

    def start(self):
        self.engine.start()
```

Now:

```python
gas_car = Car(GasEngine())
electric_car = Car(ElectricMotor())
```

Both cars use the same `Car` class.

But they contain different objects.

```text
       Car
      /   \
     /     \
    ▼       ▼
GasEngine  ElectricMotor
```

This is a major advantage of composition.

The `Car` doesn't need to inherit from every possible engine type.

It simply uses an object that provides the behavior it needs.

---

# 12. Java Version

The same design works in Java.

```java
class GasEngine {

    void start() {
        System.out.println("Gas engine starting.");
    }
}


class ElectricMotor {

    void start() {
        System.out.println("Electric motor starting.");
    }
}
```

The `Car` receives an engine-like object.

A simple introductory Java approach can use an interface later when we formally cover interfaces, but for this lesson we can keep the example focused on composition itself.

For example, we can demonstrate the idea using a common superclass:

```java
class Engine {

    void start() {
        System.out.println("Engine starting.");
    }
}


class GasEngine extends Engine {

    @Override
    void start() {
        System.out.println("Gas engine starting.");
    }
}


class ElectricEngine extends Engine {

    @Override
    void start() {
        System.out.println("Electric engine starting.");
    }
}
```

Then:

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

Create different cars:

```java
Car gasCar =
    new Car(new GasEngine());

Car electricCar =
    new Car(new ElectricEngine());
```

The important concept is not the inheritance inside the engine hierarchy.

The important concept is that:

```text
Car
 │
 └── contains → Engine
```

The `Car` is composed from another object.

---

# 13. Composition and Delegation

A useful term to introduce here is **delegation**.

When a `Car` receives a request to start:

```python
car.start()
```

the `Car` can delegate part of that work:

```python
self.engine.start()
```

The work is effectively divided:

```text
Car.start()
    │
    └── delegates engine work to
                │
                ▼
          Engine.start()
```

This is an important object-oriented design technique.

One object does not have to know how to perform every task itself.

It can ask another object to do the work.

---

# 14. A More General Example

Cars are useful for introducing composition, but the same idea applies everywhere.

Consider a computer.

```text
Computer
 ├── CPU
 ├── Memory
 ├── Storage
 └── NetworkCard
```

Each component can be represented by an object.

Python:

```python
class CPU:
    def execute(self):
        print("CPU executing instructions.")


class Storage:
    def read(self):
        print("Reading from storage.")


class Computer:
    def __init__(self):
        self.cpu = CPU()
        self.storage = Storage()

    def run(self):
        self.storage.read()
        self.cpu.execute()
```

The `Computer` is composed of other objects.

It delegates specialized work to those objects.

---

# 15. Composition Doesn't Mean the Objects Must Be Created Inside

An important refinement is that the containing object can receive another object from outside.

Instead of:

```python
class Car:
    def __init__(self):
        self.engine = Engine()
```

we can write:

```python
class Car:
    def __init__(self, engine):
        self.engine = engine
```

Then:

```python
engine = Engine()
car = Car(engine)
```

The `Car` now uses an existing `Engine`.

This makes the relationship:

```text
Engine object
     │
     ▼
   Car
```

The important idea is:

> Composition means an object uses other objects. It does not require that the containing object create those objects itself.

This becomes important later when discussing dependency injection and more advanced design techniques.

---

# 16. Composition vs. Inheritance: Flexibility

Consider two designs.

### Inheritance

```text
          Engine
             ▲
             │
            Car
```

This would imply:

```text
Car IS-A Engine
```

which is incorrect.

### Composition

```text
          Car
           │
          HAS-A
           │
           ▼
         Engine
```

This accurately models the problem.

Composition also allows us to change the component without changing the containing object's basic design.

For example:

```text
Car
 ├── GasEngine

Car
 ├── ElectricEngine

Car
 ├── HybridEngine
```

The `Car` concept remains the same.

Only the component changes.

---

# 17. Composition Often Reduces Coupling

A useful introductory explanation is:

> Inheritance creates a strong relationship between a child class and its parent class.

A subclass depends on the superclass's structure and behavior.

Composition can create a looser relationship:

```text
Car ─────uses─────► Engine
```

The `Car` cares about what it needs from the `Engine`, rather than becoming a specialized type of `Engine`.

This can make programs easier to modify.

For example, changing the implementation of `Engine` does not necessarily require changing the conceptual design of `Car`.

---

# 18. Composition Is Not "Better" Than Inheritance

Be careful not to THINK:

> "Never use inheritance."

That is not the lesson.

Both are useful.

Use inheritance when there is a genuine:

```text
IS-A
```

relationship.

Use composition when there is a:

```text
HAS-A
```

or:

```text
USES-A
```

relationship.

For example:

```text
Dog IS-A Animal
```

Inheritance makes sense.

```text
Car HAS-A Engine
```

Composition makes sense.

The goal is to model the relationship accurately.

---

# 19. A Useful Design Question

When deciding between inheritance and composition, ask:

### Question 1

> Is the new class a specialized version of the existing class?

If yes:

```text
IS-A
```

Inheritance may be appropriate.

### Question 2

> Does the object simply need another object to perform some task?

If yes:

```text
HAS-A / USES-A
```

Composition may be appropriate.

---

# 20. Complete Example

Here is a compact example that combines the major ideas.

## Python

```python
class Engine:

    def start(self):
        print("Engine starting.")

    def stop(self):
        print("Engine stopping.")


class Car:

    def __init__(self, engine):
        self.engine = engine

    def start(self):
        print("Car starting...")
        self.engine.start()

    def stop(self):
        print("Car stopping...")
        self.engine.stop()
```

Create the objects:

```python
engine = Engine()
car = Car(engine)
```

Use the car:

```python
car.start()
car.stop()
```

The relationship is:

```text
        Car
         │
         │ has-a
         ▼
       Engine
```

The `Car` delegates engine-related behavior to the `Engine`.

---

## Java

```java
class Engine {

    void start() {
        System.out.println("Engine starting.");
    }

    void stop() {
        System.out.println("Engine stopping.");
    }
}


class Car {

    private Engine engine;

    Car(Engine engine) {
        this.engine = engine;
    }

    void start() {
        System.out.println("Car starting...");
        engine.start();
    }

    void stop() {
        System.out.println("Car stopping...");
        engine.stop();
    }
}
```

Create the objects:

```java
Engine engine = new Engine();
Car car = new Car(engine);
```

Use the car:

```java
car.start();
car.stop();
```

Again:

```text
        Car
         │
         │ has-a
         ▼
       Engine
```

---

# 21. Composition Vocabulary

You should know these terms.

| Term                    | Meaning                                                               |
| ----------------------- | --------------------------------------------------------------------- |
| **Composition**         | Building an object using other objects                                |
| **Has-a relationship**  | One object contains or owns another object                            |
| **Uses-a relationship** | One object uses another object to perform work                        |
| **Delegation**          | An object asks another object to perform part of its work             |
| **Component**           | An object used as part of another object's implementation             |
| **Coupling**            | The degree to which components depend on one another                  |
| **Encapsulation**       | Keeping an object's implementation details behind its public behavior |

---

# 22. Inheritance vs. Composition

| Question            | Inheritance                         | Composition                   |
| ------------------- | ----------------------------------- | ----------------------------- |
| Relationship        | **Is-a**                            | **Has-a / Uses-a**            |
| Example             | `Dog` is an `Animal`                | `Car` has an `Engine`         |
| Main idea           | Specialize a type                   | Build from components         |
| Reuse               | Inherited behavior                  | Delegated behavior            |
| Flexibility         | Often more tightly coupled          | Often more flexible           |
| Can swap component? | Generally not the primary mechanism | Yes                           |
| Represents          | Type relationship                   | Structural/usage relationship |

---

# 23. The Big Mental Model

Inheritance:

```text
                Animal
                   ▲
                   │
                  Dog

             "Dog IS-A Animal"
```

Composition:

```text
                 Car
                  │
                HAS-A
                  │
                  ▼
                Engine

             "Car HAS-A Engine"
```

The distinction is simple but extremely powerful.

---

# 24. Final Takeaway

You should be able to explain:

> **Composition is the practice of building objects out of other objects.**

An object can contain references to other objects and delegate work to them.

The fundamental relationship is:

```text
HAS-A
```

or:

```text
USES-A
```

For example:

```text
Car HAS-A Engine
Computer HAS-A CPU
House HAS-A Kitchen
```

Composition is often preferred when we want to build flexible systems from independent components.

Inheritance is appropriate when we have a genuine:

```text
IS-A
```

relationship.

The key design question is:

> **"Is this object a specialized version of that other object, or does it simply need to use one?"**

If it **is** one:

```text
Inheritance
```

If it **has/uses** one:

```text
Composition
```

And one of the most useful object-oriented design principles to remember is:

> **Don't use inheritance merely because you want to reuse code. Model the relationship between the objects first.**
