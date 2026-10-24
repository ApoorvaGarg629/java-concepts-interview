# LinkedHashSet

▸ What is LinkedHashSet?

`LinkedHashSet` is a `Set` implementation that combines the uniqueness of `HashSet` with predictable iteration order.

It maintains **insertion order**.

**Interview one-liner:**

> LinkedHashSet is a HashSet that maintains insertion order using a linked list along with a hash table.

▸ Package

    import java.util.LinkedHashSet;

▸ Basic Example

    LinkedHashSet<Integer> set = new LinkedHashSet<>();

    set.add(30);
    set.add(10);
    set.add(20);

    System.out.println(set);

Output:

    [30, 10, 20]

The elements are returned in the same order in which they were inserted.

▸ Important Characteristics

- Stores unique elements
- Does not allow duplicate elements
- Maintains insertion order
- Allows one `null` element
- Not synchronized
- Not thread-safe
- Provides average O(1) time for `add()`, `remove()`, and `contains()`
- Extends `HashSet`
- Uses a hash table internally
- Maintains a doubly linked list to preserve insertion order

▸ Class Hierarchy

    Object
       |
    AbstractCollection
       |
    AbstractSet
       |
    HashSet
       |
    LinkedHashSet

`LinkedHashSet` extends `HashSet`.

▸ LinkedHashSet vs HashSet

    HashSet
    → Unique elements
    → No guaranteed iteration order

    LinkedHashSet
    → Unique elements
    → Maintains insertion order

Example:

    Set<Integer> set = new LinkedHashSet<>();

    set.add(3);
    set.add(1);
    set.add(2);

    System.out.println(set);

Output:

    [3, 1, 2]

▸ Why Does LinkedHashSet Maintain Insertion Order?

`LinkedHashSet` uses a linked structure in addition to the hash table.

Conceptually:

    Hash table
        +
    Doubly linked list

The hash table provides fast lookup.

The linked list maintains insertion order.

Conceptually:

    10 <-> 20 <-> 30 <-> 40

So iteration follows:

    10 → 20 → 30 → 40

**Interview one-liner:**

> LinkedHashSet maintains insertion order by linking its entries together while using hashing for fast lookup.

▸ Internal Working

`LinkedHashSet` is built on top of `HashSet`.

The important relationship is:

    LinkedHashSet
          |
          v
       HashSet
          |
          v
      HashMap
          |
          v
    Hash table

Conceptually, a `HashSet` uses a `HashMap` internally to store its elements as keys.

`LinkedHashSet` uses the LinkedHashMap-style linked entry structure to preserve insertion order.

▸ How add() Works

Suppose:

    set.add(10);

Conceptually:

1. Calculate the hash of `10`.
2. Find the appropriate bucket.
3. Check whether `10` already exists.
4. If it exists, do not add it.
5. If it does not exist, create a new entry.
6. Store it in the hash structure.
7. Link it into the insertion-order linked structure.

Example:

    set.add(10);
    set.add(20);
    set.add(30);

Conceptually:

    Hash structure
    → 10, 20, 30

    Linked structure
    → 10 <-> 20 <-> 30

▸ Duplicate Elements

A Set does not allow duplicate elements.

Example:

    LinkedHashSet<Integer> set = new LinkedHashSet<>();

    set.add(10);
    set.add(20);
    set.add(10);

Result:

    [10, 20]

The second `10` is ignored.

`add()` returns `false` when the element already exists.

Example:

    boolean result = set.add(10);

If `10` already exists:

    result = false

If `10` is newly added:

    result = true

▸ Does LinkedHashSet Allow null?

Yes.

It allows one `null` element.

Example:

    LinkedHashSet<String> set = new LinkedHashSet<>();

    set.add(null);
    set.add("Java");
    set.add(null);

Result:

    [null, Java]

The second `null` is ignored because duplicates are not allowed.

▸ Does LinkedHashSet Allow Duplicate Values?

There is no separate key/value concept in a Set.

Each element must be unique.

Example:

    set.add("Java");
    set.add("Java");

Only one `"Java"` is stored.

▸ Main Methods

Common methods include:

    add()
    remove()
    contains()
    size()
    isEmpty()
    clear()
    iterator()
    toArray()
    addAll()
    removeAll()
    retainAll()
    containsAll()

