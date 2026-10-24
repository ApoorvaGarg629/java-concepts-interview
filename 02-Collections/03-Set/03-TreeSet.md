# TreeSet

▸ What is TreeSet?

`TreeSet` is a `Set` implementation that stores **unique elements in sorted order**.

It is based on a **Red-Black Tree**.

**Interview one-liner:**

> TreeSet is a Red-Black Tree-based Set that stores unique elements in sorted order using natural ordering or a provided Comparator.

▸ Package

    import java.util.TreeSet;

▸ Basic Example

    TreeSet<Integer> set = new TreeSet<>();

    set.add(30);
    set.add(10);
    set.add(20);

    System.out.println(set);

Output:

    [10, 20, 30]

Although elements were inserted as:

    30 → 10 → 20

TreeSet maintains them in sorted order:

    10 → 20 → 30

▸ Important Characteristics

- Stores unique elements
- Does not allow duplicate elements
- Maintains elements in sorted order
- Uses natural ordering by default
- Can use a custom `Comparator`
- Based on a Red-Black Tree
- Basic operations take O(log n)
- Generally does not allow `null`
- Not synchronized
- Not thread-safe
- Implements `NavigableSet`
- Implements `SortedSet`

▸ Class Hierarchy

    Object
       |
    AbstractCollection
       |
    AbstractSet
       |
    TreeSet

TreeSet implements:

    NavigableSet
          |
      SortedSet
          |
        Set

Conceptually:

    Set
     |
    SortedSet
     |
    NavigableSet
     |
    TreeSet

▸ What Data Structure Does TreeSet Use?

TreeSet is backed by a `TreeMap`.

Conceptually:

    TreeSet
       |
       v
    TreeMap
       |
       v
    Red-Black Tree

The elements of the TreeSet are stored as keys in the underlying TreeMap.

The values are internal placeholder objects.

**Interview one-liner:**

> TreeSet is backed by a TreeMap and uses a Red-Black Tree to maintain elements in sorted order.

▸ Why Does TreeSet Use a Red-Black Tree?

A normal Binary Search Tree can become unbalanced.

Example:

    10
      \
       20
         \
          30
            \
             40

This can behave like a linked list and operations can degrade toward:

    O(n)

A Red-Black Tree keeps itself approximately balanced.

Therefore:

    add()      → O(log n)
    remove()   → O(log n)
    contains() → O(log n)

**Interview one-liner:**

> TreeSet uses a self-balancing Red-Black Tree to maintain sorted order while keeping basic operations at O(log n).

▸ Sorted Order

TreeSet sorts elements according to:

1. Natural ordering
2. Custom Comparator

Example with natural ordering:

    TreeSet<Integer> set = new TreeSet<>();

    set.add(50);
    set.add(10);
    set.add(30);

Output:

    [10, 30, 50]

▸ Natural Ordering

Natural ordering is defined by the element's `Comparable` implementation.

For example:

    Integer implements Comparable<Integer>
    String implements Comparable<String>

Therefore:

    TreeSet<Integer>

automatically sorts integers.

And:

    TreeSet<String>

sorts Strings lexicographically.

▸ Custom Comparator

You can provide a Comparator to define a custom ordering.

Example:

    TreeSet<Integer> set =
        new TreeSet<>(Comparator.reverseOrder());

    set.add(10);
    set.add(30);
    set.add(20);

Output:

    [30, 20, 10]

The Comparator changes the ordering from ascending to descending.

▸ Comparable vs Comparator

    Comparable
    → Natural ordering
    → compareTo()
    → Implemented by the element class

    Comparator
    → Custom ordering
    → compare()
    → Provided externally

Example:

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

Using Comparator:

    Comparator<Student> comparator =
        Comparator.comparingInt(student -> student.id);

    TreeSet<Student> set =
        new TreeSet<>(comparator);

▸ Duplicate Elements

TreeSet does not allow duplicate elements.

Example:

    TreeSet<Integer> set = new TreeSet<>();

    set.add(10);
    set.add(20);
    set.add(10);

Result:

    [10, 20]

The second `10` is ignored.

`add()` returns:

    true

when the element is newly added.

It returns:

    false

when the element is considered already present.

▸ Important: TreeSet Uses Comparison for Uniqueness

TreeSet determines element equivalence using:

    compareTo()

or:

    Comparator.compare()

If:

    compareTo() == 0

TreeSet treats the elements as equivalent for Set purposes.

This is important even if `equals()` says the objects are different.

**Interview one-liner:**

