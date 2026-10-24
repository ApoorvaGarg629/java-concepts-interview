# Thread Safety

▸ What is Thread Safety?

A class or piece of code is thread-safe when it behaves correctly even when multiple threads access it concurrently.

Thread safety is mainly concerned with:
- Shared mutable data
- Concurrent modifications
- Visibility between threads
- Atomicity of operations
- Correct synchronization

Example of a non-thread-safe class:

    class Counter {

        private int count = 0;

        public void increment() {
            count++;
        }

        public int getCount() {
            return count;
        }
    }

If multiple threads call `increment()` simultaneously, the final result may be incorrect.

---

▸ What is a Race Condition?

A race condition occurs when the result of a program depends on the timing or interleaving of multiple threads accessing shared data.

Example:

    class Counter {

        private int count = 0;

        public void increment() {
            count++;
        }
    }

Suppose:

    count = 0

Two threads execute `count++`:

    Thread-1:
    read count -> 0

    Thread-2:
    read count -> 0

    Thread-1:
    calculate 0 + 1 -> 1

    Thread-2:
    calculate 0 + 1 -> 1

    Thread-1:
    write 1

    Thread-2:
    write 1

Final result:

    count = 1

Expected:

    count = 2

One update was lost.

---

▸ Why is `count++` Not Atomic?

`count++` looks like one operation but internally involves multiple steps:

    read count
    add 1
    write count

Therefore:

    count++;

is a read-modify-write operation.

Another thread can execute between these steps.

Interview one-liner:

    count++ is not atomic because it consists of a read, modification, and write.

---

▸ Atomicity

An operation is atomic when it happens as one indivisible unit from the perspective of other threads.

Example:

    count++;

is not atomic.

Using an atomic class:

    AtomicInteger count = new AtomicInteger();

    count.incrementAndGet();

The increment operation is performed atomically.

---

▸ Race Condition vs Data Race

A data race occurs when multiple threads access the same memory location concurrently, at least one access is a write, and there is no proper synchronization/ordering.

A race condition is a broader concept where correctness depends on the timing/order of concurrent operations.

Interview point:

    A data race is one way a race condition can occur, but race condition is the broader concept.

---

▸ How to Prevent Race Conditions

Common approaches:

- `synchronized`
- `Lock` / `ReentrantLock`
- Atomic classes
- Concurrent collections
- Immutable objects
- Thread confinement
- `ThreadLocal`
- Proper message passing / coordination

Choose the mechanism based on the type of shared state and required behavior.

---

▸ Example: Fixing Race Condition with `synchronized`

Non-thread-safe:

    class Counter {

        private int count;

        public void increment() {
            count++;
        }
    }

Thread-safe version:

    class Counter {

        private int count;

        public synchronized void increment() {
            count++;
        }
    }

Now only one thread at a time can execute `increment()` on the same object.

---

▸ Example: Fixing Race Condition with `AtomicInteger`

    import java.util.concurrent.atomic.AtomicInteger;

    class Counter {

        private final AtomicInteger count = new AtomicInteger();

        public void increment() {
            count.incrementAndGet();
        }

        public int getCount() {
            return count.get();
        }
    }

For simple atomic counters, `AtomicInteger` is often more appropriate than manually synchronizing the increment.

---

▸ What is `volatile`?

`volatile` is a Java keyword used to guarantee visibility of changes to a variable across threads and provide ordering guarantees around volatile accesses.

Example:

    class Task {

        private volatile boolean running = true;

        public void stop() {
            running = false;
        }

        public void run() {
            while (running) {
                // do work
            }
        }
    }

If one thread executes:

    running = false;

other threads reading `running` will see the updated value according to the Java Memory Model.

---

▸ Why is `volatile` Needed?

Without proper synchronization, one thread may not immediately observe another thread's update to a shared variable.

Example:

    class Task {

        private boolean running = true;

        public void stop() {
            running = false;
        }

        public void run() {
            while (running) {
                // work
            }
        }
    }

Making it:

    private volatile boolean running = true;

provides the required visibility for this simple flag.

---

▸ What Does `volatile` Guarantee?

`volatile` provides:

- Visibility
- Ordering guarantees

It does NOT generally provide:

- Mutual exclusion
- Atomicity for compound operations

Interview one-liner:

    volatile guarantees visibility and ordering, but not general atomicity.

---

▸ Why Does `volatile` Not Make `count++` Thread-Safe?

Consider:

    volatile int count;

    count++;

Even though `count` is volatile, the operation still consists of:

    read
    modify
    write

Possible execution:

    Thread-1 reads 10
    Thread-2 reads 10
    Thread-1 writes 11
    Thread-2 writes 11

Final result:

    11