Example:

    LinkedHashSet<String> set = new LinkedHashSet<>();

    set.add("Java");
    set.add("Spring");

    set.contains("Java");
    set.remove("Spring");
    set.size();
    set.isEmpty();

▸ Iteration

LinkedHashSet maintains insertion order during iteration.

Example:

    LinkedHashSet<String> set = new LinkedHashSet<>();

    set.add("Java");
    set.add("Python");
    set.add("C++");

    for (String value : set) {
        System.out.println(value);
    }

Output:

    Java
    Python
    C++

▸ Iterator

You can explicitly use an Iterator:

    Iterator<String> iterator = set.iterator();

    while (iterator.hasNext()) {
        String value = iterator.next();
        System.out.println(value);
    }

The iterator follows insertion order.

▸ iterator.remove()

You can safely remove the current element using `iterator.remove()`.

Example:

    Iterator<Integer> iterator = set.iterator();

    while (iterator.hasNext()) {
        Integer value = iterator.next();

        if (value == 20) {
            iterator.remove();
        }
    }

This removes `20` from the underlying set.

**Interview one-liner:**

> `iterator.remove()` removes the last element returned by `next()` and is the safe way to remove an element during iteration.

▸ LinkedHashSet vs HashSet vs TreeSet

    HashSet
    → Unique elements
    → No guaranteed order
    → Average O(1)

    LinkedHashSet
    → Unique elements
    → Insertion order
    → Average O(1)

    TreeSet
    → Unique elements
    → Sorted order
    → O(log n)

▸ Quick Comparison

| Feature | HashSet | LinkedHashSet | TreeSet |
|---|---|---|---|
| Duplicate elements | No | No | No |
| Ordering | No guaranteed order | Insertion order | Sorted order |
| Data structure | Hash table | Hash table + linked structure | Red-Black Tree |
| Average add | O(1) | O(1) | O(log n) |
| Average remove | O(1) | O(1) | O(log n) |
| Average contains | O(1) | O(1) | O(log n) |
| Null | One null | One null | Generally no null with natural ordering |
| Thread-safe | No | No | No |

▸ When Should You Use LinkedHashSet?

Use `LinkedHashSet` when:

- You need unique elements.
- You want to preserve insertion order.
- You want average O(1) lookup.
- You want to remove duplicates while preserving the original order.

Example:

    List<Integer> numbers =
        Arrays.asList(10, 20, 10, 30, 20, 40);

    LinkedHashSet<Integer> unique =
        new LinkedHashSet<>(numbers);

Result:

    [10, 20, 30, 40]

This is a common use case.

▸ Remove Duplicates While Preserving Order

Given:

    [10, 20, 10, 30, 20, 40]

Using:

    LinkedHashSet<Integer> set =
        new LinkedHashSet<>(list);

Result:

    [10, 20, 30, 40]

The first occurrence is retained and duplicates are removed.

The original insertion order is preserved.

▸ LinkedHashSet vs HashSet for Duplicate Removal

If you only need to remove duplicates:

    HashSet

may be sufficient.

If you need to remove duplicates **and preserve the original order**:

    LinkedHashSet

is a better choice.

**Interview one-liner:**

> Use LinkedHashSet when you need uniqueness without losing insertion order.

▸ Is LinkedHashSet Sorted?

No.

This is an important interview trap.

Example:

    LinkedHashSet<Integer> set = new LinkedHashSet<>();

    set.add(50);
    set.add(10);
    set.add(30);

Output:

    [50, 10, 30]

It does not sort the elements.

For sorted elements, use:

    TreeSet

▸ Is LinkedHashSet Thread-Safe?

No.

`LinkedHashSet` is not synchronized and is not thread-safe.

If multiple threads access and modify the same set concurrently, use appropriate synchronization or a concurrent collection depending on the requirement.

▸ Can LinkedHashSet Store Custom Objects?

Yes.

Example:

    class Student {
        int id;
        String name;

        Student(int id, String name) {
            this.id = id;
            this.name = name;
        }
    }

You can store:

    LinkedHashSet<Student> students =
        new LinkedHashSet<>();

For correct duplicate detection based on object content, the class should properly override:

    equals()
    hashCode()

▸ equals() and hashCode()

LinkedHashSet uses hashing and equality to determine whether an element already exists.

