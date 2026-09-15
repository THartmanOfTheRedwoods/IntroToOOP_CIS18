# Classes and Objects — Python and Java

## Video Goal

In this lesson, we introduce the two fundamental ideas behind Object-Oriented Programming:

* **Classes** — definitions or blueprints for creating objects
* **Objects** — individual instances created from a class

The goal is not to cover all of Object-Oriented Programming. We are deliberately focusing on the basic ideas needed to begin thinking in terms of objects.

---

# 1. Why Do We Use Objects?

Before introducing classes, start with the problem they solve.

Suppose we are writing a program for a university.

We might have information about students:

```text
Name
Student ID
Major
GPA
```

Using basic variables, we could write:

```python
student_name = "Alice"
student_id = 12345
student_major = "Computer Science"
student_gpa = 3.8
```

This works for one student.

But what happens when we have 100 students?

We might end up with:

```python
student1_name = "Alice"
student1_id = 12345
student1_major = "Computer Science"

student2_name = "Bob"
student2_id = 12346
student2_major = "Mathematics"

student3_name = "Charlie"
student3_id = 12347
student3_major = "Physics"
```

The data belongs together conceptually, but the program has no explicit representation of the concept **Student**.

Object-oriented programming lets us model that concept directly.

Instead of thinking:

> "I have a bunch of variables that happen to describe a student."

we can think:

> "I have a Student object."

That is one of the fundamental ideas behind OOP.

---

# 2. What Is a Class?

A **class** is a definition describing what an object will contain and what it can do.

A useful analogy is a blueprint.

A blueprint for a house is not itself a house.

It describes what a house will look like.

Similarly:

```text
Class
  ↓
describes objects
```

A class can define:

* **State** — information/data belonging to an object
* **Behavior** — operations the object can perform

For example, a `Student` might have:

### State

```text
name
student_id
major
gpa
```

### Behavior

```text
study()
change_major()
display_info()
```

The class describes these things.

The actual students in our program are **objects**.

---

# 3. What Is an Object?

An **object** is an instance of a class.

If:

```text
Student
```

is the class, then:

```text
Alice
Bob
Charlie
```

can be individual objects created from that class.

Think:

```text
                 Class
                Student
                   |
       -------------------------
       |           |           |
    Object       Object       Object
    Alice         Bob        Charlie
```

All three objects are instances of the same class.

But each object has its own state.

---

# 4. Creating a Simple Class

Let's start with the smallest possible example.

## Python

```python
class Student:
    pass
```

This defines a class named `Student`.

We can now create an object:

```python
student1 = Student()
```

`student1` is an object whose class is `Student`.

We can create another:

```python
student2 = Student()
```

Now we have two different objects.

```text
Student class
     |
     +---- student1
     |
     +---- student2
```

The class defines what kind of object can be created.

The objects are the actual instances.

---

## Java

Java requires the class definition to be explicit:

```java
class Student {
}
```

We can create an object using `new`:

```java
Student student1 = new Student();
```

And another:

```java
Student student2 = new Student();
```

Again:

```text
Student class
     |
     +---- student1
     |
     +---- student2
```

---

# 5. Classes Contain Data — Attributes / Fields

A useful class usually describes some state.

For a student, we might want:

```text
name
student ID
major
GPA
```

In Python, these are commonly called **attributes**.

In Java, they are commonly called **fields**.

---

## Python

```python
class Student:
    def __init__(self, name, student_id, major):
        self.name = name
        self.student_id = student_id
        self.major = major
```

Now:

```python
student1 = Student("Alice", 12345, "Computer Science")
```

The object contains:

```text
student1
 ├── name       → "Alice"
 ├── student_id → 12345
 └── major      → "Computer Science"
```

We can access the attributes:

```python
print(student1.name)
print(student1.student_id)
print(student1.major)
```

Output:

```text
Alice
12345
Computer Science
```

---

## Java

The equivalent Java class is:

```java
class Student {
    String name;
    int studentId;
    String major;

    Student(String name, int studentId, String major) {
        this.name = name;
        this.studentId = studentId;
        this.major = major;
    }
}
```

Create an object:

```java
Student student1 =
    new Student("Alice", 12345, "Computer Science");
```

Access the fields:

```java
System.out.println(student1.name);
System.out.println(student1.studentId);
System.out.println(student1.major);
```

---

# 6. Constructors / Initialization

A major point to explain is:

> When we create an object, we often need to initialize its state.

In Python, this is commonly done with:

```python
__init__()
```

Example:

```python
class Student:
    def __init__(self, name, student_id, major):
        self.name = name
        self.student_id = student_id
        self.major = major
```

When we write:

```python
student1 = Student("Alice", 12345, "Computer Science")
```

Python calls `__init__()` to initialize the new object's state.

---

## Java

Java uses a **constructor**.

```java
class Student {
    String name;
    int studentId;
    String major;

    Student(String name, int studentId, String major) {
        this.name = name;
        this.studentId = studentId;
        this.major = major;
    }
}
```

When we write:

```java
Student student1 =
    new Student("Alice", 12345, "Computer Science");
```

the constructor is called.

The important conceptual point is:

> A constructor establishes the initial state of an object.

---

# 7. What Is `self`?

Python introduces a concept that students coming from Python may initially find strange when moving to Java.

```python
self
```

`self` refers to the **current object**.

Consider:

```python
class Student:
    def __init__(self, name):
        self.name = name
```

The line:

```python
self.name = name
```

means:

> Store the value of `name` in the `name` attribute belonging to this particular object.

For example:

```python
student1 = Student("Alice")
student2 = Student("Bob")
```

Conceptually:

```text
student1
    self.name → "Alice"

student2
    self.name → "Bob"
```

`self` allows the same class definition to operate on different objects.

---

# 8. Java's `this`

Java has essentially the same concept, but calls it:

```java
this
```

For example:

```java
class Student {
    String name;

    Student(String name) {
        this.name = name;
    }
}
```

Here:

```java
this.name
```

means:

> The `name` field belonging to this object.

So students should recognize this relationship:

```text
Python       Java
-------      -------
self         this
```

Both refer to the current object.

---

# 9. Objects Have Behavior — Methods

So far, our objects contain data.

But objects can also have behavior.

A method is a function associated with a class/object.

For example, a student could have a method:

```text
study()
```

---

## Python

```python
class Student:
    def __init__(self, name, major):
        self.name = name
        self.major = major

    def study(self):
        print(self.name, "is studying.")
```

Create an object:

```python
student1 = Student("Alice", "Computer Science")
```

Call its method:

```python
student1.study()
```

Output:

```text
Alice is studying.
```

Notice that the method operates on the particular object.

---

## Java

```java
class Student {
    String name;
    String major;

    Student(String name, String major) {
        this.name = name;
        this.major = major;
    }

    void study() {
        System.out.println(name + " is studying.");
    }
}
```

Create the object:

```java
Student student1 =
    new Student("Alice", "Computer Science");
```

Call the method:

```java
student1.study();
```

Output:

```text
Alice is studying.
```

---

# 10. Data and Behavior Belong Together

This is one of the most important ideas to emphasize.

Without objects, we might have:

```text
Student data
    ↓
name
major
GPA

Student functions
    ↓
study()
change_major()
calculate_gpa()
```

Object-oriented programming allows us to group the related data and behavior together.

```text
             Student Object
          ┌───────────────────┐
          │ name              │
          │ major             │
          │ GPA               │
          │                   │
          │ study()           │
          │ change_major()    │
          └───────────────────┘
```

The object represents a concept in our program.

This is a major shift in how we think about program design.

---

# 11. One Class Can Create Many Objects

This is an important demonstration.

## Python

```python
class Student:
    def __init__(self, name, major):
        self.name = name
        self.major = major

    def study(self):
        print(self.name, "is studying.")
```

Create several objects:

```python
alice = Student("Alice", "Computer Science")
bob = Student("Bob", "Mathematics")
charlie = Student("Charlie", "Physics")
```

Each object has the same general structure:

```text
alice
    name  → Alice
    major → Computer Science

bob
    name  → Bob
    major → Mathematics

charlie
    name  → Charlie
    major → Physics
```

But the objects have different state.

We can call the same method on each:

```python
alice.study()
bob.study()
charlie.study()
```

Output:

```text
Alice is studying.
Bob is studying.
Charlie is studying.
```

