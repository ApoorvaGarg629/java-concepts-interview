# Locks and Concurrency Control

▸ Overview

Locks and concurrency-control utilities are used when multiple threads access shared resources and we need to control how they execute.

Main topics covered:

- ReentrantLock
- Lock interface
- tryLock()
- lockInterruptibly()
- unlock()
- ReentrantLock vs synchronized
- ReadWriteLock
- ReentrantReadWriteLock
- CountDownLatch
- CyclicBarrier
- Semaphore
- Common interview scenarios
- Common interview traps


▸ Lock Interface

`Lock` is an interface in `java.util.concurrent.locks` that provides more flexible locking than `synchronized`.

Important methods:

- `lock()`
- `unlock()`
- `tryLock()`
- `tryLock(timeout, unit)`
- `lockInterruptibly()`
- `newCondition()`

Example:

    Lock lock = new ReentrantLock();

    lock.lock();

    try {
        // Critical section
    } finally {
        lock.unlock();
    }

Always release a lock inside `finally` so the lock is released even if an exception occurs.

Interview one-liner:

> `Lock` provides explicit and more flexible locking control than `synchronized`.


▸ ReentrantLock

`ReentrantLock` is a concrete implementation of the `Lock` interface.

It is called **reentrant** because the same thread can acquire the same lock multiple times without deadlocking itself.

Example:

    Lock lock = new ReentrantLock();

    lock.lock();

    try {
        lock.lock();

        try {
            System.out.println("Same thread acquired lock again");
        } finally {
            lock.unlock();
        }

    } finally {
        lock.unlock();
    }

The thread must release the lock the same number of times it acquired it.

If a thread acquires the lock twice, it must call `unlock()` twice.


▸ Why ReentrantLock Is Reentrant

Example:

    class A {

        private final Lock lock = new ReentrantLock();

        public void method1() {
            lock.lock();

            try {
                method2();
            } finally {
                lock.unlock();
            }
        }

        public void method2() {
            lock.lock();

            try {
                System.out.println("Inside method2");
            } finally {
                lock.unlock();
            }
        }
    }

If `method1()` acquires the lock and calls `method2()`, the same thread can acquire the same lock again.

The lock maintains a hold count.

    First lock()  -> hold count = 1
    Second lock() -> hold count = 2
    First unlock() -> hold count = 1
    Second unlock() -> hold count = 0

Interview one-liner:

> ReentrantLock allows the same thread to acquire the same lock multiple times, maintaining a hold count until all acquisitions are released.


▸ Basic ReentrantLock Example

    class Counter {

        private int count = 0;

        private final Lock lock = new ReentrantLock();

        public void increment() {
            lock.lock();

            try {
                count++;
            } finally {
                lock.unlock();
            }
        }

        public int getCount() {
            lock.lock();

            try {
                return count;
            } finally {
                lock.unlock();
            }
        }
    }

The critical section is protected by the lock.

The `finally` block guarantees that `unlock()` is called.


▸ ReentrantLock vs synchronized

| Feature | synchronized | ReentrantLock |
|---|---|---|
| Type | Language construct | API |
| Explicit lock/unlock | No | Yes |
| Automatic release | Yes | No |
| tryLock() | No | Yes |
| Timed lock acquisition | No | Yes |
| Interruptible lock acquisition | No | Yes |
| Fairness option | No direct option | Yes |
| Multiple Conditions | No | Yes |
| Reentrant | Yes | Yes |
| Easy to use safely | Yes | Requires careful unlock |

`synchronized` is simpler when basic mutual exclusion is enough.

`ReentrantLock` is useful when advanced locking behavior is required.


▸ tryLock()

`tryLock()` attempts to acquire the lock without waiting indefinitely.

Example:

    if (lock.tryLock()) {
        try {
            // Critical section
        } finally {
            lock.unlock();
        }
    } else {
        System.out.println("Could not acquire lock");
    }

If the lock is available:

    tryLock() -> true

If another thread owns the lock:

    tryLock() -> false

The current thread does not wait indefinitely.

Interview use case:

> Use `tryLock()` when a thread should perform some alternative action instead of waiting indefinitely for a lock.


▸ Timed tryLock()