> TreeSet uses comparison, not directly `equals()`, to determine whether two elements occupy the same position in the sorted set.

▸ equals() vs compareTo() Trap

Suppose:

    obj1.equals(obj2) == false

but:

    obj1.compareTo(obj2) == 0

TreeSet can treat them as duplicates.

Therefore, for a well-behaved sorted set, the ordering should ideally be consistent with `equals()`.

▸ Does TreeSet Allow null?

With natural ordering, TreeSet generally does **not** allow `null`.

Example:

    TreeSet<Integer> set = new TreeSet<>();

    set.add(null);

This can result in:

    NullPointerException

The reason is that TreeSet needs to compare the element with other elements to maintain sorted order.

▸ Can TreeSet Store null with a Comparator?

Yes, if the Comparator explicitly defines how null should be ordered.

Example:

    Comparator<Integer> comparator =
        Comparator.nullsFirst(Integer::compareTo);

    TreeSet<Integer> set =
        new TreeSet<>(comparator);

    set.add(null);
    set.add(10);
    set.add(20);

Conceptually:

    [null, 10, 20]

**Interview point:**

> Natural ordering does not support null elements, but a Comparator can explicitly define null ordering.

▸ Does TreeSet Allow Duplicate Values?

There is no separate key-value concept in a Set.

Every element must be unique according to the TreeSet's ordering.

Example:

    set.add(10);
    set.add(10);

Only one `10` is stored.

▸ How add() Works

Suppose:

    set.add(20);

Conceptually:

1. Start at the root.
2. Compare the new element with the current node.
3. If smaller, move left.
4. If larger, move right.
5. If comparison returns 0, treat it as an existing element.
6. If an empty position is found, insert the element.
7. Rebalance the Red-Black Tree if necessary.

Example:

    set.add(20);
    set.add(10);
    set.add(30);

Conceptually:

          20
         /  \
       10    30

▸ How contains() Works

For:

    set.contains(30);

TreeSet compares the target element with tree nodes.

Conceptually:

    30 compared with 20
        → greater
        → move right

    30 compared with 30
        → equal
        → found

Time complexity:

    O(log n)

▸ How remove() Works

When:

    set.remove(20);

TreeSet:

1. Searches for the element.
2. Finds the corresponding node.
3. Removes it.
4. Rebalances the Red-Black Tree if necessary.

Time complexity:

    O(log n)

▸ Red-Black Tree

A Red-Black Tree is a self-balancing Binary Search Tree.

Each node has a color:

    RED
    BLACK

The tree follows rules that prevent it from becoming highly unbalanced.

Important properties:

- Every node is either red or black.
- The root is black.
- Null leaf references are treated as black.
- A red node cannot have a red child.
- Every path from a node to its descendant null leaves contains the same number of black nodes.

These rules keep the tree approximately balanced.

▸ Rotations

Red-Black Trees use rotations and recoloring to maintain balance.

Common rotations:

    Left Rotation
    Right Rotation

Example:

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

The exact rotation/recoloring process is an implementation detail, but knowing that TreeSet uses Red-Black Tree balancing is important for interviews.

▸ Main Methods

TreeSet provides common Set methods:

    add()
    remove()
    contains()
    size()
    isEmpty()
    clear()
    iterator()
    addAll()
    removeAll()
    retainAll()
    containsAll()

It also provides NavigableSet methods:

    first()
    last()
    lower()
    floor()
    ceiling()
    higher()
    pollFirst()
    pollLast()
    descendingSet()

▸ first()

Returns the smallest element according to the set's ordering.

Example:

    TreeSet<Integer> set = new TreeSet<>();

    set.add(30);
    set.add(10);
    set.add(20);

    set.first();

Result:

    10

▸ last()

Returns the largest element.

    set.last();

Result:

    30

▸ lower()

Returns the greatest element strictly less than the given element.

Example:

    TreeSet<Integer> set = new TreeSet<>();

    set.add(10);
    set.add(20);
    set.add(30);

    set.lower(20);

Result:

    10

▸ floor()

Returns the greatest element less than or equal to the given element.

Example:

    set.floor(20);

Result:

    20

Example:

    set.floor(25);

Result:

    20

▸ ceiling()

Returns the smallest element greater than or equal to the given element.

Example:

    set.ceiling(20);

Result:

    20

Example:

    set.ceiling(25);

Result:

    30

▸ higher()

Returns the smallest element strictly greater than the given element.

Example:

    set.higher(20);

Result:

    30

▸ Navigation Summary

