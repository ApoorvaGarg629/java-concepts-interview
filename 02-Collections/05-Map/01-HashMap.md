# HashMap

## ▸ What is HashMap?

`HashMap` is a collection that stores data as **key-value pairs**.

It implements the `Map` interface and uses hashing internally for efficient insertion, lookup, and removal.

---

## ▸ Main Characteristics

- Stores **key-value pairs**
- Implements `Map`
- Keys are **unique**
- Values can be duplicated
- Allows **one null key**
- Allows **multiple null values**
- Does not guarantee insertion order
- Not synchronized
- Not thread-safe
- Uses hashing internally
- Average O(1) for `put()`, `get()`, `remove()`, and `containsKey()`
- Uses `hashCode()` and `equals()` for key lookup
- Can contain different types if raw types are used, but generics are recommended

---

## ▸ HashMap Class Hierarchy

    Object
       ↓
    AbstractMap
       ↓
    HashMap

`HashMap` implements:

    Map<K, V>
    Cloneable
    Serializable

---

## ▸ Important HashMap Methods

### ▸ put()

Adds or updates a key-value pair.

    HashMap<Integer, String> map = new HashMap<>();

    map.put(1, "Java");
    map.put(2, "Python");

Result:

    {1=Java, 2=Python}

If the key already exists, its value is replaced.

    map.put(1, "C++");

Now:

    {1=C++, 2=Python}

### ▸ Interview point

> **HashMap does not allow duplicate keys; inserting an existing key replaces its old value.**

---

## ▸ get()

Returns the value associated with a key.

    map.get(1);

If:

    1 → "Java"

then:

    map.get(1) → "Java"

If the key does not exist:

    map.get(100) → null

---

## ▸ getOrDefault()

Returns the value if the key exists; otherwise returns a default value.

    map.getOrDefault(10, "Unknown");

If key `10` does not exist:

    "Unknown"

---

## ▸ containsKey()

Checks whether a key exists.

    map.containsKey(1);

Returns:

    true

or:

    false

### ▸ Interview point

> **containsKey() checks keys, not values.**

---

## ▸ containsValue()

Checks whether a value exists.

    map.containsValue("Java");

Returns:

    true
    false

Unlike key lookup, value lookup generally requires scanning entries.

---

## ▸ remove()

Removes the mapping for a key.

    map.remove(1);

If:

    1 → "Java"

is present, it is removed.

---

## ▸ remove(key, value)

Removes the mapping only if both key and value match.

    map.remove(1, "Java");

Returns:

    true

if the exact mapping was removed.

Otherwise:

    false

---

## ▸ replace()

Replaces the value for an existing key.

    map.replace(1, "Java 21");

---

## ▸ putIfAbsent()

Adds a mapping only if the key does not already have a mapping.

    map.putIfAbsent(1, "Java");

If key `1` already exists, its existing value remains unchanged.

---

## ▸ size()

Returns the number of key-value mappings.

    map.size();

---

## ▸ isEmpty()

Checks whether the map contains no mappings.

    map.isEmpty();

---

## ▸ clear()

Removes all mappings.

    map.clear();

---

## ▸ keySet()

Returns a Set view of all keys.

    Set<Integer> keys = map.keySet();

---

## ▸ values()

Returns a Collection view of all values.

    Collection<String> values = map.values();

Values can contain duplicates.

---

## ▸ entrySet()

Returns a Set of key-value entries.

    Set<Map.Entry<Integer, String>> entries =
        map.entrySet();

This is commonly used to iterate over both keys and values.

---

## ▸ Iterating HashMap

### ▸ Using keySet()

    for (Integer key : map.keySet()) {
        System.out.println(key + " = " + map.get(key));
    }

This performs a separate lookup for each key.

---

### ▸ Using entrySet()

Preferred when both key and value are required:

    for (Map.Entry<Integer, String> entry : map.entrySet()) {
        System.out.println(
            entry.getKey() + " = " + entry.getValue()
        );
    }

### ▸ Interview point

> **Use entrySet() when you need both key and value during iteration.**

---

## ▸ HashMap Internal Working

This is the most important HashMap interview topic.

HashMap internally uses a **hash table**.

Conceptually:

    HashMap
       ↓
    Array of buckets
       ↓
    Each bucket can contain entries

Each entry contains information conceptually similar to:

    hash
    key
    value
    next

Modern Java implementations can use a tree structure inside a heavily collided bucket.

---

## ▸ Internal Entry Structure

Conceptually:

    Node<K,V>

contains:

    hash
    key
    value
    next

Example:

    hash → calculated hash
    key  → "Java"
    value → 100
    next → next entry in same bucket

The exact internal implementation can vary by JDK version, so focus on the conceptual model for interviews.

---

## ▸ What Happens During put()?

Suppose:

    map.put("Java", 100);

Conceptually:

    "Java"
       ↓
    hashCode()
       ↓
    hash calculation
       ↓
    bucket index
       ↓
    check bucket
       ↓
    existing key?
       ↓
    equals()
       ↓
    update existing value
    OR
    add new entry

### ▸ One-line approach

> **HashMap uses the key's hashCode() to locate a bucket and equals() to determine whether the key already exists.**

---

## ▸ What Happens During get()?

Suppose:

    map.get("Java");

Conceptually:

    "Java"
       ↓
    hashCode()
       ↓
    hash calculation
       ↓
    bucket index
       ↓
    compare keys using equals()
       ↓
    return value

### ▸ One-line approach

> **get() hashes the key to find the bucket and then uses equals() to find the matching key.**

---

## ▸ hashCode() in HashMap

HashMap uses the key's `hashCode()` to determine where the key should be stored.