You can also wait for a limited amount of time.

    if (lock.tryLock(2, TimeUnit.SECONDS)) {
        try {
            // Critical section
        } finally {
            lock.unlock();
        }
    } else {
        System.out.println("Lock not available");
    }

This waits for at most 2 seconds.

`tryLock(timeout, unit)` can throw `InterruptedException`.


▸ lockInterruptibly()

`lockInterruptibly()` allows a thread waiting to acquire a lock to respond to interruption.

Example:

    try {
        lock.lockInterruptibly();

        try {
            // Critical section
        } finally {
            lock.unlock();
        }

    } catch (InterruptedException e) {
        Thread.currentThread().interrupt();
    }

Difference:

    lock()
        waits for the lock and is not interruptible while waiting.

    lockInterruptibly()
        waits for the lock but can be interrupted while waiting.

Interview one-liner:

> `lockInterruptibly()` allows a thread blocked while acquiring a lock to respond to interruption.


▸ Fair vs Non-Fair ReentrantLock

By default, `ReentrantLock` is non-fair.

    ReentrantLock lock = new ReentrantLock();

A fair lock can be created using:

    ReentrantLock lock = new ReentrantLock(true);

A fair lock attempts to give access to waiting threads in approximately arrival order.

A non-fair lock can allow a newly arriving thread to acquire the lock before an already waiting thread.

Fairness can reduce starvation in some situations but may reduce throughput.

Interview one-liner:

> A fair ReentrantLock attempts to grant the lock to waiting threads in arrival order, while a non-fair lock can allow barging.


▸ Important ReentrantLock Methods

    lock()
    unlock()
    tryLock()
    tryLock(timeout, unit)
    lockInterruptibly()
    isLocked()
    isHeldByCurrentThread()
    getHoldCount()
    getQueueLength()
    hasQueuedThreads()

Important:

`isHeldByCurrentThread()` checks whether the current thread owns the lock.

Example:

    if (lock.isHeldByCurrentThread()) {
        lock.unlock();
    }


▸ Lock Ownership

A lock must be unlocked by the thread that owns it.

Example:

    lock.lock();

    // Thread A owns the lock

Thread B should not attempt to unlock Thread A's `ReentrantLock`.

With `ReentrantLock`, incorrect unlocking can result in `IllegalMonitorStateException`.


▸ Why unlock() Should Be in finally

Bad:

    lock.lock();

    doSomething();

    lock.unlock();

If `doSomething()` throws an exception, `unlock()` may never execute.

Better:

    lock.lock();

    try {
        doSomething();
    } finally {
        lock.unlock();
    }

Interview one-liner:

> Always release an explicit lock in a `finally` block to prevent lock leaks.


▸ Conditions with ReentrantLock

`ReentrantLock` supports `Condition`, which provides functionality similar to `wait()` and `notify()` but with more flexibility.

Example:

    Lock lock = new ReentrantLock();

    Condition condition = lock.newCondition();

    lock.lock();

    try {
        while (!ready) {
            condition.await();
        }

        // Continue execution

    } finally {
        lock.unlock();
    }

Another thread can signal:

    lock.lock();

    try {
        ready = true;
        condition.signal();
    } finally {
        lock.unlock();
    }

Important methods:

    await()
    signal()
    signalAll()

The current thread must hold the associated lock before calling these methods.


▸ Condition vs wait/notify

| Condition | wait/notify |
|---|---|
| Used with Lock | Used with intrinsic monitor |
| `await()` | `wait()` |
| `signal()` | `notify()` |
| `signalAll()` | `notifyAll()` |
| Can have multiple conditions per lock | One intrinsic wait-set per monitor |

Example:

    Lock lock = new ReentrantLock();

    Condition notEmpty = lock.newCondition();
    Condition notFull = lock.newCondition();

This allows separate waiting conditions for producers and consumers.


▸ ReadWriteLock

`ReadWriteLock` provides two locks:

- Read lock
- Write lock

The idea is:

    Multiple readers
        can read simultaneously

    Writer
        requires exclusive access

This is useful when:

- Reads are frequent
- Writes are less frequent
- Multiple readers can safely access the data simultaneously


▸ ReentrantReadWriteLock

`ReentrantReadWriteLock` is a common implementation of `ReadWriteLock`.

