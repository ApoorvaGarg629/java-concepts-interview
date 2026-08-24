# 02. Object-Oriented Programming (OOP)

> OOP is the most frequently asked Java interview topic. It focuses on organizing code into classes and objects while promoting reusability, flexibility, and maintainability.

**Previous:** [Java Basics](01-Java-Basics.md)  
**Next:** [Strings](03-Strings.md)

---

# What is OOP? ⭐⭐⭐

**Object-Oriented Programming (OOP)** is a programming paradigm that models software using **objects** containing data (fields) and behavior (methods).

---

# Class vs Object ⭐⭐⭐

## Class

A **class** is a blueprint that defines the properties and behavior of an object.

```java
class Student {
    String name;
    int age;

    void study() {
        System.out.println(name + " is studying");
    }
}
```

## Object

An **object** is an instance of a class.

```java
Student s1 = new Student();

s1.name = "Rahul";
s1.age = 21;

s1.study();
```

### Difference

| Class | Object |
|--------|---------|
| Blueprint | Runtime instance |
| Logical entity | Physical entity |
| Defines fields & methods | Occupies memory |

> **Interview:** A class is a blueprint, while an object is its runtime instance.

---

# Four Pillars of OOP ⭐⭐⭐

1. Encapsulation
2. Inheritance
3. Polymorphism
4. Abstraction

---

# 1. Encapsulation ⭐⭐⭐

**Encapsulation** is the process of hiding data by making fields private and providing controlled access through methods.

## Without Encapsulation

```java
class Employee {
    String name;
    double salary;
}
```

## With Encapsulation

```java
class Employee {
    private double salary;

    public void setSalary(double salary) {
        this.salary = salary;
    }

    public double getSalary() {
        return salary;
    }
}
```

### Benefits

- Data hiding
- Controlled access
- Validation before modification
- Better maintainability

> **Interview:** Encapsulation = Private fields + Public getters/setters.

---

# 2. Inheritance ⭐⭐⭐

**Inheritance** allows a child class to reuse the properties and methods of a parent class.

```java
class Animal {
    void eat() {
        System.out.println("Eating");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Barking");
    }
}
```

Usage:

```java
Dog dog = new Dog();

dog.eat();
dog.bark();
```

## Types of Inheritance

| Type | Supported in Java |
|------|-------------------|
| Single | Yes |
| Multilevel | Yes |
| Hierarchical | Yes |
| Multiple | No (Classes) |
| Hybrid | No (Classes) |

### Diamond Problem ⭐⭐⭐

**Definition:** The Diamond Problem occurs when a class inherits the same method from two parent classes, creating ambiguity.

**Interview Answer:** Java avoids the Diamond Problem by **not supporting multiple inheritance for classes** and uses **interfaces** instead.

---

# super Keyword ⭐⭐

`super` refers to the **parent class**.

## Call Parent Constructor

```java
class Animal {
    Animal() {
        System.out.println("Animal");
    }
}

class Dog extends Animal {
    Dog() {
        super();
        System.out.println("Dog");
    }
}
```

## Call Parent Method

```java
super.eat();
```

## Access Parent Variable

```java
super.name;
```

---

# 3. Polymorphism ⭐⭐⭐

**Polymorphism** means **one method can have multiple implementations depending on the object.**

There are two types.

## Compile-Time Polymorphism (Method Overloading)

Same method name with different parameter lists.

```java
class Calculator {

    int add(int a, int b) {
        return a + b;
    }

    int add(int a, int b, int c) {
        return a + b + c;
    }
}
```

### Rules

- Same method name
- Different parameters
- Return type alone cannot change

Invalid:

```java
int add(int a)
double add(int a)
```

---

## Runtime Polymorphism (Method Overriding)

A child class provides its own implementation of a parent method.

```java
class Animal {
    void sound() {
        System.out.println("Animal");
    }
}

class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Bark");
    }
}
```

Usage:

```java
Animal a = new Dog();
a.sound();
```

Output:

```text
Bark
```

> Runtime polymorphism happens because Java uses **virtual method dispatch**.

---

# Virtual Methods ⭐⭐⭐

A **virtual method** is a method whose implementation is chosen at **runtime** based on the actual object.