For:

    [10, 20, 30]

For target:

    20

    lower(20)   → 10
    floor(20)   → 20
    ceiling(20) → 20
    higher(20)  → 30

Easy way to remember:

    lower   → strictly smaller
    floor   → smaller or equal
    ceiling → greater or equal
    higher  → strictly greater

▸ pollFirst()

Returns and removes the first element.

Example:

    Integer value = set.pollFirst();

If:

    [10, 20, 30]

then:

    value = 10

and the set becomes:

    [20, 30]

▸ pollLast()

Returns and removes the last element.

Example:

    Integer value = set.pollLast();

If:

    [10, 20, 30]

then:

    value = 30

and the set becomes:

    [10, 20]

▸ descendingSet()

Returns a reverse-order view of the set.

Example:

    TreeSet<Integer> set = new TreeSet<>();

    set.add(10);
    set.add(20);
    set.add(30);

    NavigableSet<Integer> descending =
        set.descendingSet();

Output:

    [30, 20, 10]

▸ Iteration

TreeSet iteration follows sorted order.

Example:

    for (Integer value : set) {
        System.out.println(value);
    }

If the set contains:

    [30, 10, 20]

Output:

    10
    20
    30

▸ Iterator

You can explicitly use an Iterator:

    Iterator<Integer> iterator = set.iterator();

    while (iterator.hasNext()) {
        Integer value = iterator.next();
        System.out.println(value);
    }

The iterator follows the TreeSet's ascending ordering.

▸ Descending Iterator

TreeSet also provides:

    descendingIterator()

Example:

    Iterator<Integer> iterator =
        set.descendingIterator();

    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }

If the set is:

    [10, 20, 30]

Output:

    30
    20
    10

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

This removes `20` from the underlying TreeSet.

**Interview one-liner:**

> `iterator.remove()` removes the last element returned by `next()` and safely removes it from the underlying collection during iteration.

▸ TreeSet vs HashSet

    HashSet
    → Unique elements
    → No guaranteed order
    → Average O(1)

    TreeSet
    → Unique elements
    → Sorted order
    → O(log n)

▸ TreeSet vs LinkedHashSet

    LinkedHashSet
    → Unique elements
    → Insertion order
    → Average O(1)

    TreeSet
    → Unique elements
    → Sorted order
    → O(log n)

Remember:

    HashSet       → Unique + no guaranteed order
    LinkedHashSet → Unique + insertion order
    TreeSet       → Unique + sorted order

▸ Quick Comparison

| Feature | HashSet | LinkedHashSet | TreeSet |
|---|---|---|---|
| Duplicate elements | No | No | No |
| Ordering | No guaranteed order | Insertion order | Sorted order |
| Data structure | Hash table | Hash table + linked structure | Red-Black Tree |
| Average add | O(1) | O(1) | O(log n) |
| Average remove | O(1) | O(1) | O(log n) |
| Average contains | O(1) | O(1) | O(log n) |
| Null | One null | One null | Generally no with natural ordering |
| Thread-safe | No | No | No |
| Custom ordering | No | No | Yes |
| Navigation methods | No | No | Yes |

▸ TreeSet vs TreeMap

Both use tree-based ordering.

    TreeSet
    → Unique elements

    TreeMap
    → Key-value pairs

Conceptually:

    TreeSet
    → element

    TreeMap
    → key → value

Both are based on Red-Black Tree structures.

▸ TreeSet vs LinkedHashMap

    TreeSet
    → Set
    → Unique elements
    → Sorted order

    LinkedHashMap
    → Map
    → Key-value pairs
    → Insertion/access order

▸ TreeSet vs HashMap

    TreeSet
    → Unique elements
    → Sorted

    HashMap
    → Key-value pairs
    → No guaranteed ordering

▸ Custom Objects as Elements

TreeSet can store custom objects.

However, TreeSet needs a way to compare those objects.

You can either:

1. Implement `Comparable`
2. Provide a `Comparator`

Example:

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

    TreeSet<Student> students =
        new TreeSet<>();

▸ TreeSet with Comparator for Custom Objects

You can provide a Comparator without modifying the class.

Example:

    Comparator<Student> comparator =
        Comparator.comparingInt(student -> student.id);

    TreeSet<Student> students =
        new TreeSet<>(comparator);

Now students are ordered by `id`.

▸ Can TreeSet Store Objects Without Comparable?

Yes, if a suitable Comparator is supplied.

Example:

    TreeSet<Student> students =
        new TreeSet<>(
            Comparator.comparingInt(student -> student.id)
        );

