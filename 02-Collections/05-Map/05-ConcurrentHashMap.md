# ConcurrentHashMap

▸ What is ConcurrentHashMap?

`ConcurrentHashMap` is a thread-safe implementation of the `Map` interface designed for concurrent access by multiple threads.

It allows multiple threads to read and update the map concurrently with much better scalability than synchronizing the entire map.

**Interview one-liner:**

> ConcurrentHashMap is a thread-safe Map designed for concurrent access, allowing high concurrency without locking the entire map.

▸ Package

    import java.util.concurrent.ConcurrentHashMap;

▸ Basic Example

    ConcurrentHashMap<Integer, String> map =
        new ConcurrentHashMap<>();

    map.put(1, "Java");
    map.put(2, "Spring");

    System.out.println(map);

Output:

    {1=Java, 2=Spring}

▸ Why Do We Need ConcurrentHashMap?

Consider a normal `HashMap` being accessed by multiple threads.

If one thread modifies the map while another thread reads or modifies it, race conditions and inconsistent results can occur.

Example:

    Thread 1 → put()
    Thread 2 → put()
    Thread 3 → get()
    Thread 4 → remove()

A normal HashMap does not provide thread-safe concurrent access.

One option is:

    Collections.synchronizedMap(new HashMap<>());

But this synchronizes map operations using a common lock, which can reduce concurrency.

`ConcurrentHashMap` is designed to provide better concurrent performance.

▸ ConcurrentHashMap vs SynchronizedMap

    HashMap
    → Not thread-safe

    synchronizedMap
    → Thread-safe
    → Common synchronization

    ConcurrentHashMap
    → Thread-safe
    → Fine-grained / internal synchronization
    → Better concurrency

**Interview one-liner:**

> `ConcurrentHashMap` provides thread safety with greater concurrency than a map synchronized using a single common lock.

▸ Important Characteristics

- Thread-safe
- Designed for concurrent access
- Does not allow `null` keys
- Does not allow `null` values
- Does not guarantee iteration order
- Does not lock the entire map for every operation
- Supports concurrent reads and updates
- Iterators are weakly consistent
- Does not throw `ConcurrentModificationException` merely because another thread modifies the map during iteration

▸ Why Does ConcurrentHashMap Not Allow null?

The main reason is to avoid ambiguity in concurrent operations.

For example:

    map.get(key)

returning `null` can clearly mean:

    → key is absent

because a `null` value cannot be stored.

This is especially useful when checking results in concurrent code.

**Interview one-liner:**

> ConcurrentHashMap disallows null keys and values so that a null result from `get()` unambiguously means that the key is absent.

▸ Is ConcurrentHashMap Thread-Safe?

Yes.

Multiple threads can safely access and modify a `ConcurrentHashMap`.

Example:

    ConcurrentHashMap<Integer, String> map =
        new ConcurrentHashMap<>();

    Thread t1 = new Thread(() -> {
        map.put(1, "Java");
    });

    Thread t2 = new Thread(() -> {
        map.put(2, "Spring");
    });

Both threads can safely operate on the same map.

▸ Does ConcurrentHashMap Lock the Entire Map?

No.

This is one of the most important interview points.

Older Java implementations used **segment-based locking**.

Modern Java implementations use a different design based on:

- CAS (Compare-And-Swap)
- Volatile reads/writes
- Synchronization on individual bins when needed

Therefore, it does not use one global lock for all operations.

**Interview one-liner:**

> Modern ConcurrentHashMap uses CAS and fine-grained synchronization at the bin level rather than locking the entire map.

▸ ConcurrentHashMap Internals

Conceptually:

    ConcurrentHashMap
           |
           v
       Hash Table
           |
       +---+---+---+---+
       |   |   |   |   |
       v   v   v   v   v
      Bin Bin Bin Bin Bin

A bin can contain:

    → A single node
    → A linked list
    → A balanced tree when treeification occurs

The exact internal implementation can vary across Java versions.

▸ Hashing

Like HashMap, ConcurrentHashMap uses hashing to determine where a key should be stored.

Conceptually:

    key
     |
     v
    hash
     |
     v
    bucket/bin
     |
     v
    entry

