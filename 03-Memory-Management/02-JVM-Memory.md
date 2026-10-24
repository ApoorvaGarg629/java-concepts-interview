# JVM Memory

▸ Overview

JVM memory is divided into different runtime data areas.

The most important areas for interviews are:

    JVM Memory
        |
        +-- Heap
        |
        +-- Stack
        |
        +-- Metaspace
        |
        +-- PC Register
        |
        +-- Native Method Stack

The most commonly discussed areas are:

    Heap
    Stack
    Metaspace

---

▸ Shared vs Thread-Local Memory

### Shared Between Threads

    Heap
    Metaspace / Method Area

### Per Thread

    JVM Stack
    PC Register
    Native Method Stack

Conceptually:

    JVM
     |
     +-----------------------+
     |                       |
     v                       v
   Shared                 Per Thread
   Memory                  Memory
     |                       |
     +-- Heap                +-- Stack
     +-- Metaspace           +-- PC Register
                             +-- Native Stack

---

▸ Heap

The Heap is the runtime memory area where Java objects and arrays are allocated.

Example:

    Student student = new Student();

The `new Student()` object is allocated on the Heap.

Conceptually:

    Stack
      |
      | student reference
      v
    Heap
      |
      v
    Student Object

Important:

The reference variable and the object are different things.

    Student student = new Student();

    student
       |
       | reference
       v
    Student object

The reference may be stored in a stack frame when it is a local variable, while the object is allocated on the Heap.

---

▸ Heap is Shared

The Heap is shared among JVM threads.

Example:

    Thread 1
        |
        +------+
               |
    Thread 2  |----> Same Heap
        |      |
        +------+

Therefore, multiple threads can access the same objects.

This is one reason synchronization and the Java Memory Model are important.

---

▸ Heap Generations

In generational garbage collectors, the Heap is commonly organized conceptually into:

    Heap
      |
      +-- Young Generation
      |      |
      |      +-- Eden
      |      +-- Survivor
      |          |
      |          +-- S0
      |          +-- S1
      |
      +-- Old Generation

The exact layout depends on the garbage collector.

Important:

Do not assume every modern GC uses exactly the same physical generation layout.

---

▸ JVM Stack

Each JVM thread has its own JVM Stack.

Example:

    Thread 1
       |
       +-- Stack 1

    Thread 2
       |
       +-- Stack 2

Each method invocation creates a Stack Frame.

Example:

    main()
      |
      v
    calculate()
      |
      v
    print()

The stack conceptually contains:

    Frame: print()
    Frame: calculate()
    Frame: main()

The most recently invoked method is at the top.

---

▸ Stack Stores

A stack frame can contain:

- Local variables
- Operand stack
- Reference to runtime constant pool
- Return information
- Data needed for method execution

Important:

The exact JVM specification describes stack frames abstractly; the physical implementation can vary.

---

▸ Stack Frame

Every method invocation creates a new Stack Frame.

Example:

    public static void main(String[] args) {
        calculate();
    }

    static void calculate() {
        int x = 10;
    }

Conceptually:

    main()
      |
      v
    calculate()

During `calculate()`:

    JVM Stack
    ----------------
    calculate() frame
    main() frame
    ----------------

When `calculate()` returns:

    JVM Stack
    ----------------
    main() frame
    ----------------

The `calculate()` frame is removed.

---

▸ Local Variables

Local variables are stored in the local variable array associated with a stack frame.

Example:

    static void test() {
        int x = 10;
        int y = 20;
    }

Conceptually:

    test() Stack Frame
       |
       +-- x = 10
       +-- y = 20

---

▸ Reference Variable vs Object

Example:

    Student s = new Student();

Conceptually:

    Stack Frame
       |
       +-- s
            |
            | reference
            v
          Heap
            |
            v
        Student Object

Important Interview Point:

> The local reference can be stored in the stack frame, while the actual object is allocated on the Heap.

---

▸ Primitive vs Reference

Example:

    int x = 10;

The local primitive value is stored in the stack frame when it is a local variable.

Example:

    Student s = new Student();

The local reference is stored in the stack frame, while the object(and its instance variables) is on the Heap.

Important:

Do not say:

> All primitives are stored on the Stack.

That is too broad.

Storage depends on where the variable exists and JVM implementation details.

---

▸ Stack Overflow

If a thread's JVM stack cannot accommodate additional frames, the JVM throws:

    StackOverflowError

Common cause:

Infinite recursion.

Example:

    static void test() {
        test();
    }

    public static void main(String[] args) {
        test();
    }

Flow:

    test()
      |
    test()
      |
    test()
      |
    test()
      |
      ...
      |
      v
    StackOverflowError

---

▸ OutOfMemoryError

