# LinkedHashMap

▸ What is LinkedHashMap?

`LinkedHashMap` is a subclass of `HashMap` that maintains a predictable iteration order of entries.

By default, the iteration order is **insertion order**.

    HashMap       → No guaranteed order
    LinkedHashMap → Insertion order
    TreeMap       → Sorted order

**Interview one-liner:**

> LinkedHashMap is a HashMap that maintains insertion order by additionally maintaining a doubly linked list of entries.

▸ Package

    import java.util.LinkedHashMap;

▸ Basic Example

    LinkedHashMap<Integer, String> map = new LinkedHashMap<>();

    map.put(3, "C");
    map.put(1, "A");
    map.put(2, "B");

    System.out.println(map);

Output:

    {3=C, 1=A, 2=B}

The entries are returned in the same order in which they were inserted.

▸ Important Characteristics

- Stores data as key-value pairs
- Allows one `null` key
- Allows multiple `null` values
- Does not allow duplicate keys
- Allows duplicate values
- Maintains insertion order by default
- Can optionally maintain access order
- Not synchronized
- Not thread-safe
- Average `put()`, `get()`, and `remove()` are O(1)
- Extends `HashMap`
- Uses hashing for efficient lookup
- Maintains a doubly linked list to preserve ordering

▸ Class Hierarchy

    Object
       |
    AbstractMap
       |
    HashMap
       |
    LinkedHashMap

`LinkedHashMap` extends `HashMap`.

Therefore, it inherits most HashMap behavior and adds ordering support.

▸ LinkedHashMap vs HashMap

    HashMap
    → Hash table
    → No guaranteed iteration order

    LinkedHashMap
    → Hash table + doubly linked list
    → Predictable iteration order

Example:

    Map<Integer, String> map = new LinkedHashMap<>();

    map.put(10, "A");
    map.put(20, "B");
    map.put(30, "C");

    for (Map.Entry<Integer, String> entry : map.entrySet()) {
        System.out.println(entry.getKey());
    }

Output:

    10
    20
    30

▸ Why Does LinkedHashMap Maintain Insertion Order?

`LinkedHashMap` maintains a doubly linked list connecting its entries.

Conceptually:

    Entry1 <-> Entry2 <-> Entry3 <-> Entry4

The hash table provides fast lookup, while the linked list maintains the order of iteration.

**Interview one-liner:**

> LinkedHashMap maintains insertion order using a doubly linked list connecting its entries in addition to the hash table used for lookup.

▸ Internal Structure

Conceptually, LinkedHashMap combines:

    Hash table
          +
    Doubly linked list

The hash table provides:

    Fast lookup

The doubly linked list provides:

    Predictable iteration order

So:

    HashMap
    → Hashing + buckets

    LinkedHashMap
    → Hashing + buckets + linked list

▸ How put() Works

When we execute:

    map.put(10, "A");

Conceptually:

1. Hash the key.
2. Calculate the bucket.
3. Check whether the key already exists.
4. If the key exists, update its value.
5. If the key does not exist, create a new entry.
6. Add the entry to the hash table.
7. Link the entry into the doubly linked list.

Example:

    map.put(10, "A");
    map.put(20, "B");
    map.put(30, "C");

Conceptually:

    Hash table:
    bucket → entries

    Linked list:
    10 <-> 20 <-> 30

▸ What Happens with Duplicate Keys?

Keys must be unique.

    map.put(10, "A");
    map.put(10, "B");

The second `put()` replaces the value.

Result:

    {10=B}

No new entry is created for the duplicate key.

In normal insertion-order mode, the existing key retains its original position.

▸ Does LinkedHashMap Allow null?

Yes.

It allows:

- One `null` key
- Multiple `null` values

Example:

    LinkedHashMap<Integer, String> map = new LinkedHashMap<>();

    map.put(null, "A");
    map.put(1, null);
    map.put(2, null);

This is valid.

▸ Does LinkedHashMap Allow Duplicate Values?

Yes.

    map.put(1, "Java");
    map.put(2, "Java");

Result:

    {1=Java, 2=Java}

Keys must be unique, but values can be duplicated.

▸ Main Methods

LinkedHashMap provides the standard Map methods:

    put()
    get()
    getOrDefault()
    containsKey()
    containsValue()
    remove()
    replace()
    putIfAbsent()
    size()
    isEmpty()
    clear()
    keySet()
    values()
    entrySet()

Example:

    LinkedHashMap<Integer, String> map = new LinkedHashMap<>();

    map.put(1, "Java");
    map.put(2, "Python");

    map.get(1);
    map.containsKey(2);
    map.remove(1);

▸ Iteration

LinkedHashMap is useful when iteration order matters.

    for (Map.Entry<Integer, String> entry : map.entrySet()) {
        System.out.println(entry.getKey() + " = " + entry.getValue());
    }

