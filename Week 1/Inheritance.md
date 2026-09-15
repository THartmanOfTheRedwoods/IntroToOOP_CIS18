# Object Inheritance — Python and Java

## Video Goal

In this lesson, we introduce **inheritance**, one of the fundamental mechanisms of Object-Oriented Programming.

The goal is to understand:

* Why inheritance exists
* Parent/base/superclass vs. child/derived/subclass
* How a subclass gets attributes and methods from a superclass
* Adding new behavior to a subclass
* Overriding inherited methods
* Calling superclass behavior with `super()`
* How inheritance affects object types
* The basic idea of polymorphism
* When inheritance is appropriate

---

# 1. Why Do We Need Inheritance?

Start with a familiar problem.

Suppose we have a program that models animals.

We might begin with:

```python
class Dog:
    def __init__(self, name):
        self.name = name

    def eat(self):
        print(self.name, "is eating.")

    def sleep(self):
        print(self.name, "is sleeping.")

    def bark(self):
        print(self.name, "says woof!")
```

Now we want a `Cat`.

We could copy everything:

```python
class Cat:
    def __init__(self, name):
        self.name = name

    def eat(self):
        print(self.name, "is eating.")

    def sleep(self):
        print(self.name, "is sleeping.")

    def meow(self):
        print(self.name, "says meow!")
```

But notice the problem.

`Dog` and `Cat` have duplicated code:

```text
Dog
 ├── name
 ├── eat()
 └── sleep()

Cat
 ├── name
 ├── eat()
 └── sleep()
```

Both are animals.

We want to define the common behavior once.

That is where inheritance comes in.

---

# 2. The Basic Idea of Inheritance

We can create an `Animal` class containing the common functionality:

```text
             Animal
            /      \
           /        \
        Dog          Cat
```

`Animal` is the **parent class**.

Other common terms are:

* **Superclass**
* **Base class**

`Dog` and `Cat` are **child classes**.

Other common terms are:

* **Subclass**
* **Derived class**

The child classes inherit functionality from the parent.

Conceptually:

```text
Animal
 ├── name
 ├── eat()
 └── sleep()

Dog
 └── bark()

Cat
 └── meow()
```

A `Dog` object has the things defined by `Animal` plus its own dog-specific behavior.

---

# 3. Python Inheritance

In Python, inheritance is specified by putting the parent class in parentheses.

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def eat(self):
        print(self.name, "is eating.")

    def sleep(self):
        print(self.name, "is sleeping.")
```

Now create a subclass:

```python
class Dog(Animal):
    def bark(self):
        print(self.name, "says woof!")
```

The important syntax is:

```python
class Dog(Animal):
```

which means:

> `Dog` inherits from `Animal`.

---

# 4. Inherited Methods

We can create a `Dog`:

```python
dog = Dog("Rex")
```

Notice that we did **not** define `__init__()` in `Dog`.

Python uses the inherited `Animal.__init__()`.

We can therefore do:

```python
print(dog.name)
```

and:

```python
dog.eat()
dog.sleep()
```

even though `eat()` and `sleep()` are defined in `Animal`.

We can also use the method specific to `Dog`:

```python
dog.bark()
```

So the object has access to both:

```text
Inherited:
    eat()
    sleep()

Dog-specific:
    bark()
```

---

# 5. Java Inheritance

Java uses the `extends` keyword.

```java
class Animal {
    String name;

    Animal(String name) {
        this.name = name;
    }

    void eat() {
        System.out.println(name + " is eating.");
    }

    void sleep() {
        System.out.println(name + " is sleeping.");
    }
}
```

Create the subclass:

```java
class Dog extends Animal {

    Dog(String name) {
        super(name);
    }

    void bark() {
        System.out.println(name + " says woof!");
    }
}
```

The important syntax is:

```java
class Dog extends Animal
```

which means:

> `Dog` inherits from `Animal`.

Create the object:

```java
Dog dog = new Dog("Rex");
```

We can use inherited methods:

```java
dog.eat();
dog.sleep();
```

and the method defined by `Dog`:

```java
dog.bark();
```

---

# 6. Adding New Behavior

Inheritance does not mean that the child class can only use inherited functionality.

A subclass can add its own attributes and methods.

For example:

```python
class Dog(Animal):
    def bark(self):
        print(self.name, "says woof!")

    def fetch(self):
        print(self.name, "is fetching the ball.")
