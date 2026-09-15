# Singleton Pattern

## Video Goal

In this lesson, we introduce the **Singleton Design Pattern** and examine how and why an object can be restricted to a single instance.

The goal is to understand:

* What the Singleton pattern is
* Why a program might need exactly one instance of a class
* How Singleton implementations work
* Eager vs. lazy initialization
* Thread-safe Singleton implementations
* Java's `enum` approach
* When Singleton is appropriate
* When Singleton should be avoided
* What OOP principles the pattern represents
* The major criticisms and tradeoffs of Singleton
* How Singleton applies to DungeonForge's `GameConfig` and `RandomSource`

> **Key idea:**
> **A Singleton ensures that a class has one instance and provides a controlled way for the rest of the program to access that instance.**

---

# 1. The Problem Singleton Solves

Sometimes a program needs **one shared object** rather than creating independent copies throughout the application.

For example, DungeonForge needs a single source of configuration:

```text
GameConfig
     │
     ├── dungeon settings
     ├── combat settings
     └── game constants
```

If different parts of the program could create different `GameConfig` objects, we could end up with inconsistent settings.

Similarly, if the game is supposed to use one seeded source of randomness:

```text
RandomSource
      │
      └── all random rolls
```

we don't want different parts of the game accidentally using unrelated random generators.

The Singleton pattern addresses this problem by controlling object creation.

---

# 2. What Makes Something a Singleton?

A Singleton has two fundamental characteristics:

### 1. Only one instance exists

The class prevents normal code from creating unlimited instances.

```text
Singleton
    │
    └── exactly one instance
```

### 2. That instance is globally accessible

Code throughout the application has a controlled way to obtain that instance.

```text
Player ───────┐
Monster ──────┤
Room ─────────┼──► Singleton
GameWorld ────┤
Main ─────────┘
```

The combination of **controlled instantiation** and **shared access** is what defines the pattern.

---

# 3. Basic Java Singleton

A classic Java implementation looks like this:

```java
class GameConfig {

    private static final GameConfig INSTANCE =
        new GameConfig();

    private GameConfig() {
    }

    public static GameConfig getInstance() {
        return INSTANCE;
    }
}
```

The important pieces are:

### Private constructor

```java
private GameConfig() {
}
```

Other classes cannot simply write:

```java
new GameConfig();
```

### One stored instance

```java
private static final GameConfig INSTANCE =
    new GameConfig();
```

The class creates and stores its single instance.

### Access method

```java
public static GameConfig getInstance()
```

Code obtains the shared instance through the class.

For example:

```java
GameConfig config = GameConfig.getInstance();
```

---

# 4. Why `static`?

The Singleton instance needs to belong to the **class**, rather than to an individual object.

That's why the instance is `static`:

```java
private static final GameConfig INSTANCE;
```

There is one copy associated with the class itself.

Likewise, `getInstance()` is static because we need a way to obtain the object **before we have an instance of the object**.

Conceptually:

```text
GameConfig class
       │
       ▼
 static INSTANCE
       │
       ▼
one GameConfig object
```

---

# 5. Eager Initialization

The previous example uses **eager initialization**.

```java
private static final GameConfig INSTANCE =
    new GameConfig();
```

The instance is created when the class is initialized.

### Advantages

* Very simple
* Thread-safe through Java's class initialization mechanism
* No synchronization code required
* The implementation is easy to understand

### Disadvantage

The object is created even if the program never actually needs it.

For a tiny configuration object this may not matter, but for an expensive object it could.

---

# 6. Lazy Initialization

**Lazy initialization** waits until the Singleton is actually requested.

```java
class GameConfig {

    private static GameConfig instance;

    private GameConfig() {
    }

    public static GameConfig getInstance() {

        if (instance == null) {
            instance = new GameConfig();
        }

        return instance;
    }
}
```

The first call creates the object:

```text
getInstance()
     ↓
instance == null?
     ↓
create object
     ↓
return object
```

Later calls simply return the existing object.

### Advantage

The object is not created until it is needed.

### Problem

This implementation is **not thread-safe**.

---

# 7. Why Thread Safety Matters

Imagine two threads call:

```java
getInstance()
```

at almost exactly the same time.

Both could execute:

```java
if (instance == null)
```

before either thread creates the object.

The result could be:

```text
Thread A ──► creates Instance #1

Thread B ──► creates Instance #2
```

Now we have violated the Singleton requirement.

Therefore:

> **A lazy Singleton must account for concurrent access if the application can access it from multiple threads.**

---

# 8. Thread-Safe Lazy Singleton

One straightforward approach is synchronization:

```java
class GameConfig {

    private static GameConfig instance;

    private GameConfig() {
    }

    public static synchronized GameConfig getInstance() {

        if (instance == null) {
            instance = new GameConfig();
        }

        return instance;
    }
}
```

