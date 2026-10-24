# JVM Architecture

▸ Overview

JVM (Java Virtual Machine) is the runtime environment that executes Java bytecode.

Java source code is not directly executed by the JVM.

The basic flow is:

    Java Source Code
          |
          v
       javac
          |
          v
     Bytecode (.class)
          |
          v
          JVM
          |
          v
      Machine Code

Interview One-Liner:

> JVM is the runtime engine that loads, verifies, and executes Java bytecode.

---

▸ Why JVM?

Java follows:

    Write Once, Run Anywhere

The Java compiler converts source code into platform-independent bytecode.

The JVM implementation for each operating system executes that bytecode.

    Java Code
        |
        v
    Bytecode
        |
        +----------+----------+
        |          |          |
       JVM        JVM        JVM
     Windows      Linux      macOS

The bytecode remains the same, while the JVM implementation is platform-specific.

---

▸ JVM Main Components

The JVM can be broadly divided into:

    JVM
     |
     +-- Class Loader Subsystem
     |
     +-- Runtime Data Areas
     |      |
     |      +-- Heap
     |      +-- Stack
     |      +-- Metaspace
     |      +-- PC Register
     |      +-- Native Method Stack
     |
     +-- Execution Engine
     |      |
     |      +-- Interpreter
     |      +-- JIT Compiler
     |      +-- Garbage Collector
     |
     +-- JNI
     |
     +-- Native Libraries

---

▸ Class Loader Subsystem

The Class Loader Subsystem loads `.class` files into JVM memory.

Its major responsibilities are:

1. Loading
2. Linking
3. Initialization

Flow:

    .class file
        |
        v
    Class Loader
        |
        v
    Loading
        |
        v
    Linking
        |
        +-- Verification
        +-- Preparation
        +-- Resolution
        |
        v
    Initialization
        |
        v
    Class ready for execution

---

▸ Loading

Loading means finding the binary representation of a class and creating the corresponding class representation inside the JVM.

Example:

    Student.class

The ClassLoader loads the class when it is needed.

---

▸ Linking

Linking consists of:

    Linking
       |
       +-- Verification
       +-- Preparation
       +-- Resolution

---

▸ Verification

The JVM verifies that the bytecode is valid and follows JVM rules.

It helps ensure:

- Bytecode is structurally valid.
- Types are used correctly.
- Illegal operations are prevented.
- Bytecode does not violate JVM security constraints.

Interview One-Liner:

> Verification checks whether loaded bytecode is valid and safe to execute.

---

▸ Preparation

Preparation allocates memory for class-level static fields and gives them default values.

Example:

    class Student {
        static int count = 10;
    }

During preparation:

    count = 0

Later, during initialization:

    count = 10

Important:

Preparation assigns default values.

Initialization executes the actual static initialization logic.

---

▸ Resolution

Resolution converts symbolic references in the runtime constant pool into direct references used by the JVM.

For example, a bytecode instruction may contain a symbolic reference to a method.

Resolution can convert that symbolic information into a reference to the actual method/class.

Important:

Resolution may occur lazily depending on the JVM implementation.

---

▸ Initialization

Initialization executes class initialization logic.

This includes:

- Static variable initializers
- Static initialization blocks

Example:

    class Test {

        static int value = 10;

        static {
            System.out.println("Class initialized");
        }
    }

Initialization happens when the JVM initializes the class.

---

▸ Class Initialization Example

    class Test {

        static int x = 10;

        static {
            x = 20;
        }
    }

Conceptually:

    Loading
       |
    Linking
       |
    Initialization
       |
       +-- x = 10
       |
       +-- static block
              |
              v
             x = 20

Final value:

    x = 20

---

▸ ClassLoader Hierarchy

Modern Java uses a delegation model.

Common ClassLoaders include:

    Bootstrap ClassLoader
            |
            v
    Platform ClassLoader
            |
            v
    Application ClassLoader

Important:

Bootstrap ClassLoader is implemented by the JVM and is not a normal Java classloader object.

---

▸ Bootstrap ClassLoader

Loads core Java classes.

Examples include classes from the Java runtime such as:

    java.lang.String
    java.lang.Object
    java.util.ArrayList

It loads classes from the runtime platform modules.

---

▸ Platform ClassLoader

Loads platform classes that are not part of the core bootstrap-loaded classes.

It replaced the older "Extension ClassLoader" terminology used before Java 9.

---

▸ Application ClassLoader

Also called the System ClassLoader.

It loads application classes from the application's classpath/module path.

Example:

    com.example.Student

---

▸ Parent Delegation Model

When a ClassLoader is asked to load a class, it generally delegates the request to its parent first.

Conceptually:

    Application ClassLoader
            |
            v
    Platform ClassLoader
            |
            v
    Bootstrap ClassLoader

If the parent cannot load the class, the child attempts to load it.

