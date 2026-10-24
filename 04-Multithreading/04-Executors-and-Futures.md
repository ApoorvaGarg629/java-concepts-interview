# Executors and Futures

▸ Why Executor Framework?

Creating a new thread for every task is usually not a good approach.

Example:

    for (int i = 0; i < 1000; i++) {
        new Thread(() -> {
            // task
        }).start();
    }

Problems:
- Creating too many threads
- Thread creation overhead
- High memory usage
- Too much context switching
- Difficult thread lifecycle management
- Difficult task management

The Executor Framework separates:

    Task
    from
    Thread management

Instead of manually creating and managing threads, we submit tasks to an executor.

---

▸ Executor Framework

The Executor Framework is provided mainly through:

    java.util.concurrent

Important interfaces/classes:

    Executor
    ExecutorService
    ScheduledExecutorService
    ThreadPoolExecutor
    Future
    CompletableFuture
    Executors

Mental model:

    Task
      |
      v
    Executor
      |
      v
    Thread Pool
      |
      v
    Worker Thread
      |
      v
    Task Execution

---

▸ Executor

`Executor` is the simplest interface in the Executor Framework.

It has one method:

    void execute(Runnable command);

Example:

    Executor executor = command -> {
        new Thread(command).start();
    };

    executor.execute(() -> {
        System.out.println("Task running");
    });

The main purpose of `Executor` is to separate task submission from the mechanism used to execute the task.

---

▸ ExecutorService

`ExecutorService` extends `Executor` and provides lifecycle management and task submission features.

Common methods:

    execute()
    submit()
    shutdown()
    shutdownNow()
    isShutdown()
    isTerminated()
    awaitTermination()

It is the interface most commonly used when working with thread pools.

Example:

    ExecutorService executor =
        Executors.newFixedThreadPool(3);

    executor.submit(() -> {
        System.out.println("Task running");
    });

    executor.shutdown();

---

▸ Executor vs ExecutorService

| Feature | Executor | ExecutorService |
|---|---|---|
| execute(Runnable) | Yes | Yes |
| submit() | No | Yes |
| Future support | No | Yes |
| Shutdown | No | Yes |
| Lifecycle management | No | Yes |
| Thread pool support | Possible | Yes |

Interview one-liner:

    Executor provides task execution; ExecutorService additionally provides task submission, Future support, and executor lifecycle management.

---

▸ Thread Pool

A thread pool is a collection of reusable worker threads used to execute submitted tasks.

Instead of:

    Task 1 -> create Thread
    Task 2 -> create Thread
    Task 3 -> create Thread

we use:

    Thread Pool
       |
       +-- Worker Thread 1
       +-- Worker Thread 2
       +-- Worker Thread 3
       |
       +-- Task Queue

Tasks are submitted to the executor and worker threads execute them.

---

▸ Why Use Thread Pools?

Benefits:

- Reuse threads
- Reduce thread creation overhead
- Control concurrency
- Limit number of active threads
- Manage task queue
- Improve resource utilization
- Simplify thread lifecycle management

---

▸ Fixed Thread Pool

Creates a thread pool with a fixed number of worker threads.

Example:

    ExecutorService executor =
        Executors.newFixedThreadPool(3);

Suppose 10 tasks are submitted:

    3 tasks
      |
      v
    Worker threads execute them

    Remaining 7 tasks
      |
      v
    Wait in the task queue

As workers finish, they pick up more tasks.

---

▸ Fixed Thread Pool Mental Model

    Thread Pool Size = 3

    +---------+---------+---------+
    | Thread 1| Thread 2| Thread 3|
    +---------+---------+---------+
         |         |         |
       Task 1    Task 2    Task 3

    Queue:
       Task 4
       Task 5
       Task 6
       Task 7
       ...

When a worker finishes Task 1:

    Thread 1 -> Task 4

---

▸ Cached Thread Pool

Created using:

    Executors.newCachedThreadPool();

It creates threads as needed and reuses idle threads when possible.

Example:

    ExecutorService executor =
        Executors.newCachedThreadPool();

Useful for workloads with many short-lived asynchronous tasks, but it can create a large number of threads under sustained load.

Interview point:

    Cached thread pools should be used carefully because thread creation can grow significantly when tasks arrive faster than idle threads can be reused.

---

▸ Single Thread Executor

Created using:

    Executors.newSingleThreadExecutor();