The `synchronized` keyword ensures that only one thread at a time can execute this method.

This makes the initialization safe, at the cost of synchronization overhead on calls to `getInstance()`.

For many applications, this simple implementation is perfectly adequate.

---

# 9. Double-Checked Locking

A more advanced approach is **double-checked locking**:

```java
class GameConfig {

    private static volatile GameConfig instance;

    private GameConfig() {
    }

    public static GameConfig getInstance() {

        if (instance == null) {

            synchronized (GameConfig.class) {

                if (instance == null) {
                    instance = new GameConfig();
                }
            }
        }

        return instance;
    }
}
```

The idea is to avoid synchronization after the Singleton has already been created.

This is a useful implementation technique to know about, but it is considerably more complicated than the basic Singleton.

For beginners, the important lesson is:

> **Concurrency makes Singleton implementation more complicated.**

---

# 10. The Initialization-on-Demand Holder Idiom

Java provides another elegant thread-safe lazy approach:

```java
class GameConfig {

    private GameConfig() {
    }

    private static class Holder {
        private static final GameConfig INSTANCE =
            new GameConfig();
    }

    public static GameConfig getInstance() {
        return Holder.INSTANCE;
    }
}
```

The nested class is not initialized until it is needed.

Java's class initialization guarantees provide the thread safety.

This gives us:

* Lazy initialization
* Thread safety
* No explicit synchronization

It is a useful pattern to know, although you do not necessarily need to memorize it.

---

# 11. Enum Singleton

Java provides an especially simple Singleton implementation using an `enum`.

```java
enum GameConfig {

    INSTANCE;

    public void configure() {
        System.out.println("Configuring game.");
    }
}
```

Use it:

```java
GameConfig.INSTANCE.configure();
```

There is only one `INSTANCE`.

Java's enum semantics provide strong guarantees around:

* Single instance
* Initialization
* Thread safety
* Serialization

For these reasons, an enum is often considered one of the safest and simplest ways to implement a Singleton in Java when an enum-based design is appropriate.

However, it changes the way the class is structured and may not always be the most intuitive implementation for beginners.

---

# 12. The Singleton Is About the Pattern, Not the Syntax

There are many ways to implement a Singleton:

```text
Eager initialization
Lazy initialization
Synchronized method
Double-checked locking
Initialization-on-demand holder
Enum
```

These are **implementation techniques**.

The underlying pattern remains the same:

```text
              Singleton
                  │
       ┌──────────┴──────────┐
       ▼                     ▼
  one instance          controlled access
```

You should understand the design goal before memorizing the implementations.

---

# 13. When Is a Singleton Appropriate?

A Singleton can make sense when there is genuinely **one logical instance for the entire application**.

Examples might include:

```text
Game Configuration
Application-wide logging service
Shared resource manager
Certain system-level services
```

For DungeonForge:

```text
GameConfig
RandomSource
```

are useful examples.

If the design requirement is:

> **If having MORE than exactly one shared source of X resource would cause a BUG.**

then Singleton may be appropriate.

---

# 14. Singleton and Composition

Singletons can be used as components in a larger object-oriented design.

For example:

```text
GameWorld
    │
    ├── uses → GameConfig
    │
    └── uses → RandomSource
```

Other objects can also use the same instances:

```text
Player ───────┐
Monster ──────┤
Room ─────────┼──► RandomSource
GameWorld ────┘
```

This ensures that all of those objects are using the same shared resource.

However, this convenience comes with an important tradeoff.

---

# 15. The Major Criticism: Global State

The biggest criticism of Singleton is that it can effectively create **global state**.

Consider:

```java
RandomSource.getInstance()
```

Any class can access it.

That is convenient, but it also means a class can have a dependency that isn't obvious from its constructor.

Compare:

```java
class Monster {

    private RandomSource random;

    Monster(RandomSource random) {
        this.random = random;
    }
}
```

The dependency is explicit:

```text
Monster ──depends on──► RandomSource
```

With:

```java
class Monster {

    void attack() {
        RandomSource.getInstance().nextInt();
    }
}
```

the dependency is hidden inside the class.

This is known as a **hidden dependency**.

---

# 16. Why Hidden Dependencies Matter

Hidden dependencies make code harder to:

* Test
* Reuse
* Understand
* Configure
* Replace

For example, testing a class that directly accesses a Singleton may require changing the global Singleton's state.

With dependency injection:

```java
Monster(RandomSource random)
```

a test can provide a different implementation or controlled object.

Therefore:

> **Singleton provides convenient global access, but convenience can come at the cost of coupling.**

---

# 17. Singleton and Testing

Singletons can make unit testing more difficult because tests may share the same global object.

For example:

```text
Test A
   ↓
Singleton state changed
   ↓
Test B
   ↓
sees changed state
```

Now tests can accidentally depend on the order in which they run.