Good `hashCode()` implementations help distribute keys across bins.

▸ Collision

A collision occurs when multiple keys map to the same bin.

Conceptually:

    Key A → Bin 3
    Key B → Bin 3
    Key C → Bin 3

ConcurrentHashMap handles collisions using structures such as:

    Linked List

and, when appropriate:

    Red-Black Tree

▸ Treeification

When too many entries accumulate in a bin, the bin can be converted from a linked list into a tree structure, improving lookup performance.

The treeification threshold in current OpenJDK implementations is commonly:

    8

But treeification also depends on the table's capacity.

The implementation uses a minimum table capacity before treeification is performed.

**Interview point:**

> ConcurrentHashMap can treeify heavily-collided bins to improve lookup performance.

▸ put()

Example:

    map.put("Java", 100);

Conceptually:

    1. Calculate hash.
    2. Locate the appropriate bin.
    3. Check whether the bin is empty.
    4. If empty, use CAS when possible to insert.
    5. If the bin is already populated, synchronize on the relevant bin/node when required.
    6. Update the existing value or add a new node.
    7. Perform resizing if required.

The important idea is that operations do not require a global lock over the entire map.

▸ get()

Example:

    Integer value = map.get("Java");

Reads are highly concurrent and generally do not require locking the entire map.

Conceptually:

    key
     |
     v
    hash
     |
     v
    bin
     |
     v
    search
     |
     v
    value

▸ Why is get() Fast?

ConcurrentHashMap uses mechanisms such as:

- Volatile reads
- Hash-based lookup
- Efficient bin traversal
- Tree-based lookup for treeified bins

Reads can happen concurrently with updates.

▸ remove()

Example:

    map.remove("Java");

ConcurrentHashMap safely removes the mapping while other threads may be accessing the map.

The operation uses internal synchronization/CAS mechanisms as required.

▸ putIfAbsent()

Very important concurrent method.

Example:

    map.putIfAbsent("Java", 100);

It adds the mapping only if the key is not already present.

Conceptually:

    if key absent
        add key-value

    if key already exists
        keep existing value

This is safer in concurrent code than:

    if (!map.containsKey(key)) {
        map.put(key, value);
    }

because the two operations are not atomic.

▸ Important Race Condition

This is NOT atomic:

    if (!map.containsKey("Java")) {
        map.put("Java", 100);
    }

Two threads can execute:

    Thread 1 → containsKey() → false
    Thread 2 → containsKey() → false

Then:

    Thread 1 → put()
    Thread 2 → put()

Instead use:

    map.putIfAbsent("Java", 100);

This combines the check and insertion atomically.

**Interview one-liner:**

> `putIfAbsent()` atomically inserts a value only when the key is not already present.

▸ compute()

Example:

    map.compute("Java", (key, value) -> {
        if (value == null) {
            return 1;
        }
        return value + 1;
    });

`compute()` calculates a new value for the specified key.

It is useful for atomic updates.

▸ computeIfAbsent()

Very commonly asked.

Example:

    map.computeIfAbsent(
        "Java",
        key -> 100
    );

The mapping function is executed when the key is absent.

Example use:

    map.computeIfAbsent(
        "Java",
        key -> new ArrayList<>()
    ).add("Collections");

This pattern is commonly used for grouping.

▸ computeIfPresent()

Updates the value only when the key is already present.

Example:

    map.computeIfPresent(
        "Java",
        (key, value) -> value + 1
    );

▸ merge()

Example:

    map.merge(
        "Java",
        1,
        Integer::sum
    );

If the key does not exist:

    Java → 1

If the key already exists with:

    Java → 5

then:

    Java → 6

`merge()` is useful for atomic counting and accumulation.

▸ Atomic Operations

ConcurrentHashMap provides several methods that combine multiple logical steps into one atomic map operation.

Important examples:

    putIfAbsent()
    compute()
    computeIfAbsent()
    computeIfPresent()
    merge()
    replace()
    replaceAll()

These methods are particularly useful in concurrent applications.

▸ Atomic vs Compound Operations

This is atomic:

    map.putIfAbsent("Java", 100);

This is not atomic as a whole:

    if (!map.containsKey("Java")) {
        map.put("Java", 100);
    }

