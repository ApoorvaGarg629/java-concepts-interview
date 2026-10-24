# Java Memory Model (JMM)

▸ Overview

The Java Memory Model (JMM) defines how threads interact through memory.

It specifies rules for:

- Visibility
- Ordering
- Atomicity
- Synchronization
- Happens-Before relationships

The JMM is especially important in multithreading.

Interview One-Liner:

> JMM defines the rules for how Java threads see and interact with shared memory and how operations are ordered.

---

▸ Why JMM?

Consider:

    boolean running = true;

Thread 1:

    while (running) {
        // work
    }

Thread 2:

    running = false;

Without proper synchronization, Thread 1 is not guaranteed to immediately observe the update made by Thread 2.

This is a visibility problem.

The JMM defines how such interactions are handled.

---

▸ Three Important Concepts

When discussing JMM, remember:

    JMM
     |
     +-- Visibility
     |
     +-- Ordering
     |
     +-- Atomicity

---

▸ Visibility

Visibility means:

> When one thread updates shared data, other threads can see the updated value.

Example:

    int count = 0;

Thread 1:

    count = 10;

Thread 2:

    System.out.println(count);

Without appropriate synchronization, Thread 2 is not guaranteed to observe the latest value.

---

▸ volatile

`volatile` provides visibility guarantees for reads and writes of the variable and imposes ordering constraints defined by the JMM.

Example:

    volatile boolean running = true;

Thread 1:

    while (running) {
        // work
    }

Thread 2:

    running = false;

The volatile write to `running` happens-before subsequent reads of `running` that observe that synchronization relationship.

Important:

`volatile` does NOT make every compound operation atomic.

---

▸ volatile Is Not Enough for Compound Operations

Example:

    volatile int count = 0;

    count++;

This is not atomic.

Conceptually:

    read count
       |
       v
    add 1
       |
       v
    write count

Two threads can interleave these operations.

Example:

    Thread 1       Thread 2

    read 0
                   read 0
    write 1
                   write 1

Final:

    1

Expected:

    2

Therefore:

> `volatile` provides visibility and ordering guarantees, but does not make compound operations such as `count++` atomic.

---

▸ Atomicity

An operation is atomic when it appears to occur as one indivisible operation.

Examples of commonly atomic operations include:

- Reading/writing a reference
- Reading/writing many primitive types under JMM guarantees

But compound operations such as:

    count++

are not atomic.

---

▸ synchronized

`synchronized` provides mutual exclusion and memory-visibility guarantees.

Example:

    synchronized void increment() {
        count++;
    }

Only one thread at a time can execute the synchronized method on the same monitor.

---

▸ synchronized and Visibility

When a thread releases a monitor by exiting a synchronized block/method, its writes become visible to a thread that subsequently acquires the same monitor.

This is part of the JMM's happens-before rules.

---

▸ Race Condition

A race condition occurs when the result depends on the timing/interleaving of multiple threads accessing shared mutable state.

Example:

    count++;

Two threads execute:

    Thread 1: count++
    Thread 2: count++

Without proper synchronization, updates can be lost.

---

▸ Happens-Before

Happens-Before is one of the most important concepts in the JMM.

If:

    A happens-before B

then the effects of A are guaranteed to be visible to B according to the JMM, and the required ordering relationship is established.

Important:

"Happens-before" does not necessarily mean A physically executes earlier in wall-clock time.

Interview One-Liner:

> Happens-Before is a JMM ordering and visibility relationship that guarantees the effects of one action are visible to another action.

---

▸ Important Happens-Before Rules

### Program Order Rule

Within a single thread, each action happens-before a subsequent action in that thread.

Example:

    int x = 10;
    int y = x;

The write to `x` happens-before the subsequent read in program order.

---

### Monitor Lock Rule

An unlock on a monitor happens-before every subsequent lock on that same monitor.

Example:

    synchronized (lock) {
        value = 10;
    }

Another thread:

    synchronized (lock) {
        System.out.println(value);
    }

The unlock by the first thread happens-before the subsequent lock by the second thread on the same monitor.

---

### volatile Rule

A write to a volatile variable happens-before every subsequent read of that same variable.

