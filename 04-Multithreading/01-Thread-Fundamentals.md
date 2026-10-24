# Thread Fundamentals

▸ Topics Covered

- Thread Fundamentals
- Process vs Thread
- Why Multithreading?
- Creating Threads
- Thread class
- Runnable
- Callable
- Runnable vs Callable
- start() vs run()
- Thread Lifecycle
- Thread States
- sleep()
- join()
- Thread Interruption
- Daemon Threads
- User Threads vs Daemon Threads
- Interview Questions
- Common Interview Traps
- One-Liner Revision


▸ What Is a Thread?

A thread is the smallest unit of execution within a process.

A single process can contain multiple threads, and these threads share the process's resources such as heap memory.

Example:

    Process
    ├── Thread 1
    ├── Thread 2
    └── Thread 3

Each thread has its own:

- Program Counter
- Stack
- Stack Frames

Threads within the same process share:

- Heap
- Method Area / Metaspace
- Class-level data
- Other process resources


▸ Process vs Thread

| Process                               | Thread                                        |
| ------------------------------------- | --------------------------------------------- |
| Independent program in execution      | Unit of execution inside a process            |
| Has its own memory space              | Shares process memory                         |
| More expensive to create              | Cheaper to create                             |
| Communication is more expensive       | Communication is easier through shared memory |
| Process failure is generally isolated | Thread failure can affect the process         |

Example:

    Browser = Process

    Browser process
    ├── UI Thread
    ├── Network Thread
    ├── Rendering Thread
    └── Background Thread


▸ Why Use Multiple Threads?

Multithreading is used to:

- Perform multiple tasks concurrently
- Improve responsiveness
- Utilize CPU resources efficiently
- Perform background work
- Handle multiple requests
- Avoid blocking the main thread

Example:

    Thread 1 → Read file
    Thread 2 → Process request
    Thread 3 → Perform logging

Important:

Multithreading does not always mean tasks execute at exactly the same time.

Concurrency means multiple tasks make progress during overlapping periods.

▸ Creating a Thread

There are two traditional ways to create a thread directly:

1. Extend Thread
2. Implement Runnable

Modern applications usually prefer submitting tasks to ExecutorService instead of manually creating many threads.

ExecutorService is covered in a separate note.


▸ Creating a Thread by Extending Thread

Example:

    class MyThread extends Thread {

        @Override
        public void run() {
            System.out.println("Thread is running");
        }
    }

    public class Main {
        public static void main(String[] args) {

            MyThread thread = new MyThread();

            thread.start();
        }
    }

Here:

    MyThread thread = new MyThread();

creates a Thread object.

    thread.start();

asks the JVM to start a new thread.

The new thread eventually executes:

    run()


▸ Implementing Runnable

Example:

    class MyTask implements Runnable {

        @Override
        public void run() {
            System.out.println("Task is running");
        }
    }

    public class Main {
        public static void main(String[] args) {

            Runnable task = new MyTask();

            Thread thread = new Thread(task);

            thread.start();
        }
    }

Here the task and the thread are separate concepts.

    Runnable → What should be executed?

    Thread → Which thread executes it?


▸ Why Runnable Is Generally Preferred Over Extending Thread

Java supports single inheritance.

If a class extends Thread:

    class MyTask extends Thread {
    }

the class cannot extend another class.

With Runnable:

    class MyTask extends SomeOtherClass implements Runnable {
    }

the class can still extend another class.

Runnable also separates:

    Task = work to perform

    Thread = mechanism that executes the work

This separation becomes even more important when using ExecutorService.


▸ Runnable

Runnable is a functional interface representing a task that does not return a result.

Definition conceptually:

    @FunctionalInterface
    public interface Runnable {
        void run();
    }

Example:

    Runnable task = () -> {
        System.out.println("Processing task");
    };

    Thread thread = new Thread(task);

    thread.start();


▸ Callable

Callable represents a task that:

- Returns a result
- Can throw checked exceptions

Conceptually:

    public interface Callable<V> {
        V call() throws Exception;
    }

Example:

    Callable<Integer> task = () -> {
        return 10 + 20;
    };

Callable is commonly used with ExecutorService and Future.


▸ Runnable vs Callable

| Feature | Runnable | Callable |
|---|---|---|
| Method | run() | call() |
| Return value | No | Yes |
| Checked exception | Cannot directly throw checked exception from run() | Can throw checked exception |
| Common use | Fire-and-forget task | Task whose result is required |
| Used with Future | Yes through submit(), but no result value | Yes |