Example:

    class DataStore {

        private final ReadWriteLock lock =
                new ReentrantReadWriteLock();

        private final Lock readLock = lock.readLock();
        private final Lock writeLock = lock.writeLock();

        private String data;

        public String read() {
            readLock.lock();

            try {
                return data;
            } finally {
                readLock.unlock();
            }
        }

        public void write(String data) {
            writeLock.lock();

            try {
                this.data = data;
            } finally {
                writeLock.unlock();
            }
        }
    }


▸ Read Lock Behavior

Multiple threads can hold the read lock simultaneously, provided there is no writer holding or waiting in a way that prevents acquisition according to the lock's policy.

Example:

    Reader 1 -> read lock
    Reader 2 -> read lock
    Reader 3 -> read lock

All can read concurrently.

But a writer requires exclusive access.


▸ Write Lock Behavior

Only one thread can hold the write lock.

While a writer holds the write lock:

    Reader 1 -> waits
    Reader 2 -> waits
    Writer 2 -> waits

The writer gets exclusive access.

Interview one-liner:

> ReadWriteLock allows concurrent reads but gives writers exclusive access.


▸ When to Use ReadWriteLock

Good use case:

    Many reads
    Few writes

Example:

    Configuration cache
    In-memory lookup table
    Frequently-read application state

If writes are very frequent, the overhead of managing separate read/write locks may not provide much benefit.


▸ Read Lock vs Write Lock

| Operation | Read Lock | Write Lock |
|---|---|---|
| Multiple readers | Yes | No |
| Multiple writers | No | No |
| Reader + writer simultaneously | No | No |
| Exclusive access | No | Yes |
| Intended for | Read-only operations | Modifications |

Important:

A read lock protects against writers, but multiple readers can coexist.


▸ Reentrancy in ReentrantReadWriteLock

`ReentrantReadWriteLock` is reentrant.

A thread holding the write lock can acquire the write lock again.

Example:

    writeLock.lock();

    try {
        writeLock.lock();

        try {
            // Reentrant acquisition
        } finally {
            writeLock.unlock();
        }

    } finally {
        writeLock.unlock();
    }


▸ Read-to-Write Upgrade

Be careful when trying to upgrade from a read lock to a write lock.

Example:

    readLock.lock();

    try {
        writeLock.lock();

        // ...
        
    } finally {
        writeLock.unlock();
        readLock.unlock();
    }

This can lead to deadlock when multiple readers attempt the same upgrade.

Example:

    Thread A -> read lock
    Thread B -> read lock

Both then try:

    write lock

The write lock cannot be acquired while readers are holding the read lock, but the readers are waiting for the write lock.

Therefore:

> Avoid directly upgrading a read lock to a write lock when multiple readers may attempt the upgrade.


▸ Write-to-Read Downgrade

Write-to-read downgrade is possible.

A thread can acquire the read lock while holding the write lock and then release the write lock.

Conceptually:

    writeLock.lock();

    try {
        // Modify data

        readLock.lock();

        try {
            // Continue with read access
        } finally {
            readLock.unlock();
        }

    } finally {
        writeLock.unlock();
    }

This is different from read-to-write upgrade.


▸ CountDownLatch

`CountDownLatch` allows one or more threads to wait until a specified number of events are completed.

Package:

    java.util.concurrent.CountDownLatch

Create:

    CountDownLatch latch = new CountDownLatch(3);

A thread waits using:

    latch.await();

Other threads signal completion using:

    latch.countDown();

The count decreases:

    3 -> 2 -> 1 -> 0

When it reaches zero, waiting threads are released.


▸ CountDownLatch Example

    CountDownLatch latch = new CountDownLatch(3);

    Runnable task = () -> {
        try {
            System.out.println(
                Thread.currentThread().getName() + " completed"
            );
        } finally {
            latch.countDown();
        }
    };

    new Thread(task).start();
    new Thread(task).start();
    new Thread(task).start();

    latch.await();

    System.out.println("All tasks completed");

The main thread waits until all three tasks call `countDown()`.


▸ Real-Life CountDownLatch Scenario

Suppose an application needs three services to initialize:

    Database initialization
    Cache initialization
    Configuration loading

The main application can wait until all three are ready.

    CountDownLatch latch = new CountDownLatch(3);

Each initialization task calls:

    latch.countDown();