Without either:

    Comparable

or:

    Comparator

TreeSet cannot determine how to order the objects.

▸ Important Comparable Trap

If:

    compareTo() == 0

TreeSet considers the elements equivalent for Set purposes.

Example:

    class Student implements Comparable<Student> {

        int id;

        @Override
        public int compareTo(Student other) {
            return Integer.compare(this.id, other.id);
        }
    }

Two students with the same `id` will be treated as duplicates even if they have different names.

Example:

    Student s1 = new Student(1);
    Student s2 = new Student(1);

    set.add(s1);
    set.add(s2);

Only one may remain because:

    s1.compareTo(s2) == 0

▸ Mutable Elements

Avoid changing fields that affect comparison after adding an object to TreeSet.

Example:

    set.add(student);

If the student's sorting field changes:

    student.id = 100;

the element's position in the tree may no longer correspond to the tree's ordering.

This can cause unexpected behavior.

**Interview one-liner:**

> Elements in a TreeSet should not be mutated in a way that changes their ordering while they are stored in the set.

▸ Does TreeSet Sort Automatically?

Yes.

Unlike HashSet and LinkedHashSet, TreeSet maintains its sorted ordering automatically.

Example:

    TreeSet<Integer> set = new TreeSet<>();

    set.add(50);
    set.add(10);
    set.add(30);

Result:

    [10, 30, 50]

▸ Does TreeSet Sort Values?

There is no separate key-value concept.

TreeSet sorts its **elements**.

TreeMap sorts its **keys**.

Remember:

    TreeSet → Sort elements
    TreeMap → Sort keys

▸ Time Complexity

For a Red-Black Tree:

    add()      → O(log n)
    remove()   → O(log n)
    contains() → O(log n)

Iteration:

    O(n)

For interview purposes, remember:

    TreeSet basic operations → O(log n)

▸ Memory

TreeSet uses more memory per element than a simple hash-based structure because each tree node needs references and balancing information.

Conceptually:

    TreeSet
    → Tree nodes
    → Left reference
    → Right reference
    → Parent/reference information
    → Color information

The exact internal representation is an implementation detail.

▸ Is TreeSet Thread-Safe?

No.

TreeSet is not synchronized and is not thread-safe.

For concurrent sorted-set requirements, consider:

    ConcurrentSkipListSet

**Interview one-liner:**

> ConcurrentSkipListSet is a thread-safe sorted Set designed for concurrent access.

▸ TreeSet vs ConcurrentSkipListSet

    TreeSet
    → Sorted
    → Not thread-safe
    → Red-Black Tree

    ConcurrentSkipListSet
    → Sorted
    → Thread-safe
    → Concurrent skip-list based

▸ Range Operations

TreeSet provides range operations through `SortedSet` and `NavigableSet`.

Important methods include:

    subSet()
    headSet()
    tailSet()

Example:

    TreeSet<Integer> set = new TreeSet<>();

    set.add(10);
    set.add(20);
    set.add(30);
    set.add(40);

▸ subSet()

Returns a view of elements within a specified range.

Example:

    set.subSet(20, 40);

Conceptually:

    [20, 30]

The upper bound is exclusive in this overload.

You can also specify whether each boundary is inclusive.

▸ headSet()

Returns a view containing elements less than the specified element.

Example:

    set.headSet(30);

Result:

    [10, 20]

▸ tailSet()

Returns a view containing elements greater than or equal to the specified element.

Example:

    set.tailSet(30);

Result:

    [30, 40]

▸ Views

Methods such as:

    subSet()
    headSet()
    tailSet()
    descendingSet()

can return views backed by the original TreeSet.

Changes through a modifiable view can affect the original set.

▸ Fail-Fast Iterator

TreeSet iterators are generally fail-fast.

Example:

    for (Integer value : set) {
        set.add(100);
    }

A structural modification during iteration can result in:

    ConcurrentModificationException

If removal is required during iteration, use:

    iterator.remove();

Fail-fast behavior is a best-effort mechanism and is not a thread-safety guarantee.

▸ Constructors

Common constructors:

    TreeSet()

    TreeSet(Comparator<? super E> comparator)

    TreeSet(Collection<? extends E> c)

    TreeSet(SortedSet<E> s)

Example:

    TreeSet<Integer> set =
        new TreeSet<>();

Example with Comparator:

    TreeSet<Integer> set =
        new TreeSet<>(Comparator.reverseOrder());