Example:

    Runnable:

    Runnable task = () -> {
        System.out.println("Hello");
    };


    Callable:

    Callable<Integer> task = () -> {
        return 100;
    };


▸ Runnable with ExecutorService

When Runnable is submitted using:

    executor.submit(runnable);

a Future is returned.

The task itself does not produce a meaningful result.

Example:

    Future<?> future = executor.submit(() -> {
        System.out.println("Running task");
    });


▸ Callable with ExecutorService

Example:

    Future<Integer> future = executor.submit(() -> {
        return 100;
    });

    Integer result = future.get();

Here:

    Callable → produces result

    Future → represents the result of the asynchronous computation


▸ start() vs run()

This is one of the most frequently asked interview questions.

Correct:

    Thread thread = new Thread(() -> {
        System.out.println("Running");
    });

    thread.start();

start() tells the JVM to start a new thread.

The new thread executes run().

Incorrect when the goal is to create a new thread:

    thread.run();

Calling run() directly is just a normal method call.

It executes on the current thread.

Example:

    Thread thread = new Thread(() -> {
        System.out.println(Thread.currentThread().getName());
    });

    thread.run();

The task executes on the calling thread.

But:

    thread.start();

causes execution on the newly started thread.


▸ Interview Question: Why Can't start() Be Called Twice?

A Thread object can be started only once.

Example:

    Thread thread = new Thread(() -> {
        System.out.println("Hello");
    });

    thread.start();

    thread.start();

The second call throws:

    IllegalThreadStateException

If you need another execution, create another Thread object.


▸ Thread.currentThread()

Returns the thread that is currently executing the code.

Example:

    System.out.println(Thread.currentThread().getName());

Common output:

    main

Inside another thread:

    Thread thread = new Thread(() -> {
        System.out.println(Thread.currentThread().getName());
    });

    thread.start();

The output will be the name of the newly created thread.


▸ Thread Naming

You can assign a name to a thread.

Example:

    Thread thread = new Thread(
        () -> System.out.println("Processing"),
        "Worker-1"
    );

    thread.start();

You can also use:

    thread.setName("Worker-1");

and:

    thread.getName();


▸ Thread Priority

Java provides thread priorities:

    Thread.MIN_PRIORITY
    Thread.NORM_PRIORITY
    Thread.MAX_PRIORITY

Values are:

    MIN_PRIORITY  = 1
    NORM_PRIORITY = 5
    MAX_PRIORITY  = 10

Example:

    thread.setPriority(Thread.MAX_PRIORITY);

Important:

Thread priority is only a scheduling hint.

Do not use thread priority to build correctness logic.


▸ Thread Lifecycle

A Java thread moves through different states during its lifetime.

Java defines these states:

    NEW
    RUNNABLE
    BLOCKED
    WAITING
    TIMED_WAITING
    TERMINATED


▸ NEW

A thread is in NEW state after the Thread object is created but before start() is called.

Example:

    Thread thread = new Thread(() -> {
        System.out.println("Hello");
    });

At this point:

    thread.getState()

returns:

    NEW


▸ RUNNABLE

After:

    thread.start();

the thread becomes eligible to run.

Java's RUNNABLE state represents a thread that is either:

- Actually running
- Ready to run and waiting for CPU scheduling

Important:

Java does not have a separate RUNNING state in Thread.State.

Both ready-to-run and currently executing threads are represented by RUNNABLE.


▸ BLOCKED

A thread enters BLOCKED when it is waiting to acquire an intrinsic monitor lock.

Example:

    synchronized (lock) {
        // critical section
    }

If Thread A owns the lock and Thread B tries to enter the same synchronized block:

    Thread A → owns lock
    Thread B → BLOCKED

Thread B waits until it can acquire the monitor.


▸ WAITING

A thread enters WAITING when it waits indefinitely for another thread to perform some action.

Common examples:

    Object.wait()

    Thread.join()

    LockSupport.park()

Example:

    synchronized (lock) {
        lock.wait();
    }

The thread waits until another thread performs an appropriate action such as:

    lock.notify();

or:

    lock.notifyAll();


▸ TIMED_WAITING

A thread enters TIMED_WAITING when it waits for a specified amount of time.

Common examples:

    Thread.sleep(1000);

    Object.wait(1000);

    Thread.join(1000);

The thread automatically becomes eligible to continue after the timeout, although actual execution depends on scheduling.


▸ TERMINATED

A thread enters TERMINATED after its run() method completes or terminates due to an uncaught exception.

Example:

    Thread thread = new Thread(() -> {
        System.out.println("Hello");
    });

    thread.start();

After the thread finishes:

    TERMINATED


