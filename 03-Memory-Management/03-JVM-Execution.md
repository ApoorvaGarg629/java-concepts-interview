# JVM Execution and Object Creation

▸ Overview

After classes are loaded, the JVM executes Java bytecode using the Execution Engine.

The Execution Engine mainly involves:

    Execution Engine
         |
         +-- Interpreter
         |
         +-- JIT Compiler
         |
         +-- Garbage Collector

This file focuses on:

- Object Creation
- Interpreter
- JIT Compiler
- Runtime execution flow

---

▸ Java Program Execution Flow

Example:

    Student s = new Student();

The overall execution flow is conceptually:

    .java
      |
      v
    javac
      |
      v
    .class bytecode
      |
      v
    ClassLoader
      |
      v
    JVM Memory
      |
      v
    Execution Engine
      |
      +-- Interpreter
      |
      +-- JIT
      |
      v
    Native Machine Code

---

▸ Interpreter

The Interpreter executes bytecode instructions one at a time.

Conceptually:

    Bytecode
       |
       v
    Interpreter
       |
       +-- instruction 1
       +-- instruction 2
       +-- instruction 3
       +-- instruction 4

Advantages:

- Fast startup.
- No initial compilation cost.

Disadvantage:

Frequently executed code may be slower if it continues to be interpreted repeatedly.

---

▸ JIT Compiler

JIT stands for:

    Just-In-Time Compiler

The JIT compiler identifies frequently executed code and compiles it into native machine code.

Instead of repeatedly interpreting the same bytecode:

    Bytecode
       |
       v
    Interpreter
       |
       v
    Frequently executed code
       |
       v
    JIT compilation
       |
       v
    Native Machine Code

The native code can then execute directly on the underlying processor.

---

▸ Why JIT?

Suppose a method is called thousands of times.

Without JIT:

    Method
      |
      v
    Interpret
      |
      v
    Interpret
      |
      v
    Interpret
      |
      v
      ...

With JIT:

    Method
      |
      v
    Execute frequently
      |
      v
    Detect hot code
      |
      v
    JIT compile
      |
      v
    Optimized native code
      |
      v
    Faster repeated execution

---

▸ Hot Code

Frequently executed code is often called:

    Hot Code

The JVM collects runtime profiling information to identify code that is worth optimizing.

Examples:

- Frequently called methods.
- Frequently executed loops.

---

▸ JIT Optimizations

JIT can perform runtime optimizations such as:

- Method inlining
- Dead code elimination
- Loop optimizations
- Escape analysis
- Constant propagation
- Other architecture-specific optimizations

Interview Point:

> JIT uses runtime information to optimize frequently executed code.

---

▸ Method Inlining

Suppose:

    int add(int a, int b) {
        return a + b;
    }

and:

    int result = add(10, 20);

The JIT may inline the method so that the call overhead can be reduced.

Conceptually:

    add(10, 20)

can become similar to:

    10 + 20

The actual optimization depends on the JVM and runtime conditions.

---

▸ Object Creation

Example:

    Student s = new Student();

There are two important concepts:

    s
    |
    +-- Reference variable

    new Student()
    |
    +-- Object

Conceptually:

    Stack Frame
       |
       +-- s
            |
            v
          Heap
            |
            v
        Student Object

---

▸ Steps of Object Creation

For:

    Student s = new Student();

Conceptually:

1. JVM evaluates the `new` operation.
2. Memory is allocated for the object.
3. Object fields receive default values.
4. Object initialization occurs.
5. Constructor executes.
6. Reference to the object is returned.
7. Reference is assigned to `s`.

Flow:

    new Student()
          |
          v
    Allocate memory
          |
          v
    Default field values
          |
          v
    Object initialization
          |
          v
    Constructor
          |
          v
    Object reference
          |
          v
          s

---

▸ Default Values During Object Creation

Before explicit initialization, instance fields receive default values.

| Type | Default |
|---|---|
| int | 0 |
| long | 0L |
| float | 0.0f |
| double | 0.0d |
| boolean | false |
| char | '\u0000' |
| Reference | null |

Example:

    class Student {
        int age;
        String name;
    }

Before explicit initialization:

    age = 0
    name = null

---

▸ Constructor Execution