If the JVM cannot allocate memory for an object or another required runtime structure, it may throw:

    OutOfMemoryError

A common example is Heap exhaustion.

Example:

    List<byte[]> list = new ArrayList<>();

    while (true) {
        list.add(new byte[1024 * 1024]);
    }

Eventually:

    OutOfMemoryError

---

▸ Stack vs Heap

| Feature       | Stack                  | Heap                                                |
| ------------- | ---------------------- | --------------------------------------------------- |
| Scope         | Per thread             | Shared                                              |
| Main purpose  | Method execution       | Object allocation                                   |
| Contains      | Stack frames           | Objects and arrays                                  |
| Thread safety | Naturally thread-local | Shared between threads                              |
| Lifetime      | Frame lifetime         | Until object becomes unreachable and GC reclaims it |
| Common error  | StackOverflowError     | OutOfMemoryError                                    |

Interview One-Liner:

> Stack is thread-local and stores method execution frames, while Heap is shared and stores objects and arrays.

---

▸ Metaspace

Metaspace stores class metadata in modern HotSpot JVMs.

It replaced the PermGen area starting with Java 8.

Examples of class-related information include:

- Class metadata
- Method metadata
- Runtime constant-pool-related information
- Information required to represent loaded classes

Important:

Metaspace is not the same thing as the Java Heap.

---

▸ PermGen vs Metaspace

Before Java 8:

    Class Metadata
         |
         v
      PermGen

Java 8 onwards:

    Class Metadata
         |
         v
     Metaspace

Main difference:

> Metaspace uses native memory rather than being part of the traditional Java Heap.

---

▸ Why Was PermGen Removed?

PermGen had a fixed-size memory region inside the JVM's managed memory model.

Metaspace uses native memory and can grow as needed, subject to available native memory and configured limits.

Benefits include:

- Better flexibility for class metadata.
- Reduced fixed-size PermGen tuning issues.

Important:

Metaspace can still run out of memory.

Example:

    OutOfMemoryError:
    Metaspace

---

▸ Stack Frame Components

A stack frame conceptually contains:

    Stack Frame
       |
       +-- Local Variable Array
       |
       +-- Operand Stack
       |
       +-- Reference to Runtime Constant Pool
       |
       +-- Other method-execution information

---

▸ Operand Stack

The operand stack is used by JVM bytecode instructions for intermediate calculations.

Example:

    int result = 10 + 20;

Conceptually:

    Push 10
       |
    Push 20
       |
    Add
       |
    Result = 30

The operand stack is part of the Stack Frame.

---

▸ Runtime Constant Pool Reference

Each stack frame contains a reference to the runtime constant pool of the class/type of the current method.

The runtime constant pool contains symbolic information used during execution.

---

▸ PC Register

Each JVM thread has its own Program Counter register.

It identifies the current JVM instruction being executed.

    Thread
       |
       +-- PC Register
       |
       +-- JVM Stack

---

▸ Native Method Stack

Each JVM thread may have a native method stack used when executing native methods.

Native methods are implemented outside normal Java bytecode execution.

---

▸ Memory Error Summary

### Stack Overflow

Cause:

    Too many nested method calls

Typical error:

    StackOverflowError

### Heap Exhaustion

Cause:

    Too many live objects / insufficient heap memory

Typical error:

    OutOfMemoryError

### Metaspace Exhaustion

Cause:

    Excessive class metadata usage

Typical error:

    OutOfMemoryError: Metaspace

---

▸ Interview Questions

### Where are objects stored?

> Objects and arrays are allocated in the Heap.

### Is the Heap shared between threads?

> Yes, the Heap is shared between JVM threads.

### Is Stack shared?

> No. Each thread has its own JVM Stack.

### What is a Stack Frame?

> A Stack Frame is created for each method invocation and contains the data required to execute that method.

### What is stored in a Stack Frame?

> Local variables, operand stack, runtime constant pool reference, and method-execution information.

### What is Metaspace?

> Metaspace is the memory area used by HotSpot to store class metadata and uses native memory.

### What replaced PermGen?

> Metaspace replaced PermGen starting with Java 8.

### What causes StackOverflowError?

> Excessive stack-frame creation, commonly due to infinite or very deep recursion.

### What causes OutOfMemoryError?

> The JVM cannot allocate required memory, commonly due to Heap or Metaspace exhaustion.

---

▸ Final One-Liners

> Heap = shared memory for objects and arrays.

> Stack = thread-local memory containing method-execution frames.

> Stack Frame = execution context created for each method invocation.

> Metaspace = native-memory area used by HotSpot for class metadata.

> StackOverflowError = stack cannot accommodate more frames.

> OutOfMemoryError = JVM cannot allocate required memory.