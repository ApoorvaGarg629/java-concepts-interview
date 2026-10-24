# Garbage Collection

▸ Overview

Garbage Collection (GC) is the JVM's automatic memory-management mechanism for reclaiming memory occupied by objects that are no longer reachable.

Basic idea:

    Object created
         |
         v
       Heap
         |
         v
    Object becomes unreachable
         |
         v
    Garbage Collector
         |
         v
    Memory reclaimed

Interview One-Liner:

> Garbage Collection automatically identifies unreachable objects and reclaims their memory.

---

▸ Why Garbage Collection?

Without automatic memory management, developers would need to manually free objects.

Java automatically manages object memory.

Example:

    Student s = new Student();

Later:

    s = null;

If there are no other references to that object, it may become eligible for garbage collection.

---

▸ Reachability

An object is generally eligible for GC when it is no longer reachable from any GC Root.

Example:

    Student s = new Student();

Initially:

    GC Root
       |
       v
       s
       |
       v
    Student Object

After:

    s = null;

The reference is removed:

    GC Root
       |
       v
      null

    Student Object
          X
      unreachable

The object becomes eligible for GC.

Important:

Eligible for GC does NOT mean immediately collected.

---

▸ Garbage Collection and GC Roots

The GC determines reachability starting from special references called:

    GC Roots

Conceptually:

    GC Roots
       |
       +----------------+
       |                |
       v                v
    Object A          Object B
       |
       v
    Object C

Objects reachable from GC Roots are considered live.

Objects not reachable from GC Roots are candidates for reclamation.

---

▸ Common GC Roots

Common categories include:

- Active Java thread references
- Local variables/references in active stack frames
- Static references
- JNI/native references

The exact root set depends on JVM implementation and execution state.

Interview One-Liner:

> GC Roots are starting points used by the garbage collector to determine which objects are reachable.

---

▸ Reachability Example

    Object A
       |
       v
    Object B
       |
       v
    Object C

If:

    GC Root -> Object A

Then:

    A reachable
    B reachable
    C reachable

If the root reference to A disappears:

    GC Root -> null

Then:

    A unreachable
    B unreachable
    C unreachable

All can become eligible for collection.

---

▸ Cyclic References

Java can collect objects even when they reference each other.

Example:

    A -> B
    B -> A

If there is no path from a GC Root:

    GC Root
       X

    A <----> B

Both objects are unreachable and can become eligible for GC.

Important Interview Point:

> Java GC is based on reachability from GC Roots, so circular references alone do not prevent garbage collection.

---

▸ Generational Garbage Collection

Many JVM garbage collectors use generational concepts based on the observation that many objects die young.

Conceptually:

    Heap
      |
      +-- Young Generation
      |
      +-- Old Generation

Young Generation:

- Newly allocated objects generally start here in generational collectors.
- Many objects become unreachable quickly.

Old Generation:

- Objects that survive sufficiently long may be promoted here.

Important:

The exact organization depends on the garbage collector.

---

▸ Young Generation

A traditional generational layout is:

    Young Generation
          |
          +-- Eden
          |
          +-- Survivor 0
          |
          +-- Survivor 1

New objects are generally allocated in:

    Eden

Objects that survive young collections may move between survivor spaces and eventually be promoted to Old Generation.

---

▸ Eden

Most new objects are initially allocated in the Eden area in traditional generational collectors.

Example:

    new Student()
          |
          v
        Eden

If the object becomes unreachable quickly, its memory can be reclaimed during young-generation collection.

---

▸ Survivor Spaces

Traditional generational collectors use two survivor spaces:

    Survivor 0
    Survivor 1

Objects surviving a young collection can be copied between survivor spaces.

Conceptually:

    Eden
      |
      v
    Survivor 0
      |
      v
    Survivor 1
      |
      v
    Old Generation

The exact process depends on the collector.

---

▸ Young GC

A collection focused on the young generation is often called:

    Young GC
    or
    Minor GC

Young collections are generally designed to reclaim short-lived objects efficiently.

Important:

"Minor GC" is common terminology, but exact event names and behavior depend on the garbage collector and JVM version.

---

▸ Old Generation

Objects that survive enough collection cycles may be promoted to Old Generation.

Example:

    Object created
         |
         v
       Eden
         |
         v
    Survivor spaces
         |
         v
    Old Generation

Long-lived objects may eventually reside in Old Generation.

---

▸ Major GC vs Full GC

Terminology around "Major GC" and "Full GC" varies across JVMs and collectors.

For interview purposes:

### Major GC

Often used to describe collection involving the Old Generation.

### Full GC

Usually refers to a collection involving a much larger portion of the Heap and potentially other memory structures depending on the collector.

Important:

> Do not assume Major GC and Full GC are universally identical terms across all collectors.

---

▸ Stop-The-World

A Stop-The-World (STW) pause means application threads are paused for a JVM operation.

Some phases of garbage collection may require STW pauses.

Conceptually:

    Application Threads
          |
          v
       PAUSED
          |
          v
      GC phase
          |
          v
       RESUME

Modern collectors perform substantial work concurrently, but they can still have stop-the-world phases.

Interview One-Liner:

> Stop-The-World means application threads are temporarily paused while the JVM performs a required operation.

---

▸ Does System.gc() Force GC?