It uses one worker thread.

Example:

    ExecutorService executor =
        Executors.newSingleThreadExecutor();

Tasks are executed sequentially.

    Task 1
      |
      v
    Task 2
      |
      v
    Task 3

There is no parallel execution of submitted tasks through that single worker.

Useful when:
- Tasks must execute sequentially
- You want a single worker managing a queue

---

▸ Scheduled Executor

Used for delayed or periodic execution.

Example:

    ScheduledExecutorService scheduler =
        Executors.newScheduledThreadPool(2);

    scheduler.schedule(
        () -> System.out.println("Executed"),
        5,
        TimeUnit.SECONDS
    );

The task executes after the specified delay.

Periodic tasks:

    scheduler.scheduleAtFixedRate(
        () -> System.out.println("Running"),
        0,
        5,
        TimeUnit.SECONDS
    );

---

▸ `ThreadPoolExecutor`

`ThreadPoolExecutor` is the main configurable implementation behind many executor-based thread pools.

It allows control over:

- Core thread count
- Maximum thread count
- Keep-alive time
- Work queue
- Thread factory
- Rejected execution handler

Example:

    ThreadPoolExecutor executor =
        new ThreadPoolExecutor(
            2,
            5,
            60,
            TimeUnit.SECONDS,
            new LinkedBlockingQueue<>()
        );

---

▸ ThreadPoolExecutor Parameters

Constructor conceptually:

    ThreadPoolExecutor(
        corePoolSize,
        maximumPoolSize,
        keepAliveTime,
        unit,
        workQueue,
        threadFactory,
        rejectionHandler
    )

Meaning:

    corePoolSize
    = number of core worker threads

    maximumPoolSize
    = maximum number of worker threads

    keepAliveTime
    = how long extra idle threads may remain alive

    workQueue
    = queue holding waiting tasks

    threadFactory
    = creates worker threads

    rejectionHandler
    = handles tasks that cannot be accepted

---

▸ Core Pool Size vs Maximum Pool Size

Suppose:

    corePoolSize = 2
    maximumPoolSize = 5

Initially:

    Task 1 -> Thread 1
    Task 2 -> Thread 2

If more tasks arrive, they are generally queued first.

When the queue cannot accept more tasks, additional threads can be created up to:

    maximumPoolSize = 5

So:

    Core Threads
        2

    Maximum Threads
        5

---

▸ ThreadPoolExecutor Task Submission Flow

A simplified execution flow is:

    New Task
       |
       v
    Are core threads available?
       |
       +-- Create worker if below corePoolSize
       |
       v
    Otherwise put task in queue
       |
       v
    If queue cannot accept task
       |
       v
    Create additional worker
       |
       v
    Up to maximumPoolSize
       |
       v
    If still cannot accept
       |
       v
    Rejection policy

Important:

    Tasks are generally queued before the pool grows beyond corePoolSize.

The exact behavior depends on the configured work queue.

---

▸ Work Queue

The work queue stores tasks waiting for worker threads.

Common queue choices include:

    LinkedBlockingQueue
    ArrayBlockingQueue
    SynchronousQueue

Queue selection can significantly affect thread-pool behavior.

---

▸ Bounded vs Unbounded Queue

Bounded queue:

    Has a fixed capacity.

Example:

    new ArrayBlockingQueue<>(100);

Unbounded queue:

    Does not have a fixed practical capacity.

Example:

    new LinkedBlockingQueue<>();

Interview point:

    Queue choice affects when ThreadPoolExecutor creates additional threads and when tasks are rejected.

---

▸ Rejection Policy

A task can be rejected when the executor cannot accept more tasks.

For example:

    maximumPoolSize reached
    +
    work queue full

ThreadPoolExecutor provides built-in rejection policies.

Common policies:

    AbortPolicy
    CallerRunsPolicy
    DiscardPolicy
    DiscardOldestPolicy

---

▸ AbortPolicy

Default rejection policy.

Throws:

    RejectedExecutionException

Example:

    new ThreadPoolExecutor.AbortPolicy()

Useful when the caller should explicitly know that task submission failed.

---

▸ CallerRunsPolicy

The thread submitting the task executes the task itself when the executor rejects it.

Example:

    new ThreadPoolExecutor.CallerRunsPolicy()

This can provide a natural form of backpressure because the submitting thread becomes busy executing the task.

---

▸ DiscardPolicy

