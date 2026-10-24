# Thread Synchronization

▸ What is Thread Synchronization?

Thread synchronization is the mechanism used to control access to shared resources when multiple threads execute concurrently.

The main goal is to prevent:
- Race conditions
- Data inconsistency
- Incorrect results caused by concurrent access

Example:

    class Counter {
        private int count = 0;

        public void increment() {
            count++;
        }
    }

If multiple threads call `increment()` at the same time, `count++` is not atomic and can produce an incorrect result.

Synchronization ensures that only the required number of threads can access the critical section at a time.

---

▸ What is a Critical Section?

A critical section is a part of code that accesses shared mutable data and must not be executed concurrently by multiple threads.

Example:

    public void increment() {
        count++;
    }

Here, `count++` is the critical section.

---

▸ `synchronized` Keyword

`synchronized` provides:
- Mutual exclusion
- Visibility guarantees
- Ordering guarantees through the Java Memory Model

Only one thread at a time can execute a synchronized block/method protected by the same monitor.

Example:

    class Counter {
        private int count = 0;

        public synchronized void increment() {
            count++;
        }
    }

If Thread-1 is executing `increment()`, another thread trying to execute the same synchronized method on the same object must wait until Thread-1 releases the monitor.

---

▸ How `synchronized` Works

Every Java object can be associated with an intrinsic lock, also called a monitor.

When a thread enters a synchronized block/method:

    Thread
       |
       v
    Acquire Monitor
       |
       v
    Execute Critical Section
       |
       v
    Release Monitor

If another thread already owns the monitor, the second thread waits until the monitor becomes available.

---

▸ Synchronized Instance Method

A synchronized instance method locks the current object, represented by `this`.

Example:

    class Counter {

        private int count;

        public synchronized void increment() {
            count++;
        }
    }

This is conceptually similar to:

    public void increment() {
        synchronized (this) {
            count++;
        }
    }

Important:

    synchronized instance method
    = lock on current object (`this`)

---

▸ Synchronized Static Method

A synchronized static method locks the `Class` object, not an individual instance.

Example:

    class Counter {

        public static synchronized void increment() {
            // critical section
        }
    }

Conceptually similar to:

    public static void increment() {
        synchronized (Counter.class) {
            // critical section
        }
    }

Important:

    synchronized instance method
    = lock on object instance

    synchronized static method
    = lock on Class object

---

▸ Synchronized Block

A synchronized block allows synchronization around only the critical section instead of the entire method.

Example:

    public void increment() {

        synchronized (this) {
            count++;
        }

        // other non-critical code
    }

This is often preferable when only a small part of the method accesses shared data.

---

▸ Why Use a Synchronized Block?

Instead of:

    public synchronized void process() {
        // 100 lines of code
    }

We can use:

    public void process() {

        // non-critical work

        synchronized (this) {
            // only critical section
        }

        // more non-critical work
    }

Benefits:
- Smaller lock scope
- Less contention
- Better concurrency
- More precise control over synchronization

---

▸ Synchronizing on a Specific Object

We can use a dedicated lock object.

Example:

    class Counter {

        private int count;
        private final Object lock = new Object();

        public void increment() {

            synchronized (lock) {
                count++;
            }
        }
    }

Using a private lock object prevents external code from accidentally synchronizing on the same lock.

---

▸ Why Prefer a Private Lock Object?

Consider:

    synchronized (this) {
        // critical section
    }

External code can also do:

    synchronized (counter) {
        // some unrelated operation
    }

This can create unexpected lock contention.

Using:

    private final Object lock = new Object();

and:

    synchronized (lock) {
        // critical section
    }

keeps the locking mechanism internal to the class.

Interview point:

    A private lock object provides better encapsulation of synchronization.

---

▸ Intrinsic Lock / Monitor

Every Java object has an intrinsic lock, also called a monitor.

A thread must acquire the object's monitor before entering a synchronized block protected by that object.

Example:

    synchronized (obj) {
        // thread owns obj's monitor here
    }

During the block:

    Thread-1
       |
       | acquires
       v
    obj monitor
       |
       v
    critical section
       |
       | releases
       v
    obj monitor

Another thread trying to acquire the same monitor must wait.

---

▸ Important Monitor Rule