Calling:

    System.gc();

requests garbage collection.

It does NOT guarantee that GC will immediately occur.

The JVM is free to decide whether and when to perform collection.

Interview One-Liner:

> `System.gc()` is only a request to the JVM; it does not guarantee immediate garbage collection.

---

▸ finalize()

Historically, Java provided:

    finalize()

for cleanup before an object was reclaimed.

However, finalization is deprecated for removal and should not be used for normal resource management.

Use:

- try-with-resources
- AutoCloseable
- Explicit cleanup mechanisms

Important:

GC is for memory management.

It should not be used as a substitute for deterministic resource management.

---

▸ Garbage Collection vs Resource Management

GC manages memory occupied by unreachable Java objects.

It does NOT reliably manage external resources such as:

- File handles
- Database connections
- Network connections
- Sockets

Use:

    try-with-resources

for resources implementing `AutoCloseable`.

Example:

    try (BufferedReader reader =
            new BufferedReader(
                new FileReader("data.txt"))) {

        // use reader

    }

The resource is closed deterministically.

---

▸ Common GC Algorithms

Different JVM collectors use different algorithms.

Common concepts include:

- Mark
- Sweep
- Compact
- Copy
- Generational collection
- Concurrent collection

---

▸ Mark and Sweep

Conceptually:

### Mark

Identify reachable objects.

    Heap
      |
      v
    Mark reachable objects

### Sweep

Reclaim memory occupied by unreachable objects.

    Reachable -> Keep
    Unreachable -> Reclaim

Potential issue:

    Fragmentation

---

▸ Mark and Compact

Mark reachable objects and then compact them to reduce fragmentation.

Conceptually:

    Before:

    [Live][Dead][Dead][Live][Dead][Live]

    After:

    [Live][Live][Live][Free][Free][Free]

---

▸ Copying Collection

A copying collector copies live objects from one region to another and reclaims the original region.

This is useful for regions where most objects die quickly.

---

▸ Garbage Collector Examples

Common HotSpot collectors include:

- Serial GC
- Parallel GC
- G1 GC
- ZGC
- Shenandoah

The choice depends on JVM version, application workload, latency requirements, heap size, and other factors.

---

▸ G1 GC

G1 stands for:

    Garbage-First Garbage Collector

It divides the Heap into regions rather than using one contiguous young/old physical layout.

It still uses generational concepts in modern configurations.

G1 attempts to prioritize regions with more reclaimable garbage while considering pause-time goals.

Important:

> G1 is region-based and designed to provide predictable pause-time behavior while handling large heaps.

---

▸ ZGC and Shenandoah

ZGC and Shenandoah are low-latency garbage collectors designed to perform substantial GC work concurrently with application threads.

They aim to keep pause times low even for large heaps.

Do not memorize exact performance numbers because they depend on workload and JVM configuration.

---

▸ GC Eligibility vs GC Execution

Important distinction:

    Object becomes unreachable
            |
            v
    Eligible for GC
            |
            v
    JVM decides when to collect
            |
            v
    GC reclaims memory

Therefore:

> Becoming unreachable does not mean the object is immediately removed from memory.

---

▸ Object Life Cycle

    Object created
         |
         v
       Eden
         |
         v
    Survives collections
         |
         v
    Survivor spaces
         |
         v
    Old Generation
         |
         v
    Becomes unreachable
         |
         v
    Eligible for GC
         |
         v
    GC reclaims memory

This is a simplified generational model.

---

▸ Interview Questions

### What is Garbage Collection?

> Garbage Collection automatically identifies unreachable objects and reclaims their memory.

### When is an object eligible for GC?

> When it is no longer reachable from any GC Root.

### Does eligible for GC mean immediately collected?

> No. It only means the object can be reclaimed when the JVM performs GC.

### What are GC Roots?

> GC Roots are starting points used to determine object reachability.

### Can Java collect circular references?

> Yes. If the objects are not reachable from any GC Root, circular references can still be collected.

### What is Young Generation?

> It is the region where newly allocated objects generally start in generational collectors.

### What is Old Generation?

> It contains objects that survive sufficiently long and are promoted from younger regions in generational collectors.

### Does System.gc() force GC?

> No. It only requests GC.

### What is Stop-The-World?

> It is a period during which application threads are paused for a JVM operation.

### Does GC close database connections?

> No. GC manages memory, not deterministic external-resource cleanup.

---

▸ Common Interview Traps

> Unreachable does not mean immediately collected.

> `System.gc()` does not guarantee GC.

> GC Roots are not the same as all objects.

> Circular references do not necessarily prevent GC.

> GC manages memory, not deterministic resource cleanup.

> Major GC and Full GC terminology can vary between collectors.

> Do not assume every modern collector has the exact same Eden/Survivor/Old physical layout.

---

▸ Final One-Liners

> GC = automatic reclamation of unreachable object memory.

> GC Roots = starting points for reachability analysis.

> Unreachable from GC Roots = eligible for GC.

> Eligible for GC does not mean immediately collected.

> Young Generation = where short-lived/new objects are generally handled in generational collectors.

> Old Generation = where sufficiently long-lived objects may be promoted.

> `System.gc()` = request, not guarantee.

> Stop-The-World = application threads temporarily paused.

> Circular references can be collected if unreachable from GC Roots.