▸ Thread Lifecycle Example

Example:

    Thread thread = new Thread(() -> {
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    });

Conceptually:

    NEW
      |
      | start()
      ↓
    RUNNABLE
      |
      | sleep()
      ↓
    TIMED_WAITING
      |
      | timeout
      ↓
    RUNNABLE
      |
      | run() completes
      ↓
    TERMINATED

The actual lifecycle can vary depending on scheduling and synchronization.


▸ Thread.State

Java provides:

    Thread.State

with these values:

    NEW
    RUNNABLE
    BLOCKED
    WAITING
    TIMED_WAITING
    TERMINATED

Example:

    Thread.State state = thread.getState();

    System.out.println(state);


▸ Thread.sleep()

sleep() pauses the currently executing thread for a specified duration.

Example:

    Thread.sleep(1000);

This pauses the current thread for approximately one second.

Important:

sleep():

- Is a static method
- Affects the current thread
- Does not release intrinsic locks/monitors owned by the thread
- Can throw InterruptedException
- Causes TIMED_WAITING

Example:

    synchronized (lock) {

        Thread.sleep(1000);

    }

The thread continues to hold the lock while sleeping.


▸ Why sleep() Is Static

sleep() always applies to the currently executing thread.

Example:

    Thread.sleep(1000);

Calling:

    thread.sleep(1000);

does not make that specific thread sleep if another thread is executing the call.

The static method applies to:

    Thread.currentThread()


▸ Thread.join()

join() allows one thread to wait for another thread to finish.

Example:

    Thread worker = new Thread(() -> {
        System.out.println("Processing...");
    });

    worker.start();

    worker.join();

Here the current thread waits until worker terminates.

Typical usage:

    Main Thread
         |
         | start worker
         ↓
    Worker Thread
         |
         | performs work
         ↓
    Worker finishes
         |
         ↓
    Main continues


▸ join() Example

    Thread thread = new Thread(() -> {

        for (int i = 1; i <= 3; i++) {
            System.out.println(i);
        }

    });

    thread.start();

    thread.join();

    System.out.println("Main continues");

Without join(), the main thread does not have to wait for the worker to finish.

With join(), the main thread waits for the worker to terminate.


▸ join() with Timeout

You can specify a maximum waiting time:

    thread.join(1000);

This means the current thread waits for up to approximately 1000 milliseconds.

It may continue earlier if the target thread terminates.


▸ sleep() vs join()

| sleep() | join() |
|---|---|
| Pauses current thread for a duration | Current thread waits for another thread |
| Static method | Instance method |
| Does not wait for another thread to finish | Specifically waits for target thread termination |
| Causes TIMED_WAITING | Can cause WAITING or TIMED_WAITING |
| Does not release intrinsic locks | Does not release locks merely because join() is called |

Note:

Neither sleep() nor join() is a mechanism for releasing a monitor.


▸ Thread Interruption

Interruption is Java's cooperative mechanism for requesting that a thread stop what it is doing or respond to cancellation.

It does not forcibly kill a thread.

Example:

    thread.interrupt();


▸ interrupt()

Calling:

    thread.interrupt();

sets the thread's interrupted status.

If the thread is blocked in certain interruptible methods such as:

    sleep()
    wait()
    join()

an InterruptedException is thrown and the interrupted status is cleared as part of throwing the exception.


▸ Checking Interruption

There are two commonly used methods:

    thread.isInterrupted()

and:

    Thread.interrupted()


▸ isInterrupted()

Checks the interrupted status of the specified Thread object.

Example:

    if (Thread.currentThread().isInterrupted()) {
        // handle interruption
    }

It does not clear the interrupted status.


▸ Thread.interrupted()

This is a static method.

It checks the current thread's interrupted status and clears it.

Example:

    if (Thread.interrupted()) {
        // interruption detected
    }

Important:

    isInterrupted()
    → checks status without clearing it

    Thread.interrupted()
    → checks status and clears it


▸ InterruptedException

Some blocking methods throw InterruptedException.

Example:

    try {
        Thread.sleep(1000);
    } catch (InterruptedException e) {
        Thread.currentThread().interrupt();
    }

A common best practice when you cannot fully handle the interruption is to restore the interrupted status:

    Thread.currentThread().interrupt();

This allows higher-level code to detect the interruption.


▸ Graceful Thread Cancellation

Do not try to forcibly terminate a thread.

Instead, use cooperative cancellation.

Example:

    class Worker implements Runnable {

        @Override
        public void run() {

            while (!Thread.currentThread().isInterrupted()) {

                // perform work

            }
        }
    }

