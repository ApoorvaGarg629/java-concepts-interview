# TreeMap

▸ What is TreeMap?

`TreeMap` is a `Map` implementation that stores key-value pairs in **sorted order of keys**.

It is based on a **Red-Black Tree**.

**Interview one-liner:**

> TreeMap is a Red-Black Tree-based Map that keeps entries sorted according to the keys' natural ordering or a provided Comparator.

▸ Package

    import java.util.TreeMap;

▸ Basic Example

    TreeMap<Integer, String> map = new TreeMap<>();

    map.put(30, "C");
    map.put(10, "A");
    map.put(20, "B");

    System.out.println(map);

Output:

    {10=A, 20=B, 30=C}

Even though the entries were inserted in the order:

    30 → 10 → 20

they are iterated in sorted key order:

    10 → 20 → 30

▸ Important Characteristics

- Stores key-value pairs
- Keys are maintained in sorted order
- Based on a Red-Black Tree
- Does not allow duplicate keys
- Allows duplicate values
- Generally does not allow `null` keys with natural ordering
- Can contain `null` values
- Not synchronized
- Not thread-safe
- `put()`, `get()`, and `remove()` take O(log n)
- Keys must be mutually comparable when natural ordering is used
- Can use a custom `Comparator`
- Implements `NavigableMap`
- Implements `SortedMap`

▸ Class Hierarchy

    Object
       |
    AbstractMap
       |
    TreeMap

TreeMap implements:

    NavigableMap
          |
      SortedMap
          |
        Map

Conceptually:

    Map
     |
    SortedMap
     |
    NavigableMap
     |
    TreeMap

▸ What Data Structure Does TreeMap Use?

TreeMap uses a **Red-Black Tree** internally.

A Red-Black Tree is a **self-balancing Binary Search Tree**.

Conceptually:

    TreeMap
       |
       v
    Red-Black Tree
       |
       v
    Sorted key-value entries

The tree remains approximately balanced, allowing operations to remain O(log n).

▸ Why Does TreeMap Use a Red-Black Tree?

A normal Binary Search Tree can become unbalanced.

Example:

    10
      \
       20
         \
          30
            \
             40

This starts behaving like a linked list.

Searching can become:

    O(n)

A Red-Black Tree automatically maintains balance so that operations remain:

    O(log n)

**Interview one-liner:**

> TreeMap uses a Red-Black Tree to maintain sorted keys while keeping search, insertion, and deletion at O(log n).

▸ TreeMap Ordering

TreeMap can sort keys in two ways:

1. Natural ordering
2. Custom Comparator

Natural ordering example:

    TreeMap<Integer, String> map = new TreeMap<>();

    map.put(30, "C");
    map.put(10, "A");
    map.put(20, "B");

Output:

    {10=A, 20=B, 30=C}

Integer implements `Comparable`, so natural ordering is used.

▸ Natural Ordering

Natural ordering means the key's class implements `Comparable`.

Example:

    String implements Comparable<String>
    Integer implements Comparable<Integer>

Therefore:

    TreeMap<Integer, String>

can automatically sort integer keys.

Similarly:

    TreeMap<String, Integer>

sorts String keys lexicographically.

▸ Custom Comparator

You can provide a `Comparator` to define your own ordering.

Example:

    TreeMap<Integer, String> map =
        new TreeMap<>(Comparator.reverseOrder());

    map.put(30, "C");
    map.put(10, "A");
    map.put(20, "B");

Output:

    {30=C, 20=B, 10=A}

Here the keys are sorted in descending order.

▸ TreeMap with Comparator

Example:

    TreeMap<String, Integer> map =
        new TreeMap<>(Comparator.reverseOrder());

    map.put("Java", 1);
    map.put("Python", 2);
    map.put("C++", 3);

The keys are maintained in reverse lexicographical order.

▸ Comparable vs Comparator

    Comparable
    → Natural ordering
    → compareTo()
    → Usually implemented by the class itself

    Comparator
    → Custom ordering
    → compare()
    → Ordering can be defined externally

Example:

    class Student implements Comparable<Student> {

        @Override
        public int compareTo(Student other) {
            return this.id - other.id;
        }
    }