This is one reason modern software design often favors **dependency injection** over global Singleton access when practical.
* In our lab this week, we'll deal with this problem by adding Methods to the Singleton class to reset our Singletons.
* We could also do this reset with java.lang.reflection or dependency injection without adding methods, but that's for another day.

---

# 18. When NOT to Use a Singleton

Avoid Singleton simply because:

> "Several classes need access to this object."

Shared access alone does not necessarily justify a Singleton.

Consider whether the object really needs to have exactly one instance.

You probably **should not** use Singleton when:

* Multiple independent instances make sense
* The object contains state that should be isolated
* You primarily want convenient global access
* The Singleton creates difficult testing problems
* Dependencies would be clearer if passed explicitly
* The object could simply be created and passed to the classes that need it

A useful question is:

> * **Does the application conceptually require one instance, or do I just want an easy way to access it?**
>    * OR
> * **Would it be a BUG if there is more than one instance**

Those are very different reasons.

---

# 19. What OOP Principles Does Singleton Represent?

Singleton touches several important OOP concepts.

### Encapsulation

The constructor is private and object creation is controlled by the class.

```java
private GameConfig() {
}
```

The class controls how its instances are created.

### Abstraction

Other parts of the program don't need to know how the Singleton manages its instance.

They simply request it:

```java
GameConfig.getInstance()
```

### Single Responsibility

A Singleton can centralize responsibility for managing one shared resource.

However, this principle can be violated if a Singleton becomes a giant "everything manager."

### Controlled Object Creation

Singleton is fundamentally about controlling the lifecycle and number of instances of a class.

---

# 20. Singleton and the "Single Responsibility" Trap

A common mistake is to create something like:

```text
GameManager
 ├── configuration
 ├── random numbers
 ├── players
 ├── combat
 ├── rooms
 ├── saving
 ├── rendering
 └── everything else
```

and make the entire thing a Singleton.

This creates a **God Object** as well as global state.

Instead:

```text
GameConfig
RandomSource
GameWorld
CombatSystem
```

should each have clearly defined responsibilities.

Singleton answers:

> **"How many instances should there be?"**

It does **not** answer:

> **"What responsibilities should this class have?"**

---

# 21. Singleton vs. Static Methods

Singleton and static utility classes are not the same thing.

A static method:

```java
RandomUtil.randomInt();
```

provides behavior without requiring an object.

A Singleton provides an actual object:

```java
RandomSource.getInstance();
```

That object can:

* Have state
* Implement an interface
* Be passed to other objects
* Participate in polymorphism
* Maintain its own lifecycle

This distinction becomes particularly important when we start combining design patterns.

---

# 22. The Singleton Tradeoff

The pattern can be summarized as a tradeoff:

```text
             Singleton
                 │
       ┌─────────┴─────────┐
       ▼                   ▼
   Advantages          Disadvantages
       │                   │
       ▼                   ▼
One instance         Global state
Easy access          Hidden dependencies
Shared resource      Harder testing
Controlled creation  Increased coupling
```

Singleton is neither inherently good nor inherently bad.

It is a design decision with consequences.

---

# 23. The DungeonForge Connection

DungeonForge provides two good examples:

```text
GameConfig
RandomSource
```

The design requirement is that the game should have a single shared source for these resources.

For example:

```text
                    DungeonForge
                         │
              ┌──────────┴──────────┐
              ▼                     ▼
         GameConfig            RandomSource
              │                     │
              └──────────┬──────────┘
                         ▼
                  Game Objects
```

This gives the game consistent configuration and a controlled source of randomness.

The important question for you is not simply:

> "How do I write a Singleton?"

but:

> **"Why does this particular object need to have exactly one instance?"**

That is the design-pattern question.

---

# 24. Final Takeaway

You should be able to explain:

> **The Singleton pattern ensures that a class has one instance and provides a controlled way to access that instance.**

The basic implementation uses:

```text
Private constructor
       +
Single stored instance
       +
Controlled access
```

Java provides several implementation techniques, including:

```text
Eager initialization
Lazy initialization
Thread-safe synchronization
Initialization-on-demand holder
Enum Singleton
```

The pattern can be useful when an application genuinely requires **one logical instance of a shared resource**, such as DungeonForge's:

```text
GameConfig
RandomSource
```

But Singleton has an important cost:

> **It can turn an object into global state and create hidden dependencies.**

Therefore, don't use Singleton simply because many classes need access to an object.

Ask:

> **"Does this resource logically need exactly one instance, or am I just looking for convenient global access?"**

Finally, remember that Singleton is a **design pattern**, not an OOP requirement.

It demonstrates concepts such as **encapsulation, controlled object creation, abstraction, and shared state**, but good object-oriented design also requires understanding its tradeoffs.

> **The goal isn't to make everything a Singleton. The goal is to recognize when exactly-one-instance semantics are actually part of the design.**