Main thread:

    latch.await();

Once the count reaches zero:

    Application continues


▸ Important CountDownLatch Properties

The count can only move toward zero.

Example:

    CountDownLatch latch = new CountDownLatch(2);

    latch.countDown();
    latch.countDown();

Now:

    count = 0

Additional calls:

    latch.countDown();

do not make the count negative.

Important:

> A CountDownLatch cannot be reset.

Once it reaches zero, it remains open.


▸ CountDownLatch vs Thread.join()

Both can make a thread wait, but their use cases are different.

`join()`:

    Wait for a specific thread to terminate.

`CountDownLatch`:

    Wait for a number of completion events.

Example:

    thread1.join();
    thread2.join();
    thread3.join();

is tied to specific threads.

Whereas:

    CountDownLatch latch = new CountDownLatch(3);

can represent three tasks/events even if the implementation is not directly tied to joining those threads.


▸ CountDownLatch vs CyclicBarrier

| CountDownLatch | CyclicBarrier |
|---|---|
| One-shot | Reusable |
| Count decreases to zero | Threads wait at a barrier |
| One or more threads can await | Participating threads generally call await |
| Tasks signal completion | Threads synchronize at a common point |
| Cannot reset | Can be reused |

Easy way to remember:

> CountDownLatch = wait until N events happen.

> CyclicBarrier = wait until N threads reach the same point.


▸ CyclicBarrier

`CyclicBarrier` allows a fixed number of threads to wait for each other at a common synchronization point.

Example:

    CyclicBarrier barrier = new CyclicBarrier(3);

Each thread calls:

    barrier.await();

The threads wait until all three reach the barrier.

Once all three arrive:

    Barrier opens
    All waiting threads continue


▸ CyclicBarrier Example

    CyclicBarrier barrier = new CyclicBarrier(3);

    Runnable task = () -> {
        try {
            System.out.println(
                Thread.currentThread().getName() +
                " reached barrier"
            );

            barrier.await();

            System.out.println(
                Thread.currentThread().getName() +
                " continues"
            );

        } catch (Exception e) {
            Thread.currentThread().interrupt();
        }
    };

    new Thread(task).start();
    new Thread(task).start();
    new Thread(task).start();

All three threads must reach:

    barrier.await();

before they can continue.


▸ CyclicBarrier Real-Life Scenario

Imagine three workers processing three parts of a job.

Each worker must finish phase 1 before any worker starts phase 2.

    Worker 1 -> Phase 1 -> barrier
    Worker 2 -> Phase 1 -> barrier
    Worker 3 -> Phase 1 -> barrier

After all three reach the barrier:

    Worker 1 -> Phase 2
    Worker 2 -> Phase 2
    Worker 3 -> Phase 2


▸ Why Is It Called CyclicBarrier?

It is called cyclic because the same barrier can be reused.

Example:

    Phase 1 -> barrier
    Phase 2 -> barrier
    Phase 3 -> barrier

After one barrier cycle completes, it can be used again.


▸ Barrier Action

`CyclicBarrier` can execute an optional action when the last participating thread reaches the barrier.

Example:

    CyclicBarrier barrier = new CyclicBarrier(
        3,
        () -> System.out.println("Phase completed")
    );

The barrier action runs once per barrier cycle.


▸ CyclicBarrier await()

`await()` can throw:

    InterruptedException
    BrokenBarrierException

It can also be used with a timeout.

    barrier.await(5, TimeUnit.SECONDS);

This can additionally throw:

    TimeoutException


▸ Broken Barrier

A barrier can become broken if one of the participating threads:

- Is interrupted while waiting
- Times out while waiting
- Causes the barrier to break

Other waiting threads can receive `BrokenBarrierException`.

Important interview point:

> CyclicBarrier has a broken state, unlike the simple countdown behavior of CountDownLatch.


▸ Semaphore

`Semaphore` controls access to a resource using a fixed number of permits.

Package:

    java.util.concurrent.Semaphore

Example:

    Semaphore semaphore = new Semaphore(3);

This means up to 3 permits are initially available.

A thread acquires a permit:

    semaphore.acquire();

After finishing:

    semaphore.release();