Example:

    volatile boolean ready;

Thread 1:

    ready = true;

Thread 2:

    if (ready) {
        // sees the synchronization effect
    }

---

### Thread Start Rule

A call to:

    thread.start();

happens-before actions in the started thread.

Example:

    int x = 10;

    Thread t = new Thread(() -> {
        System.out.println(x);
    });

    t.start();

The actions before `start()` are ordered before actions in the started thread.

---

### Thread Join Rule

All actions in a thread happen-before another thread successfully returns from `join()` on that thread.

Example:

    Thread t = new Thread(() -> {
        result = 100;
    });

    t.start();
    t.join();

    System.out.println(result);

After `join()` returns, the joining thread is guaranteed to see the actions performed by the completed thread according to the happens-before rule.

---

### Transitivity Rule

If:

    A happens-before B

and:

    B happens-before C

then:

    A happens-before C

This is called transitivity.

---

▸ Happens-Before Example

    Thread 1

    data = 100;
       |
       v
    ready = true
    volatile


    Thread 2

    if (ready) {
        use(data);
    }

The volatile write to `ready` happens-before a subsequent read of `ready` that observes it.

Therefore, the earlier write to `data` is also ordered before the read of `data` through the happens-before chain.

---

▸ Why volatile Example Works

Code:

    int data = 100;
    volatile boolean ready = false;

Thread 1:

    data = 200;
    ready = true;

Thread 2:

    if (ready) {
        System.out.println(data);
    }

Happens-Before:

    data = 200
        |
        v
    ready = true
        |
        | volatile
        v
    read ready
        |
        v
    read data

The volatile write establishes the necessary visibility/order relationship.

---

▸ synchronized Happens-Before Example

    int data = 0;

Thread 1:

    synchronized (lock) {
        data = 100;
    }

Thread 2:

    synchronized (lock) {
        System.out.println(data);
    }

The unlock from Thread 1 happens-before the subsequent lock by Thread 2 on the same monitor.

Therefore, Thread 2 can see the write to `data`.

---

▸ final Fields and JMM

The Java Memory Model provides special initialization-safety guarantees for properly constructed objects containing `final` fields.

Example:

    class Student {

        private final int age;

        Student(int age) {
            this.age = age;
        }
    }

When the object is constructed correctly and `this` does not escape during construction, other threads receive stronger guarantees regarding the initialized values of final fields.

Important:

Do not let `this` escape from the constructor.

---

▸ this Escape

Bad example:

    class Test {

        Test() {
            SomeRegistry.register(this);
        }
    }

The object becomes visible to other code before construction has completed.

This can cause unsafe publication issues.

Interview Point:

> Do not publish `this` from a constructor because another thread may observe the object before construction is complete.

---

▸ Safe Publication

An object is safely published when another thread is guaranteed to see its properly initialized state.

Common mechanisms include:

- Synchronization
- Volatile references
- Static initialization
- Final-field initialization guarantees
- Concurrent collection mechanisms
- Proper thread start/join relationships

---

▸ Atomicity vs Visibility vs Ordering

| Concept | Meaning |
|---|---|
| Atomicity | Operation appears indivisible |
| Visibility | One thread sees another thread's update |
| Ordering | Operations appear in the required order under JMM rules |

Example:

    volatile

Provides:

    Visibility
    Ordering

But not general:

    Atomicity of compound operations

Example:

    synchronized

Provides:

    Mutual exclusion
    Visibility
    Ordering

---

▸ volatile vs synchronized

| Feature | volatile | synchronized |
|---|---|---|
| Visibility | Yes | Yes |
| Ordering | Yes | Yes |
| Mutual exclusion | No | Yes |
| Compound operation atomicity | No | Yes |
| Locking | No | Yes |

Interview One-Liner:

> Use `volatile` for visibility/order of a shared variable when operations do not require mutual exclusion; use `synchronized` when you need atomic compound operations and mutual exclusion.

---

▸ JMM and CPU Caches

Modern CPUs use multiple levels of caches.

Conceptually:

    Main Memory
       |
       +----------------+
       |                |
       v                v
    CPU Core 1        CPU Core 2
       |                |
      Cache            Cache