Example:

    class Student {

        int age;

        Student() {
            age = 20;
        }
    }

When:

    Student s = new Student();

The object is created and the constructor initializes its state.

Final:

    s.age == 20

---

▸ Object Initialization Order

For a normal object creation involving inheritance, initialization broadly follows:

    Parent class initialization
          |
          v
    Parent instance initialization
          |
          v
    Parent constructor
          |
          v
    Child instance initialization
          |
          v
    Child constructor

A child constructor implicitly or explicitly invokes the parent constructor.

---

▸ new vs Constructor

These are not the same concept.

`new`:

> Allocates memory for the object and starts object creation.

Constructor:

> Initializes the newly created object.

Example:

    Student s = new Student();

`new` and the constructor work together, but they perform different roles.

---

▸ Object Creation and Heap

Objects are generally allocated on the Heap.

Example:

    Student s = new Student();

Conceptually:

    Stack
      |
      +-- s
           |
           v
         Heap
           |
           v
       Student object

Important:

Modern JVMs can perform optimizations such as escape analysis, so the physical allocation behavior may differ from this simplified model.

Interview Answer:

> Java objects are conceptually allocated on the Heap, although JVM optimizations can eliminate or transform an allocation in some cases.

---

▸ JIT and Object Allocation

The JIT compiler can optimize object-related code.

One important optimization is:

    Escape Analysis

If an object does not escape a method or thread, the JIT may optimize its allocation.

Possible optimizations include:

- Scalar replacement
- Allocation elimination
- Lock elimination

Do not assume every object is physically allocated on the Heap exactly as written in source code; JVM optimizations can change the physical implementation.

---

▸ Interpreter vs JIT

| Feature | Interpreter | JIT |
|---|---|---|
| Execution | Bytecode instruction by instruction | Compiled native code |
| Startup | Fast | Requires compilation |
| Repeated execution | Usually slower | Usually faster after optimization |
| Optimization | Limited | Extensive runtime optimization |
| Main purpose | Quick execution/startup | Optimize hot code |

Interview One-Liner:

> Interpreter provides fast startup by executing bytecode directly, while JIT compiles frequently executed code into optimized native machine code.

---

▸ JIT Compilation Flow

    Bytecode
       |
       v
    Interpreter
       |
       v
    Runtime profiling
       |
       v
    Hot code detected
       |
       v
    JIT Compiler
       |
       v
    Optimized Native Code

---

▸ Compilation vs Execution

Java uses multiple stages.

### Compile Time

    .java
       |
       v
      javac
       |
       v
    .class

### Runtime

    .class
       |
       v
      JVM
       |
       v
    Interpreter / JIT
       |
       v
    Native execution

Important:

`javac` and JIT perform different jobs.

`javac`:

> Java source code to JVM bytecode.

JIT:

> Frequently executed JVM bytecode to optimized native machine code at runtime.

---

▸ Interview Questions

### What is JIT?

> JIT is a runtime compiler that converts frequently executed bytecode into optimized native machine code.

### Why is JIT needed?

> To improve performance of frequently executed code by avoiding repeated interpretation and applying runtime optimizations.

### What is hot code?

> Code that executes frequently enough to be considered for JIT optimization.

### What is object creation in Java?

> Object creation involves allocating storage, initializing fields, executing initialization logic and constructors, and producing an object reference.

### Where are objects created?

> Conceptually, Java objects are allocated on the Heap.

### Is every object physically guaranteed to be on the Heap?

> No. JVM optimizations such as escape analysis and scalar replacement can eliminate or transform an allocation.

### What is the difference between javac and JIT?

> `javac` converts Java source code into bytecode at compile time, while JIT converts frequently executed bytecode into optimized native code at runtime.

### What is method inlining?

> JIT replaces a method call with the method's body when beneficial, reducing call overhead and enabling further optimization.

---

▸ Final One-Liners

> Interpreter executes bytecode instruction by instruction.

> JIT compiles frequently executed code into optimized native machine code.

> Hot code = frequently executed code that can benefit from JIT optimization.

> `javac` converts source code to bytecode; JIT converts hot bytecode to native code.

> `new` starts object creation and allocation; the constructor initializes the object.

> Objects are conceptually allocated on the Heap, but JVM optimizations can eliminate or transform allocations.