The second example contains two separate operations.

**Interview rule:**

> Thread-safe individual operations do not automatically make a sequence of operations atomic.

▸ replace()

Example:

    map.replace("Java", 200);

It replaces the value only if the key exists.

There is also a conditional form:

    map.replace("Java", 100, 200);

This replaces:

    100 → 200

only if the current value is `100`.

▸ replaceAll()

Example:

    map.replaceAll(
        (key, value) -> value + 10
    );

This updates values according to the supplied function.

▸ Weakly Consistent Iterator

ConcurrentHashMap iterators are **weakly consistent**.

This means:

- They do not throw `ConcurrentModificationException` merely because another thread modifies the map.
- They can reflect some modifications that happen after iteration begins.
- They do not provide a snapshot of the map at one exact point in time.
- They are designed to work safely with concurrent modifications.

Example:

    for (Map.Entry<String, Integer> entry : map.entrySet()) {
        System.out.println(entry);
    }

Another thread can modify the map while this iteration is happening.

**Interview one-liner:**

> ConcurrentHashMap iterators are weakly consistent: they tolerate concurrent modifications and may reflect some changes made during iteration.

▸ ConcurrentModificationException

Unlike many normal collection iterators, ConcurrentHashMap's iterators do not throw `ConcurrentModificationException` merely because another thread modifies the map during iteration.

This does NOT mean the iteration is a snapshot.

It means the iterator is designed for concurrent modification.

▸ Fail-Fast vs Weakly Consistent

    ArrayList / HashMap iterator
    → Generally fail-fast
    → Concurrent structural modification may cause ConcurrentModificationException

    ConcurrentHashMap iterator
    → Weakly consistent
    → Does not throw ConcurrentModificationException merely due to concurrent modification

▸ size()

`size()` is thread-safe, but in a concurrently changing map the result represents a state that may change immediately after it is obtained.

Example:

    int size = map.size();

Another thread can add/remove entries immediately afterward.

Therefore, do not assume that:

    if (map.size() == 0)

remains true in a concurrent environment.

▸ isEmpty()

Similarly, `isEmpty()` is thread-safe, but the result can become outdated immediately because another thread may modify the map.

For concurrent algorithms, avoid relying on a check followed by a separate operation unless the complete operation is designed to be atomic.

▸ Concurrency Level

Older implementations of ConcurrentHashMap exposed a `concurrencyLevel` constructor parameter as part of the segmented-locking design.

Modern implementations do not use segments in the old sense.

The parameter is retained for compatibility and can influence sizing behavior, but it should not be interpreted as the number of locks.

**Interview point:**

> Modern ConcurrentHashMap does not use the old segment-based locking model, even though the concurrency-level constructor still exists.

▸ Initial Capacity

You can specify an initial capacity:

    ConcurrentHashMap<String, Integer> map =
        new ConcurrentHashMap<>(100);

This can reduce resizing when you know the approximate number of entries.

▸ Load Factor

ConcurrentHashMap also has a load-factor-related constructor.

Example:

    ConcurrentHashMap<String, Integer> map =
        new ConcurrentHashMap<>(
            100,
            0.75f
        );

For interview purposes, remember that capacity and load factor influence when the table needs to resize.

▸ Resizing

When the table becomes sufficiently full, ConcurrentHashMap resizes its internal table.

A major advantage is that resizing can be performed cooperatively by multiple threads.

Threads encountering a resize can help transfer bins to the new table.

This allows resizing to happen without stopping all map operations behind one global lock.

**Interview one-liner:**

> During resizing, multiple threads can help transfer entries to the new table.

▸ Why ConcurrentHashMap Is Better Than Hashtable

`Hashtable` is a legacy synchronized Map.

Conceptually:

    Hashtable
    → Synchronizes map operations
    → Lower concurrency

    ConcurrentHashMap
    → Designed for concurrent access
    → Fine-grained/internal synchronization
    → Better scalability

**Interview one-liner:**

> ConcurrentHashMap is preferred over legacy Hashtable for modern concurrent applications because it provides better scalability and concurrency.

▸ ConcurrentHashMap vs Hashtable