Simply discards the rejected task.

No exception is thrown.

This should only be used when silently losing tasks is acceptable.

---

▸ DiscardOldestPolicy

Removes the oldest task waiting in the queue and attempts to submit the new task again.

Use carefully because an older task can be lost.

---

▸ ThreadFactory

`ThreadFactory` controls how worker threads are created.

It can be used to customize:

- Thread names
- Daemon status
- Thread priority
- Uncaught exception handler

Example:

    ThreadFactory factory = task -> {
        Thread thread = new Thread(task);
        thread.setName("worker-thread");
        return thread;
    };

---

▸ `execute()` vs `submit()`

`execute()`:

    executor.execute(runnable);

Used for submitting a `Runnable`.

It does not return a result.

`submit()`:

    Future<?> future = executor.submit(runnable);

or:

    Future<Integer> future =
        executor.submit(callable);

Returns a `Future`.

---

▸ `Runnable` vs `Callable` with ExecutorService

Runnable:

    Runnable task = () -> {
        System.out.println("Task");
    };

    executor.submit(task);

Callable:

    Callable<Integer> task = () -> {
        return 100;
    };

    Future<Integer> future =
        executor.submit(task);

Main difference:

    Runnable
    = no return value

    Callable
    = returns a value and can throw checked exceptions

---

▸ What is `Future`?

`Future` represents the result of an asynchronous computation.

Example:

    Future<Integer> future =
        executor.submit(() -> {
            return 100;
        });

The task may still be running when `submit()` returns.

Later:

    Integer result = future.get();

retrieves the result.

---

▸ Future Mental Model

    Main Thread
        |
        | submit()
        v
    Executor
        |
        v
    Worker Thread
        |
        | performs task
        v
    Result

Meanwhile:

    Main Thread
        |
        v
    Future
        |
        v
    get()
        |
        v
    Result

---

▸ `Future.get()`

Example:

    Future<Integer> future =
        executor.submit(() -> 100);

    Integer result = future.get();

`get()` waits if the task has not completed yet.

Therefore:

    future.get()

is a blocking operation.

It can throw:

    InterruptedException
    ExecutionException

Timed version:

    future.get(5, TimeUnit.SECONDS);

This can additionally throw:

    TimeoutException

---

▸ Why Can `Future.get()` Reduce Concurrency?

Example:

    Future<Integer> future1 =
        executor.submit(task1);

    Future<Integer> future2 =
        executor.submit(task2);

    int result1 = future1.get();
    int result2 = future2.get();

Both tasks may execute concurrently in the pool.

But the calling thread can block on `future1.get()` before it retrieves `future2`.

The tasks themselves can still run concurrently; the problem is that result consumption is blocking.

---

▸ Future Cancellation

A task can be cancelled using:

    future.cancel(true);

The boolean indicates whether the executor should attempt to interrupt the running task.

Important:

    cancel(true)
    does not forcibly kill the thread.

It requests interruption if the task is already running.

The task must respond appropriately to interruption.

---

▸ `isDone()`

Checks whether the task has completed.

Example:

    if (future.isDone()) {
        System.out.println("Task completed");
    }

Returns true when the task completed, failed, or was cancelled.

---

▸ `isCancelled()`

Checks whether the Future was cancelled.

Example:

    if (future.isCancelled()) {
        System.out.println("Task was cancelled");
    }

---

▸ Future Limitations

`Future` is useful but has limitations.

Common limitations:

- `get()` is blocking
- Difficult to compose multiple asynchronous operations
- Difficult to express dependent tasks
- Limited error-handling composition
- Combining multiple Futures is cumbersome

For richer asynchronous workflows, Java provides:

    CompletableFuture

---

▸ ExecutorService Lifecycle

An ExecutorService has a lifecycle.

Typical lifecycle:

    Running
       |
       v
    shutdown()
       |
       v
    No new tasks accepted
       |
       v
    Existing tasks complete
       |
       v
    Terminated

---

▸ `shutdown()`

Gracefully shuts down the executor.

Example:

    executor.shutdown();

After shutdown:

    new tasks are rejected

Already submitted tasks:

    are allowed to complete

Important:

    shutdown() does not immediately interrupt currently running tasks.

---

▸ `shutdownNow()`

Attempts to stop the executor more aggressively.

Example:

    List<Runnable> waitingTasks =
        executor.shutdownNow();

It:

- Prevents new tasks
- Attempts to interrupt running tasks
- Returns tasks that were waiting in the queue

Important:

    shutdownNow() is a best-effort interruption request, not forced thread termination.

---

▸ `shutdown()` vs `shutdownNow()`

| Feature | shutdown() | shutdownNow() |
|---|---|---|
| Accept new tasks | No | No |
| Existing queued tasks | Usually execute | Returned without execution |
| Running tasks | Allowed to finish | Interrupt requested |
| Immediate stop | No | Attempts |
| Graceful | Yes | More aggressive |

---

▸ `awaitTermination()`

Used to wait for executor termination.

Example:

    executor.shutdown();

    try {
        if (!executor.awaitTermination(10, TimeUnit.SECONDS)) {
            executor.shutdownNow();
        }
    } catch (InterruptedException e) {
        executor.shutdownNow();
        Thread.currentThread().interrupt();
    }

Typical graceful shutdown pattern:

    shutdown()
    -> wait
    -> if necessary shutdownNow()

---

▸ Important Executor Lifecycle Pattern

Use:

    try {
        executor.submit(task);
    } finally {
        executor.shutdown();
    }

For applications where you need to wait for completion, combine shutdown with `awaitTermination()`.

---

▸ `CompletableFuture`

`CompletableFuture` represents an asynchronous computation that can be explicitly completed and composed with other asynchronous computations.

It implements:

    Future
    CompletionStage

Example:

    CompletableFuture<Integer> future =
        CompletableFuture.supplyAsync(() -> 100);

    Integer result = future.join();

---

▸ `supplyAsync()` vs `runAsync()`

`supplyAsync()`:

    returns a result.

Example:

    CompletableFuture<Integer> future =
        CompletableFuture.supplyAsync(() -> 100);

`runAsync()`:

    does not return a result.

Example:

    CompletableFuture<Void> future =
        CompletableFuture.runAsync(() -> {
            System.out.println("Running");
        });

Interview one-liner:

    supplyAsync() is for asynchronous computations with a result; runAsync() is for asynchronous tasks without a result.

---

▸ Default Executor of CompletableFuture

When using:

    CompletableFuture.supplyAsync(() -> {
        return 100;
    });

without specifying an executor, the async task generally uses the common ForkJoinPool.

You can provide your own executor:

    ExecutorService executor =
        Executors.newFixedThreadPool(4);

    CompletableFuture<Integer> future =
        CompletableFuture.supplyAsync(
            () -> 100,
            executor
        );

Using an explicit executor gives better control over thread-pool configuration and isolation.

---

▸ `thenApply()`

Used to transform the result of a completed stage.

Example:

    CompletableFuture<Integer> future =
        CompletableFuture.supplyAsync(() -> 10);

    CompletableFuture<Integer> result =
        future.thenApply(value -> value * 2);

Result:

    20

Mental model:

    10
     |
     v
    thenApply()
     |
     v
    20

---

▸ `thenAccept()`

Consumes a result but does not produce another result.

Example:

    CompletableFuture.supplyAsync(() -> 100)
        .thenAccept(value -> {
            System.out.println(value);
        });

Return type:

    CompletableFuture<Void>

---

▸ `thenRun()`

Runs an action after completion but does not receive the previous result.

Example:

    CompletableFuture.supplyAsync(() -> 100)
        .thenRun(() -> {
            System.out.println("Completed");
        });

---

▸ `thenApply()` vs `thenAccept()` vs `thenRun()`

| Method | Receives Result | Produces Result |
|---|---|---|
| thenApply() | Yes | Yes |
| thenAccept() | Yes | No |
| thenRun() | No | No |

Memory trick:

    thenApply
    = transform

    thenAccept
    = consume

    thenRun
    = just run

---

▸ `thenApplyAsync()`

`thenApplyAsync()` performs the continuation asynchronously, generally using the common pool unless an executor is supplied.

Example:

    future.thenApplyAsync(value -> value * 2);

With custom executor:

    future.thenApplyAsync(
        value -> value * 2,
        executor
    );

---

▸ `thenApply()` vs `thenApplyAsync()`

`thenApply()`:

    Continuation may execute in the thread that completes the previous stage.

`thenApplyAsync()`:

    Continuation is scheduled for asynchronous execution, using the default or supplied executor.

Interview point:

    Async variants provide explicit asynchronous scheduling; non-async variants do not guarantee execution on a separate thread.