If:

    obj1.equals(obj2) == true

then:

    obj1.hashCode() == obj2.hashCode()

must also be true.

Same hash code does not necessarily mean objects are equal.

    same hashCode ≠ same object

Different objects can have the same hash code, causing a collision.

▸ Custom Object Example

    class Student {
        int id;
        String name;

        Student(int id, String name) {
            this.id = id;
            this.name = name;
        }

        @Override
        public boolean equals(Object obj) {
            if (this == obj) {
                return true;
            }

            if (!(obj instanceof Student)) {
                return false;
            }

            Student other = (Student) obj;

            return id == other.id &&
                   Objects.equals(name, other.name);
        }

        @Override
        public int hashCode() {
            return Objects.hash(id, name);
        }
    }

Now two Student objects with the same `id` and `name` are treated as equal.

▸ Mutable Objects

Avoid modifying fields used by `equals()` or `hashCode()` after adding an object to the set.

Example:

    set.add(student);

If fields used in `hashCode()` are changed afterward, the set may not be able to locate the object correctly.

**Interview one-liner:**

> Elements stored in a hash-based Set should ideally be immutable with respect to fields used by `equals()` and `hashCode()`.

▸ Capacity and Load Factor

LinkedHashSet is based on HashSet and uses hash-table capacity and load-factor concepts.

Common defaults:

    Initial capacity = 16
    Load factor = 0.75

Conceptually:

    threshold = capacity × load factor

For example:

    16 × 0.75 = 12

When the number of entries crosses the relevant threshold, the hash table is resized.

▸ Resizing

When resizing occurs, the underlying hash table is expanded and entries are redistributed.

The linked structure maintains the insertion order.

Therefore, resizing does not mean that iteration order is lost.

▸ Time Complexity

Average:

    add()      → O(1)
    remove()   → O(1)
    contains() → O(1)

Iteration:

    O(n)

`O(n)` is required to visit all elements.

▸ Memory

LinkedHashSet uses more memory than HashSet because it maintains additional links for preserving insertion order.

Conceptually:

    HashSet
    → Hash table

    LinkedHashSet
    → Hash table + linked structure

Therefore, LinkedHashSet has additional memory overhead compared with HashSet.

▸ Why is LinkedHashSet Slower Than HashSet?

Both provide average O(1) operations.

However, LinkedHashSet maintains additional linked-list information.

Therefore:

    HashSet
    → Less memory overhead

    LinkedHashSet
    → More memory overhead
    → Maintains insertion order

The practical performance difference is usually secondary to the ordering requirement.

▸ LinkedHashSet vs LinkedHashMap

`LinkedHashSet` stores only unique elements.

`LinkedHashMap` stores key-value pairs.

    LinkedHashSet
    → element

    LinkedHashMap
    → key + value

Example:

    LinkedHashSet<String> set =
        new LinkedHashSet<>();

    set.add("Java");

Versus:

    LinkedHashMap<Integer, String> map =
        new LinkedHashMap<>();

    map.put(1, "Java");

▸ HashSet Internal Relationship

A useful interview concept:

    HashSet
    → internally backed by HashMap

Conceptually:

    set.add("Java");

can be thought of as storing:

    HashMap
    key   = "Java"
    value = dummy object

LinkedHashSet follows the same overall idea but uses the linked entry structure to preserve order.

▸ Does LinkedHashSet Have Key-Value Pairs?

No.

A Set stores elements only.

Conceptually:

    Set
    → element
    → element
    → element

A Map stores:

    key → value

Therefore:

    LinkedHashSet → elements
    LinkedHashMap → key-value pairs

▸ Fail-Fast Iterator

LinkedHashSet iterators are generally fail-fast.

Example:

    for (Integer value : set) {
        set.add(100);
    }

A structural modification while iterating can result in:

    ConcurrentModificationException

If removal is required during iteration, use:

    iterator.remove();

Fail-fast behavior is a best-effort mechanism and should not be treated as a thread-safety guarantee.

▸ clone()

LinkedHashSet supports cloning.

The clone is a **shallow copy**.

Example:

    LinkedHashSet<String> set1 =
        new LinkedHashSet<>();

    set1.add("Java");

    LinkedHashSet<String> set2 =
        (LinkedHashSet<String>) set1.clone();