Example:

    String key = "Java";

    int hash = key.hashCode();

The hash helps identify the bucket.

### ▸ Important

HashMap does not simply use the raw `hashCode()` directly as the array index.

A hash calculation is performed to help distribute keys across buckets.

---

## ▸ Bucket

A bucket is a position in the internal hash table.

Conceptually:

    bucket[0]
    bucket[1]
    bucket[2]
    bucket[3]
    ...
    bucket[n]

Multiple keys can end up in the same bucket because hash collisions are possible.

---

## ▸ Hash Collision

A collision occurs when different keys map to the same bucket.

Example:

    Key A → bucket 5
    Key B → bucket 5

Both entries are stored in the same bucket.

HashMap handles the collision by maintaining multiple entries within that bucket.

---

## ▸ Collision Handling

Historically, collided entries were maintained using a linked structure.

Modern Java HashMap can transform a heavily populated bucket into a **Red-Black Tree** when certain thresholds are reached.

Conceptually:

    Bucket
      ↓
    Linked entries

and under high collision conditions:

    Bucket
      ↓
    Red-Black Tree

### ▸ Interview one-liner

> **HashMap handles collisions using linked nodes and can treeify a heavily collided bucket into a Red-Black Tree.**

---

## ▸ Red-Black Tree in HashMap

A heavily collided bucket can be converted from a linked structure into a Red-Black Tree.

This improves lookup performance within that bucket.

Instead of potentially:

    O(n)

search within a long collision chain, tree-based lookup can be approximately:

    O(log n)

under the relevant conditions.

### ▸ Important

Do not say:

> "Every HashMap bucket is a Red-Black Tree."

That is incorrect.

Most buckets are not treeified.

---

## ▸ Treeification Threshold

In modern JDK HashMap implementations, a bucket can be treeified when its number of nodes reaches a threshold of:

    8

However, treeification also depends on the table capacity being sufficiently large.

A commonly remembered threshold is:

    TREEIFY_THRESHOLD = 8

### ▸ Important interview point

> **A collision bucket may be treeified around 8 entries, provided the table is large enough; otherwise HashMap may resize instead.**

---

## ▸ Untreeification Threshold

When entries in a treeified bucket decrease sufficiently, the bucket can be converted back to a linked structure.

A commonly remembered implementation threshold is:

    UNTREEIFY_THRESHOLD = 6

### ▸ Interview note

This is an implementation detail and is less important than understanding why treeification exists.

---

## ▸ Minimum Treeify Capacity

HashMap uses a minimum table capacity before treeification.

A commonly remembered value is:

    MIN_TREEIFY_CAPACITY = 64

If the table is smaller than this, HashMap may resize instead of treeifying.

### ▸ Interview-friendly explanation

> **Before treeifying a collision-heavy bucket, HashMap may resize the table if the overall table is still small.**

---

## ▸ equals() and hashCode() Contract

HashMap relies on both methods for key lookup.

The important rule is:

> **If two keys are equal according to equals(), they must have the same hashCode().**

Formally:

    a.equals(b) == true
    =>
    a.hashCode() == b.hashCode()

But:

    a.hashCode() == b.hashCode()

does not guarantee:

    a.equals(b) == true

because collisions are possible.

---

## ▸ Easy Way to Remember hashCode() and equals()

> **hashCode() finds the bucket; equals() confirms the key.**

This is one of the most important HashMap interview lines.

---

## ▸ What If Two Keys Have Same hashCode()?

They may end up in the same bucket.

Then HashMap uses:

    equals()

to determine whether they are actually the same logical key.

If:

    equals() == false

both keys can exist.

---

## ▸ What If equals() Is True but hashCode() Is Different?

This violates the Java contract.

HashMap may place the two logically equal keys into different buckets.

As a result, lookup and duplicate-key handling can fail.

### ▸ Interview answer

> **Equal objects must always have the same hash code when used as HashMap keys.**

---

## ▸ Duplicate Keys

HashMap does not allow duplicate keys.

Example:

    map.put(1, "Java");
    map.put(1, "Python");

Final mapping:

    1 → "Python"

The second `put()` replaces the first value.

### ▸ Important

The key is not duplicated.

The value is updated.

---

## ▸ Duplicate Values

HashMap allows duplicate values.

Example:

    map.put(1, "Java");
    map.put(2, "Java");

This is valid.

Result:

    1 → Java
    2 → Java

---

## ▸ Null Key

HashMap allows **one null key**.

Example:

    map.put(null, "Java");

This is valid.

Adding another null key:

    map.put(null, "Python");

updates the existing null-key mapping.

Final result:

    null → Python

---

## ▸ Null Values

HashMap allows multiple null values.

Example:

    map.put(1, null);
    map.put(2, null);

Both mappings are valid.

---

## ▸ Why Only One Null Key?

Keys must be unique.

Therefore:

    null → value1
    null → value2

cannot exist as two separate key mappings.

The second insertion replaces the first value.

---

## ▸ Does HashMap Maintain Insertion Order?

No.

HashMap does not guarantee insertion order.

Do not rely on the order produced by:

    map.entrySet()

or:

    map.keySet()

### ▸ Interview answer

> **HashMap does not guarantee insertion order.**

If insertion order is required, use:

    LinkedHashMap

---

## ▸ HashMap vs LinkedHashMap

| Feature | HashMap | LinkedHashMap |
|---|---|---|
| Key-value pairs | Yes | Yes |
| Duplicate keys | No | No |
| Insertion order | Not guaranteed | Maintained |
| Null key | One | One |
| Average lookup | O(1) | O(1) |
| Extra linked structure | No | Yes |

### ▸ Easy memory trick

> **HashMap = key-value + no guaranteed order**