The entries are returned according to the configured ordering mode.

▸ entrySet() vs keySet()

If you need both key and value:

    for (Map.Entry<Integer, String> entry : map.entrySet()) {
        System.out.println(entry.getKey() + " = " + entry.getValue());
    }

`entrySet()` is generally preferred because it directly provides both key and value.

If you only need keys:

    for (Integer key : map.keySet()) {
        System.out.println(key);
    }

▸ Insertion Order

By default:

    LinkedHashMap<Integer, String> map = new LinkedHashMap<>();

    map.put(3, "C");
    map.put(1, "A");
    map.put(2, "B");

Iteration order:

    3
    1
    2

It follows insertion order, not key order.

Important:

    {3=C, 1=A, 2=B}

is not sorted.

▸ Access Order

LinkedHashMap can also maintain **access order** instead of insertion order.

Constructor:

    LinkedHashMap(int initialCapacity,
                  float loadFactor,
                  boolean accessOrder)

Example:

    LinkedHashMap<Integer, String> map =
        new LinkedHashMap<>(16, 0.75f, true);

The `true` means:

    accessOrder = true

Now entries are ordered based on access from least recently accessed to most recently accessed.

▸ Insertion Order vs Access Order

Insertion order:

    new LinkedHashMap<>()

Order is based on insertion.

Access order:

    new LinkedHashMap<>(16, 0.75f, true)

Order is affected by accesses.

Example:

    LinkedHashMap<Integer, String> map =
        new LinkedHashMap<>(16, 0.75f, true);

    map.put(1, "A");
    map.put(2, "B");
    map.put(3, "C");

Initial order:

    1 -> 2 -> 3

Now:

    map.get(1);

Order becomes:

    2 -> 3 -> 1

Because key `1` was accessed and moved to the end.

**Interview one-liner:**

> In access-order mode, recently accessed entries move toward the end of the iteration order.

▸ Why is Access Order Useful?

Access order is useful when implementing an **LRU (Least Recently Used) cache**.

The least recently accessed entry stays at the beginning.

The most recently accessed entry stays at the end.

Conceptually:

    Least Recently Used
            |
            v
    1 <-> 2 <-> 3 <-> 4
                      ^
                      |
              Most Recently Used

▸ LinkedHashMap for LRU Cache

LinkedHashMap provides a convenient mechanism for implementing an LRU cache.

Example:

    LinkedHashMap<Integer, String> cache =
        new LinkedHashMap<Integer, String>(3, 0.75f, true) {

            @Override
            protected boolean removeEldestEntry(
                    Map.Entry<Integer, String> eldest) {
                return size() > 3;
            }
        };

The `true` enables access-order mode.

When the size exceeds `3`, the eldest entry is removed.

▸ removeEldestEntry()

`removeEldestEntry()` is a protected method that can be overridden to automatically remove the eldest entry.

Example:

    @Override
    protected boolean removeEldestEntry(
            Map.Entry<Integer, String> eldest) {
        return size() > 100;
    }

Meaning:

    If size > 100
        remove the eldest entry

It is commonly used for bounded caches.

▸ Is LinkedHashMap Thread-Safe?

No.

LinkedHashMap is **not thread-safe**.

If multiple threads modify the same LinkedHashMap concurrently, external synchronization or another appropriate concurrent collection/design is required.

Remember:

    LinkedHashMap    → Not thread-safe
    Hashtable        → Synchronized
    ConcurrentHashMap → Designed for concurrent access

▸ LinkedHashMap vs Hashtable

| Feature | LinkedHashMap | Hashtable |
|---|---|---|
| Thread-safe | No | Yes |
| Null key | One allowed | Not allowed |
| Null values | Allowed | Not allowed |
| Ordering | Insertion/access order | No guaranteed insertion order |
| Legacy | No | Yes |

▸ LinkedHashMap vs TreeMap

| Feature | LinkedHashMap | TreeMap |
|---|---|---|
| Ordering | Insertion/access order | Sorted by key |
| Data structure | Hash table + linked list | Red-Black Tree |
| Average get | O(1) | O(log n) |
| Null key | One allowed | Generally not allowed with natural ordering |
| Main use | Preserve order | Sorted keys |

Remember:

    LinkedHashMap → Preserve order
    TreeMap       → Sort keys

▸ LinkedHashMap vs HashMap vs TreeMap

    HashMap
    → Fast lookup
    → No guaranteed iteration order

    LinkedHashMap
    → Fast lookup
    → Maintains insertion/access order

    TreeMap
    → Sorted keys
    → O(log n) operations

▸ Time Complexity

Average:

    put()           → O(1)
    get()           → O(1)
    remove()        → O(1)
    containsKey()   → O(1)
    containsValue() → O(n)

Iteration:

    O(n)