Example from another collection:

    List<Integer> list =
        Arrays.asList(30, 10, 20);

    TreeSet<Integer> set =
        new TreeSet<>(list);

Result:

    [10, 20, 30]

▸ clone()

TreeSet supports cloning.

The clone is a **shallow copy**.

Example:

    TreeSet<Integer> set1 =
        new TreeSet<>();

    set1.add(10);

    TreeSet<Integer> set2 =
        (TreeSet<Integer>) set1.clone();

A new TreeSet object is created, but the elements themselves are not deeply copied.

▸ Copy Constructor

You can create a new TreeSet:

    TreeSet<Integer> set2 =
        new TreeSet<>(set1);

This creates a new set containing the same element references.

It is a shallow copy.

▸ Important Interview Traps

**1. Is TreeSet sorted?**

Yes. It maintains elements in sorted order.

**2. What data structure does TreeSet use?**

A Red-Black Tree through its TreeMap backing.

**3. What is the time complexity of add()?**

O(log n).

**4. Does TreeSet allow duplicates?**

No.

**5. Does TreeSet allow null?**

Natural ordering does not support null.

A Comparator can explicitly define null ordering.

**6. Is TreeSet thread-safe?**

No.

**7. How does TreeSet determine duplicates?**

Using `compareTo()` or `Comparator.compare()`.

**8. Does TreeSet sort values?**

No separate values exist. It sorts its elements.

**9. What is the difference between HashSet and TreeSet?**

> HashSet provides average O(1) operations without guaranteed ordering, while TreeSet maintains sorted order with O(log n) operations.

**10. What is the difference between LinkedHashSet and TreeSet?**

> LinkedHashSet maintains insertion order, while TreeSet maintains sorted order.

**11. Why does TreeSet need Comparable or Comparator?**

> Because it must compare elements to maintain sorted order.

**12. Can TreeSet store custom objects?**

> Yes, if the objects implement Comparable or a Comparator is provided.

**13. What happens if compareTo() returns 0?**

> TreeSet treats the elements as equivalent and does not store them as separate elements.

**14. What is lower()?**

> Returns the greatest element strictly less than the given element.

**15. What is floor()?**

> Returns the greatest element less than or equal to the given element.

**16. What is ceiling()?**

> Returns the smallest element greater than or equal to the given element.

**17. What is higher()?**

> Returns the smallest element strictly greater than the given element.

**18. What is ConcurrentSkipListSet?**

> It is a thread-safe sorted Set designed for concurrent access.

**19. Can TreeSet perform range queries?**

> Yes, using methods such as `subSet()`, `headSet()`, and `tailSet()`.

**20. Is TreeSet backed by TreeMap?**

> Yes. TreeSet uses a TreeMap internally to store its elements.

▸ Quick Revision Table

| Feature | TreeSet |
|---|---|
| Type | Set |
| Duplicate elements | Not allowed |
| Ordering | Sorted |
| Data structure | Red-Black Tree |
| Backed by | TreeMap |
| Average add | O(log n) |
| Average remove | O(log n) |
| Average contains | O(log n) |
| Null | Generally not allowed with natural ordering |
| Thread-safe | No |
| Custom ordering | Yes |
| Navigation methods | Yes |
| Range operations | Yes |
| Main advantage | Unique + sorted elements |

▸ Must Remember

    TreeSet
    → Unique + sorted elements

    Internal backing
    → TreeMap

    TreeMap
    → Red-Black Tree

    add()
    → O(log n)

    remove()
    → O(log n)

    contains()
    → O(log n)

    Duplicate elements
    → Not allowed

    Natural ordering
    → Comparable

    Custom ordering
    → Comparator

    Null
    → Not allowed with natural ordering

    Thread-safe
    → No

    Concurrent sorted Set
    → ConcurrentSkipListSet

    HashSet
    → Unique + no guaranteed order

    LinkedHashSet
    → Unique + insertion order

    TreeSet
    → Unique + sorted order

    lower()
    → Strictly smaller

    floor()
    → Smaller or equal

    ceiling()
    → Greater or equal

    higher()
    → Strictly greater

    Range operations
    → subSet(), headSet(), tailSet()

▸ One-Liner Comparison

    HashSet
    → Unique elements, no guaranteed order

    LinkedHashSet
    → Unique elements, insertion order

    TreeSet
    → Unique elements, sorted order

▸ Final Interview Definition

> TreeSet is a NavigableSet implementation backed by a Red-Black Tree through TreeMap that stores unique elements in sorted order using natural ordering or a provided Comparator.