Another thread can request cancellation:

    workerThread.interrupt();


▸ Why Interruption Is Important

Interruption is commonly used with:

- ExecutorService
- Future
- BlockingQueue
- Thread.sleep()
- Object.wait()
- Thread.join()

It allows applications to cancel or stop waiting tasks in a controlled manner.


▸ Daemon Threads

A daemon thread is a background thread that does not prevent the JVM from shutting down.

Example:

    Thread thread = new Thread(() -> {

        while (true) {
            System.out.println("Background work");
        }

    });

    thread.setDaemon(true);

    thread.start();

If all user threads finish, the JVM can terminate even if daemon threads are still running.


▸ setDaemon()

A thread must be marked as daemon before it is started.

Correct:

    thread.setDaemon(true);
    thread.start();

Incorrect:

    thread.start();
    thread.setDaemon(true);

Calling setDaemon() after the thread has started throws:

    IllegalThreadStateException


▸ User Threads vs Daemon Threads

| User Thread | Daemon Thread |
|---|---|
| Keeps JVM alive | Does not keep JVM alive |
| Used for important application work | Usually used for background support work |
| JVM waits for user threads | JVM does not wait for daemon threads |

Example use cases for daemon threads:

- Background monitoring
- Cleanup/support tasks
- Background housekeeping

Do not use daemon threads for critical work that must complete before JVM shutdown.


▸ Important Daemon Thread Behavior

When the JVM decides to shut down because no user threads remain, daemon threads are not given a guarantee to finish their work.

Therefore:

    Critical business task → User thread

    Background support task → Daemon thread


▸ Thread Lifecycle vs Thread Methods

It is important not to confuse methods with Thread states.

| Method | Typical Effect |
|---|---|
| start() | Starts thread execution |
| run() | Contains task logic |
| sleep() | TIMED_WAITING |
| wait() | WAITING / TIMED_WAITING |
| join() | WAITING / TIMED_WAITING |
| interrupt() | Requests interruption |
| yield() | Gives scheduler a hint |
| setDaemon() | Configures daemon status before start |

yield() is only a scheduling hint and should not be used for synchronization or correctness.


▸ Thread.sleep() vs Object.wait()

This is a very common interview question.

    sleep()

- Belongs to Thread
- Static
- Pauses current thread
- Does not release intrinsic monitor
- Used mainly for timing/delays

    wait()

- Belongs to Object
- Must be called while owning that object's monitor
- Releases that monitor while waiting
- Used for thread coordination
- Must be resumed by notification or interruption/timeout depending on the form


▸ Thread.join() vs Object.wait()

join() internally uses waiting mechanics, but conceptually their purposes are different.

    join()
    → Wait for another thread to finish.

    wait()
    → Wait for a condition/notification while releasing the object's monitor.


▸ Important Thread Interview Traps

▸ Trap 1: run() Creates a New Thread

False.

    thread.run();

is just a normal method call.

    thread.start();

starts a new thread.


▸ Trap 2: sleep() Releases Lock

False.

sleep() does not release intrinsic locks owned by the thread.


▸ Trap 3: interrupt() Kills a Thread

False.

interrupt() is a cooperative cancellation/request mechanism.


▸ Trap 4: Daemon Thread Always Runs in Background Until Completion

False.

The JVM does not wait for daemon threads to complete.


▸ Trap 5: Thread Has a RUNNING State

Not in Java's Thread.State enum.

Java combines running and ready-to-run execution into:

    RUNNABLE


▸ Trap 6: A Thread Can Be Started Multiple Times

False.

A Thread object can be started only once.


▸ Trap 7: Thread Priority Guarantees Execution Order

False.

Priority is only a scheduling hint and should not be used to guarantee ordering.


▸ Trap 8: Calling thread.sleep() Sleeps That Thread

Not necessarily.

sleep() is static and pauses the currently executing thread.


▸ Trap 9: InterruptedException Means Thread Is Automatically Terminated

False.

InterruptedException signals interruption while the thread is in an interruptible blocking operation.

The application decides how to respond.


▸ Interview Questions

Q1. What is a thread?

A thread is an independent path of execution within a process.

Q2. What is the difference between a process and a thread?

A process has its own memory space, while threads share the memory/resources of their process.

Q3. How can you create a thread in Java?

Traditionally by extending Thread or implementing Runnable; in modern applications, tasks are commonly submitted to ExecutorService.

Q4. Runnable vs Callable?

Runnable does not return a result, while Callable returns a result and can throw checked exceptions.

Q5. What is the difference between start() and run()?