▸ Semaphore Example

    Semaphore semaphore = new Semaphore(2);

    Runnable task = () -> {
        try {
            semaphore.acquire();

            System.out.println(
                Thread.currentThread().getName() +
                " entered"
            );

            Thread.sleep(2000);

        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();

        } finally {
            semaphore.release();
        }
    };

If 10 threads run this task:

    2 threads -> can enter
    8 threads -> wait

When one thread releases its permit:

    1 waiting thread can acquire it


▸ Semaphore vs Lock

A normal lock generally provides:

    One owner at a time

A Semaphore can provide:

    N permits
    N threads can access the resource simultaneously

Example:

    ReentrantLock
        1 thread at a time

    Semaphore(5)
        Up to 5 permits


▸ Semaphore Use Cases

Common examples:

- Database connection limits
- API request limits
- Connection pools
- Limiting concurrent file operations
- Limiting access to expensive resources

Example:

    Semaphore semaphore = new Semaphore(10);

This can limit concurrent access to approximately 10 permits.


▸ Binary Semaphore

A Semaphore with one permit is called a binary semaphore.

    Semaphore semaphore = new Semaphore(1);

Conceptually:

    1 permit
    1 thread at a time

However, it is not identical to a `ReentrantLock`.

A Semaphore does not require the releasing thread to be the thread that acquired the permit.


▸ Fair Semaphore

Semaphore can also be fair:

    Semaphore semaphore =
        new Semaphore(3, true);

Fairness attempts to grant permits to waiting threads in FIFO order.

Default:

    new Semaphore(3)

is non-fair.


▸ acquire() vs tryAcquire()

`acquire()` waits until a permit is available.

    semaphore.acquire();

`tryAcquire()` does not wait indefinitely.

    if (semaphore.tryAcquire()) {
        try {
            // Use resource
        } finally {
            semaphore.release();
        }
    }

Timed version:

    semaphore.tryAcquire(2, TimeUnit.SECONDS);


▸ Important Semaphore Methods

    acquire()
    acquire(int permits)
    tryAcquire()
    tryAcquire(timeout, unit)
    release()
    release(int permits)
    availablePermits()

Example:

    semaphore.availablePermits();

returns the currently available permits.


▸ Semaphore Important Trap

If a thread successfully calls:

    semaphore.acquire();

it should normally ensure that a matching:

    semaphore.release();

occurs when the resource is no longer needed.

Forgetting to release permits can eventually cause other threads to wait indefinitely.


▸ Locks and Concurrency Utilities Comparison

| Utility | Main Purpose |
|---|---|
| synchronized | Basic mutual exclusion |
| ReentrantLock | Flexible explicit locking |
| ReadWriteLock | Multiple readers, exclusive writers |
| CountDownLatch | Wait for N events/tasks |
| CyclicBarrier | Threads wait for each other at a common point |
| Semaphore | Limit concurrent access using permits |

Easy memory trick:

    ReentrantLock
    -> One flexible lock

    ReadWriteLock
    -> Many readers / one writer

    CountDownLatch
    -> Wait for N completions

    CyclicBarrier
    -> Wait for N threads

    Semaphore
    -> Allow N concurrent accesses


▸ CountDownLatch vs Semaphore

They may look similar because both use a count/permit concept, but their purpose is different.

CountDownLatch:

    "Wait until these events are completed."

Semaphore:

    "Limit how many threads can access this resource simultaneously."


▸ CyclicBarrier vs CountDownLatch

Example requirement:

    "Start only after 5 services have initialized."

Use:

    CountDownLatch

Example requirement:

    "5 worker threads must all finish phase 1 before any starts phase 2."

Use:

    CyclicBarrier


▸ Semaphore vs ReentrantLock

Example:

    Only one database migration should run.

Use:

    ReentrantLock

Example:

    Maximum 10 database connections can be used simultaneously.

Use:

    Semaphore(10)


▸ ReentrantLock vs synchronized Scenario

Question:

> When would you choose ReentrantLock instead of synchronized?

Answer:

Use `ReentrantLock` when you need advanced features such as:

- `tryLock()`
- Timed lock acquisition
- Interruptible lock acquisition
- Fairness
- Multiple `Condition` objects
- More explicit lock-management behavior

For simple mutual exclusion, `synchronized` is usually simpler.


▸ Scenario-Based Interview Questions