Using Comparator:

    Comparator<Student> comparator =
        Comparator.comparingInt(s -> s.id);

    TreeMap<Student, String> map =
        new TreeMap<>(comparator);

▸ Duplicate Keys

TreeMap does not allow duplicate keys.

Example:

    TreeMap<Integer, String> map = new TreeMap<>();

    map.put(10, "Java");
    map.put(10, "Spring");

Result:

    {10=Spring}

The second value replaces the first value.

▸ Duplicate Values

TreeMap allows duplicate values.

Example:

    map.put(1, "Java");
    map.put(2, "Java");

Result:

    {1=Java, 2=Java}

Keys must be unique, but values do not have to be unique.

▸ How put() Works

Suppose:

    map.put(20, "Java");

Conceptually:

1. Start at the root.
2. Compare the new key with the current node's key.
3. If the new key is smaller, move left.
4. If the new key is larger, move right.
5. If the key is equal, replace the existing value.
6. If an empty position is found, insert the new entry.
7. Rebalance the Red-Black Tree if required.

Example:

    Insert 20
    Insert 10
    Insert 30

Conceptually:

          20
         /  \
       10    30

▸ How get() Works

For:

    map.get(30);

TreeMap compares the target key with tree nodes.

Conceptually:

    30 compared with 20
        → greater
        → move right

    30 compared with 30
        → equal
        → return value

Because the tree is balanced, lookup takes:

    O(log n)

▸ How remove() Works

When removing a key:

    map.remove(20);

TreeMap:

1. Searches for the key.
2. Finds the corresponding node.
3. Removes the node.
4. Rebalances the Red-Black Tree if necessary.

Time complexity:

    O(log n)

▸ What is a Red-Black Tree?

A Red-Black Tree is a self-balancing Binary Search Tree where each node has a color:

    RED
    BLACK

It follows specific rules to keep the tree approximately balanced.

The exact balancing process involves rotations and recoloring.

For interviews, remember:

> Red-Black Tree is a self-balancing BST that guarantees O(log n) search, insertion, and deletion.

▸ Red-Black Tree Properties

Important properties include:

- Every node is either red or black.
- The root is black.
- Null leaf references are treated as black.
- A red node cannot have a red child.
- Every path from a node to its descendant null leaves has the same number of black nodes.

These rules keep the tree approximately balanced.

▸ Rotations

Red-Black Trees use rotations to maintain balance.

Common rotations:

    Left Rotation
    Right Rotation

Rotations restructure the tree without breaking the Binary Search Tree ordering.

Example concept:

    Before:

        10
          \
           20
             \
              30

After balancing:

          20
         /  \
       10    30

▸ Does TreeMap Maintain Insertion Order?

No.

TreeMap does not maintain insertion order.

It maintains **sorted key order**.

Example:

    map.put(30, "C");
    map.put(10, "A");
    map.put(20, "B");

Output:

    {10=A, 20=B, 30=C}

Remember:

    HashMap       → No guaranteed order
    LinkedHashMap → Insertion order
    TreeMap       → Sorted key order

▸ Does TreeMap Sort Values?

No.

TreeMap sorts based on **keys**, not values.

Example:

    map.put(3, "Apple");
    map.put(1, "Zebra");
    map.put(2, "Mango");

Output:

    {1=Zebra, 2=Mango, 3=Apple}

The values are not sorted.

▸ Null Keys

With natural ordering, TreeMap generally does not allow a `null` key because it needs to compare keys.

Example:

    TreeMap<Integer, String> map = new TreeMap<>();

    map.put(null, "Java");

This results in:

    NullPointerException

The reason is that the tree needs to compare the null key with other keys.

▸ Null Values

TreeMap allows `null` values.

Example:

    TreeMap<Integer, String> map = new TreeMap<>();

    map.put(1, null);
    map.put(2, "Java");

This is valid.

So remember:

    null key    → Generally not allowed with natural ordering
    null value  → Allowed

▸ Can TreeMap Have a Null Key with a Comparator?

A custom Comparator can potentially define how `null` should be ordered.

For example:

    Comparator<Integer> comparator =
        Comparator.nullsFirst(Integer::compareTo);

    TreeMap<Integer, String> map =
        new TreeMap<>(comparator);

    map.put(null, "Unknown");
    map.put(10, "Java");