start() creates a new thread of execution, while run() is a normal method call when invoked directly.

Q6. Can start() be called twice?

No. Calling start() more than once on the same Thread object throws IllegalThreadStateException.

Q7. What are the states of a Java thread?

NEW, RUNNABLE, BLOCKED, WAITING, TIMED_WAITING, and TERMINATED.

Q8. What is the difference between BLOCKED and WAITING?

BLOCKED means waiting to acquire a monitor lock, while WAITING means waiting indefinitely for another thread to perform an action.

Q9. What is the difference between WAITING and TIMED_WAITING?

WAITING has no specified timeout, while TIMED_WAITING has a specified waiting duration.

Q10. Does sleep() release a lock?

No. sleep() does not release intrinsic monitors owned by the thread.

Q11. What does join() do?

join() makes the current thread wait until the target thread terminates.

Q12. What does interrupt() do?

It requests a thread to respond to interruption; it does not forcibly terminate the thread.

Q13. What is the difference between isInterrupted() and interrupted()?

isInterrupted() checks the status without clearing it, while Thread.interrupted() checks and clears the current thread's interrupted status.

Q14. What is a daemon thread?

A daemon thread is a background thread that does not prevent the JVM from shutting down.

Q15. Can setDaemon(true) be called after start()?

No. Daemon status must be set before the thread is started.

Q16. Why is Runnable generally preferred over extending Thread?

It separates the task from the thread mechanism and avoids Java's single-inheritance limitation.

Q17. Can a Java application have multiple threads?

Yes. A process can contain multiple threads that execute concurrently and share process resources.

Q18. Does multithreading always mean parallel execution?

No. Multithreading provides concurrency; parallel execution requires multiple execution resources such as CPU cores.

Q19. What happens if an exception escapes from a thread's run() method?

The thread terminates, and the uncaught exception can be handled by the thread's uncaught exception handler.

Q20. What is cooperative cancellation?

It is a design where a thread periodically checks a cancellation/interruption signal and stops its work gracefully instead of being forcibly terminated.


▸ Frequently Asked Comparisons

Runnable vs Callable

    Runnable
    → No result
    → run()
    → No checked exception directly

    Callable
    → Returns result
    → call()
    → Can throw checked exception


start() vs run()

    start()
    → Creates new thread execution

    run()
    → Normal method call


sleep() vs wait() vs join()

    sleep()
    → Pause current thread
    → Does not release monitor

    wait()
    → Wait for coordination
    → Releases object's monitor

    join()
    → Wait for another thread to finish


User Thread vs Daemon Thread

    User Thread
    → JVM waits for it

    Daemon Thread
    → JVM does not wait for it


isInterrupted() vs interrupted()

    isInterrupted()
    → Does not clear flag

    Thread.interrupted()
    → Clears flag


▸ Quick Revision Table

| Concept | Remember |
|---|---|
| Thread | Unit of execution |
| Process | Independent program execution |
| Runnable | Task without result |
| Callable | Task with result |
| start() | Starts new thread |
| run() | Normal method if called directly |
| NEW | Created but not started |
| RUNNABLE | Ready/running |
| BLOCKED | Waiting for monitor |
| WAITING | Waiting indefinitely |
| TIMED_WAITING | Waiting with timeout |
| TERMINATED | Execution finished |
| sleep() | Pause current thread |
| wait() | Wait and release monitor |
| join() | Wait for another thread |
| interrupt() | Request interruption |
| Daemon | Background thread that does not keep JVM alive |


▸ One-Liner Revision

> A thread is the smallest unit of execution within a process.

> Threads share the process's heap and resources but have their own stack and program counter.

> Runnable defines a task without a return value, while Callable defines a task that returns a result.

> start() creates a new thread of execution, while run() is just a normal method call when invoked directly.

> Java thread states are NEW, RUNNABLE, BLOCKED, WAITING, TIMED_WAITING, and TERMINATED.

> BLOCKED means waiting for a monitor lock, while WAITING means waiting indefinitely for another thread's action.

> sleep() pauses the current thread without releasing its intrinsic locks.

> wait() releases the object's monitor and waits for notification, interruption, or timeout depending on the overload.

> join() makes the current thread wait for another thread to terminate.

> interrupt() requests cooperative interruption; it does not forcibly kill a thread.

> isInterrupted() checks interruption without clearing the flag, while Thread.interrupted() checks and clears the current thread's flag.

> A daemon thread runs in the background and does not prevent the JVM from shutting down.

> Use Runnable/Callable for tasks and ExecutorService to manage threads in production applications.
> 