Two synchronized blocks can block each other only when they use the same monitor.

Example:

    synchronized (lock) {
        // section A
    }

    synchronized (lock) {
        // section B
    }

Only one thread can execute either section at a time for that `lock`.

But:

    synchronized (lock1) {
        // section A
    }

    synchronized (lock2) {
        // section B
    }

These can execute concurrently because they use different monitors.

---

▸ Example: Same Object vs Different Objects

    class Counter {

        public synchronized void increment() {
            System.out.println("Increment");
        }
    }

    Counter c1 = new Counter();
    Counter c2 = new Counter();

    Thread t1 = new Thread(c1::increment);
    Thread t2 = new Thread(c1::increment);

`t1` and `t2` use the same object `c1`.

Therefore, they compete for the same monitor.

But:

    Thread t1 = new Thread(c1::increment);
    Thread t2 = new Thread(c2::increment);

Here they use different objects.

Therefore, both can execute concurrently.

Interview one-liner:

    Synchronization is per monitor, not per method.

---

▸ Does `synchronized` Make the Entire Class Thread-Safe?

No.

Adding `synchronized` to one method does not automatically make the entire class thread-safe.

Example:

    class Counter {

        private int count;

        public synchronized void increment() {
            count++;
        }

        public int getCount() {
            return count;
        }
    }

Only `increment()` is synchronized.

Thread safety depends on how all shared mutable state is accessed.

---

▸ Synchronization and Visibility

Synchronization provides visibility between threads.

When one thread releases a monitor, its actions before the unlock become visible to another thread that subsequently acquires the same monitor.

Example:

    class Example {

        private int value;

        public synchronized void setValue(int value) {
            this.value = value;
        }

        public synchronized int getValue() {
            return value;
        }
    }

The synchronization establishes the required happens-before relationship.

Interview one-liner:

    synchronized provides both mutual exclusion and memory visibility.

---

▸ `synchronized` and Atomicity

Synchronization can make a sequence of operations atomic with respect to other threads using the same lock.

Example:

    synchronized (lock) {
        count++;
    }

The complete read-modify-write operation is protected.

Without synchronization:

    count++;

can be interrupted between:

    read count
    add 1
    write count

Therefore multiple threads can lose updates.

---

▸ What Happens When a Thread Cannot Acquire a Monitor?

Suppose Thread-1 owns the monitor:

    synchronized (lock) {
        // Thread-1
    }

Thread-2 tries:

    synchronized (lock) {
        // Thread-2
    }

Thread-2 cannot acquire the monitor.

Thread-2 enters the `BLOCKED` state until it can acquire the monitor.

Important:

    BLOCKED
    = waiting to acquire an intrinsic monitor lock

---

▸ `BLOCKED` vs `WAITING`

`BLOCKED`:

    Thread is waiting to acquire a monitor.

Example:

    synchronized (lock) {
        // another thread already owns lock
    }

`WAITING`:

    Thread is waiting indefinitely for another thread to perform an action.

Examples:

    wait()
    join()
    LockSupport.park()

Interview one-liner:

    BLOCKED means waiting for a lock; WAITING means waiting for another thread/event.

---

▸ `wait()`, `notify()`, `notifyAll()`

These methods belong to `Object`.

They are used for thread coordination through an object's monitor.

Methods:

    wait()
    notify()
    notifyAll()

They must be called while the current thread owns the object's monitor.

Example:

    synchronized (lock) {
        lock.wait();
    }

and:

    synchronized (lock) {
        lock.notify();
    }

Calling these methods without owning the monitor results in:

    IllegalMonitorStateException

---

▸ Why Does `wait()` Release the Lock?

Suppose:

    synchronized (lock) {
        lock.wait();
    }

When the thread calls `wait()`:

1. The thread releases the monitor.
2. The thread enters the WAITING state.
3. Another thread can acquire the monitor.
4. Another thread can call `notify()` or `notifyAll()`.
5. The waiting thread becomes eligible to continue.
6. It must reacquire the same monitor before continuing.

Important:

    wait() releases the monitor.

This is one of the most frequently asked interview questions.

---

▸ `wait()` Does NOT Immediately Continue After `notify()`

Example:

    synchronized (lock) {
        lock.wait();
    }

Another thread:

    synchronized (lock) {
        lock.notify();
    }