| Feature | Hashtable | ConcurrentHashMap |
|---|---|---|
| Thread-safe | Yes | Yes |
| Legacy | Yes | No |
| null key | No | No |
| null value | No | No |
| Concurrency | Lower | Higher |
| Locking | Synchronized methods | Fine-grained/internal synchronization + CAS |
| Iterator | Fail-fast/enumeration behavior | Weakly consistent |
| Modern choice | Usually no | Yes |

▸ ConcurrentHashMap vs HashMap

| Feature | HashMap | ConcurrentHashMap |
|---|---|---|
| Thread-safe | No | Yes |
| null key | One allowed | Not allowed |
| null value | Allowed | Not allowed |
| Ordering | No guaranteed order | No guaranteed order |
| Concurrent access | Not designed for it | Designed for it |
| Iterator | Generally fail-fast | Weakly consistent |
| Performance | Better for single-threaded use | Better for concurrent use |

▸ ConcurrentHashMap vs synchronizedMap

Example:

    Map<String, Integer> map =
        Collections.synchronizedMap(
            new HashMap<>()
        );

This provides thread-safe access but uses synchronization around map operations.

ConcurrentHashMap is specifically designed for higher concurrency.

    synchronizedMap
    → Simple thread-safe wrapper

    ConcurrentHashMap
    → Concurrent collection designed from the ground up for concurrent access

▸ Important Difference in Iteration

For a synchronized map, iteration usually requires external synchronization.

Example:

    synchronized (map) {
        for (String key : map.keySet()) {
            System.out.println(key);
        }
    }

With ConcurrentHashMap, its iterator is designed to tolerate concurrent modifications.

**Interview one-liner:**

> ConcurrentHashMap provides weakly consistent iterators, so iteration does not require locking the entire map against concurrent updates.

▸ Does ConcurrentHashMap Guarantee Ordering?

No.

ConcurrentHashMap does not guarantee:

    insertion order

or:

    sorted order

If ordering is required, use an appropriate ordered/concurrent data structure.

For example:

    ConcurrentSkipListMap

provides a concurrent sorted Map.

▸ ConcurrentHashMap vs ConcurrentSkipListMap

    ConcurrentHashMap
    → Thread-safe
    → Hash-based
    → No ordering guarantee
    → Average O(1) basic operations

    ConcurrentSkipListMap
    → Thread-safe
    → Sorted by keys
    → O(log n) expected operations
    → Supports NavigableMap operations

**Interview one-liner:**

> Use ConcurrentHashMap for high-performance concurrent hash-based access and ConcurrentSkipListMap when concurrent sorted-key operations are required.

▸ Does ConcurrentHashMap Support Map Interface?

Yes.

    ConcurrentHashMap implements ConcurrentMap

and:

    ConcurrentMap extends Map

Therefore it can be used wherever a Map is expected.

▸ ConcurrentMap

`ConcurrentMap` provides atomic methods useful for concurrent map operations.

Examples:

    putIfAbsent()
    remove(key, value)
    replace()
    replaceAll()

`ConcurrentHashMap` is a major implementation of `ConcurrentMap`.

▸ Atomic remove()

This is useful:

    map.remove("Java", 100);

It removes the mapping only if:

    key = "Java"

and:

    value = 100

match.

This check-and-remove operation is atomic.

▸ Atomic replace()

Example:

    map.replace(
        "Java",
        100,
        200
    );

The replacement happens only when the current value matches `100`.

This is useful for compare-and-update operations.

▸ Frequency Counting Example

ConcurrentHashMap is commonly used for thread-safe counting.

Example:

    ConcurrentHashMap<String, Integer> counts =
        new ConcurrentHashMap<>();

    counts.merge(
        "Java",
        1,
        Integer::sum
    );

If multiple threads execute this operation, the merge operation provides the required atomic map update.

▸ Grouping Example

Example:

    ConcurrentHashMap<String, List<String>> map =
        new ConcurrentHashMap<>();

    map.computeIfAbsent(
        "Java",
        key -> new ArrayList<>()
    ).add("Collections");

Be careful: while `computeIfAbsent()` provides an atomic map-level update, the `ArrayList` stored as the value is not automatically thread-safe.