> **LinkedHashMap = key-value + insertion order**

---

## ▸ HashMap vs TreeMap

| Feature | HashMap | TreeMap |
|---|---|---|
| Ordering | No guarantee | Sorted by key |
| Internal structure | Hash table | Red-Black Tree |
| Average `get()` | O(1) | O(log n) |
| Average `put()` | O(1) | O(log n) |
| Null key | One allowed | Not allowed with natural ordering |
| Duplicate keys | No | No |

### ▸ Easy memory trick

> **HashMap = hashing + fast lookup**

> **TreeMap = sorted keys + O(log n)**

---

## ▸ HashMap vs Hashtable

| Feature | HashMap | Hashtable |
|---|---|---|
| Legacy | No | Yes |
| Synchronized | No | Yes |
| Thread-safe | No | Yes |
| Null key | One allowed | Not allowed |
| Null values | Allowed | Not allowed |
| Modern recommendation | Yes | Generally no |

### ▸ Interview answer

> **HashMap is unsynchronized and allows nulls, while Hashtable is a legacy synchronized map that does not allow null keys or values.**

---

## ▸ HashMap vs ConcurrentHashMap

| Feature | HashMap | ConcurrentHashMap |
|---|---|---|
| Thread-safe | No | Yes |
| Null key | Allowed | Not allowed |
| Null values | Allowed | Not allowed |
| Concurrent operations | Not designed for this | Designed for this |
| Typical use | Single-threaded / externally synchronized | Concurrent applications |

### ▸ Interview point

> **Use ConcurrentHashMap when multiple threads need concurrent access to a map.**

---

## ▸ HashMap Capacity

HashMap internally maintains a table of buckets.

Capacity represents the number of buckets in the table.

Example conceptually:

    capacity = 16

means the table has 16 bucket positions.

Capacity is different from size.

---

## ▸ HashMap Size vs Capacity

### ▸ Size

Number of key-value mappings:

    map.size()

### ▸ Capacity

Number of buckets in the internal table.

For example:

    size = 10
    capacity = 16

There are 10 mappings stored across a table with 16 buckets.

### ▸ Interview point

> **Size is the number of mappings; capacity is the number of buckets available in the hash table.**

---

## ▸ Default Initial Capacity

The commonly used default initial capacity of HashMap is:

    16

### ▸ Important nuance

The internal table is typically allocated lazily, so creating a HashMap does not necessarily mean a 16-slot array is immediately allocated.

---

## ▸ Load Factor

The default load factor is:

    0.75

It controls when the HashMap should resize.

Conceptually:

    threshold = capacity × load factor

For:

    capacity = 16
    load factor = 0.75

the threshold is:

    16 × 0.75 = 12

When the threshold is reached, the table can resize.

---

## ▸ Why Load Factor Exists

There is a trade-off between:

    Memory usage
    and
    Number of collisions

A lower load factor:

- Uses more memory
- Keeps the table less crowded
- Can reduce collisions

A higher load factor:

- Uses memory more efficiently
- Allows more entries per bucket table
- Can increase collisions

### ▸ Interview one-liner

> **Load factor balances memory usage and hash-table performance.**

---

## ▸ Resizing

When the number of mappings reaches the resize threshold, HashMap increases the table capacity.

Conceptually:

    capacity = 16
    threshold = 12

After resizing, capacity increases and entries are redistributed according to the new table size.

### ▸ Interview answer

> **HashMap resizes when its size reaches the load-factor threshold to maintain efficient lookup performance.**

---

## ▸ Rehashing

After resizing, existing entries need to be redistributed according to the new table capacity.

This process is commonly called rehashing.

Conceptually:

    Old table
       ↓
    Resize
       ↓
    New table
       ↓
    Redistribute entries

### ▸ Important

Do not think that every entry necessarily receives a completely new hash code.

The hash value and its use in determining the bucket are recalculated relative to the new table capacity.

---

## ▸ Why HashMap Capacity Usually Grows by 2x

HashMap traditionally uses power-of-two table sizes.

Conceptually:

    16
    ↓
    32
    ↓
    64
    ↓
    128

Doubling helps maintain efficient bucket-index calculations.

### ▸ Interview point

> **HashMap table capacities are typically powers of two and grow by doubling during resize.**

---

## ▸ HashMap Index Calculation

The bucket index is derived from the key's hash and the table size.

For power-of-two capacities, HashMap can efficiently calculate an index using a bitwise operation conceptually similar to:

    (n - 1) & hash

where:

    n = table length

### ▸ Interview point

> **HashMap uses the hash and table length to efficiently determine the bucket index.**

---

## ▸ Hash Spreading

HashMap performs additional hash processing to improve distribution of keys across buckets.

Conceptually:

    key.hashCode()
         ↓
    hash spreading
         ↓
    bucket index

The exact implementation should be understood as JDK-dependent.

### ▸ Interview-friendly answer

> **HashMap processes the key's hash code to improve distribution before determining the bucket index.**

---

## ▸ Why Power of Two Matters

HashMap's table length is maintained as a power of two.

This allows efficient index calculation using bitwise operations.

It also helps the resize process redistribute entries efficiently.

---

## ▸ Mutable Keys

This is a very important HashMap edge case.

Suppose:

    Student student = new Student(1, "John");

    map.put(student, "Employee");

If fields used by:

    equals()
    hashCode()

are changed after insertion:

    student.id = 2;

then the hash code may change.

The entry may remain in the bucket determined by the old hash.

A lookup using the modified object may search a different bucket.

Therefore:

    map.get(student)

may return:

    null

even though the entry is still physically present in the map.

### ▸ Interview rule

> **Never mutate fields used by equals() and hashCode() while an object is being used as a HashMap key.**