In Java, **all non-static, non-final, and non-private methods are virtual by default**.

```java
class Animal {
    void sound() {
        System.out.println("Animal");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Bark");
    }
}

Animal obj = new Dog();
obj.sound();      // Bark
```

**Interview:** Java achieves runtime polymorphism using virtual methods (dynamic method dispatch).

---

# Pure Virtual Methods ⭐⭐⭐

Java doesn't have the `pure virtual` keyword like C++, but an **abstract method** serves the same purpose.

## Abstract Class

```java
abstract class Vehicle {

    abstract void start();
}
```

## Interface

```java
interface Camera {

    void click();
}
```

**Interview:** A pure virtual method in Java is an **abstract method**, commonly declared in abstract classes and interfaces.

---

# Overloading vs Overriding ⭐⭐⭐

| Overloading | Overriding |
|-------------|------------|
| Same class | Parent & Child |
| Compile time | Runtime |
| Different parameters | Same parameters |
| Improves readability | Enables dynamic behavior |

---

# 4. Abstraction ⭐⭐⭐

**Abstraction** means exposing only essential functionality while hiding implementation details.

Java provides abstraction using:

- Abstract Classes
- Interfaces

---

# Abstract Class ⭐⭐⭐

An abstract class can contain both **abstract** and **concrete** methods.

```java
abstract class Vehicle {

    abstract void start();

    void fuel() {
        System.out.println("Petrol");
    }
}
```

Child class:

```java
class Car extends Vehicle {

    @Override
    void start() {
        System.out.println("Car Starts");
    }
}
```

### Features

- Cannot instantiate directly
- Can have constructors
- Can have instance variables
- Supports partial abstraction

---

# Interface ⭐⭐⭐

An interface defines a **contract** that implementing classes must follow.

```java
interface Camera {

    void click();
}
```

Implementation:

```java
class Phone implements Camera {

    @Override
    public void click() {
        System.out.println("Photo");
    }
}
```

### Since Java 8

Interfaces can contain:

- Abstract methods
- ```java
interface Animal {
    void sound();
}

class Dog implements Animal {
    public void sound() {
        System.out.println("Bark");
    }
}
```

- Default methods
- ```java
interface Animal {
    default void sleep() {
        System.out.println("Sleeping");
    }
}

class Dog implements Animal { }

Dog d = new Dog();
d.sleep();
```

- Static methods
- ```java
interface Animal {
    static void info() {
        System.out.println("Animals are living beings");
    }
}

Animal.info();
```


Since Java 9:

- Private method: is used **only inside the interface** to avoid repeating common logic. It cannot be accessed or overridden by implementing classes.

```java
interface Logger {

    default void logInfo() {
        print("INFO");
    }

    default void logError() {
        print("ERROR");
    }

    private void print(String level) {
        System.out.println("Log Level: " + level);
    }
}

class App implements Logger { }

App app = new App();
app.logInfo();   // Log Level: INFO
app.logError();  // Log Level: ERROR
```

> **Interview:** Private methods in interfaces improve code reuse by sharing logic between default and static methods within the interface.

---

# Abstract Class vs Interface ⭐⭐⭐

| Abstract Class | Interface |
|---------------|-----------|
| `extends` | `implements` |
| Has constructors | No constructors |
| Instance variables allowed | Only constants |
| Single inheritance | Multiple interfaces |
| Partial abstraction | Contract |

**Use Abstract Class:** When related classes share state and implementation.

**Use Interface:** When unrelated classes need the same behavior.

---

# this Keyword ⭐⭐

`this` refers to the **current object**.

## Resolve Variable Shadowing

```java
class Student {

    String name;

    Student(String name) {
        this.name = name;
    }
}
```

## Call Another Constructor

```java
class Student {

    Student() {
        this("Unknown");
    }

    Student(String name) {
    }
}
```

---

# Constructors ⭐⭐⭐

A constructor initializes an object.

## Default Constructor

```java
class Student {

    Student() {
        System.out.println("Created");
    }
}
```

## Parameterized Constructor

```java
class Student {

    String name;

    Student(String name) {
        this.name = name;
    }
}
```

### Constructor vs Method