---

## Java

```java
class Student {
    String name;
    String major;

    Student(String name, String major) {
        this.name = name;
        this.major = major;
    }

    void study() {
        System.out.println(name + " is studying.");
    }
}
```

Create multiple objects:

```java
Student alice =
    new Student("Alice", "Computer Science");

Student bob =
    new Student("Bob", "Mathematics");

Student charlie =
    new Student("Charlie", "Physics");
```

Then:

```java
alice.study();
bob.study();
charlie.study();
```

The same class definition creates multiple independent objects.

---

# 12. Objects Have Their Own State

This is worth demonstrating explicitly.

## Python

```python
class Student:
    def __init__(self, name, major):
        self.name = name
        self.major = major
```

Create two objects:

```python
alice = Student("Alice", "Computer Science")
bob = Student("Bob", "Mathematics")
```

Now change Alice:

```python
alice.major = "Physics"
```

Print both:

```python
print(alice.major)
print(bob.major)
```

Output:

```text
Physics
Mathematics
```

Changing Alice did not change Bob.

Why?

Because they are two separate objects with separate state.

---

## Java

```java
Student alice =
    new Student("Alice", "Computer Science");

Student bob =
    new Student("Bob", "Mathematics");
```

Change Alice:

```java
alice.major = "Physics";
```

Print:

```java
System.out.println(alice.major);
System.out.println(bob.major);
```

Output:

```text
Physics
Mathematics
```

Again, each object maintains its own state.

---

# 13. Objects Are Different Even If Their Data Is the Same

This is a useful concept to introduce early.

Consider:

```python
alice = Student("Alice", "Computer Science")
another_alice = Student("Alice", "Computer Science")
```

The objects contain the same information.

But they are still two different objects.

```text
alice
   ↓
┌────────────────────┐
│ name: Alice        │
│ major: CS          │
└────────────────────┘


another_alice
   ↓
┌────────────────────┐
│ name: Alice        │
│ major: CS          │
└────────────────────┘
```

They have equal-looking state, but they are separate instances.

In Python:

```python
print(alice is another_alice)
```

produces:

```text
False
```

The distinction is:

> **State** is the data contained by an object.

> **Identity** is the fact that an object is a particular individual object.

This becomes important when you start working with references and mutable objects.

---

# 14. Variables Can Refer to Objects

This is an especially important point when transitioning between Python and Java.

Consider:

```python
alice = Student("Alice", "Computer Science")
```

It is useful to think of `alice` as a variable that refers to an object.

Conceptually:

```text
alice
  │
  ▼
┌──────────────────────┐
│ Student object       │
│                      │
│ name: Alice          │
│ major: CS            │
└──────────────────────┘
```

The variable and the object are not the same thing.

The variable gives us a way to refer to the object.

---

## Java

The same basic mental model applies:

```java
Student alice =
    new Student("Alice", "Computer Science");
```

Conceptually:

```text
alice
  │
  ▼
┌──────────────────────┐
│ Student object       │
│                      │
│ name: Alice          │
│ major: CS            │
└──────────────────────┘
```

Java makes this especially important because object variables are references to objects.

---

# 15. A Small Complete Example

At this point, let's put everything together.

## Python

```python
class BankAccount:

    def __init__(self, owner, balance):
        self.owner = owner
        self.balance = balance

    def deposit(self, amount):
        self.balance += amount

    def withdraw(self, amount):
        if amount <= self.balance:
            self.balance -= amount

    def display(self):
        print(self.owner, self.balance)
```

Create objects:

```python
account1 = BankAccount("Alice", 1000)
account2 = BankAccount("Bob", 500)
```

Use them:

```python
account1.deposit(250)
account2.withdraw(100)

account1.display()
account2.display()
```

Output:

```text
Alice 1250
Bob 400
```

The important thing to point out:

* `BankAccount` is the **class**
* `account1` and `account2` are **objects**
* `owner` and `balance` are **attributes**
* `deposit()`, `withdraw()`, and `display()` are **methods**
* `__init__()` initializes the object
* `self` refers to the current object
* each object has its own state

---

## Java