If multiple threads modify the same list concurrently, the list itself needs appropriate synchronization or a concurrent collection.

**Important interview point:**

> Making the map thread-safe does not automatically make mutable objects stored as values thread-safe.

▸ ConcurrentHashMap with Collection Values

This:

    ConcurrentHashMap<String, List<Integer>>

does NOT mean the lists are thread-safe.

The map is thread-safe.

The `List` objects may not be.

For concurrent list updates, you might use:

    ConcurrentHashMap<String, CopyOnWriteArrayList<Integer>>

or another appropriate concurrent collection depending on the workload.

▸ Atomicity vs Thread Safety

ConcurrentHashMap guarantees thread-safe map operations.

But this sequence:

    Integer value = map.get("count");
    map.put("count", value + 1);

is not atomic as a whole.

Two threads can read the same value.

Example:

    Initial value = 5

    Thread 1 → get() → 5
    Thread 2 → get() → 5

    Thread 1 → put(6)
    Thread 2 → put(6)

Expected:

    7

Actual:

    6

Use:

    map.merge(
        "count",
        1,
        Integer::sum
    );

for an atomic map-level update.

**Very important interview point:**

> ConcurrentHashMap makes individual map operations thread-safe, but compound read-modify-write logic must use atomic methods such as merge, compute, or putIfAbsent.

▸ Why Not Just Synchronize HashMap?

You can synchronize a HashMap:

    synchronized (map) {
        map.put(key, value);
    }

But if every operation requires the same lock, threads have to wait for each other.

Conceptually:

    Thread 1
       |
      LOCK
       |
    operation
       |
    UNLOCK

    Thread 2
       |
      WAIT

ConcurrentHashMap allows much more concurrency by avoiding one global lock for all operations.

▸ Performance

For a single-threaded application:

    HashMap

is generally the simpler and often faster choice because it has no concurrency-management overhead.

For concurrent access:

    ConcurrentHashMap

is usually preferred over manually synchronizing a HashMap.

**Interview rule:**

> Do not use ConcurrentHashMap just because it is thread-safe; choose it when concurrent access is actually required.

▸ Common Use Cases

ConcurrentHashMap is useful for:

- Shared in-memory caches
- Frequency counters
- Thread-safe lookup tables
- Application metadata shared across threads
- Concurrent aggregation
- Session or configuration maps
- Producer/consumer-related shared state
- Concurrent grouping and indexing

▸ Example: Thread-Safe Cache

    ConcurrentHashMap<String, String> cache =
        new ConcurrentHashMap<>();

    cache.put("user1", "John");

    String value = cache.get("user1");

Multiple threads can safely access the cache.

For lazy initialization:

    cache.computeIfAbsent(
        "user1",
        key -> loadUser(key)
    );

This avoids manually performing a separate check and insertion.

▸ Common Mistake

Do not write:

    if (!cache.containsKey(key)) {
        cache.put(key, value);
    }

Use:

    cache.putIfAbsent(key, value);

because the latter performs the check-and-insert atomically.

▸ Common Interview Question

**Q: Why is ConcurrentHashMap faster than Hashtable?**

> Hashtable synchronizes its map operations using a common synchronization mechanism, while ConcurrentHashMap allows much greater concurrency using mechanisms such as CAS and fine-grained synchronization.

▸ Common Interview Question

**Q: Why doesn't ConcurrentHashMap allow null?**

> Because in concurrent operations, returning null from get() must unambiguously indicate that the key is absent.

▸ Common Interview Question

**Q: Does ConcurrentHashMap use segments?**

> Older Java implementations used segments, but modern implementations use a bucket/bin-based design with CAS and synchronized blocks on individual bins when required.

▸ Common Interview Question

**Q: Is ConcurrentHashMap completely lock-free?**

> No. It uses lock-free techniques such as CAS where possible, but synchronization can still be used for certain updates and bin operations.

▸ Common Interview Question

**Q: Can multiple threads read ConcurrentHashMap simultaneously?**

> Yes. ConcurrentHashMap is designed to support highly concurrent reads.

▸ Common Interview Question

**Q: Can one thread write while another thread reads?**

> Yes. ConcurrentHashMap is specifically designed to support concurrent reads and updates.