`notify()` does not directly transfer the lock to the waiting thread.

The notified thread must first reacquire the monitor.

Therefore:

    notify()
    -> wakes one waiting thread
    -> thread must reacquire the monitor
    -> then it can continue

---

▸ `notify()`

`notify()` wakes one thread waiting on the same object's monitor.

Example:

    synchronized (lock) {
        lock.notify();
    }

If multiple threads are waiting, Java does not provide a general guarantee about which waiting thread will be selected.

---

▸ `notifyAll()`

`notifyAll()` wakes all threads waiting on the same object's monitor.

Example:

    synchronized (lock) {
        lock.notifyAll();
    }

All waiting threads become eligible to compete for the monitor.

Only one can acquire the monitor at a time.

---

▸ `notify()` vs `notifyAll()`

| Feature | notify() | notifyAll() |
|---|---|---|
| Threads woken | One | All waiting threads |
| Monitor required | Yes | Yes |
| Can cause missed progress if wrong thread wakes | Possible | Less likely |
| Typical use | Simple single-condition coordination | Multiple waiting conditions |

Interview point:

    notify() wakes one waiting thread; notifyAll() wakes all waiting threads.

---

▸ Why `wait()` Should Usually Be Inside a `while` Loop

Incorrect:

    synchronized (lock) {
        if (queue.isEmpty()) {
            lock.wait();
        }

        removeItem();
    }

Preferred:

    synchronized (lock) {
        while (queue.isEmpty()) {
            lock.wait();
        }

        removeItem();
    }

Why `while`?

Because after waking up, the condition must be checked again.

Reasons include:
- Another thread may have consumed the resource first.
- Multiple threads may wake.
- The condition may no longer be true.
- Spurious wakeups are permitted.

Interview one-liner:

    Always re-check the waiting condition after wait(), typically using a while loop.

---

▸ Example: Producer-Consumer with `wait()` and `notifyAll()`

    class SharedQueue {

        private final Queue<Integer> queue = new LinkedList<>();
        private final int capacity = 5;

        public synchronized void produce(int value)
                throws InterruptedException {

            while (queue.size() == capacity) {
                wait();
            }

            queue.add(value);
            notifyAll();
        }

        public synchronized int consume()
                throws InterruptedException {

            while (queue.isEmpty()) {
                wait();
            }

            int value = queue.poll();
            notifyAll();

            return value;
        }
    }

Flow:

Producer:
    Queue full
    -> wait()
    -> releases monitor
    -> Consumer gets monitor
    -> consumes item
    -> notifyAll()
    -> Producer can continue after reacquiring monitor

Consumer:
    Queue empty
    -> wait()
    -> releases monitor
    -> Producer gets monitor
    -> produces item
    -> notifyAll()
    -> Consumer can continue after reacquiring monitor

---

▸ `sleep()` vs `wait()`

| Feature | sleep() | wait() |
|---|---|---|
| Defined in | Thread | Object |
| Purpose | Pause execution | Thread coordination |
| Releases monitor? | No | Yes |
| Must hold monitor? | No | Yes |
| Interruptible? | Yes | Yes |
| Throws | InterruptedException | InterruptedException |
| State | TIMED_WAITING | WAITING / TIMED_WAITING |

Example:

    Thread.sleep(1000);

does not release any lock currently held.

But:

    synchronized (lock) {
        lock.wait();
    }

releases `lock`.

---

▸ `wait()` vs `join()`

`wait()`:

    Used for communication/coordination between threads.

`join()`:

    Used when one thread needs to wait for another thread to finish.

Example:

    Thread worker = new Thread(() -> {
        // work
    });

    worker.start();
    worker.join();

The current thread waits until `worker` terminates.

Important:

    wait() releases the object's monitor.

    join() waits for another thread to terminate.

---

▸ `sleep()` vs `wait()` vs `join()`

| Feature | sleep() | wait() | join() |
|---|---|---|---|
| Class | Thread | Object | Thread |
| Main purpose | Pause | Coordination | Wait for thread completion |
| Releases monitor | No | Yes | No, unless called while holding another lock? |
| Requires monitor | No | Yes | No |
| Interruptible | Yes | Yes | Yes |
| Typical state | TIMED_WAITING | WAITING/TIMED_WAITING | WAITING/TIMED_WAITING |