---

## ▸ Why Immutable Keys Are Preferred

Immutable objects are safer as HashMap keys because their state does not change after insertion.

Common examples:

    String
    Integer
    Long

If the key's hash-relevant state never changes, the mapping remains consistently locatable.

---

## ▸ String as HashMap Key

`String` is a common HashMap key because it is immutable and correctly implements:

    equals()
    hashCode()

Example:

    HashMap<String, Integer> map = new HashMap<>();

    map.put("Java", 100);

    System.out.println(map.get("Java"));

Output:

    100

---

## ▸ Custom Object as HashMap Key

Example:

    class Employee {
        int id;
        String name;

        Employee(int id, String name) {
            this.id = id;
            this.name = name;
        }
    }

If logical equality depends on:

    id
    name

then override:

    equals()
    hashCode()

Example:

    @Override
    public boolean equals(Object obj) {
        if (this == obj) {
            return true;
        }

        if (!(obj instanceof Employee)) {
            return false;
        }

        Employee other = (Employee) obj;

        return id == other.id &&
               Objects.equals(name, other.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id, name);
    }

---

## ▸ What Happens If Custom Key Does Not Override equals() and hashCode()?

Two different object instances with the same field values may still be treated as different keys.

Example:

    Employee e1 = new Employee(1, "John");
    Employee e2 = new Employee(1, "John");

    map.put(e1, "A");
    map.put(e2, "B");

Without appropriate equality/hash implementation, both may exist as separate keys.

---

## ▸ HashMap and Reference Equality

If a custom class does not override `equals()`, it inherits `Object.equals()` behavior.

This generally means equality is based on object identity.

Therefore:

    new Employee(1, "John")
    new Employee(1, "John")

are different object instances and are not automatically considered equal.

---

## ▸ `get()` with Equal but Different Object

Suppose:

    Employee e1 = new Employee(1, "John");
    Employee e2 = new Employee(1, "John");

    map.put(e1, "Developer");

If `equals()` and `hashCode()` are correctly implemented so that:

    e1.equals(e2) == true

then:

    map.get(e2)

can return:

    "Developer"

This demonstrates why correct `equals()` and `hashCode()` implementations matter.

---

## ▸ HashMap `containsKey()` vs `containsValue()`

    map.containsKey(key)

uses hash-based key lookup.

But:

    map.containsValue(value)

generally scans through the stored entries.

Therefore:

    containsKey() → average O(1)
    containsValue() → O(n)

---

## ▸ HashMap `get()` vs `containsKey()`

Important edge case:

    map.get(key)

returns:

    null

when there is no mapping.

But the map may actually contain the key mapped to null:

    map.put("Java", null);

Then:

    map.get("Java") → null

So you cannot always distinguish:

    key does not exist
    vs
    key exists with null value

using `get()` alone.

Use:

    map.containsKey("Java")

to distinguish them.

---

## ▸ Important Interview Scenario: null Value

Example:

    map.put("Java", null);

Now:

    map.get("Java")        // null
    map.containsKey("Java") // true

Therefore:

> **get() returning null does not necessarily mean the key is absent.**

---

## ▸ HashMap `put()` Return Value

`put()` returns the previous value associated with the key.

Example:

    map.put(1, "Java");

    String oldValue = map.put(1, "Python");

Then:

    oldValue = "Java"

and the map contains:

    1 → "Python"

If the key did not previously exist:

    map.put(2, "C++")

returns:

    null

---

## ▸ HashMap `remove()` Return Value

`remove(key)` returns the previous value associated with the removed key.

Example:

    map.put(1, "Java");

    String value = map.remove(1);

Then:

    value = "Java"

and the mapping is removed.

---

## ▸ HashMap Clone

HashMap implements `Cloneable`.

Example:

    HashMap<Integer, String> map1 = new HashMap<>();

    map1.put(1, "Java");

    HashMap<Integer, String> map2 =
        (HashMap<Integer, String>) map1.clone();

`clone()` creates a **shallow copy**.

The new HashMap object is different, but referenced key/value objects are shared.

### ▸ Interview answer

> **HashMap.clone() creates a shallow copy of the map.**

---

## ▸ HashMap Copy Constructor

A map can also be copied using:

    HashMap<Integer, String> map2 =
        new HashMap<>(map1);

This creates a new HashMap containing the same key and value references.

It is a shallow copy.

---

## ▸ clone() vs Copy Constructor

| Feature | `clone()` | Copy Constructor |
|---|---|---|
| New HashMap | Yes | Yes |
| Shallow copy | Yes | Yes |
| Casting required | Yes | No |
| Readability | Lower | Higher |

---

## ▸ HashMap Views

HashMap provides three important collection views:

    keySet()
    values()
    entrySet()

### ▸ keySet()

Returns all keys as a Set.

    Set<Integer> keys = map.keySet();

### ▸ values()

Returns all values as a Collection.

    Collection<String> values = map.values();

### ▸ entrySet()

Returns all key-value mappings as Set entries.

    Set<Map.Entry<Integer, String>> entries =
        map.entrySet();

---

## ▸ Why entrySet() Is Preferred for Iteration

If you need both key and value:

    for (Map.Entry<Integer, String> entry :
         map.entrySet()) {

        System.out.println(
            entry.getKey() + " = " + entry.getValue()
        );
    }

This directly provides both.

Using:

    keySet()

requires:

    map.get(key)

for every key.

### ▸ Interview answer

> **Use entrySet() when both keys and values are required because each entry already contains both.**

---

## ▸ HashMap Iterator