Here the Comparator explicitly defines how null should be compared.

**Interview point:**

> TreeMap's ability to handle a null key depends on the ordering mechanism; natural ordering does not support null keys, while a Comparator can explicitly define null ordering.

▸ Main Methods

TreeMap provides normal Map methods:

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

It also provides NavigableMap methods such as:

    firstKey()
    lastKey()
    lowerKey()
    floorKey()
    ceilingKey()
    higherKey()
    firstEntry()
    lastEntry()
    pollFirstEntry()
    pollLastEntry()
    descendingMap()

▸ firstKey()

Returns the smallest key according to the map's ordering.

Example:

    TreeMap<Integer, String> map = new TreeMap<>();

    map.put(30, "C");
    map.put(10, "A");
    map.put(20, "B");

    map.firstKey();

Result:

    10

▸ lastKey()

Returns the largest key according to the map's ordering.

    map.lastKey();

Result:

    30

▸ lowerKey()

Returns the greatest key that is strictly less than the given key.

Example:

    TreeMap<Integer, String> map = new TreeMap<>();

    map.put(10, "A");
    map.put(20, "B");
    map.put(30, "C");

    map.lowerKey(20);

Result:

    10

▸ floorKey()

Returns the greatest key less than or equal to the given key.

Example:

    map.floorKey(20);

Result:

    20

Example:

    map.floorKey(25);

Result:

    20

▸ ceilingKey()

Returns the smallest key greater than or equal to the given key.

Example:

    map.ceilingKey(20);

Result:

    20

Example:

    map.ceilingKey(25);

Result:

    30

▸ higherKey()

Returns the smallest key strictly greater than the given key.

Example:

    map.higherKey(20);

Result:

    30

▸ Key Navigation Summary

For keys:

    10, 20, 30

For target:

    20

    lowerKey(20)   → 10
    floorKey(20)   → 20
    ceilingKey(20) → 20
    higherKey(20)  → 30

**Easy way to remember:**

    lower   → strictly smaller
    floor   → smaller or equal
    ceiling → greater or equal
    higher  → strictly greater

▸ firstEntry()

Returns the key-value entry with the smallest key.

    Map.Entry<Integer, String> entry =
        map.firstEntry();

▸ lastEntry()

Returns the key-value entry with the largest key.

    Map.Entry<Integer, String> entry =
        map.lastEntry();

▸ pollFirstEntry()

Returns and removes the first entry.

    Map.Entry<Integer, String> entry =
        map.pollFirstEntry();

▸ pollLastEntry()

Returns and removes the last entry.

    Map.Entry<Integer, String> entry =
        map.pollLastEntry();

▸ descendingMap()

Returns a reverse-order view of the map.

Example:

    NavigableMap<Integer, String> descending =
        map.descendingMap();

If the original order is:

    10, 20, 30

the descending view gives:

    30, 20, 10

▸ Iteration

TreeMap iteration follows sorted key order.

Example:

    for (Map.Entry<Integer, String> entry : map.entrySet()) {
        System.out.println(
            entry.getKey() + " = " + entry.getValue()
        );
    }

If keys are:

    30, 10, 20

Output:

    10 = A
    20 = B
    30 = C

▸ entrySet() vs keySet()

If both key and value are needed:

    for (Map.Entry<Integer, String> entry : map.entrySet()) {
        System.out.println(
            entry.getKey() + " = " + entry.getValue()
        );
    }

If only keys are needed:

    for (Integer key : map.keySet()) {
        System.out.println(key);
    }

`entrySet()` is generally preferred when both key and value are required.

▸ TreeMap vs HashMap

| Feature | HashMap | TreeMap |
|---|---|---|
| Data structure | Hash table | Red-Black Tree |
| Ordering | No guaranteed order | Sorted by key |
| Average get | O(1) | O(log n) |
| Average put | O(1) | O(log n) |
| Average remove | O(1) | O(log n) |
| Null key | One allowed | Generally not with natural ordering |
| Navigation methods | No | Yes |
| Main use | Fast lookup | Sorted keys |

▸ TreeMap vs LinkedHashMap