The average O(1) behavior comes from hashing.

▸ Capacity and Load Factor

LinkedHashMap inherits HashMap's hash-table capacity and load-factor behavior.

Common defaults:

    Initial capacity = 16
    Load factor = 0.75

Conceptually:

    threshold = capacity × load factor

For example:

    16 × 0.75 = 12

When the table grows beyond the relevant threshold, resizing occurs.

▸ Resizing

When the hash table needs more capacity, it is resized.

The capacity is commonly doubled.

Example:

    16 → 32 → 64 → 128

Entries are redistributed according to the new table capacity.

The linked-list ordering is maintained separately.

▸ Does Resizing Destroy Insertion Order?

No.

The linked structure maintains the configured iteration order even when the underlying hash table is resized.

▸ Hashing in LinkedHashMap

LinkedHashMap inherits HashMap's hashing mechanism.

Conceptually:

    key
     |
    hashCode()
     |
    hash spreading
     |
    bucket index
     |
    entry

`hashCode()` helps locate the bucket.

`equals()` is then used to determine whether keys are equal.

▸ equals() and hashCode() Contract

If two keys are equal:

    key1.equals(key2) == true

Then they must have the same hash code:

    key1.hashCode() == key2.hashCode()

But:

    same hashCode ≠ same object

Different keys can have the same hash code, resulting in a collision.

▸ Mutable Keys

Avoid using mutable objects as keys if their fields involved in `equals()` and `hashCode()` can change after insertion.

Example problem:

    map.put(student, "Data");

If fields used by `hashCode()` or `equals()` are changed later, the map may no longer be able to locate the entry correctly.

**Interview one-liner:**

> Map keys should ideally be immutable because changing fields used by `equals()` or `hashCode()` after insertion can make the entry difficult to retrieve.

▸ String as a Key

`String` is a good key because it is immutable and properly implements `equals()` and `hashCode()`.

Example:

    LinkedHashMap<String, Integer> map = new LinkedHashMap<>();

    map.put("Java", 10);
    map.put("Python", 20);

▸ get() Returning null

If:

    map.get(key)

returns `null`, it can mean:

1. The key does not exist.
2. The key exists and its value is `null`.

Use:

    map.containsKey(key)

when you need to distinguish these cases.

▸ put() Return Value

`put()` returns the previous value associated with the key.

Example:

    map.put(1, "Java");

    String oldValue = map.put(1, "Spring");

Now:

    oldValue = "Java"

If the key did not previously exist, `put()` returns `null`.

▸ remove() Return Value

`remove(key)` returns the value associated with the removed key.

Example:

    String value = map.remove(1);

If key `1` existed:

    value = associated value

If it did not exist:

    value = null

▸ clone()

LinkedHashMap supports cloning.

The clone is a **shallow copy**.

Example:

    LinkedHashMap<Integer, String> map1 = new LinkedHashMap<>();

    map1.put(1, "Java");

    LinkedHashMap<Integer, String> map2 =
        (LinkedHashMap<Integer, String>) map1.clone();

A new LinkedHashMap object is created, but the keys and values themselves are not deeply copied.

▸ Copy Constructor

You can also create a new LinkedHashMap using a copy constructor:

    LinkedHashMap<Integer, String> map2 =
        new LinkedHashMap<>(map1);

This creates a new map containing the same key-value references.

It is also a **shallow copy**.

▸ Fail-Fast Iterator

Iterators obtained from LinkedHashMap are generally fail-fast.

Example:

    for (Integer key : map.keySet()) {
        map.put(10, "Java");
    }

A structural modification during iteration can result in:

    ConcurrentModificationException

If removal is required during iteration, use the iterator's `remove()` method where supported.

Example:

    Iterator<Integer> iterator = map.keySet().iterator();

    while (iterator.hasNext()) {
        Integer key = iterator.next();

        if (key == 10) {
            iterator.remove();
        }
    }

**Important:**

Fail-fast behavior is a best-effort mechanism and is not a thread-safety guarantee.

▸ Why is LinkedHashMap Faster Than TreeMap for Lookup?

LinkedHashMap uses hashing:

    get() → Average O(1)

TreeMap uses a Red-Black Tree:

    get() → O(log n)

Therefore, if you need predictable insertion order rather than sorted order, LinkedHashMap is usually preferred.

▸ When Should You Use LinkedHashMap?

Use LinkedHashMap when:

- You need key-value storage.
- You want fast average lookup.
- You need predictable iteration order.
- You want to preserve insertion order.
- You need access-order behavior.
- You are implementing an LRU-style cache.

▸ When Should You Not Use LinkedHashMap?

Do not choose LinkedHashMap when:

- You need sorted keys → use TreeMap.
- You need concurrent map operations → consider ConcurrentHashMap.
- You do not care about ordering → HashMap is usually sufficient.