▸ Common Interview Question

**Q: Can ConcurrentHashMap have multiple writers?**

> Yes. Multiple threads can safely perform updates concurrently.

▸ Common Interview Question

**Q: Does ConcurrentHashMap maintain insertion order?**

> No. It does not guarantee insertion order.

▸ Common Interview Question

**Q: Which collection should you use for a thread-safe sorted Map?**

> ConcurrentSkipListMap.

▸ Common Interview Question

**Q: Which collection should you use for a thread-safe sorted Set?**

> ConcurrentSkipListSet.

▸ Common Interview Question

**Q: What happens if another thread modifies ConcurrentHashMap during iteration?**

> The iterator remains usable and does not throw ConcurrentModificationException merely because of that concurrent modification; it is weakly consistent.

▸ Common Interview Question

**Q: Is ConcurrentHashMap faster than HashMap?**

> Not necessarily. HashMap is generally preferable for single-threaded use; ConcurrentHashMap is designed for safe and scalable concurrent access.

▸ Common Interview Question

**Q: Is `get()` atomic?**

> Yes, the individual map operation is thread-safe, but a sequence such as get followed by put is not automatically atomic.

▸ Common Interview Question

**Q: How do you atomically increment a value?**

Use:

    map.merge(
        key,
        1,
        Integer::sum
    );

▸ Common Interview Question

**Q: How do you insert only if a key is absent?**

Use:

    map.putIfAbsent(key, value);

▸ Common Interview Question

**Q: Does a thread-safe map make its values thread-safe?**

> No. Only the map's operations are thread-safe; mutable objects stored as values may still require their own synchronization.

▸ Quick Comparison

| Feature | HashMap | Hashtable | synchronizedMap | ConcurrentHashMap |
|---|---|---|---|---|
| Thread-safe | No | Yes | Yes | Yes |
| null key | One | No | Depends on backing map | No |
| null values | Yes | No | Depends on backing map | No |
| Concurrency | No | Low | Lower | High |
| Ordering | No guarantee | No guarantee | Depends on backing map | No guarantee |
| Modern concurrent choice | No | No | Sometimes | Yes |
| Iterator | Generally fail-fast | Legacy behavior | Requires synchronization during iteration | Weakly consistent |

▸ Quick Revision

    HashMap
    → Not thread-safe

    Hashtable
    → Thread-safe but legacy

    synchronizedMap
    → Thread-safe wrapper around another Map

    ConcurrentHashMap
    → Thread-safe + high concurrency

    null key
    → Not allowed

    null value
    → Not allowed

    Ordering
    → No guarantee

    Basic lookup
    → Average O(1)

    Internal structure
    → Hash table + linked/tree bins

    Modern synchronization
    → CAS + bin-level synchronization

    Old implementation
    → Segments

    Iterator
    → Weakly consistent

    putIfAbsent()
    → Atomic insert-if-absent

    computeIfAbsent()
    → Atomic compute-if-absent

    merge()
    → Atomic merge/update

    Concurrent sorted Map
    → ConcurrentSkipListMap

▸ Must Remember for Interview

> ConcurrentHashMap is a thread-safe hash-based Map designed for high concurrency.

> It does not allow null keys or null values.

> Modern ConcurrentHashMap does not use the old segment-based locking design.

> It uses CAS and fine-grained synchronization where required instead of locking the entire map.

> Multiple threads can read and update the map concurrently.

> Its iterators are weakly consistent, not fail-fast.

> `putIfAbsent()`, `compute()`, `computeIfAbsent()`, and `merge()` are important atomic operations.

> Thread-safe map operations do not automatically make compound operations atomic.

> A thread-safe ConcurrentHashMap does not make mutable values stored inside it thread-safe.

> Use `ConcurrentSkipListMap` when you need a thread-safe sorted Map.

▸ One-Liner to Remember

> **ConcurrentHashMap = HashMap + thread safety + high concurrency + no null + weakly consistent iteration.**

▸ Final Interview Definition

> ConcurrentHashMap is a thread-safe, hash-based Map implementation designed for high-concurrency access, using CAS and fine-grained synchronization to allow multiple threads to operate on the map without locking the entire map.