The JMM defines the rules that Java programs can rely on despite hardware-level caching and instruction reordering.

Important:

Do not reduce the JMM to simply "variables are stored in main memory and each thread has a local copy."

That is an oversimplification.

---

▸ Instruction Reordering

Compilers and processors can reorder operations as long as single-threaded semantics are preserved.

Example:

    x = 1;
    y = 2;

The JVM/CPU may internally reorder operations if it does not violate the required semantics.

The JMM defines what reorderings are observable between threads.

Synchronization constructs establish the required ordering guarantees.

---

▸ Happens-Before vs Program Order

Program order:

> Actions in the same thread follow the program's execution order as defined by the JMM.

Happens-before:

> A stronger cross-thread visibility and ordering relationship established by JMM rules.

Example:

    Thread 1
    write data
       |
       v
    volatile write

       |

       v

    Thread 2
    volatile read
       |
       v
    read data

The happens-before relationship connects the relevant actions across threads.

---

▸ Happens-Before Quick Diagram

    A
    |
    | happens-before
    v
    B

Guarantee:

    Effects of A
        |
        v
    Visible/ordered before B

Important:

    happens-before
        !=
    necessarily executes earlier in physical time

---

▸ Common Happens-Before Rules to Memorize

Remember:

    Program Order
        |
        v
    Unlock
        |
        v
    Lock
        |
        v
    Volatile Write
        |
        v
    Volatile Read
        |
        v
    Thread Start
        |
        v
    Thread Join
        |
        v
    Transitivity

More precisely:

- Program-order rule.
- Monitor unlock happens-before subsequent lock on the same monitor.
- Volatile write happens-before subsequent read of the same variable.
- Actions before `Thread.start()` happen-before actions in the started thread.
- Actions in a thread happen-before another thread successfully returns from `join()`.
- Happens-before is transitive.

---

▸ Interview Questions

### What is JMM?

> JMM defines the rules for visibility, ordering, and interaction between threads accessing shared memory.

### What is happens-before?

> Happens-before is a JMM relationship that guarantees visibility and ordering between actions.

### Does happens-before mean one operation physically happens earlier?

> No. It defines a guaranteed ordering and visibility relationship, not necessarily wall-clock execution order.

### Does volatile make count++ thread-safe?

> No. `volatile` does not make compound operations such as `count++` atomic.

### What does volatile provide?

> Visibility and ordering guarantees for accesses to the volatile variable.

### What does synchronized provide?

> Mutual exclusion plus visibility and ordering guarantees.

### What is a race condition?

> A race condition occurs when the result depends on the timing or interleaving of concurrent operations on shared state.

### What is safe publication?

> Safe publication ensures that another thread sees an object's properly initialized state.

### What is the Thread Start happens-before rule?

> Actions before `Thread.start()` happen-before actions executed by the started thread.

### What is the Thread Join happens-before rule?

> Actions in a thread happen-before another thread successfully returns from `join()` on that thread.

### What is transitivity?

> If A happens-before B and B happens-before C, then A happens-before C.

---

▸ Common Interview Traps

> volatile does not make `count++` atomic.

> volatile does not provide mutual exclusion.

> synchronized provides both mutual exclusion and visibility.

> Happens-before is about visibility and ordering, not simply "what happened first in real time."

> The JMM should not be simplified to "each thread has its own copy of every variable."

> ConcurrentHashMap thread safety comes from its concurrency mechanisms and memory-visibility guarantees; it is not simply because it uses volatile variables.

> Safe publication is important when sharing objects between threads.

---

▸ Final One-Liners

> JMM = rules for visibility, ordering, and thread interaction.

> Visibility = one thread sees another thread's update.

> Atomicity = operation appears indivisible.

> Ordering = operations have a guaranteed order under JMM rules.

> volatile = visibility + ordering, not general atomicity.

> synchronized = mutual exclusion + visibility + ordering.

> Happens-Before = visibility and ordering relationship between actions.

> `Thread.start()` establishes a happens-before relationship from prior actions to the started thread.

> `Thread.join()` establishes a happens-before relationship from the completed thread to the joining thread after join returns.

> If A happens-before B and B happens-before C, then A happens-before C.