Important clarification:

`join()` internally relies on synchronization/waiting mechanisms, but from the caller's perspective it does not mean "release every lock the caller currently owns."

---

▸ Common Interview Trap: `sleep()` Inside synchronized Block

Example:

    synchronized (lock) {
        Thread.sleep(5000);
    }

During those 5 seconds:

- Current thread is sleeping.
- Current thread still owns `lock`.
- Other threads cannot enter another synchronized block using the same `lock`.

Therefore:

    sleep() does NOT release the monitor.

---

▸ Common Interview Trap: `wait()` Inside synchronized Block

Example:

    synchronized (lock) {
        lock.wait();
    }

During waiting:

- Current thread releases `lock`.
- Another thread can acquire `lock`.
- Current thread waits for notification/interruption/other permitted wakeup.
- After waking, it must reacquire `lock` before continuing.

Therefore:

    wait() releases the monitor.

---

▸ Can `wait()` Be Called Without `synchronized`?

No.

Incorrect:

    lock.wait();

If the current thread does not own `lock`'s monitor:

    IllegalMonitorStateException

Correct:

    synchronized (lock) {
        lock.wait();
    }

---

▸ Can `notify()` Be Called Without `synchronized`?

No.

Incorrect:

    lock.notify();

Correct:

    synchronized (lock) {
        lock.notify();
    }

The calling thread must own the same object's monitor.

---

▸ What Happens When a Thread Calls `wait()`?

Interview answer:

    The thread releases the object's monitor, enters WAITING or TIMED_WAITING state, and waits until it is notified, interrupted, times out if applicable, or otherwise becomes eligible to continue; before continuing it must reacquire the same monitor.

---

▸ What Happens When a Thread Calls `notify()`?

Interview answer:

    notify() wakes one thread waiting on the same object's monitor, but the awakened thread must reacquire the monitor before it can continue.

---

▸ What Happens When a Thread Calls `notifyAll()`?

Interview answer:

    notifyAll() wakes all threads waiting on the same object's monitor, and they then compete to reacquire the monitor one at a time.

---

▸ Reentrant Synchronization

Java's intrinsic locks are reentrant.

This means a thread that already owns a monitor can acquire the same monitor again without blocking itself.

Example:

    class Example {

        public synchronized void method1() {
            method2();
        }

        public synchronized void method2() {
            System.out.println("Inside method2");
        }
    }

When `method1()` calls `method2()` on the same object, the same thread can acquire the monitor again.

The monitor maintains ownership/reentrancy information.

Interview one-liner:

    Java intrinsic locks are reentrant, so a thread can acquire the same monitor multiple times.

---

▸ Example of Reentrancy

    class Example {

        public synchronized void outer() {
            inner();
        }

        public synchronized void inner() {
            System.out.println("Inner");
        }
    }

Calling:

    example.outer();

does not deadlock.

The same thread already owns the object's monitor when `inner()` is called.

---

▸ Synchronization Does Not Prevent Deadlock

Synchronization can be used incorrectly and lead to deadlock.

Example:

    synchronized (lock1) {
        synchronized (lock2) {
            // work
        }
    }

Another thread:

    synchronized (lock2) {
        synchronized (lock1) {
            // work
        }
    }

Possible situation:

    Thread-1 owns lock1
    Thread-2 owns lock2

Thread-1 waits for lock2.

Thread-2 waits for lock1.

Neither can continue.

This is a deadlock.

---

▸ Synchronized Method vs Synchronized Block

| Feature | Synchronized Method | Synchronized Block |
|---|---|---|
| Lock scope | Entire method | Specific section |
| Control | Less precise | More precise |
| Lock object | `this` for instance method | Explicitly specified |
| Performance | May hold lock longer | Can reduce lock duration |
| Readability | Simple | Better for complex locking |

Interview preference:

    Use synchronized blocks when only a small section requires synchronization.

---

▸ Instance Lock vs Class Lock

Instance method:

    public synchronized void method() {
    }

locks:

    this

Static method:

    public static synchronized void method() {
    }

locks:

    ClassName.class

Therefore these are different locks:

    synchronized instance method
    !=
    synchronized static method

A thread executing a synchronized instance method does not automatically block a thread executing a synchronized static method.