You can iterate using:

    Iterator<Map.Entry<Integer, String>> iterator =
        map.entrySet().iterator();

    while (iterator.hasNext()) {
        Map.Entry<Integer, String> entry = iterator.next();

        System.out.println(
            entry.getKey() + " = " + entry.getValue()
        );
    }

---

## ▸ Fail-Fast Behavior

HashMap iterators are generally **fail-fast**.

If the map is structurally modified after an iterator is created, except through the iterator's own supported removal operation, the iterator may throw:

    ConcurrentModificationException

Example:

    Iterator<Integer> iterator = map.keySet().iterator();

    map.put(100, "Java");

    iterator.next();

This may throw:

    ConcurrentModificationException

### ▸ Important

Fail-fast behavior is not a thread-safety mechanism.

---

## ▸ Removing While Iterating

Use the iterator's `remove()` method when appropriate.

    Iterator<Integer> iterator =
        map.keySet().iterator();

    while (iterator.hasNext()) {
        Integer key = iterator.next();

        if (key == 10) {
            iterator.remove();
        }
    }

---

## ▸ HashMap Thread Safety

HashMap is **not thread-safe**.

If multiple threads access and modify the same HashMap concurrently, external synchronization or a concurrent map implementation may be required.

For synchronized access:

    Map<Integer, String> map =
        Collections.synchronizedMap(new HashMap<>());

For concurrent map operations:

    ConcurrentHashMap<Integer, String> map =
        new ConcurrentHashMap<>();

---

## ▸ HashMap vs ConcurrentHashMap

Use:

    HashMap

when:

- Concurrent modification is not required.
- The map is local to one thread.
- External synchronization is already provided.

Use:

    ConcurrentHashMap

when:

- Multiple threads access the map concurrently.
- Concurrent updates are required.
- Null keys and null values are not required.

---

## ▸ Why ConcurrentHashMap Does Not Allow null

ConcurrentHashMap does not allow:

    null keys
    null values

One important reason is that a `null` result from operations such as `get()` can be used consistently to indicate absence, without ambiguity from a stored null value.

---

## ▸ HashMap Time Complexity

| Operation | Average | Worst Case |
|---|---:|---:|
| `put()` | O(1) | O(log n) with tree bins under relevant conditions |
| `get()` | O(1) | O(log n) with tree bins |
| `remove()` | O(1) | O(log n) with tree bins |
| `containsKey()` | O(1) | O(log n) with tree bins |
| `containsValue()` | O(n) | O(n) |

### ▸ Interview answer

> **HashMap provides average O(1) time for key-based put, get, remove, and containsKey operations.**

---

## ▸ Why HashMap Is O(1) Average

HashMap does not normally scan every key.

It uses:

    hashCode()
        ↓
    bucket
        ↓
    candidate entries
        ↓
    equals()
        ↓
    matching key

This makes average lookup close to constant time when hashes are well distributed.

---

## ▸ Worst-Case Performance

If many keys collide into the same bucket, lookup can become slower.

Modern HashMap implementations can treeify heavily collided buckets.

Therefore, under tree-bin conditions, lookup can be approximately:

    O(log n)

rather than:

    O(n)

### ▸ Interview note

For most interviews, remember:

    Average → O(1)
    Treeified collision bucket → O(log n)

---

## ▸ HashMap Memory

HashMap uses additional memory for:

- Bucket table
- Entry nodes
- Keys
- Values
- Collision structures
- Tree nodes when treeification occurs

Therefore, HashMap uses more memory than a simple array because it maintains hash-table structures.

---

## ▸ HashMap and Generics

Use generics to specify key and value types.

    HashMap<String, Integer> map =
        new HashMap<>();

This means:

    Key   → String
    Value → Integer

Incorrect:

    map.put(10, "Java");

The compiler prevents this because the expected types are:

    String
    Integer

---

## ▸ HashMap Raw Type

Avoid:

    HashMap map = new HashMap();

because it removes compile-time type safety.

Prefer:

    HashMap<String, Integer> map =
        new HashMap<>();

---

## ▸ Diamond Operator

You can simplify:

    HashMap<String, Integer> map =
        new HashMap<String, Integer>();

to:

    HashMap<String, Integer> map =
        new HashMap<>();

The compiler infers the generic types from the left-hand side.

---

## ▸ Map Reference vs HashMap Reference

Prefer programming to the interface:

    Map<String, Integer> map =
        new HashMap<>();

instead of:

    HashMap<String, Integer> map =
        new HashMap<>();

Use the `HashMap` reference when you specifically need HashMap-specific behavior.

### ▸ Interview point

> **Use the interface type for flexibility when you only need Map operations.**

---

## ▸ HashMap and Map

`Map` is an interface.

`HashMap` is a concrete implementation.

Example:

    Map<String, Integer> map =
        new HashMap<>();

This follows:

    Interface reference
          ↓
    Implementation object

---

## ▸ HashMap Constructor

Common constructors:

    HashMap<Integer, String> map1 =
        new HashMap<>();

    HashMap<Integer, String> map2 =
        new HashMap<>(32);

    HashMap<Integer, String> map3 =
        new HashMap<>(32, 0.75f);

    HashMap<Integer, String> map4 =
        new HashMap<>(anotherMap);

---

## ▸ Initial Capacity Constructor

Example:

    HashMap<Integer, String> map =
        new HashMap<>(100);

This specifies an initial capacity.

It does not mean:

    100 mappings

are already stored.

---

## ▸ Initial Capacity and Performance

If you know that a map will contain many entries, choosing an appropriate initial capacity can reduce the number of resizes.

Example:

    HashMap<Integer, String> map =
        new HashMap<>(1000);

This can be useful when the expected size is known.

---

## ▸ HashMap Resize Example

Suppose conceptually:

    capacity = 16
    load factor = 0.75