---

▸ Combining CompletableFutures

Suppose:

    future1 -> user
    future2 -> orders

We may need both results before continuing.

Use:

    thenCombine()

Example:

    CompletableFuture<String> user =
        CompletableFuture.supplyAsync(() -> "Alice");

    CompletableFuture<Integer> orders =
        CompletableFuture.supplyAsync(() -> 5);

    CompletableFuture<String> result =
        user.thenCombine(
            orders,
            (name, count) -> name + " has " + count + " orders"
        );

Result:

    Alice has 5 orders

---

▸ `thenCompose()`

Used when the second asynchronous operation depends on the result of the first.

Without `thenCompose()`:

    CompletableFuture<CompletableFuture<Result>>

With `thenCompose()`:

    CompletableFuture<Result>

Example:

    CompletableFuture<User> userFuture =
        getUser();

    CompletableFuture<Order> orderFuture =
        userFuture.thenCompose(
            user -> getOrders(user)
        );

Mental model:

    Future<User>
         |
         v
    getOrders(user)
         |
         v
    Future<Order>

Interview one-liner:

    thenCompose() chains dependent asynchronous operations and flattens nested CompletableFutures.

---

▸ `thenCombine()` vs `thenCompose()`

`thenCombine()`:

    Two independent futures
    -> combine their results

`thenCompose()`:

    Second future depends on first future's result

Example:

    thenCombine()
    Future<A> + Future<B>
    -> Future<C>

    thenCompose()
    Future<A> -> Future<B>

---

▸ Exception Handling with CompletableFuture

Use:

    exceptionally()

Example:

    CompletableFuture<Integer> future =
        CompletableFuture.supplyAsync(() -> {
            throw new RuntimeException("Failed");
        });

    future.exceptionally(ex -> {
        System.out.println("Error: " + ex.getMessage());
        return 0;
    });

---

▸ `handle()`

`handle()` receives both the result and exception.

Example:

    future.handle((result, exception) -> {

        if (exception != null) {
            return 0;
        }

        return result;
    });

It can transform either success or failure into a new result.

---

▸ `exceptionally()` vs `handle()`

| Feature | exceptionally() | handle() |
|---|---|---|
| Called on failure | Yes | Yes |
| Receives result | No | Yes |
| Receives exception | Yes | Yes |
| Can recover | Yes | Yes |
| Runs on success | Normally no | Yes |

---

▸ `whenComplete()`

Used to perform an action after completion without changing the result.

Example:

    future.whenComplete((result, exception) -> {
        System.out.println("Completed");
    });

It is useful for logging, metrics, cleanup, or observing completion.

---

▸ `join()` vs `get()` in CompletableFuture

`get()`:

    String result = future.get();

Can throw checked exceptions such as:

    InterruptedException
    ExecutionException

`join()`:

    String result = future.join();

Wraps failures in unchecked `CompletionException` rather than requiring checked exception handling.

Interview one-liner:

    get() uses checked exception handling; join() throws unchecked CompletionException for exceptional completion.

---

▸ `allOf()`

Waits for multiple CompletableFutures to complete.

Example:

    CompletableFuture<Void> all =
        CompletableFuture.allOf(
            future1,
            future2,
            future3
        );

    all.join();

Important:

    allOf() returns CompletableFuture<Void>.

It does not directly return a list of results.

The individual futures can be queried after completion.

---

▸ `anyOf()`

Completes when any one of the supplied futures completes.

Example:

    CompletableFuture<Object> result =
        CompletableFuture.anyOf(
            future1,
            future2,
            future3
        );

Useful when the first completed result is sufficient.

---

▸ `allOf()` vs `anyOf()`

    allOf()
    = wait for all

    anyOf()
    = continue when any one completes

---

▸ CompletableFuture Example

    ExecutorService executor =
        Executors.newFixedThreadPool(4);

    CompletableFuture<Integer> future =
        CompletableFuture.supplyAsync(
            () -> 10,
            executor
        )
        .thenApply(value -> value * 2)
        .thenApply(value -> value + 5)
        .exceptionally(ex -> 0);

    System.out.println(future.join());

Flow:

    10
     |
     v
    *2
     |
     v
    20
     |
     v
    +5
     |
     v
    25

---

▸ ExecutorService vs CompletableFuture

They solve related but different problems.

ExecutorService:

    Focuses on task execution and thread-pool management.