---

▸ Example: Instance and Static Locks

    class Example {

        public synchronized void instanceMethod() {
            // locks this
        }

        public static synchronized void staticMethod() {
            // locks Example.class
        }
    }

Thread-1:

    example.instanceMethod();

Thread-2:

    Example.staticMethod();

These can execute concurrently because they use different monitors.

---

▸ Synchronization and `volatile`

`synchronized` and `volatile` solve different problems.

`synchronized`:
- Mutual exclusion
- Visibility
- Ordering
- Protects critical sections

`volatile`:
- Visibility
- Ordering
- Does not provide mutual exclusion
- Does not make compound operations atomic

Example:

    volatile int count;

This does NOT make:

    count++;

thread-safe.

For compound operations, use appropriate synchronization or atomic/concurrent utilities.

---

▸ Common Interview Questions

▸ What is synchronization?

Synchronization controls concurrent access to shared resources so that operations requiring mutual exclusion and visibility are handled safely.

▸ What is a monitor?

A monitor is the intrinsic locking mechanism associated with an object and used by synchronized code.

▸ What does synchronized provide?

Mutual exclusion plus memory visibility and ordering guarantees.

▸ What lock does a synchronized instance method use?

The current object (`this`).

▸ What lock does a synchronized static method use?

The corresponding `Class` object.

▸ Does synchronized guarantee fairness?

No. Java intrinsic monitors do not provide a general fairness guarantee.

▸ Does synchronized prevent deadlock?

No. Incorrect lock ordering can still cause deadlock.

▸ Does sleep() release the lock?

No.

▸ Does wait() release the lock?

Yes, it releases the monitor on which `wait()` was called.

▸ Why must wait(), notify(), and notifyAll() be called inside synchronized code?

Because the current thread must own the object's monitor.

▸ What happens if wait() is called without owning the monitor?

`IllegalMonitorStateException`.

▸ Why use while instead of if with wait()?

Because the condition must be rechecked after waking, including because of spurious wakeups and competition with other threads.

▸ Can a thread acquire the same synchronized lock twice?

Yes. Intrinsic locks are reentrant.

▸ What state does a thread enter when waiting to acquire a synchronized lock?

`BLOCKED`.

▸ Can different synchronized methods execute concurrently?

It depends on the lock. Synchronized instance methods on the same object use the same monitor and cannot execute concurrently for that object. Methods on different objects can.

---

▸ Most Important Interview Traps

- `synchronized` provides mutual exclusion and visibility.
- `synchronized` does not automatically make every class operation thread-safe.
- Instance synchronized method locks `this`.
- Static synchronized method locks `ClassName.class`.
- `sleep()` does not release a lock.
- `wait()` releases the monitor.
- `wait()`, `notify()`, and `notifyAll()` require ownership of the monitor.
- `wait()` should normally be used inside a `while` condition check.
- `notify()` does not immediately give the lock to the awakened thread.
- `notifyAll()` wakes all waiting threads, but only one can acquire the monitor at a time.
- Intrinsic locks are reentrant.
- Synchronization can still cause deadlock if multiple locks are acquired in the wrong order.
- `volatile` does not replace synchronization for compound operations such as `count++`.
- `BLOCKED` means waiting for a monitor; `WAITING` means waiting for another event/action.
- Synchronization is associated with a monitor/lock, not simply with a method name.

---

▸ Quick Revision

    synchronized
    = mutual exclusion + visibility + ordering

    synchronized instance method
    = lock on this

    synchronized static method
    = lock on ClassName.class

    synchronized block
    = lock only the required critical section

    wait()
    = release monitor + wait

    notify()
    = wake one waiting thread

    notifyAll()
    = wake all waiting threads

    sleep()
    = pause current thread + keep monitor

    join()
    = wait for another thread to finish

    BLOCKED
    = waiting to acquire monitor

    WAITING
    = waiting indefinitely for another action

    intrinsic lock
    = object's monitor

    intrinsic lock
    = reentrant

    synchronized
    !=
    deadlock prevention

---

▸ One-Liner to Remember the Entire Topic

`synchronized` protects shared data using an object's monitor, `wait()` releases that monitor while waiting, and `notify()/notifyAll()` coordinate waiting threads.