Then:

    threshold = 12

As entries increase:

    1
    2
    3
    ...
    12

At the threshold, HashMap can resize.

The table may grow from:

    16 → 32

and entries are redistributed.

---

## ▸ Why Poor hashCode() Is a Problem

A poor hash function can send many keys to the same bucket.

Example conceptually:

    Key 1 → Bucket 5
    Key 2 → Bucket 5
    Key 3 → Bucket 5
    Key 4 → Bucket 5

This creates collisions.

More collisions mean more work during lookup.

### ▸ Interview point

> **A well-distributed hashCode() reduces collisions and improves HashMap performance.**

---

## ▸ Can hashCode() Return the Same Value for Different Objects?

Yes.

Example:

    A.hashCode() = 100
    B.hashCode() = 100

This is completely valid.

If:

    A.equals(B) == false

both can be stored as different keys.

---

## ▸ Can HashMap Have Multiple Entries in One Bucket?

Yes.

This happens because of hash collisions.

A bucket can contain multiple entries.

---

## ▸ Can HashMap Have Duplicate Values?

Yes.

Example:

    map.put(1, "Java");
    map.put(2, "Java");

Both are valid.

---

## ▸ Can HashMap Have Duplicate Keys?

No.

If the key already exists, `put()` replaces the associated value.

---

## ▸ Can HashMap Store null?

Yes.

    One null key
    Multiple null values

---

## ▸ HashMap Ordering

Never depend on HashMap's iteration order.

Even if you observe:

    {1=A, 2=B, 3=C}

you should not assume that order is guaranteed.

Changes to:

- Hash codes
- Capacity
- Resizing
- JDK implementation
- Entries

can affect iteration order.

### ▸ Interview answer

> **HashMap provides no guaranteed iteration order.**

---

## ▸ LinkedHashMap for Order

If insertion order is required:

    Map<String, Integer> map =
        new LinkedHashMap<>();

This maintains insertion order.

---

## ▸ TreeMap for Sorting

If keys need to be sorted:

    Map<String, Integer> map =
        new TreeMap<>();

Keys are maintained in sorted order according to natural ordering or a comparator.

---

## ▸ HashMap and Sorting

HashMap itself does not sort its entries.

If you need sorted keys:

    TreeMap

If you need sorted values, you generally need to create a separate sorted view/list based on the values.

---

## ▸ HashMap and Access Order

HashMap does not maintain access order.

`LinkedHashMap` can be configured for access-order behavior.

This is useful for structures such as LRU caches.

---

## ▸ HashMap and LRU Cache

A common Java approach for an LRU cache uses:

    LinkedHashMap

rather than HashMap because LinkedHashMap can maintain access order.

Example concept:

    LinkedHashMap<K, V>

with access-order enabled.

---

## ▸ HashMap and Serialization

HashMap implements:

    Serializable

Therefore, HashMap objects can participate in Java serialization.

---

## ▸ HashMap and clone()

HashMap implements:

    Cloneable

Its `clone()` method produces a shallow copy.

---

## ▸ HashMap and Null: Interview Trap

Question:

> Can HashMap have a null key?

Answer:

> Yes, HashMap allows one null key.

Question:

> Can HashMap have multiple null values?

Answer:

> Yes, multiple null values are allowed.

Question:

> Can HashMap have multiple null keys?

Answer:

> No, because keys must be unique.

---

## ▸ HashMap and `get(null)`

Example:

    map.put(null, "Java");

Then:

    map.get(null)

returns:

    "Java"

---

## ▸ HashMap and `put(null, value)`

Example:

    map.put(null, "Java");
    map.put(null, "Python");

Final mapping:

    null → Python

The second value replaces the first.

---

## ▸ HashMap and Mutable Key Interview Trap

Question:

> What happens if a HashMap key is modified after insertion?

Answer:

> If fields used by hashCode() or equals() change, the key may no longer be found using normal lookup because its new hash may point to a different bucket.

### ▸ One-line answer

> **Never mutate hashCode()/equals()-relevant fields of a key while it is stored in a HashMap.**

---

## ▸ HashMap Internal Flow to Memorize

For `put()`:

    key
     ↓
    hashCode()
     ↓
    hash processing
     ↓
    bucket index
     ↓
    check bucket
     ↓
    equals()
     ↓
    existing key?
       ↙       ↘
     Yes        No
      ↓          ↓
    replace     add

For `get()`:

    key
     ↓
    hashCode()
     ↓
    bucket index
     ↓
    equals()
     ↓
    return value

---

## ▸ Most Important HashMap Interview Traps

### ▸ Trap 1: Does HashMap allow duplicate keys?

No.

---

### ▸ Trap 2: Does HashMap allow duplicate values?

Yes.

---

### ▸ Trap 3: How many null keys can HashMap have?

One.

---

### ▸ Trap 4: How many null values can HashMap have?

Multiple.

---

### ▸ Trap 5: Does HashMap maintain insertion order?

No.

---

### ▸ Trap 6: Is HashMap thread-safe?

No.

---

### ▸ Trap 7: What does HashMap use internally?

A hash table consisting of buckets containing map entries.

---

### ▸ Trap 8: What does HashMap use to identify a key?

`hashCode()` and `equals()`.

---

### ▸ Trap 9: Does same hashCode() mean same key?

No.

---

### ▸ Trap 10: If equals() is true, what must be true about hashCode()?

The hash codes must be the same.

---

### ▸ Trap 11: What happens when the same key is inserted twice?

The old value is replaced.

---

### ▸ Trap 12: Does HashMap provide get(index)?

No.

HashMap is not index-based.

---

### ▸ Trap 13: What is the average complexity of get()?

O(1).