```

Now:

```python
dog = Dog("Rex")

dog.eat()       # inherited
dog.sleep()     # inherited
dog.bark()      # Dog
dog.fetch()     # Dog
```

The subclass extends the functionality of the parent.

That is one of the most important ideas behind inheritance:

> **A subclass gets existing functionality and can add additional functionality.**

---

# 7. The Child Can Have Its Own State

A subclass can also have additional attributes.

For example:

```python
class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name)
        self.breed = breed

    def bark(self):
        print(self.name, "says woof!")
```

Now a `Dog` contains:

```text
Animal state:
    name

Dog state:
    breed
```

Create one:

```python
dog = Dog("Rex", "Labrador")
```

We can access:

```python
print(dog.name)
print(dog.breed)
```

The object contains both the inherited state and its subclass-specific state.

---

# 8. `super()` in Python

The `super()` function allows a subclass to access functionality from its parent class.

Consider:

```python
class Animal:
    def __init__(self, name):
        self.name = name
```

The subclass needs to initialize the inherited `name` attribute:

```python
class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name)
        self.breed = breed
```

This line:

```python
super().__init__(name)
```

means, conceptually:

> Call the parent class's initializer.

Without it, the `Animal` initialization would not happen automatically when `Dog` defines its own `__init__()`.

---

# 9. `super` in Java

Java uses the same concept, but without parentheses when referring to the superclass constructor.

```java
class Animal {
    String name;

    Animal(String name) {
        this.name = name;
    }
}
```

The subclass:

```java
class Dog extends Animal {
    String breed;

    Dog(String name, String breed) {
        super(name);
        this.breed = breed;
    }
}
```

This:

```java
super(name);
```

calls the superclass constructor.

The important comparison is:

```text
Python                 Java
------                 ----
super().__init__(...)  super(...)
```

Both allow the subclass to invoke the parent's initialization.

---

# 10. Method Overriding

Sometimes a subclass inherits a method but needs different behavior.

For example, all animals might have a `make_sound()` method.

```python
class Animal:
    def make_sound(self):
        print("Some animal sound.")
```

A dog needs different behavior:

```python
class Dog(Animal):
    def make_sound(self):
        print("Woof!")
```

The `Dog` method **overrides** the inherited method.

Now:

```python
dog = Dog("Rex")
dog.make_sound()
```

produces:

```text
Woof!
```

rather than:

```text
Some animal sound.
```

The subclass has replaced the inherited implementation with its own implementation.

---

# 11. Java Method Overriding

Java works similarly.

```java
class Animal {
    void makeSound() {
        System.out.println("Some animal sound.");
    }
}
```

The subclass:

```java
class Dog extends Animal {

    @Override
    void makeSound() {
        System.out.println("Woof!");
    }
}
```

The annotation:

```java
@Override
```

tells Java that we intend to override a method from the superclass.

This is useful because the compiler can check that the method actually overrides something.

---

# 12. Inheritance Is More Than Code Reuse

It is tempting to explain inheritance as:

> "Inheritance lets us reuse code."

That is true, but incomplete.

The more important idea is that inheritance represents an **"is-a" relationship**.

A:

```text
Dog is an Animal.
Cat is an Animal.
```

Therefore:

```text
Dog → Animal
Cat → Animal
```

This is a good candidate for inheritance.

Compare that with:

```text
Car has an Engine.
```

That is not an "is-a" relationship.

It is a **has-a** relationship, which is a different object-oriented concept.

For this lesson, the key rule is:

> Use inheritance when the subclass is genuinely a specialized kind of the superclass.

---

# 13. A Useful Test: "Is a..."

Ask:

> Is a Dog an Animal?

Yes.

> Is a Cat an Animal?

Yes.

> Is a Car an Engine?

No.

> Is a Student a Person?

Potentially yes.

> Is a Bicycle a Vehicle?

Yes.

This simple test can help you identify potential inheritance relationships.

---

# 14. Polymorphism — The First Introduction

Inheritance leads naturally to one of the most important ideas in OOP: **polymorphism**.

We can have:

```python
class Animal:
    def make_sound(self):
        print("Some sound")