| Feature | LinkedHashMap | TreeMap |
|---|---|---|
| Ordering | Insertion/access order | Sorted key order |
| Data structure | Hash table + linked structure | Red-Black Tree |
| Average get | O(1) | O(log n) |
| Null key | One allowed | Generally not with natural ordering |
| LRU support | Yes | No |
| Navigation | Limited | Rich navigation methods |

Remember:

    LinkedHashMap → Preserve insertion/access order
    TreeMap       → Sort keys

▸ TreeMap vs TreeSet

Both are based on tree-based ordering.

    TreeMap
    → Key-value pairs

    TreeSet
    → Unique elements

Conceptually:

    TreeMap
    → key → value

    TreeSet
    → element

Both use sorted tree-based structures.

▸ TreeMap vs HashSet

    TreeMap
    → Map
    → Key-value pairs
    → Sorted keys

    HashSet
    → Set
    → Unique elements
    → No guaranteed order

▸ Custom Objects as Keys

TreeMap can use custom objects as keys.

However, TreeMap needs a way to compare those keys.

You can either:

1. Implement `Comparable`
2. Provide a `Comparator`

Example using Comparable:

    class Student implements Comparable<Student> {

        int id;

        Student(int id) {
            this.id = id;
        }

        @Override
        public int compareTo(Student other) {
            return Integer.compare(this.id, other.id);
        }
    }

Then:

    TreeMap<Student, String> map =
        new TreeMap<>();

▸ TreeMap with Custom Comparator

Instead of modifying the class:

    Comparator<Student> comparator =
        Comparator.comparingInt(student -> student.id);

    TreeMap<Student, String> map =
        new TreeMap<>(comparator);

This is useful when you want different sorting strategies without changing the Student class.

▸ Important Difference: TreeMap Uses Comparison

HashMap primarily uses:

    hashCode()
    equals()

TreeMap primarily uses:

    compareTo()
    or
    Comparator.compare()

This is a very important interview difference.

    HashMap
    → hashCode() + equals()

    TreeMap
    → compareTo() / Comparator

▸ Can TreeMap Use a Key Without Comparable?

Yes, if a suitable `Comparator` is provided to the TreeMap.

Example:

    Comparator<Student> comparator =
        Comparator.comparingInt(student -> student.id);

    TreeMap<Student, String> map =
        new TreeMap<>(comparator);

Without either:

    Comparable

or:

    Comparator

TreeMap cannot determine the ordering of custom keys.

▸ Comparison Must Be Consistent

The ordering used by TreeMap determines whether two keys are considered equivalent for map purposes.

If:

    compareTo() == 0

or:

    comparator.compare(key1, key2) == 0

TreeMap treats the keys as equivalent for its ordering and does not store them as separate keys, even if `equals()` would say they are different.

**Important interview point:**

> TreeMap uses comparison to determine key ordering and key equivalence, so a comparison result of zero means the keys are treated as the same key by the map.

▸ equals() vs compareTo() Trap

Consider:

    key1.equals(key2) == false

but:

    key1.compareTo(key2) == 0

TreeMap can treat them as the same key.

This is different from HashMap, where key equality is based on `equals()` after hash-based lookup.

▸ Time Complexity

For a Red-Black Tree:

    put()          → O(log n)
    get()          → O(log n)
    remove()       → O(log n)
    containsKey()  → O(log n)
    firstKey()     → O(log n) or effectively constant depending on implementation/path
    lastKey()      → O(log n) or effectively constant depending on implementation/path

Iteration over all entries:

    O(n)

For interview purposes, remember:

    TreeMap basic operations → O(log n)

▸ Why Not Use HashMap If You Need Sorting?

HashMap is optimized for hash-based lookup but does not maintain sorted keys.

If you need sorted keys as part of the data structure's behavior:

    TreeMap

is the appropriate choice.

You can also sort a separate collection of keys from a HashMap, but TreeMap provides sorted-map behavior directly.

▸ When Should You Use TreeMap?

Use TreeMap when:

- You need key-value storage.
- Keys must remain sorted.
- You need range-based queries.
- You need methods such as `floorKey()`, `ceilingKey()`, `lowerKey()`, or `higherKey()`.
- You need first/last key operations.
- You need a custom key ordering.

▸ When Should You Not Use TreeMap?

Do not choose TreeMap when:

- You only need fast lookup and don't care about ordering → HashMap.
- You need insertion order → LinkedHashMap.
- You need concurrent map operations → consider ConcurrentHashMap or another suitable concurrent design.

▸ Range Operations

One of TreeMap's major advantages is support for range-based operations through `NavigableMap` and `SortedMap`.

Example:

    TreeMap<Integer, String> map = new TreeMap<>();

    map.put(10, "A");
    map.put(20, "B");
    map.put(30, "C");
    map.put(40, "D");

You can obtain views of portions of the map using methods such as:

    subMap()
    headMap()
    tailMap()

▸ subMap()

Returns a view of a portion of the map between specified keys.

Example:

    map.subMap(20, 40);

This represents the range:

    20
    30

The upper bound is exclusive in this overload.

You can also specify whether the boundaries are inclusive.

▸ headMap()

Returns a view of entries whose keys are less than the specified key.

Example:

    map.headMap(30);

Result conceptually:

    10
    20

▸ tailMap()

Returns a view of entries whose keys are greater than or equal to the specified key.

Example:

    map.tailMap(30);

Result conceptually:

    30
    40

▸ Views

Methods such as:

    keySet()
    navigableKeySet()
    entrySet()
    descendingMap()
    subMap()
    headMap()
    tailMap()

can return views backed by the original TreeMap.

Changes through a modifiable view can affect the original map.

▸ Fail-Fast Iterator

Iterators obtained from TreeMap are generally fail-fast.

Example:

    for (Integer key : map.keySet()) {
        map.put(50, "E");
    }

A structural modification during iteration can result in:

    ConcurrentModificationException

If removal is required during iteration, use the iterator's `remove()` method where supported.

Example:

    Iterator<Integer> iterator =
        map.keySet().iterator();

    while (iterator.hasNext()) {

        Integer key = iterator.next();

        if (key == 20) {
            iterator.remove();
        }
    }

Fail-fast behavior is a best-effort mechanism and is not a thread-safety guarantee.

▸ Is TreeMap Thread-Safe?

No.

TreeMap is not synchronized and is not thread-safe.

If multiple threads access and modify the same TreeMap concurrently, appropriate synchronization or a concurrent collection should be considered.

For concurrent sorted-map requirements, consider:

    ConcurrentSkipListMap

▸ TreeMap vs ConcurrentSkipListMap

    TreeMap
    → Sorted map
    → Not thread-safe

    ConcurrentSkipListMap
    → Sorted concurrent map
    → Designed for concurrent access

**Interview one-liner:**

> Use ConcurrentSkipListMap when you need a sorted map with concurrent access.

▸ Constructors

Common constructors include:

    TreeMap()

    TreeMap(Comparator<? super K> comparator)

    TreeMap(Map<? extends K, ? extends V> m)

    TreeMap(SortedMap<K, ? extends V> m)

Example:

    TreeMap<Integer, String> map =
        new TreeMap<>();

Example with Comparator:

    TreeMap<Integer, String> map =
        new TreeMap<>(Comparator.reverseOrder());

Example from another Map:

    TreeMap<Integer, String> map2 =
        new TreeMap<>(map1);

▸ clone()

TreeMap supports cloning.

The clone is a **shallow copy**.

Example:

    TreeMap<Integer, String> map1 =
        new TreeMap<>();

    map1.put(1, "Java");

    TreeMap<Integer, String> map2 =
        (TreeMap<Integer, String>) map1.clone();

A new TreeMap object is created, but the keys and values themselves are not deeply copied.

▸ Copy Constructor

You can create a new TreeMap:

    TreeMap<Integer, String> map2 =
        new TreeMap<>(map1);

This creates a new map containing the same key and value references.

It is a shallow copy.

▸ put() Return Value

`put()` returns the previous value associated with the key.

Example:

    map.put(1, "Java");

    String oldValue =
        map.put(1, "Spring");

Now:

    oldValue = "Java"

If the key did not previously exist, `put()` returns `null`.

▸ get() Returning null

If:

    map.get(key)

returns `null`, it can mean:

1. The key does not exist.
2. The key exists and its value is `null`.

Use:

    map.containsKey(key)

to distinguish the two cases.

▸ remove() Return Value

`remove(key)` returns the value associated with the removed key.