Expected:

    12

Therefore:

    volatile != atomic

---

▸ When Should You Use `volatile`?

`volatile` is useful when:

- One thread writes a variable.
- Other threads read the variable.
- The operation does not require a compound read-modify-write operation.
- You need visibility between threads.

Common example:

    volatile boolean running;

This is a good use case because the operation is simply reading/writing a flag.

---

▸ Common `volatile` Use Case: Stop Flag

    class Worker implements Runnable {

        private volatile boolean running = true;

        public void stop() {
            running = false;
        }

        @Override
        public void run() {

            while (running) {
                // perform work
            }

            System.out.println("Worker stopped");
        }
    }

Thread-1:

    worker.run();

Thread-2:

    worker.stop();

The volatile variable ensures the worker thread can observe the updated value.

---

▸ `volatile` and Happens-Before

A write to a volatile variable happens-before every subsequent read of that same volatile variable.

Example:

    volatile boolean ready;

Thread-1:

    data = 100;
    ready = true;

Thread-2:

    if (ready) {
        System.out.println(data);
    }

The volatile write to `ready` establishes the necessary visibility relationship for actions before that write.

---

▸ Atomic Classes

Java provides atomic classes in:

    java.util.concurrent.atomic

Common classes:

    AtomicInteger
    AtomicLong
    AtomicBoolean
    AtomicReference
    AtomicIntegerArray
    AtomicLongArray
    AtomicReferenceArray

They provide atomic operations without requiring explicit synchronized blocks for each operation.

---

▸ `AtomicInteger`

Example:

    AtomicInteger count = new AtomicInteger(0);

    count.incrementAndGet();

    count.decrementAndGet();

    count.getAndIncrement();

    count.getAndDecrement();

    count.get();

    count.set(100);

---

▸ `incrementAndGet()` vs `getAndIncrement()`

`incrementAndGet()`:

    int value = count.incrementAndGet();

Returns the value after incrementing.

Example:

    count = 10

    incrementAndGet()
    -> 11

`getAndIncrement()`:

    int value = count.getAndIncrement();

Returns the value before incrementing.

Example:

    count = 10

    getAndIncrement()
    -> returns 10
    -> count becomes 11

Interview one-liner:

    incrementAndGet returns the updated value; getAndIncrement returns the previous value.

---

▸ Atomic Compare-and-Set

Atomic classes commonly provide:

    compareAndSet(expectedValue, newValue)

Example:

    AtomicInteger count = new AtomicInteger(10);

    boolean result = count.compareAndSet(10, 20);

If current value is `10`:

    10 == expected 10
    -> update to 20
    -> returns true

If current value is `15`:

    15 != expected 10
    -> no update
    -> returns false

---

▸ What is CAS?

CAS stands for:

    Compare-And-Swap

The conceptual operation is:

    if (currentValue == expectedValue) {
        currentValue = newValue;
        return true;
    }

    return false;

The comparison and update happen atomically.

CAS is a fundamental technique used by many lock-free and non-blocking concurrent algorithms.

---

▸ CAS Example

Suppose:

    AtomicInteger value = new AtomicInteger(10);

Thread-1:

    value.compareAndSet(10, 20);

Thread-2:

    value.compareAndSet(10, 30);

Only one thread can successfully change `10` because once one update succeeds, the current value is no longer `10`.

Possible result:

    Thread-1 -> true
    Thread-2 -> false

or the reverse depending on timing.

---

▸ CAS Retry Loop

A CAS-based operation can conceptually look like:

    while (true) {

        int current = value.get();
        int next = current + 1;

        if (value.compareAndSet(current, next)) {
            break;
        }
    }

If another thread changes the value between `get()` and `compareAndSet()`, CAS fails.

The thread retries using the latest value.

---

▸ CAS vs `synchronized`

| Feature | CAS / Atomic | synchronized |
|---|---|---|
| Main technique | Compare-and-swap | Monitor locking |
| Blocking | Usually non-blocking | Can block |
| Best for | Simple atomic state updates | Complex critical sections |
| Mutual exclusion | Not general-purpose | Yes |
| Example | AtomicInteger | synchronized block |

Important:

    Atomic classes do not replace synchronized in every situation.

If multiple variables must be changed together while maintaining an invariant, a lock may be more appropriate.

---

▸ When Atomic Classes Are a Good Choice

Good use cases:

    AtomicInteger counter

    AtomicBoolean flag

    AtomicReference<State> state

Especially when you need:

- Simple atomic updates
- Counters
- Flags
- Compare-and-set state transitions

---

▸ When Atomic Classes Are Not Enough

Suppose:

    int balance;
    int transactionCount;

You need:

    balance -= amount;
    transactionCount++;