| Constructor | Method |
|-------------|--------|
| Same name as class | Any name |
| No return type | Has return type |
| Auto invoked | Called explicitly |

---

# Object Class ⭐⭐

Every Java class implicitly extends `Object`.

Common methods:

| Method       | Purpose               |
| ------------ | --------------------- |
| `toString()` | String representation |
| `equals()`   | Compare objects       |
| `hashCode()` | Hash value            |
| `getClass()` | Runtime class         |

---

# instanceof ⭐⭐

Checks whether an object belongs to a particular type.

```java
Animal a = new Dog();

System.out.println(a instanceof Dog);
```

Output:

```text
true
```

---

# Upcasting & Downcasting ⭐⭐⭐

## Upcasting

Child → Parent

```java
Dog dog = new Dog();
Animal animal = dog;
```

Safe and implicit.

## Downcasting

Parent → Child

```java
Animal animal = new Dog();
Dog dog = (Dog) animal;
```

Use `instanceof` before downcasting.

---

# Access Modifiers ⭐⭐⭐

| Modifier | Same Class | Package | Subclass | Other Package |
|----------|------------|---------|----------|---------------|
| public | ✅ | ✅ | ✅ | ✅ |
| protected | ✅ | ✅ | ✅ | ❌ |
| default | ✅ | ✅ | ❌ | ❌ |
| private | ✅ | ❌ | ❌ | ❌ |

**Memory Tip:** `private` is the most restrictive, `public` is the least restrictive.

---

# # Shallow Copy vs Deep Copy vs Lazy Copy ⭐⭐⭐

Object copying is a very common Java interview topic. The key difference is **whether nested objects are shared or duplicated**.

---

## Class used in examples

```java
class Address {
    String city;

    Address(String city) {
        this.city = city;
    }
}

class Student {
    String name;
    Address address;

    Student(String name, Address address) {
        this.name = name;
        this.address = address;
    }
}
```

---

# 1. Shallow Copy

A shallow copy creates a **new outer object**, but the nested objects are **shared**.

```java
Address addr = new Address("Delhi");

Student s1 = new Student("Rahul", addr);

// Shallow copy
Student s2 = new Student(s1.name, s1.address);

s2.address.city = "Mumbai";

System.out.println(s1.address.city);   // Mumbai
```

### Memory Representation

- `s1` → Address (Delhi)
- `s2` → **same Address**

Changing the address through one object affects the other.

**Interview:** Shallow copy copies references, not nested objects.

---

# 2. Deep Copy

A deep copy duplicates both the outer object and its nested objects.

```java
Address addr = new Address("Delhi");

Student s1 = new Student("Rahul", addr);

// Deep copy
Student s2 = new Student(
    s1.name,
    new Address(s1.address.city)
);

s2.address.city = "Mumbai";

System.out.println(s1.address.city);   // Delhi
System.out.println(s2.address.city);   // Mumbai
```

### Memory Representation

- `s1` → Address (Delhi)
- `s2` → New Address (Delhi)

Both objects become completely independent.

**Interview:** Deep copy duplicates the complete object graph.

---

# 3. Lazy Copy (Copy-On-Write Concept)

Lazy copy delays creating a new copy **until modification happens**.

Instead of copying immediately:

```java
List<String> list1 = new CopyOnWriteArrayList<>();

list1.add("A");
list1.add("B");

List<String> list2 = list1;
```

A new internal copy is created **only when a write occurs**.

```java
list1.add("C");
```

Readers continue using the old snapshot safely while writers work on a new copy.

> **Interview:** Lazy copy postpones copying until write time to improve performance and thread safety.

---

# Shallow vs Deep vs Lazy

| Feature | Shallow | Deep | Lazy |
|---------|----------|------|------|
| New outer object | ✅ | ✅ | Initially shared |
| Nested objects | Shared | Copied | Copied on write |
| Memory usage | Low | High | Optimized |
| Independent objects | ❌ | ✅ | Eventually |

---

# Object.clone() Method ⭐⭐⭐

`clone()` creates a **field-by-field copy** of an object.

By default, it performs a **shallow copy**.

## Cloneable Interface

A class must implement `Cloneable`; otherwise `clone()` throws `CloneNotSupportedException`.