▸ Important Interview Traps

**1. Is LinkedHashMap sorted?**

No.

It maintains insertion order by default; it does not sort keys.

**2. Does LinkedHashMap allow duplicate keys?**

No.

A duplicate key replaces the previous value.

**3. Does it allow null?**

Yes.

One null key and multiple null values are allowed.

**4. Is LinkedHashMap thread-safe?**

No.

**5. How does it maintain order?**

Using a doubly linked list connecting entries.

**6. Can it maintain access order?**

Yes.

Use the constructor with:

    accessOrder = true

**7. What is the main difference from HashMap?**

LinkedHashMap additionally maintains predictable iteration order.

**8. What is the main difference from TreeMap?**

LinkedHashMap maintains insertion/access order, while TreeMap sorts by key.

**9. Can LinkedHashMap be used for an LRU cache?**

Yes.

Use access-order mode and override `removeEldestEntry()`.

**10. Is the clone deep or shallow?**

Shallow.

▸ Quick Comparison

| Feature | HashMap | LinkedHashMap | TreeMap |
|---|---|---|---|
| Data structure | Hash table | Hash table + doubly linked list | Red-Black Tree |
| Ordering | No guaranteed order | Insertion/access order | Sorted by key |
| Average get | O(1) | O(1) | O(log n) |
| Null key | One | One | Generally no with natural ordering |
| Duplicate keys | No | No | No |
| Duplicate values | Yes | Yes | Yes |
| Thread-safe | No | No | No |
| LRU support | No direct support | Yes | No |
| Main use | Fast lookup | Fast lookup + order | Sorted map |

▸ Interview Questions

**Q1. What is LinkedHashMap?**

> LinkedHashMap is a HashMap subclass that maintains predictable iteration order using a doubly linked list of entries.

**Q2. What order does LinkedHashMap maintain by default?**

> Insertion order.

**Q3. Can LinkedHashMap maintain access order?**

> Yes, by using the constructor with `accessOrder = true`.

**Q4. How does LinkedHashMap maintain insertion order?**

> It maintains a doubly linked list connecting its entries.

**Q5. Is LinkedHashMap thread-safe?**

> No, LinkedHashMap is not synchronized or thread-safe.

**Q6. Does LinkedHashMap allow null keys?**

> Yes, it allows one null key.

**Q7. Does LinkedHashMap allow null values?**

> Yes, it allows multiple null values.

**Q8. Does LinkedHashMap allow duplicate keys?**

> No. A duplicate key replaces its previous value.

**Q9. Is LinkedHashMap sorted?**

> No. It maintains insertion order by default, not sorted order.

**Q10. Why use LinkedHashMap instead of HashMap?**

> Use LinkedHashMap when you need predictable iteration order in addition to hash-based lookup.

**Q11. Why use LinkedHashMap instead of TreeMap?**

> Use LinkedHashMap when you need insertion/access order; use TreeMap when you need keys sorted.

**Q12. What is the time complexity of get() in LinkedHashMap?**

> Average O(1).

**Q13. Can LinkedHashMap be used to implement an LRU cache?**

> Yes. Enable access-order mode and override `removeEldestEntry()` to remove the least recently used entry.

**Q14. What does accessOrder = true mean?**

> It makes iteration order depend on entry access, so recently accessed entries move toward the end.

**Q15. Is LinkedHashMap a separate implementation of Map?**

> Yes. It extends HashMap and adds predictable ordering through linked entry connections.

**Q16. What happens when an existing key is inserted again?**

> Its value is replaced; no new key-value entry is created.

**Q17. Does updating an existing key change its insertion position?**

> In normal insertion-order mode, no. The existing entry retains its position.

**Q18. What is the difference between insertion order and access order?**

> Insertion order follows when entries were inserted; access order changes based on accesses such as `get()`.

**Q19. What is `removeEldestEntry()` used for?**

> It is commonly overridden to automatically remove the oldest entry, especially for bounded LRU caches.

**Q20. Is LinkedHashMap synchronized?**

> No. External synchronization or a concurrent design is required when multiple threads modify it concurrently.

▸ Must Remember

    LinkedHashMap
    → HashMap + doubly linked list

    Default
    → Insertion order

    Optional
    → Access order

    Lookup
    → Average O(1)

    Null
    → One null key + multiple null values

    Duplicate key
    → Replaces old value

    Thread-safe
    → No

    LRU cache
    → Yes, using access-order mode

    HashMap
    → No guaranteed order

    LinkedHashMap
    → Insertion/access order

    TreeMap
    → Sorted order

▸ Final Interview Definition

> LinkedHashMap is a hash-table-based Map implementation that extends HashMap and maintains predictable iteration order using a doubly linked list; by default it preserves insertion order and can optionally preserve access order.