and both updates must happen together.

Using separate atomic variables does not automatically make the entire operation atomic.

You may need:

    synchronized

or:

    ReentrantLock

to protect the complete invariant.

Interview one-liner:

    Atomic variables protect individual atomic operations; they do not automatically make a multi-step business operation atomic.

---

▸ ABA Problem

The ABA problem can occur with CAS.

Example:

    Initial value = A

Thread-1 reads:

    A

Thread-2 changes:

    A -> B -> A

Thread-1 performs:

    compareAndSet(A, C)

CAS sees:

    current == A

and succeeds.

But Thread-1 does not know that the value changed to B and then returned to A.

This is the ABA problem.

Solutions can include:

    AtomicStampedReference

which associates a version/stamp with the reference.

---

▸ `AtomicReference`

`AtomicReference` provides atomic operations on object references.

Example:

    AtomicReference<String> state =
        new AtomicReference<>("NEW");

    state.compareAndSet("NEW", "RUNNING");

Useful for atomic state transitions.

---

▸ `ThreadLocal`

`ThreadLocal` provides each thread with its own independent copy of a variable.

Example:

    ThreadLocal<Integer> threadLocal =
        ThreadLocal.withInitial(() -> 0);

    threadLocal.set(10);

    int value = threadLocal.get();

Thread-1 and Thread-2 have separate values.

Conceptually:

    Thread-1
        |
        v
    ThreadLocal -> 10

    Thread-2
        |
        v
    ThreadLocal -> 20

They do not share the same value.

---

▸ Why Use `ThreadLocal`?

`ThreadLocal` is useful when data should belong to a thread instead of being shared between threads.

Common examples:

- Request-specific context
- User/request metadata
- Transaction context
- Non-thread-safe objects that should be confined to one thread
- Per-thread formatting/context objects

---

▸ Example of ThreadLocal

    class UserContext {

        private static final ThreadLocal<String> user =
            new ThreadLocal<>();

        public static void setUser(String name) {
            user.set(name);
        }

        public static String getUser() {
            return user.get();
        }

        public static void clear() {
            user.remove();
        }
    }

Thread-1:

    UserContext.setUser("Alice");

Thread-2:

    UserContext.setUser("Bob");

Thread-1 gets:

    Alice

Thread-2 gets:

    Bob

The values are isolated between threads.

---

▸ How Does ThreadLocal Work?

Conceptually, each thread maintains a mapping associated with `ThreadLocal` keys.

Mental model:

    Thread-1
        |
        +-- ThreadLocal-A -> "Alice"

    Thread-2
        |
        +-- ThreadLocal-A -> "Bob"

The same `ThreadLocal` object can therefore return different values depending on which thread calls `get()`.

---

▸ `ThreadLocal.get()`

Returns the value associated with the current thread.

Example:

    ThreadLocal<String> local =
        ThreadLocal.withInitial(() -> "Default");

    System.out.println(local.get());

The value belongs to the currently executing thread.

---

▸ `ThreadLocal.set()`

Stores a value for the current thread.

Example:

    local.set("Hello");

Another thread calling:

    local.get();

does not automatically see `"Hello"`.

---

▸ `ThreadLocal.remove()`

Removes the current thread's value.

Example:

    local.remove();

This is especially important when using thread pools.

---

▸ Why is `ThreadLocal.remove()` Important with Thread Pools?

With a normal manually created thread:

    thread starts
    -> performs task
    -> thread terminates

The thread-local data naturally becomes irrelevant when the thread is gone.

But with a thread pool:

    Thread-1
       |
       +-- Task A
       |
       +-- Task B
       |
       +-- Task C

The same worker thread can execute many unrelated tasks.

If Task A stores:

    threadLocal.set(userData);

and does not remove it, Task B may see the old value.

This can cause:
- Incorrect data
- Data leakage between requests/tasks
- Memory retention

Therefore:

    try {
        threadLocal.set(value);

        // task
    } finally {
        threadLocal.remove();
    }

Interview one-liner:

    Always clean up ThreadLocal values in pooled threads when the value is request/task scoped.

---

▸ ThreadLocal vs Shared Variable

Shared variable:

    private String user;

Multiple threads can access the same variable.

ThreadLocal:

    private ThreadLocal<String> user;

Each thread gets its own value.

| Feature | Shared Variable | ThreadLocal |
|---|---|---|
| Data shared? | Yes | No |
| Synchronization needed? | Often, if mutable | Usually not for the ThreadLocal value itself |
| Value per thread? | No | Yes |
| Typical use | Shared state | Thread-confined state |

---

▸ ThreadLocal Does Not Make Objects Globally Thread-Safe