Example:

    String value = map.remove(1);

If key `1` existed:

    value = associated value

Otherwise:

    value = null

▸ Important Interview Traps

**1. Is TreeMap sorted?**

Yes. It sorts entries based on keys.

**2. Does TreeMap sort values?**

No. It sorts keys.

**3. What data structure does TreeMap use?**

A Red-Black Tree.

**4. What is the time complexity of get()?**

O(log n).

**5. Does TreeMap allow duplicate keys?**

No.

**6. Does TreeMap allow duplicate values?**

Yes.

**7. Does TreeMap allow null keys?**

Natural ordering does not support null keys.

A Comparator can explicitly define null ordering.

**8. Does TreeMap allow null values?**

Yes.

**9. Is TreeMap thread-safe?**

No.

**10. How does TreeMap determine key ordering?**

Using natural ordering through `Comparable` or a supplied `Comparator`.

**11. What is the difference between HashMap and TreeMap?**

> HashMap provides hash-based average O(1) lookup with no guaranteed ordering, while TreeMap maintains sorted keys with O(log n) operations.

**12. What is the difference between LinkedHashMap and TreeMap?**

> LinkedHashMap maintains insertion/access order, while TreeMap maintains sorted key order.

**13. What is floorKey()?**

> Returns the greatest key less than or equal to the given key.

**14. What is ceilingKey()?**

> Returns the smallest key greater than or equal to the given key.

**15. What is lowerKey()?**

> Returns the greatest key strictly less than the given key.

**16. What is higherKey()?**

> Returns the smallest key strictly greater than the given key.

**17. Why does TreeMap need Comparable or Comparator?**

> Because it must compare keys to maintain sorted order.

**18. Can custom objects be TreeMap keys?**

> Yes, if they are comparable or a Comparator is supplied.

**19. What happens if compareTo() returns 0 for two different objects?**

> TreeMap treats them as equivalent keys and does not store them as separate keys.

**20. Can TreeMap be used for range queries?**

> Yes. Methods such as `subMap()`, `headMap()`, and `tailMap()` support range-based operations.

**21. What is the alternative for a concurrent sorted map?**

> `ConcurrentSkipListMap`.

▸ Quick Comparison

| Feature | HashMap | LinkedHashMap | TreeMap |
|---|---|---|---|
| Structure | Hash table | Hash table + linked structure | Red-Black Tree |
| Ordering | No guaranteed order | Insertion/access order | Sorted key order |
| Average get | O(1) | O(1) | O(log n) |
| Average put | O(1) | O(1) | O(log n) |
| Average remove | O(1) | O(1) | O(log n) |
| Null key | One allowed | One allowed | Generally no with natural ordering |
| Duplicate keys | No | No | No |
| Duplicate values | Yes | Yes | Yes |
| Thread-safe | No | No | No |
| Custom ordering | No | No | Yes |
| Range queries | No | No | Yes |
| Navigation methods | No | No | Yes |

▸ Must Remember

    TreeMap
    → Map with sorted keys

    Internal structure
    → Red-Black Tree

    Basic operations
    → O(log n)

    Ordering
    → Natural ordering or Comparator

    Duplicate keys
    → Not allowed

    Duplicate values
    → Allowed

    Null key
    → Not allowed with natural ordering

    Null value
    → Allowed

    Thread-safe
    → No

    HashMap
    → Fast lookup, no guaranteed order

    LinkedHashMap
    → Fast lookup, insertion/access order

    TreeMap
    → Sorted keys

    lowerKey()
    → Strictly smaller

    floorKey()
    → Smaller or equal

    ceilingKey()
    → Greater or equal

    higherKey()
    → Strictly greater

    Range operations
    → subMap(), headMap(), tailMap()

    Concurrent sorted map
    → ConcurrentSkipListMap

▸ One-Liner Comparison

    HashMap
    → Fast lookup, no guaranteed order

    LinkedHashMap
    → Fast lookup + insertion/access order

    TreeMap
    → Sorted keys + O(log n) operations

▸ Final Interview Definition

> TreeMap is a NavigableMap implementation based on a Red-Black Tree that maintains keys in sorted order using natural ordering or a supplied Comparator, with O(log n) time for basic map operations.