---

### ▸ Trap 14: What happens with many collisions?

A bucket may contain multiple entries and can be treeified under appropriate conditions.

---

### ▸ Trap 15: What is the default load factor?

0.75.

---

### ▸ Trap 16: What is the commonly remembered default initial capacity?

16.

---

### ▸ Trap 17: What happens when the resize threshold is reached?

The table can grow and entries are redistributed.

---

### ▸ Trap 18: Is HashMap's fail-fast iterator thread-safe?

No.

---

### ▸ Trap 19: Can a mutable object be a HashMap key?

Yes, but changing hashCode()/equals()-relevant fields after insertion can break lookup behavior.

---

### ▸ Trap 20: Does get(key) returning null always mean the key is absent?

No.

The key may exist and be mapped to null.

Use:

    containsKey()

to distinguish the cases.

---

## ▸ Interview Q&A

### ▸ Q1. What is HashMap?

> HashMap is a Map implementation that stores key-value pairs using hashing and provides average O(1) key-based lookup.

---

### ▸ Q2. How does HashMap work internally?

> HashMap uses a hash table of buckets; it hashes the key to find a bucket and uses equals() to locate the matching key within that bucket.

---

### ▸ Q3. What is a bucket?

> A bucket is a position in the internal hash table where one or more map entries can be stored.

---

### ▸ Q4. What happens when put() is called?

> HashMap calculates the key's hash, determines the bucket, checks existing keys using equals(), and either updates the existing value or inserts a new entry.

---

### ▸ Q5. What happens when get() is called?

> HashMap hashes the key, locates the bucket, compares keys using equals(), and returns the corresponding value.

---

### ▸ Q6. Why does HashMap use hashCode() and equals()?

> hashCode() helps locate the bucket, while equals() confirms whether the key matches an existing key.

---

### ▸ Q7. What is the contract between hashCode() and equals()?

> If two objects are equal according to equals(), they must have the same hashCode().

---

### ▸ Q8. Can two different keys have the same hashCode()?

> Yes. This is a hash collision.

---

### ▸ Q9. What happens during a collision?

> Multiple entries can occupy the same bucket; under high collision conditions, the bucket can be treeified.

---

### ▸ Q10. What is treeification?

> Treeification converts a heavily collided bucket from a linked structure into a Red-Black Tree to improve lookup performance.

---

### ▸ Q11. What is the average time complexity of HashMap get()?

> O(1) average.

---

### ▸ Q12. What is the worst-case complexity with tree bins?

> Approximately O(log n) for lookup within a treeified bucket.

---

### ▸ Q13. Does HashMap allow duplicate keys?

> No. A new value replaces the value associated with an existing key.

---

### ▸ Q14. Does HashMap allow duplicate values?

> Yes.

---

### ▸ Q15. Does HashMap allow null?

> Yes, it allows one null key and multiple null values.

---

### ▸ Q16. Is HashMap synchronized?

> No, HashMap is not synchronized.

---

### ▸ Q17. Is HashMap thread-safe?

> No.

---

### ▸ Q18. What should be used for concurrent map access?

> `ConcurrentHashMap` is generally used when concurrent access is required.

---

### ▸ Q19. Why does ConcurrentHashMap not allow null?

> It avoids ambiguity between a missing mapping and a mapping whose value is null.

---

### ▸ Q20. Does HashMap maintain insertion order?

> No, HashMap does not guarantee iteration order.

---

### ▸ Q21. Which Map maintains insertion order?

> `LinkedHashMap`.

---

### ▸ Q22. Which Map maintains sorted key order?

> `TreeMap`.

---

### ▸ Q23. What is the default load factor of HashMap?

> 0.75.

---

### ▸ Q24. What is the commonly remembered default initial capacity?

> 16.

---

### ▸ Q25. What happens when HashMap reaches its threshold?

> HashMap can resize the table and redistribute the entries.

---

### ▸ Q26. Why does HashMap resize?

> To maintain efficient bucket distribution and reduce excessive collisions.

---

### ▸ Q27. What is rehashing?

> Rehashing refers to redistributing existing entries according to the new table capacity after resizing.

---

### ▸ Q28. Why is a good hashCode() important?

> A well-distributed hashCode() reduces collisions and improves HashMap performance.

---

### ▸ Q29. Can HashMap use a mutable object as a key?

> Yes, but modifying fields involved in equals() or hashCode() after insertion can make the key difficult or impossible to find.

---

### ▸ Q30. Why are immutable objects preferred as HashMap keys?

> Their state does not change, so their hashCode() remains stable while they are stored in the map.

---

### ▸ Q31. What does put() return?

> It returns the previous value associated with the key, or null if there was no previous mapping.

---

### ▸ Q32. What does remove() return?

> It returns the value associated with the removed key, or null if no mapping existed.

---

### ▸ Q33. What is the difference between get() and containsKey()?

> get() returns the value and may return null for both an absent key and a key mapped to null; containsKey() explicitly checks whether the key exists.

---

### ▸ Q34. Why use entrySet() for iteration?

> Because each entry directly provides both the key and value without requiring another map lookup.

---

### ▸ Q35. What is the difference between HashMap and Hashtable?

> HashMap is unsynchronized and allows nulls, while Hashtable is a legacy synchronized map that does not allow null keys or values.

---

### ▸ Q36. What is the difference between HashMap and ConcurrentHashMap?

> HashMap is not designed for concurrent access, while ConcurrentHashMap is designed for concurrent map operations.

---

### ▸ Q37. What is the difference between HashMap and LinkedHashMap?

> HashMap does not guarantee order, while LinkedHashMap maintains insertion order.

---

### ▸ Q38. What is the difference between HashMap and TreeMap?