Example:

    ThreadLocal<List<String>> list;

Each thread may have its own list.

This avoids sharing that list between those threads.

But if the same list is explicitly shared elsewhere, ThreadLocal does not magically make it thread-safe.

Interview point:

    ThreadLocal provides thread confinement; it does not make shared objects thread-safe.

---

▸ Thread Safety Strategies

Common strategies:

    1. Synchronization
       Protect shared mutable state with locks.

    2. Atomic classes
       Use atomic operations for simple shared state.

    3. Immutability
       Avoid modification after object creation.

    4. Thread confinement
       Keep mutable data inside one thread.

    5. ThreadLocal
       Give each thread its own independent state.

    6. Concurrent collections
       Use thread-safe collection implementations.

    7. Message passing
       Coordinate through controlled communication instead of shared mutable state.

---

▸ Synchronization vs Atomic vs ThreadLocal

| Requirement | Suitable Approach |
|---|---|
| Protect complex critical section | synchronized / Lock |
| Simple atomic counter | AtomicInteger |
| Atomic state transition | AtomicReference / CAS |
| Visibility of a simple flag | volatile |
| Per-thread state | ThreadLocal |
| Concurrent map access | ConcurrentHashMap |
| Avoid shared mutable state | Immutable objects / thread confinement |

---

▸ Important Interview Scenarios

▸ Scenario: Multiple threads increment a counter

Question:

    Multiple threads increment an integer counter.
    How would you make it thread-safe?

Possible solutions:

    synchronized

or:

    AtomicInteger

Example:

    AtomicInteger counter = new AtomicInteger();

    counter.incrementAndGet();

---

▸ Scenario: One thread needs to stop another thread

Use a shared volatile flag:

    private volatile boolean running = true;

Thread-1:

    while (running) {
        // work
    }

Thread-2:

    running = false;

`volatile` provides the visibility required for the flag.

---

▸ Scenario: Each request needs its own user context

Use:

    ThreadLocal<UserContext>

Remember to remove it when using thread pools:

    try {
        context.set(userContext);

        // process request
    } finally {
        context.remove();
    }

---

▸ Scenario: Multiple values must change atomically

Suppose:

    balance
    transactionCount

must always remain consistent.

Do not assume two separate `AtomicInteger` variables automatically make the complete operation atomic.

Use:

    synchronized

or:

    Lock

to protect the complete operation.

---

▸ Scenario: Can volatile solve every thread-safety problem?

No.

`volatile` solves visibility and ordering problems but does not provide mutual exclusion or general atomicity.

Example:

    volatile int count;

    count++;

is still not thread-safe.

---

▸ Scenario: Can AtomicInteger solve every thread-safety problem?

No.

Atomic classes are excellent for individual atomic operations but cannot automatically make a complex sequence involving multiple variables or conditions atomic.

---

▸ Scenario: Why not synchronize everything?

Excessive synchronization can:
- Reduce concurrency
- Increase contention
- Increase latency
- Make deadlocks more likely if multiple locks are used incorrectly

Synchronize the required shared state and keep critical sections appropriately small.

---

▸ Most Important Interview Traps

- `count++` is not atomic.
- `volatile` does not make `count++` atomic.
- `volatile` provides visibility and ordering, not mutual exclusion.
- `synchronized` provides mutual exclusion and visibility.
- Atomic classes use atomic operations such as CAS.
- CAS can fail when another thread changes the value.
- Atomic classes are not a replacement for all synchronization.
- `ThreadLocal` gives each thread its own value.
- `ThreadLocal` does not make shared objects thread-safe.
- Always consider `ThreadLocal.remove()` when using thread pools.
- A race condition occurs when correctness depends on timing/interleaving.
- Multiple atomic variables do not automatically make a multi-variable operation atomic.
- Immutability is one of the simplest ways to achieve thread safety.
- Concurrent collections should be preferred over manually synchronizing standard collections when their semantics fit the problem.

---

▸ Quick Revision

    Race Condition
    = result depends on timing/interleaving of threads

    count++
    = read + modify + write
    = not atomic

    volatile
    = visibility + ordering
    != atomicity
    != mutual exclusion

    AtomicInteger
    = atomic operations on integer

    CAS
    = compare expected value and update atomically

    synchronized
    = mutual exclusion + visibility + ordering

    ThreadLocal
    = one independent value per thread

    ThreadLocal.remove()
    = important cleanup for thread-pool threads

    Thread Safety
    = correct behavior under concurrent access

---

▸ One-Liner to Remember the Entire Topic

    Race condition comes from unsafe shared access; use synchronized for critical sections, volatile for visibility, atomic classes for simple atomic updates, and ThreadLocal for per-thread state.