Q: Multiple threads need to update the same object, but only one should modify it at a time. What would you use?

A:

    synchronized
    or
    ReentrantLock

Choose `ReentrantLock` if advanced lock-control features are required.


Q: Multiple threads are reading a shared cache, but writes are rare. What would you consider?

A:

    ReentrantReadWriteLock

Multiple readers can proceed concurrently while writes remain exclusive.


Q: A thread should wait until 5 independent tasks finish. Which utility?

A:

    CountDownLatch(5)


Q: Five worker threads must all reach the same point before continuing. Which utility?

A:

    CyclicBarrier(5)


Q: Only 3 threads should access an expensive resource at the same time. Which utility?

A:

    Semaphore(3)


Q: A thread should try to acquire a lock but should not wait forever. Which method?

A:

    tryLock()


Q: A thread waiting for a lock should be cancellable through interruption. Which method?

A:

    lockInterruptibly()


Q: Why must ReentrantLock be unlocked in finally?

A:

    To guarantee lock release even when an exception occurs.


Q: Can multiple threads hold a ReentrantReadWriteLock read lock?

A:

    Yes, multiple readers can hold the read lock concurrently.


Q: Can multiple threads hold the write lock?

A:

    No, the write lock is exclusive.


Q: Can the same thread acquire a ReentrantLock multiple times?

A:

    Yes. It is reentrant and maintains a hold count.


Q: Can a Semaphore have multiple permits?

A:

    Yes. The number of permits controls how many threads can acquire it concurrently.


Q: Is a Semaphore a lock?

A:

    It provides concurrency control, but it is permit-based rather than ownership-based like ReentrantLock.


▸ Common Interview Traps

- `ReentrantLock` requires explicit `unlock()`.
- Always prefer `finally` for `unlock()`.
- `tryLock()` does not wait indefinitely.
- `lockInterruptibly()` allows interruption while waiting for a lock.
- `ReentrantLock` is reentrant.
- `ReentrantReadWriteLock` allows multiple concurrent readers.
- Write lock is exclusive.
- Read-to-write upgrade can cause deadlock.
- `CountDownLatch` cannot be reset.
- `CyclicBarrier` can be reused.
- `Semaphore` controls permits, not ownership.
- `CountDownLatch` waits for events; Semaphore limits concurrent access.
- `CyclicBarrier` synchronizes participating threads at a common point.
- A fair lock does not necessarily mean higher performance.
- Explicit locks must always be released correctly.


▸ Quick Revision

    Lock
    -> Interface for explicit locking

    ReentrantLock
    -> Flexible, explicit, reentrant lock

    tryLock()
    -> Try acquiring without waiting indefinitely

    lockInterruptibly()
    -> Lock acquisition can respond to interruption

    ReadWriteLock
    -> Separate read and write locks

    ReentrantReadWriteLock
    -> Multiple readers, one exclusive writer

    CountDownLatch
    -> Wait until count reaches zero

    CyclicBarrier
    -> Threads wait for each other

    Semaphore
    -> Limit concurrent access using permits


▸ Most Frequently Asked Interview Questions

- What is ReentrantLock?
- Why is ReentrantLock called reentrant?
- ReentrantLock vs synchronized?
- Why should unlock() be inside finally?
- What is tryLock()?
- What is lockInterruptibly()?
- What is a fair lock?
- What is ReadWriteLock?
- Can multiple threads acquire the read lock?
- Can multiple threads acquire the write lock?
- What is read-to-write lock upgrade?
- What is CountDownLatch?
- CountDownLatch vs Thread.join()?
- CountDownLatch vs CyclicBarrier?
- Why is CountDownLatch not reusable?
- What is CyclicBarrier?
- Why is CyclicBarrier called cyclic?
- What is Semaphore?
- Semaphore vs ReentrantLock?
- What is a binary semaphore?
- When would you use Semaphore?
- What happens if a Semaphore permit is never released?
- Which utility would you use to limit concurrent access to a resource?


▸ One-Liner Revision

> `ReentrantLock` provides flexible explicit locking, `ReadWriteLock` allows concurrent reads with exclusive writes, `CountDownLatch` waits for N events, `CyclicBarrier` makes N threads wait at a common point, and `Semaphore` limits concurrent access using permits.