```java
class BankAccount {

    String owner;
    double balance;

    BankAccount(String owner, double balance) {
        this.owner = owner;
        this.balance = balance;
    }

    void deposit(double amount) {
        balance += amount;
    }

    void withdraw(double amount) {
        if (amount <= balance) {
            balance -= amount;
        }
    }

    void display() {
        System.out.println(owner + " " + balance);
    }
}
```

Create objects:

```java
BankAccount account1 =
    new BankAccount("Alice", 1000);

BankAccount account2 =
    new BankAccount("Bob", 500);
```

Use them:

```java
account1.deposit(250);
account2.withdraw(100);

account1.display();
account2.display();
```

Output:

```text
Alice 1250.0
Bob 400.0
```

---

# 16. The Core Vocabulary

You should leave this video knowing these terms.

| Term            | Meaning                                                 |
| --------------- | ------------------------------------------------------- |
| **Class**       | A definition describing a type of object                |
| **Object**      | An instance of a class                                  |
| **Instance**    | Another word for an object created from a class         |
| **Attribute**   | Data associated with an object, commonly used in Python |
| **Field**       | Data associated with an object, commonly used in Java   |
| **Method**      | A function associated with a class/object               |
| **Constructor** | Code used to initialize a newly created object          |
| **State**       | The current data stored in an object                    |
| **Behavior**    | What an object can do through its methods               |
| **Instance**    | A particular object created from a class                |
| **Identity**    | Which particular object something is                    |

---

# 17. Python vs. Java Vocabulary

If you're coming from Python you should see the correspondence with Java.

| Concept                 | Python            | Java                |
| ----------------------- | ----------------- | ------------------- |
| Define class            | `class Student:`  | `class Student { }` |
| Create object           | `Student()`       | `new Student()`     |
| Constructor/initializer | `__init__()`      | `Student(...)`      |
| Current object          | `self`            | `this`              |
| Object data             | Attribute         | Field               |
| Object behavior         | Method            | Method              |
| Call method             | `student.study()` | `student.study()`   |

The syntax differs, but the underlying object-oriented concepts are the same.

---

# 18. The Big Mental Model

The most important thing for you to understand is:

```text
                 CLASS
                   │
        describes what an object
        looks like and can do
                   │
          ┌────────┴────────┐
          │                 │
          ▼                 ▼
       OBJECT            OBJECT
          │                 │
       state             state
          │                 │
       behavior          behavior
```

For example:

```text
                 Student
                   │
        ┌──────────┼──────────┐
        │          │          │
        ▼          ▼          ▼
      Alice        Bob      Charlie
```

The class provides the common definition.

Each object has its own state.

Each object can perform the behavior defined by the class.

---

# 19. Why Think This Way?

The real purpose of classes and objects is not simply to use different syntax.

They give us a way to organize programs around **things and concepts**.

Instead of asking:

> "What variables and functions do I need?"

we can start asking:

> "What things exist in the problem I'm modeling?"

For a banking application:

```text
BankAccount
Customer
Transaction
```

For a game:

```text
Player
Enemy
Weapon
Game
```

For a university:

```text
Student
Course
Professor
Classroom
```

For a library:

```text
Book
Library
Member
Loan
```

Each concept can become a class.

The objects represent the individual instances of those concepts.

---

# 20. A Useful Rule of Thumb

When designing an object-oriented program, ask:

### 1. What are the things (NOUNS) in my problem?

These often become classes.

### 2. What information does each thing need to remember (state / adjectives)?

These become attributes/fields.

### 3. What can each thing do (behavior / verbs)?

These become methods.

For example:

```text
Problem: Model a bank account

Thing:
    BankAccount

Information:
    owner
    balance

Behavior:
    deposit()
    withdraw()
    display()
```

That is the basic object-oriented thought process.

---

# 21. Final Takeaway

You should now be able to explain:

> A **class** is a definition for a kind of object.

> An **object** is an instance of a class.

> Objects have **state**, represented by attributes/fields.

> Objects have **behavior**, represented by methods.

> A **constructor** initializes an object's initial state.

> A class can be used to create many independent objects.

> Each object has its own state and identity.

And most importantly:

> **Object-oriented programming lets us model a program as a collection of objects that contain state and provide behavior.**

That is the foundation on which the rest of the OOP material can be built.