> HashMap provides hash-based average O(1) lookup without guaranteed ordering, while TreeMap maintains sorted key order with O(log n) operations.

---

### ▸ Q39. Can HashMap have multiple entries in one bucket?

> Yes, because different keys can produce collisions.

---

### ▸ Q40. What happens if equals() is overridden but hashCode() is not?

> Equal keys may be placed in different buckets, causing incorrect lookup or duplicate-key behavior.

---

## ▸ HashMap vs HashSet

HashSet internally uses a HashMap.

Conceptually:

    HashSet element
          ↓
    HashMap key
          ↓
    dummy value

Example:

    HashSet:

    "Java"

internally behaves conceptually like:

    "Java" → PRESENT

### ▸ Interview answer

> **HashSet uses elements as HashMap keys and stores a dummy value for each key.**

---

## ▸ HashMap vs HashSet Quick Comparison

| Feature | HashMap | HashSet |
|---|---|---|
| Stores | Key-value pairs | Unique elements |
| Duplicate keys/elements | No duplicate keys | No duplicate elements |
| Null | One null key + multiple null values | One null element |
| Internal structure | Hash table | HashMap |
| Main method | `put()` | `add()` |
| Lookup | `get(key)` | `contains(element)` |

---

## ▸ HashMap and Java 8+ Important Internal Change

One important change in modern Java HashMap implementations is that heavily collided buckets can be converted into Red-Black Trees.

This improves worst-case lookup behavior under high collision conditions.

### ▸ Interview one-liner

> **Java 8 introduced bucket treeification to improve HashMap performance when collisions become excessive.**

---

## ▸ HashMap Internal Picture to Remember

    HashMap
       ↓
    Bucket Array
       ↓
    ┌─────────────┐
    │ Bucket 0    │ → Entry
    │ Bucket 1    │ → Entry → Entry
    │ Bucket 2    │
    │ Bucket 3    │ → Entry
    │ ...         │
    └─────────────┘

A heavily collided bucket can become:

    Bucket
       ↓
    Red-Black Tree

---

## ▸ Complete put() Flow to Remember

    map.put(key, value)
            ↓
       key.hashCode()
            ↓
       hash processing
            ↓
       calculate bucket index
            ↓
       bucket empty?
         ↙       ↘
       Yes        No
        ↓          ↓
      insert     compare keys
                   ↓
                equals()
                ↙      ↘
             equal   different
               ↓         ↓
           replace     collision
                         ↓
                   add entry
                         ↓
              treeify if required

---

## ▸ Complete get() Flow to Remember

    map.get(key)
          ↓
     key.hashCode()
          ↓
     hash processing
          ↓
     calculate bucket index
          ↓
     search bucket
          ↓
     equals()
       ↙    ↘
    match   no match
      ↓        ↓
   return    null
   value

---

## ▸ One-Liner Approach

> **HashMap = hashCode() finds the bucket + equals() finds the key + value is returned.**

---

## ▸ Quick Revision

| Topic | HashMap |
|---|---|
| Type | Class |
| Interface | `Map` |
| Stores | Key-value pairs |
| Duplicate keys | No |
| Duplicate values | Yes |
| Null keys | One |
| Null values | Multiple |
| Insertion order | Not guaranteed |
| Thread-safe | No |
| Average `put()` | O(1) |
| Average `get()` | O(1) |
| Average `remove()` | O(1) |
| Average `containsKey()` | O(1) |
| `containsValue()` | O(n) |
| Default load factor | 0.75 |
| Common default initial capacity | 16 |
| Collision handling | Linked nodes / tree bins |
| Tree structure | Red-Black Tree |
| Mutable keys | Risky |
| `clone()` | Shallow copy |
| Ordered alternative | LinkedHashMap |
| Sorted alternative | TreeMap |
| Concurrent alternative | ConcurrentHashMap |

---

## ▸ Must Remember

- `HashMap` stores **key-value pairs**.
- `HashMap` implements **Map**.
- Keys must be **unique**.
- Values can be duplicated.
- HashMap allows **one null key**.
- HashMap allows **multiple null values**.
- HashMap does **not guarantee insertion order**.
- HashMap is **not synchronized**.
- HashMap is **not thread-safe**.
- HashMap uses a **hash table of buckets** internally.
- `hashCode()` helps determine the bucket.
- `equals()` confirms the matching key.
- **hashCode() finds the bucket; equals() finds the key.**
- Different keys can have the same hash code.
- Same hash code does not mean equal keys.
- Equal keys must have the same hash code.
- Hash collisions are normal and must be handled.
- Modern HashMap implementations can treeify heavily collided buckets.
- Tree bins use **Red-Black Trees**.
- Default load factor is **0.75**.
- Common default initial capacity is **16**.
- HashMap can resize when the threshold is reached.
- Resizing redistributes entries according to the new table capacity.
- `put()` with an existing key replaces its value.
- `get()` returning null does not always mean the key is absent.
- `containsKey()` can distinguish an absent key from a key mapped to null.
- Avoid mutating fields used by `equals()` and `hashCode()` for keys stored in the map.
- Immutable objects such as `String` are safer HashMap keys.
- `entrySet()` is preferred when both key and value are needed during iteration.
- HashMap iterators are generally fail-fast.
- Fail-fast behavior does not make HashMap thread-safe.
- `LinkedHashMap` maintains insertion order.
- `TreeMap` maintains sorted key order.
- `ConcurrentHashMap` is designed for concurrent access.

---

## ▸ Final Interview Definition

> **HashMap is a hash-based Map implementation that stores unique keys with associated values, uses hashCode() and equals() for key lookup, and provides average O(1) insertion, lookup, and removal.**