```java
class Student implements Cloneable {

    String name;

    Student(String name){
        this.name = name;
    }

    @Override
    protected Student clone() throws CloneNotSupportedException {
        return (Student) super.clone();
    }
}
```

Usage:

```java
Student s1 = new Student("Rahul");
Student s2 = s1.clone();

s2.name = "Amit";

System.out.println(s1.name);   // Rahul
```

Since `String` is immutable, this works fine.

---

# clone() with Nested Objects

The default clone is still **shallow**.

```java
class Student implements Cloneable {

    String name;
    Address address;

    @Override
    protected Student clone() throws CloneNotSupportedException {
        return (Student) super.clone();
    }
}
```

```java
Student s2 = s1.clone();

s2.address.city = "Mumbai";

System.out.println(s1.address.city);   // Mumbai
```

Both students still share the same `Address`.

---

# Deep Copy using clone()

Override `clone()` and clone nested objects manually.

```java
class Address implements Cloneable {

    String city;

    @Override
    protected Address clone() throws CloneNotSupportedException {
        return (Address) super.clone();
    }
}

class Student implements Cloneable {

    String name;
    Address address;

    @Override
    protected Student clone() throws CloneNotSupportedException {

        Student copy = (Student) super.clone();

        copy.address = address.clone();

        return copy;
    }
}
```

Now both objects are fully independent.

---

# Cloneable Interface ⭐⭐

`Cloneable` is a **marker interface**.

It contains **no methods** and simply tells JVM that cloning is allowed.

```java
class Employee implements Cloneable {
}
```

Without it:

```java
Employee e2 = e1.clone();   // CloneNotSupportedException
```

### Interview

**Q: Why implement Cloneable?**

**A:** It enables `Object.clone()` to create a copy without throwing `CloneNotSupportedException`.

---

# CopyOnWriteArrayList ⭐⭐⭐

`CopyOnWriteArrayList` is a thread-safe version of `ArrayList`.

Every **write operation creates a new copy** of the underlying array.

```java
CopyOnWriteArrayList<String> list = new CopyOnWriteArrayList<>();

list.add("A");
list.add("B");

for(String s : list){
    System.out.println(s);

    list.add("C");
}
```

Output:

```text
A
B
```

No `ConcurrentModificationException` occurs because iteration happens on a snapshot.

### Internal Working

- Read → Uses existing array
- Write → Creates a new array, modifies it, then replaces the reference

### ArrayList vs CopyOnWriteArrayList

| Feature | ArrayList | CopyOnWriteArrayList |
|---------|-----------|----------------------|
| Thread-safe | ❌ | ✅ |
| Read performance | Fast | Very Fast |
| Write performance | Fast | Slower (copies array) |
| ConcurrentModificationException | Yes | No |
| Best use case | Frequent writes | Frequent reads |

> **Interview:** Use `CopyOnWriteArrayList` when reads are much more frequent than writes, such as configuration caches or observer lists.
---

# Frequently Asked Interview Questions ⭐⭐⭐

### Why doesn't Java support multiple inheritance?

To avoid ambiguity (Diamond Problem); Java uses interfaces instead.

### Can an abstract class have a constructor?

**Yes.** It runs when a child object is created.

### Can constructors be overridden?

**No.** Constructors are not inherited.

### Can static methods be overridden?

**No.** They are hidden, not overridden.

### What is the difference between abstraction and encapsulation?

- **Abstraction:** Hides implementation.
- **Encapsulation:** Hides data.

### What is a virtual method in Java?

A non-static, non-final method whose implementation is resolved at runtime.

### What is a pure virtual method in Java?

An **abstract method** declared in an abstract class or interface.

---

# Revision Checklist

- [ ] Class vs Object
- [ ] Four Pillars of OOP
- [ ] Encapsulation
- [ ] Inheritance
- [ ] Diamond Problem
- [ ] `this` vs `super`
- [ ] Constructor types
- [ ] Method Overloading
- [ ] Method Overriding
- [ ] Virtual Methods
- [ ] Abstract Class vs Interface
- [ ] Upcasting & Downcasting
- [ ] Shallow vs Deep vs Lazy Copy
- [ ] Access Modifiers

---

# What's Next

Continue with **[03. Strings](03-Strings.md)**.