Interview One-Liner:

> Parent delegation means a classloader first asks its parent to load a class before attempting to load it itself.

---

▸ Why Parent Delegation?

It provides:

1. Consistent loading of core Java classes.
2. Protection against replacing core classes with application classes.
3. Avoidance of unnecessary duplicate class loading.

For example, an application cannot simply provide its own version of:

    java.lang.String

and expect it to replace the JDK's String class.

---

▸ Class Identity

In Java:

    Class Identity = Fully Qualified Class Name + ClassLoader

Therefore, the same class name loaded by two different ClassLoaders can represent two different types to the JVM.

This is important in:

- Application servers
- Plugins
- Containers
- Modular applications

---

▸ ClassNotFoundException vs NoClassDefFoundError

### ClassNotFoundException

Usually occurs when code explicitly tries to load a class and the class cannot be found.

Example:

    Class.forName("com.example.Student");

If the class cannot be found:

    ClassNotFoundException

### NoClassDefFoundError

Usually occurs when a class that was available during compilation/runtime setup cannot be loaded when it is actually needed.

Interview One-Liner:

> ClassNotFoundException is generally a checked exception related to explicitly loading a missing class, while NoClassDefFoundError is a linkage error indicating a required class definition could not be found at runtime.

---

▸ JVM Runtime Data Areas

The JVM runtime memory is broadly divided into:

    Runtime Data Areas
          |
          +-- Heap
          +-- JVM Stack
          +-- PC Register
          +-- Native Method Stack
          +-- Method Area
                 |
                 v
              Metaspace
              in modern HotSpot

Some areas are shared between threads while others are created per thread.

### Shared

    Heap
    Method Area / Metaspace

### Per Thread

    JVM Stack
    PC Register
    Native Method Stack

---

▸ Execution Engine

The Execution Engine executes JVM bytecode.

Major components include:

    Execution Engine
         |
         +-- Interpreter
         |
         +-- JIT Compiler
         |
         +-- Garbage Collection

---

▸ Interpreter

The interpreter executes bytecode instruction by instruction.

Advantages:

- Fast startup.
- No compilation overhead initially.

Disadvantage:

- Repeatedly executing the same bytecode through interpretation can be slower than optimized machine code.

---

▸ JIT Compiler

JIT means:

    Just-In-Time Compiler

It identifies frequently executed code and compiles it into optimized native machine code.

Flow:

    Bytecode
       |
       v
    Interpreter
       |
       v
    Frequently executed code
       |
       v
    JIT Compiler
       |
       v
    Native Machine Code

This is covered in detail in:

    03-JVM-Execution.md

---

▸ PC Register

PC stands for Program Counter.

Each JVM thread has its own PC register.

It keeps track of the current JVM instruction being executed by that thread.

Conceptually:

    Thread
       |
       +-- PC Register
       |
       +-- JVM Stack
       |
       +-- Native Method Stack

---

▸ Native Method Stack

The Native Method Stack supports execution of native methods.

Native methods are methods implemented in languages such as C/C++ and accessed through mechanisms such as JNI.

---

▸ JNI

JNI stands for:

    Java Native Interface

It allows Java code to interact with native code.

Example use cases:

- Operating-system-specific APIs
- Native libraries
- Performance-sensitive native functionality
- Existing C/C++ libraries

---

▸ JVM Architecture Summary

    Java Source
         |
         v
       javac
         |
         v
      Bytecode
         |
         v
    Class Loader
         |
         v
    Runtime Data Areas
         |
         v
    Execution Engine
         |
         +-- Interpreter
         |
         +-- JIT Compiler
         |
         v
    Native Machine Code

---

▸ Interview Questions

### What is JVM?

> JVM is the runtime environment that loads, verifies, and executes Java bytecode.

### Is JVM platform independent?

> Java bytecode is platform independent, but the JVM implementation is platform specific.

### What are the main components of JVM?

> Class Loader Subsystem, Runtime Data Areas, Execution Engine, JNI, and Native Libraries.

### What are the three main steps of ClassLoader Subsystem?

> Loading, Linking, and Initialization.

### What are the steps in Linking?

> Verification, Preparation, and Resolution.

### What is parent delegation?

> A classloader delegates class loading to its parent before attempting to load the class itself.

### What are the common ClassLoaders?

> Bootstrap, Platform, and Application ClassLoader.

### What is JIT?

> JIT compiles frequently executed bytecode into optimized native machine code at runtime.

### Which JVM memory areas are shared?

> Heap and the method-area-related class metadata are shared among threads.

### Which JVM memory areas are thread-local?

> JVM Stack, PC Register, and Native Method Stack are associated with individual threads.

---

▸ Final One-Liner

> JVM loads Java classes through the ClassLoader subsystem, stores runtime data in memory areas, and executes bytecode through the Execution Engine using interpretation and JIT compilation.