CompletableFuture:

    Focuses on asynchronous computation and composition.

They can be used together.

Example:

    ExecutorService executor =
        Executors.newFixedThreadPool(4);

    CompletableFuture<Integer> future =
        CompletableFuture.supplyAsync(
            () -> calculate(),
            executor
        );

---

▸ Common Interview Scenarios

▸ Scenario: You have 100 independent tasks

Do not create 100 threads manually.

Use an appropriate thread pool:

    ExecutorService executor =
        Executors.newFixedThreadPool(10);

Submit the tasks and let the pool manage worker threads.

---

▸ Scenario: Task Returns a Result

Use:

    Callable<T>

with:

    submit()

Example:

    Future<Integer> future =
        executor.submit(() -> calculate());

---

▸ Scenario: Need Multiple Dependent Async Operations

Use:

    CompletableFuture
        .thenCompose()

Example:

    getUser()
        .thenCompose(user -> getOrders(user));

---

▸ Scenario: Need to Transform an Async Result

Use:

    thenApply()

Example:

    getUser()
        .thenApply(user -> user.getName());

---

▸ Scenario: Need Two Independent Results

Use:

    thenCombine()

Example:

    getUser()
        and
    getOrders()

then:

    thenCombine()

---

▸ Scenario: Need to Wait for Multiple Async Tasks

Use:

    CompletableFuture.allOf()

---

▸ Scenario: Need the First Completed Task

Use:

    CompletableFuture.anyOf()

---

▸ Scenario: Need Graceful Executor Shutdown

Use:

    shutdown();

then:

    awaitTermination();

and if necessary:

    shutdownNow();

---

▸ Scenario: Executor Queue Is Full

Configure a rejection policy such as:

    AbortPolicy
    CallerRunsPolicy
    DiscardPolicy
    DiscardOldestPolicy

Choose based on the application's desired behavior.

---

▸ Most Important Interview Traps

- Executor separates task submission from thread management.
- ExecutorService extends Executor.
- `execute()` does not return a Future.
- `submit()` returns a Future.
- `Future.get()` is blocking.
- `Future.cancel(true)` requests interruption; it does not forcibly kill a thread.
- `shutdown()` allows already submitted tasks to complete.
- `shutdownNow()` attempts interruption of running tasks and returns queued tasks.
- Always consider executor lifecycle; thread pools should not be left running unnecessarily.
- Fixed thread pool limits worker-thread concurrency, but queued tasks can still grow depending on the queue.
- `ThreadPoolExecutor` queue choice affects when additional threads are created.
- `maximumPoolSize` does not mean the pool immediately creates that many threads.
- `CompletableFuture.supplyAsync()` returns a result.
- `CompletableFuture.runAsync()` does not return a result.
- Default async CompletableFuture stages generally use the common ForkJoinPool unless an executor is supplied.
- `thenApply()` transforms a result.
- `thenAccept()` consumes a result.
- `thenRun()` runs an action without receiving the result.
- `thenCompose()` is for dependent asynchronous operations.
- `thenCombine()` combines independent asynchronous operations.
- `allOf()` waits for all futures.
- `anyOf()` completes when any future completes.
- `get()` uses checked exceptions; `join()` uses unchecked CompletionException for exceptional completion.
- `CompletableFuture` does not automatically mean a new thread is created for every stage.

---

▸ Quick Revision

    Executor
    = executes tasks

    ExecutorService
    = executes tasks + manages lifecycle + Future

    ThreadPool
    = reusable worker threads

    ThreadPoolExecutor
    = configurable thread-pool implementation

    execute()
    = submit Runnable, no result

    submit()
    = submit task, returns Future

    Future
    = represents asynchronous result

    future.get()
    = wait for result

    shutdown()
    = graceful shutdown

    shutdownNow()
    = attempt interruption + return queued tasks

    CompletableFuture
    = asynchronous computation + composition

    supplyAsync()
    = async task with result

    runAsync()
    = async task without result

    thenApply()
    = transform result

    thenAccept()
    = consume result

    thenRun()
    = run action

    thenCompose()
    = chain dependent futures

    thenCombine()
    = combine independent futures

    allOf()
    = wait for all

    anyOf()
    = first completed

---

▸ One-Liner to Remember the Entire Topic

    ExecutorService manages worker threads and task execution, Future represents an asynchronous result, and CompletableFuture provides composable asynchronous workflows.