A new set object is created, but the elements themselves are not deeply copied.

▸ Copy Constructor

You can also create a new LinkedHashSet:

    LinkedHashSet<Integer> set2 =
        new LinkedHashSet<>(set1);

This creates a new set containing the same element references.

It is a shallow copy.

▸ Constructors

Common constructors:

    LinkedHashSet()

    LinkedHashSet(int initialCapacity)

    LinkedHashSet(int initialCapacity, float loadFactor)

    LinkedHashSet(Collection<? extends E> c)

Example:

    LinkedHashSet<Integer> set =
        new LinkedHashSet<>();

Example with initial capacity:

    LinkedHashSet<Integer> set =
        new LinkedHashSet<>(32);

Example from another collection:

    List<Integer> list =
        Arrays.asList(10, 20, 30);

    LinkedHashSet<Integer> set =
        new LinkedHashSet<>(list);

▸ Null and Ordering

Important combination to remember:

    LinkedHashSet
    → Allows one null
    → Maintains insertion order

Example:

    set.add(null);
    set.add(10);
    set.add(20);

Iteration:

    null
    10
    20

▸ Important Interview Traps

**1. Does LinkedHashSet allow duplicates?**

No.

**2. Does LinkedHashSet maintain insertion order?**

Yes.

**3. Is LinkedHashSet sorted?**

No.

**4. Does LinkedHashSet allow null?**

Yes, one null element.

**5. Is LinkedHashSet thread-safe?**

No.

**6. What is the difference between HashSet and LinkedHashSet?**

> HashSet does not guarantee iteration order, while LinkedHashSet maintains insertion order.

**7. What is the difference between LinkedHashSet and TreeSet?**

> LinkedHashSet maintains insertion order, while TreeSet maintains sorted order.

**8. Why does LinkedHashSet use more memory than HashSet?**

> Because it maintains additional linked information to preserve insertion order.

**9. What is the average time complexity of add()?**

> O(1).

**10. Can LinkedHashSet contain custom objects?**

> Yes, but `equals()` and `hashCode()` should be correctly implemented when logical equality is required.

**11. Can LinkedHashSet remove duplicates from a List while preserving order?**

> Yes. Creating a LinkedHashSet from the List removes duplicates while preserving the first-occurrence order.

**12. Is LinkedHashSet synchronized?**

> No.

**13. What happens when the same element is added twice?**

> The second insertion is ignored and `add()` returns false.

**14. How does LinkedHashSet know an element already exists?**

> It uses the element's hash code to locate the bucket and `equals()` to confirm equality.

**15. Does LinkedHashSet store key-value pairs?**

> No. It stores unique elements.

**16. What does iterator.remove() do?**

> It removes the last element returned by `next()` from the underlying set.

**17. Is the clone of LinkedHashSet deep or shallow?**

> Shallow.

**18. Does resizing change insertion order?**

> No. The linked structure preserves the configured iteration order.

▸ Quick Revision Table

| Feature | LinkedHashSet |
|---|---|
| Type | Set |
| Duplicate elements | Not allowed |
| Ordering | Insertion order |
| Sorted | No |
| Null | One null allowed |
| Thread-safe | No |
| Average add | O(1) |
| Average remove | O(1) |
| Average contains | O(1) |
| Internal concept | Hash table + linked structure |
| Extends | HashSet |
| Main advantage | Uniqueness + insertion order |
| Common use | Remove duplicates while preserving order |

▸ Must Remember

    LinkedHashSet
    → HashSet + insertion order

    Duplicate elements
    → Not allowed

    Null
    → One null allowed

    Ordering
    → Insertion order

    Sorted
    → No

    add()
    → Average O(1)

    remove()
    → Average O(1)

    contains()
    → Average O(1)

    Thread-safe
    → No

    HashSet
    → Unique + no guaranteed order

    LinkedHashSet
    → Unique + insertion order

    TreeSet
    → Unique + sorted order

    Common use
    → Remove duplicates while preserving order

▸ One-Liner Comparison

    HashSet       → Unique elements, no guaranteed order
    LinkedHashSet → Unique elements, insertion order
    TreeSet       → Unique elements, sorted order

▸ Final Interview Definition

> LinkedHashSet is a HashSet implementation that stores unique elements and maintains predictable insertion order using a linked structure in addition to hash-based storage.