class Dog(Animal):
    def make_sound(self):
        print("Woof!")


class Cat(Animal):
    def make_sound(self):
        print("Meow!")
```

Now:

```python
animals = [
    Dog(),
    Cat()
]

for animal in animals:
    animal.make_sound()
```

Output:

```text
Woof!
Meow!
```

The code does not need to know whether `animal` is a `Dog` or a `Cat`.

It simply asks the object:

```python
animal.make_sound()
```

Each object provides its own implementation.

That is the basic idea of polymorphism:

> **Different types of objects can be treated through a common interface while providing their own behavior.**

For an introductory inheritance video, this is enough. A deeper discussion of polymorphism can come later.

---

# 15. Java Polymorphism

The same concept works in Java.

```java
class Animal {
    void makeSound() {
        System.out.println("Some sound");
    }
}

class Dog extends Animal {
    @Override
    void makeSound() {
        System.out.println("Woof!");
    }
}

class Cat extends Animal {
    @Override
    void makeSound() {
        System.out.println("Meow!");
    }
}
```

Now:

```java
Animal[] animals = {
    new Dog(),
    new Cat()
};

for (Animal animal : animals) {
    animal.makeSound();
}
```

Output:

```text
Woof!
Meow!
```

The variable is declared as:

```java
Animal animal
```

but the actual object may be a `Dog` or a `Cat`.

Java calls the appropriate overridden method for the actual object.

---

# 16. Parent Type vs. Actual Object

This distinction is important.

Consider:

```java
Animal animal = new Dog();
```

There are two different concepts here.

### Declared/reference type

```text
Animal
```

### Actual object type

```text
Dog
```

Conceptually:

```text
animal
   │
   ▼
┌───────────────┐
│ Dog object    │
│               │
│ Animal stuff  │
│ Dog stuff     │
└───────────────┘
```

This is one of the reasons inheritance is so powerful.

A `Dog` can be used wherever an `Animal` is expected because a Dog **is an Animal**.

---

# 17. A Complete Example

Here is a compact example that combines the major concepts.

## Python

```python
class Animal:

    def __init__(self, name):
        self.name = name

    def make_sound(self):
        print("Some animal sound.")

    def eat(self):
        print(self.name, "is eating.")


class Dog(Animal):

    def __init__(self, name, breed):
        super().__init__(name)
        self.breed = breed

    def make_sound(self):
        print("Woof!")

    def fetch(self):
        print(self.name, "is fetching.")


class Cat(Animal):

    def make_sound(self):
        print("Meow!")
```

Use the classes:

```python
dog = Dog("Rex", "Labrador")
cat = Cat("Whiskers")

dog.eat()          # inherited
dog.make_sound()   # overridden
dog.fetch()        # Dog-specific

cat.eat()          # inherited
cat.make_sound()   # overridden
```

This demonstrates:

* `Animal` is the superclass
* `Dog` and `Cat` are subclasses
* `Dog` and `Cat` inherit `eat()`
* `Dog` overrides `make_sound()`
* `Cat` overrides `make_sound()`
* `Dog` adds `fetch()`
* `Dog` has additional state: `breed`
* `super()` calls the parent initializer

---

## Java

```java
class Animal {

    String name;

    Animal(String name) {
        this.name = name;
    }

    void makeSound() {
        System.out.println("Some animal sound.");
    }

    void eat() {
        System.out.println(name + " is eating.");
    }
}


class Dog extends Animal {

    String breed;

    Dog(String name, String breed) {
        super(name);
        this.breed = breed;
    }

    @Override
    void makeSound() {
        System.out.println("Woof!");
    }

    void fetch() {
        System.out.println(name + " is fetching.");
    }
}


class Cat extends Animal {

    Cat(String name) {
        super(name);
    }

    @Override
    void makeSound() {
        System.out.println("Meow!");
    }
}
```

Use the classes:

```java
Dog dog = new Dog("Rex", "Labrador");
Cat cat = new Cat("Whiskers");

dog.eat();          // inherited
dog.makeSound();    // overridden
dog.fetch();        // Dog-specific

cat.eat();          // inherited
cat.makeSound();    // overridden
```

---

# 18. Inheritance Vocabulary

You should know these terms.

| Term                  | Meaning                                                             |
| --------------------- | ------------------------------------------------------------------- |
| **Superclass**        | The parent/base class                                               |
| **Subclass**          | The child/derived class                                             |
| **Inheritance**       | Mechanism where a subclass receives functionality from a superclass |
| **Extends**           | Java keyword used to establish inheritance                          |
| **Override**          | Replace an inherited method with a subclass implementation          |
| **`super()`**         | Python mechanism for accessing superclass functionality             |
| **`super`**           | Java mechanism for accessing superclass functionality               |
| **Is-a relationship** | Relationship commonly represented by inheritance                    |
| **Polymorphism**      | Different object types can be used through a common parent type     |

---

# 19. Python vs. Java

| Concept            | Python                       | Java                              |
| ------------------ | ---------------------------- | --------------------------------- |
| Create subclass    | `class Dog(Animal):`         | `class Dog extends Animal`        |
| Parent constructor | `super().__init__(...)`      | `super(...)`                      |
| Current object     | `self`                       | `this`                            |
| Override method    | Define method with same name | Same method + usually `@Override` |
| Parent method      | `super().method()`           | `super.method()`                  |
| Parent class       | Base/superclass              | Base/superclass                   |
| Child class        | Subclass                     | Subclass                          |

---

# 20. Common Mistakes

### Mistake 1: Thinking the child replaces the parent

Inheritance does not mean:

```text
Dog replaces Animal
```

It means:

```text
Dog extends Animal
```

The Dog still has the inherited functionality unless it overrides it.

---

### Mistake 2: Confusing inheritance with code copying

The subclass does not literally copy and paste the superclass's code.

The subclass **inherits** the superclass's behavior and participates in the superclass/subclass relationship.

---

### Mistake 3: Using inheritance just to avoid duplication

Code reuse alone is not a sufficient reason to use inheritance.

Ask:

> "Is this really an is-a relationship?"

If not, inheritance may be the wrong tool.

---

### Mistake 4: Forgetting `super()` when initializing inherited state

If a subclass defines its own constructor/initializer, you need to understand how the parent portion of the object gets initialized.

Python:

```python
super().__init__(name)
```

Java:

```java
super(name);
```

---

### Mistake 5: Thinking every inherited method must be overridden

A subclass does **not** have to override inherited methods.

It can simply use them:

```python
dog.eat()
```

while overriding only behavior that needs to be different:

```python
dog.make_sound()
```

---

# 21. The Big Mental Model

The most important diagram for you is:

```text
                    Animal
                       │
          ┌────────────┴────────────┐
          │                         │
          ▼                         ▼
        Dog                        Cat
          │                         │
     ┌────┴────┐                    │
     │         │                    │
   bark()    fetch()             meow()
```

`Dog` and `Cat` inherit common functionality from `Animal`.

They can then:

* use inherited behavior
* add their own behavior
* override inherited behavior
* have additional state

---

# 22. The Three Questions to Ask

When considering inheritance, ask:

### 1. Is there an "is-a" relationship?

```text
Dog is an Animal.
```

### 2. What should be common?

Put shared state and behavior in the superclass.

```text
Animal
 ├── name
 ├── eat()
 └── sleep()
```

### 3. What is specialized?

Put specialized state and behavior in the subclass.

```text
Dog
 ├── breed
 ├── bark()
 └── fetch()
```

This gives us:

```text
               General
                  ↓
               Animal
                  ↓
              Specialized
               /       \
             Dog       Cat
```

---

# 23. Final Takeaway

You should be able to explain:

> **Inheritance allows one class to be based on another class.**

The parent class provides common state and behavior.

The child class can:

* inherit that functionality
* add new functionality
* add new state
* override existing behavior

The fundamental relationship is:

```text
Child IS-A Parent
```

For example:

```text
Dog IS-A Animal
Cat IS-A Animal
```

Inheritance also provides the foundation for polymorphism, where different subclasses can be treated as instances of their common superclass while providing their own behavior.

The key mental model is:

```text
Superclass
    ↓
common state + common behavior
    ↓
Subclass
    ↓
specialized state + specialized behavior
```

And the most important question to leave with is:

> **"Is this object really a specialized kind of that other object?"**

If the answer is yes, inheritance may be an appropriate way to model the relationship.
