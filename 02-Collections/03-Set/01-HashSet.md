# HashSet

## ▸ What is HashSet?

`HashSet` is a collection that stores **unique elements** and does not guarantee insertion order.

It is part of the Java Collections Framework and implements the `Set` interface.

### ▸ One-line definition

> **HashSet stores unique elements using hashing and provides average O(1) time for add, remove, and contains operations.**

---

## ▸ Import

    import java.util.HashSet;

---

## ▸ Basic Usage

    HashSet<Integer> set = new HashSet<>();

    set.add(10);
    set.add(20);
    set.add(30);

    System.out.println(set);

Output order is not guaranteed.

---

## ▸ Main Characteristics

- Stores **unique elements**
- Does **not guarantee insertion order**
- Allows **one `null` element**
- Allows duplicate values to be attempted, but duplicates are not stored
- Average **O(1)** for `add()`, `remove()`, and `contains()`
- Uses **hashing internally**
- Not synchronized
- Not thread-safe
- Uses `hashCode()` and `equals()` to determine duplicates
- Backed internally by a `HashMap`

### ▸ Interview one-liner

> **HashSet is a hash-table-based Set that stores unique elements and uses hashCode() and equals() for duplicate detection.**

---

## ▸ HashSet Class Hierarchy

    Object
       ↓
    AbstractCollection
       ↓
    AbstractSet
       ↓
    HashSet
       ↓
    Set

`HashSet` implements:

    Set<E>
    Cloneable
    Serializable

---

## ▸ HashSet Internal Working

The most important interview point:

> **HashSet internally uses a HashMap.**

Conceptually:

    HashSet<E>
        ↓
    HashMap<E, Object>

When you add an element to a `HashSet`, it is stored as a **key** in an internal `HashMap`.

The value is a dummy constant object.

Conceptually:

    HashSet:

    element → dummy value

    HashMap:

    key       value
    ----------------
    "Java"    PRESENT
    "Python"  PRESENT
    "C++"     PRESENT

The actual implementation uses a shared dummy object as the value.

---

## ▸ Why Does HashSet Use HashMap?

`HashMap` already provides:

- Hashing
- Bucket management
- Collision handling
- Resizing
- `hashCode()` handling
- `equals()` comparison

`HashSet` reuses this functionality and only needs to ensure that each key is unique.

### ▸ Interview answer

> **HashSet uses HashMap internally because HashMap already provides efficient hash-based storage and key uniqueness.**

---

## ▸ Adding an Element

Suppose:

    HashSet<String> set = new HashSet<>();

    set.add("Java");

Internally, HashSet effectively performs:

    map.put("Java", PRESENT);

The element `"Java"` becomes a key in the internal HashMap.

---

## ▸ How Does HashSet Prevent Duplicates?

HashSet uses:

    hashCode()
    +
    equals()

to determine whether an element already exists.

Suppose:

    set.add("Java");
    set.add("Java");

For the second `"Java"`:

1. `hashCode()` is calculated.
2. The hash determines the bucket.
3. Existing elements in that bucket are checked.
4. `equals()` determines whether the element is already present.
5. If equal, the new element is not added.

Therefore:

    set = [Java]

---

## ▸ Hashing

Hashing converts an object into an integer hash value using:

    hashCode()

Example:

    String value = "Java";

    int hash = value.hashCode();

The hash value helps determine where the object should be stored.

### ▸ One-line definition

> **Hashing converts an object into a hash value that helps determine its bucket/location in a hash table.**

---

## ▸ What is hashCode()?

`hashCode()` is a method defined in the `Object` class.

    public int hashCode()

It returns an integer hash value for an object.

Hash-based collections use this value to efficiently locate potential matching elements.

---

## ▸ What is equals()?

`equals()` is used to determine whether two objects are logically equal.

    public boolean equals(Object obj)

HashSet uses `equals()` after identifying the appropriate bucket to determine whether an existing element is equal to the new element.

---

## ▸ hashCode() and equals() Contract

This is one of the most frequently asked HashSet interview topics.

### ▸ Rule

> **If two objects are equal according to equals(), they must return the same hashCode().**

Formally:

    a.equals(b) == true
    =>
    a.hashCode() == b.hashCode()

But the reverse is not required:

    a.hashCode() == b.hashCode()

does **not** mean:

    a.equals(b) == true

Different objects can have the same hash code.

This is called a **hash collision**.

---

## ▸ Why Both hashCode() and equals() Are Needed

Suppose HashSet has thousands of objects.

Checking `equals()` against every object would be expensive.

Hashing first narrows the search to a bucket.

Conceptually:

    Object
       ↓
    hashCode()
       ↓
    bucket
       ↓
    equals()
       ↓
    duplicate or new element

### ▸ Easy way to remember

> **hashCode() finds the bucket; equals() confirms the match.**

---

## ▸ Hash Collision

A hash collision occurs when two different objects produce the same hash value.

Example:

    Object A → hashCode = 100
    Object B → hashCode = 100

They go to the same bucket.

But they may still be different objects:

    A.equals(B) == false

HashSet can store both.

### ▸ Interview one-liner

> **A collision occurs when different objects map to the same hash bucket; HashSet resolves it using equality comparison.**

---

## ▸ Collision Handling

HashMap, which backs HashSet, handles collisions by storing multiple entries in the same bucket.

Modern Java can use:

    Linked List
    or
    Red-Black Tree

for entries within a bucket, depending on the conditions.

### ▸ Important

You do not need to memorize every internal threshold unless specifically asked.

For interviews, remember:

> **HashMap buckets can use linked structures and can be treeified into Red-Black Trees under high collision conditions.**

---

## ▸ HashSet Duplicate Detection Flow

Suppose:

    set.add("Java");

The general process is:

    "Java"
       ↓
    hashCode()
       ↓
    hash calculation
       ↓
    bucket/index
       ↓
    check existing entry
       ↓
    equals()
       ↓
    duplicate?
      ↙   ↘
    Yes    No
     ↓      ↓
    reject  add

### ▸ One-line approach

> **HashSet first uses hashCode() to locate the bucket and then equals() to check whether an equal element already exists.**

---

## ▸ Does HashSet Use hashCode() or equals()?

It uses **both**.

The usual sequence is:

    hashCode()
        ↓
    bucket
        ↓
    equals()

### ▸ Interview answer

> **HashSet uses hashCode() to locate the bucket and equals() to determine equality among candidates in that bucket.**

---

## ▸ What Happens If hashCode() Is Same but equals() Is False?

Both objects can be stored.

Example:

    A.hashCode() == B.hashCode()
    A.equals(B) == false

Result:

    A
    B

Both can exist in the HashSet.

### ▸ Important

Same hash code does **not** mean objects are equal.

---

## ▸ What Happens If equals() Is True but hashCode() Is Different?

This violates the `equals()`/`hashCode()` contract.

Example:

    A.equals(B) == true
    A.hashCode() != B.hashCode()

HashSet may place them in different buckets and fail to detect them as duplicates.

### ▸ Interview answer

> **If equals() is overridden, hashCode() must also be overridden consistently.**

---

## ▸ Custom Object in HashSet

Consider:

    class Student {
        int id;
        String name;

        Student(int id, String name) {
            this.id = id;
            this.name = name;
        }
    }

Now:

    HashSet<Student> students = new HashSet<>();

    students.add(new Student(1, "John"));
    students.add(new Student(1, "John"));

Without overriding `equals()` and `hashCode()`, these two objects are generally treated as different objects.

Why?

Because the default `Object.equals()` compares object identity.

---

## ▸ Correct equals() and hashCode()

For logical equality, override both:

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

Now:

    students.add(new Student(1, "John"));
    students.add(new Student(1, "John"));

Only one logical duplicate is stored.

---

## ▸ Why Override Both?

If you override only:

    equals()

but not:

    hashCode()

you can violate the contract required by hash-based collections.

Similarly, overriding only `hashCode()` is not enough to define logical equality.

### ▸ Interview rule

> **Always override equals() and hashCode() together when defining logical equality for objects stored in HashSet or used as HashMap keys.**

---

## ▸ What Does add() Return?

`HashSet.add()` returns:

    true

if the element was actually added.

It returns:

    false

if the element already exists.

Example:

    HashSet<Integer> set = new HashSet<>();

    System.out.println(set.add(10)); // true
    System.out.println(set.add(10)); // false

---

## ▸ add() Example

    HashSet<String> set = new HashSet<>();

    boolean first = set.add("Java");
    boolean second = set.add("Java");

Results:

    first  = true
    second = false

Only one `"Java"` exists in the set.

---

## ▸ Does HashSet Maintain Insertion Order?

No.

HashSet does **not guarantee insertion order**.

Example:

    set.add(30);
    set.add(10);
    set.add(20);

You should not expect:

    [30, 10, 20]

The iteration order depends on the internal hashing and bucket arrangement.

### ▸ Interview answer

> **HashSet does not guarantee insertion order.**

If insertion order is required, use:

    LinkedHashSet

---

## ▸ HashSet vs LinkedHashSet

| Feature | HashSet | LinkedHashSet |
|---|---|---|
| Unique elements | Yes | Yes |
| Insertion order | Not guaranteed | Maintained |
| Hashing | Yes | Yes |
| Performance | Generally slightly better | Slightly more overhead |
| Null | One allowed | One allowed |

### ▸ Easy memory trick

> **HashSet = uniqueness, LinkedHashSet = uniqueness + insertion order.**

---

## ▸ HashSet vs TreeSet

| Feature | HashSet | TreeSet |
|---|---|---|
| Ordering | No guaranteed order | Sorted order |
| Internal structure | HashMap | TreeMap-like tree structure |
| Average add | O(1) | O(log n) |
| Average search | O(1) | O(log n) |
| Null | One null allowed | Generally does not allow null with natural ordering |
| Duplicates | Not allowed | Not allowed |

### ▸ Easy memory trick

> **HashSet = hashing, TreeSet = sorting.**

---

## ▸ HashSet vs ArrayList

| Feature | HashSet | ArrayList |
|---|---|---|
| Duplicates | No | Yes |
| Order | Not guaranteed | Insertion order |
| Index access | No | Yes |
| `contains()` average | O(1) | O(n) |
| `add()` average | O(1) | O(1) amortized |
| Null | One allowed | Multiple allowed |

### ▸ When to use?

Use `HashSet` when:

> **You mainly need uniqueness and fast lookup.**

Use `ArrayList` when:

> **You need ordered elements and index-based access.**

---

## ▸ Null in HashSet

`HashSet` allows **one null element**.

Example:

    HashSet<String> set = new HashSet<>();

    set.add(null);
    set.add(null);

The second `null` is considered a duplicate.

Result:

    [null]

### ▸ Interview answer

> **HashSet allows one null because null is treated as a single unique element.**

---

## ▸ Duplicate null

Example:

    set.add(null);
    set.add(null);
    set.add(null);

Only one null is stored.

    [null]

---

## ▸ HashSet Does Not Support Index Access

This does not exist:

    set.get(0);

HashSet does not provide index-based access.

If you need indexed access, use:

    ArrayList

---

## ▸ Iterating HashSet

Using enhanced for loop:

    for (Integer value : set) {
        System.out.println(value);
    }

Using Iterator:

    Iterator<Integer> iterator = set.iterator();

    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }

---

## ▸ HashSet Iterator

`HashSet` provides an iterator through the `Set`/`Collection` interfaces.

Example:

    Iterator<Integer> iterator = set.iterator();

    while (iterator.hasNext()) {
        Integer value = iterator.next();
        System.out.println(value);
    }

---

## ▸ Removing While Iterating

Do not directly modify the HashSet while using a normal iterator.

Incorrect:

    for (Integer value : set) {
        if (value == 10) {
            set.remove(value);
        }
    }

This can result in:

    ConcurrentModificationException

Use the iterator's `remove()` instead:

    Iterator<Integer> iterator = set.iterator();

    while (iterator.hasNext()) {
        Integer value = iterator.next();

        if (value == 10) {
            iterator.remove();
        }
    }

---

## ▸ HashSet is Not Thread-Safe

`HashSet` is not synchronized.

Multiple threads modifying the same HashSet concurrently can cause problems.

If synchronization is required, one option is:

    Set<Integer> set =
        Collections.synchronizedSet(new HashSet<>());

For highly concurrent use cases, choose a collection designed for concurrency based on the required operations.

---

## ▸ HashSet Capacity

HashSet internally uses a HashMap, so its capacity behavior is tied to the underlying hash table.

You can specify an initial capacity:

    HashSet<Integer> set = new HashSet<>(100);

This creates a HashSet with a requested initial capacity.

### ▸ Important

Capacity is not the same thing as the number of elements currently stored.

Example:

    HashSet<Integer> set = new HashSet<>(100);

This does not mean:

    100 elements

are already stored.

---

## ▸ Load Factor

Hash-based collections use a **load factor** to determine when resizing should occur.

Default HashMap load factor:

    0.75

Conceptually:

    threshold = capacity × load factor

When the number of entries reaches the threshold, the hash table is resized.

### ▸ Example

If:

    capacity = 16
    load factor = 0.75

Then approximately:

    threshold = 16 × 0.75
              = 12

When the threshold is reached, resizing can occur.

---

## ▸ Why Does HashSet Resize?

As more elements are added, buckets become more crowded.

Resizing increases the number of buckets and helps maintain efficient lookup performance.

Conceptually:

    More elements
         ↓
    More collisions
         ↓
    Resize table
         ↓
    Redistribute entries
         ↓
    Better distribution

---

## ▸ Rehashing

When the underlying HashMap resizes, entries need to be redistributed according to the new table size.

This process is commonly referred to as **rehashing**.

### ▸ Interview one-liner

> **Rehashing redistributes existing entries when the hash table grows so that elements are placed according to the new table capacity.**

---

## ▸ HashSet Time Complexity

| Operation | Average | Worst Case |
|---|---:|---:|
| `add()` | O(1) | O(log n) with tree bins under modern HashMap conditions |
| `remove()` | O(1) | O(log n) with tree bins |
| `contains()` | O(1) | O(log n) with tree bins |
| `size()` | O(1) | O(1) |

The commonly expected interview answer is:

> **HashSet provides average O(1) add, remove, and contains.**

Worst-case behavior can depend on collisions and the underlying implementation.

---

## ▸ Why is HashSet O(1) Average?

HashSet uses hashing.

Instead of scanning every element:

    element 1
    element 2
    element 3
    ...
    element n

it calculates a hash and goes directly to a likely bucket.

Conceptually:

    hashCode()
        ↓
    bucket
        ↓
    candidate elements
        ↓
    equals()

Therefore the average lookup is O(1).

---

## ▸ What Happens During contains()?

Example:

    set.contains("Java");

Conceptually:

    "Java"
       ↓
    hashCode()
       ↓
    calculate bucket
       ↓
    search bucket
       ↓
    equals()
       ↓
    found / not found

### ▸ One-line approach

> **contains() uses hashCode() to locate the bucket and equals() to verify the matching element.**

---

## ▸ What Happens During remove()?

Example:

    set.remove("Java");

Conceptually:

    "Java"
       ↓
    hashCode()
       ↓
    bucket
       ↓
    equals()
       ↓
    locate element
       ↓
    remove

Average complexity:

    O(1)

---

## ▸ HashSet and Mutable Objects

This is an important interview edge case.

Suppose an object is added to a HashSet:

    Student student = new Student(1, "John");

    set.add(student);

Later, a field used by `hashCode()` is changed:

    student.id = 2;

Now the object's hash code may change.

The object may remain physically inside the old bucket, while a lookup calculates a different bucket.

This can cause:

    set.contains(student)

to return:

    false

even though the object is still present in the set.

### ▸ Interview rule

> **Do not mutate fields used by equals() and hashCode() while an object is stored in a HashSet.**

---

## ▸ Mutable Object Example

Conceptually:

    Student s = new Student(1, "John");

    set.add(s);

    s.id = 2;

Now:

    old hashCode → bucket A
    new hashCode → bucket B

The object may still physically reside in bucket A.

A lookup using the new hash may search bucket B and fail.

---

## ▸ Why Immutable Keys Are Safer

Immutable objects are safer for hash-based collections because their state does not change after insertion.

Common examples:

    String
    Integer
    Long

This is one reason immutable objects work well as HashSet elements and HashMap keys.

---

## ▸ HashSet clone()

HashSet implements `Cloneable`.

Example:

    HashSet<Integer> set1 = new HashSet<>();

    set1.add(10);
    set1.add(20);

    HashSet<Integer> set2 =
        (HashSet<Integer>) set1.clone();

`clone()` creates a **shallow copy**.

The new HashSet object is different, but the referenced elements are shared.

### ▸ Interview answer

> **HashSet.clone() creates a shallow copy of the set.**

---

## ▸ HashSet Copy Constructor

You can also create a copy using:

    HashSet<Integer> set2 =
        new HashSet<>(set1);

This creates a new HashSet containing the same element references.

It is also a **shallow copy**.

### ▸ clone() vs Copy Constructor

| Feature | `clone()` | Copy Constructor |
|---|---|---|
| New HashSet object | Yes | Yes |
| Shallow copy | Yes | Yes |
| Type casting | Required with `clone()` | No |
| Preferred for clarity | Usually no | Usually yes |

---

## ▸ HashSet and Generics

HashSet supports generics.

Example:

    HashSet<String> names = new HashSet<>();

    names.add("John");
    names.add("David");

This provides compile-time type safety.

Without generics:

    HashSet set = new HashSet();

the set can contain different types.

---

## ▸ HashSet with Custom Objects

Example:

    class Employee {
        int id;
        String name;
    }

You can store:

    HashSet<Employee> employees = new HashSet<>();

But if logical uniqueness is based on fields such as `id`, you should override:

    equals()
    hashCode()

Otherwise, different object instances may be treated as different elements.

---

## ▸ Common Interview Scenario

Question:

> Two Student objects have the same id and name. Will HashSet store both?

Answer:

> **Not necessarily; it depends on equals() and hashCode(). If they are implemented consistently so the objects are logically equal, HashSet stores only one. Otherwise, both may be stored.**

---

## ▸ Common Interview Scenario: Only hashCode()

Question:

> What happens if I override hashCode() but not equals()?

Answer:

> Objects may have the same hash and enter the same bucket, but default equals() may still consider them different, so both can be stored.

---

## ▸ Common Interview Scenario: Only equals()

Question:

> What happens if I override equals() but not hashCode()?

Answer:

> Equal objects may produce different hash codes and therefore be placed in different buckets, causing HashSet to fail to recognize them as duplicates.

---

## ▸ Common Interview Scenario: Same Hash Code

Question:

> Can two different objects have the same hashCode()?

Yes.

This is called a:

    Hash collision

And:

    same hashCode()
    ≠
    same object
    ≠
    equals() must be true

---

## ▸ Common Interview Scenario: HashSet Order

Question:

> Does HashSet maintain insertion order?

Answer:

> **No. HashSet does not guarantee insertion order.**

Use:

    LinkedHashSet

when insertion order must be maintained.

---

## ▸ Common Interview Scenario: Sorting HashSet

Question:

> Does HashSet sort elements?

Answer:

> **No. HashSet does not maintain sorted order.**

For sorted unique elements, use:

    TreeSet

---

## ▸ Common Interview Scenario: Null

Question:

> How many null values can HashSet store?

Answer:

> **One null value.**

---

## ▸ Common Interview Scenario: Duplicates

Question:

> What happens when you add a duplicate element?

Answer:

> `add()` returns `false` and the set remains unchanged.

---

## ▸ Common Interview Scenario: Thread Safety

Question:

> Is HashSet thread-safe?

Answer:

> **No. HashSet is not synchronized or thread-safe.**

---

## ▸ Common Interview Scenario: Internal Data Structure

Question:

> What data structure does HashSet use internally?

Answer:

> **HashSet internally uses a HashMap.**

---

## ▸ Common Interview Scenario: HashSet vs HashMap

Question:

> What is the difference between HashSet and HashMap?

Answer:

> **HashSet stores unique elements, while HashMap stores key-value pairs; HashSet internally uses a HashMap.**

---

## ▸ HashSet vs HashMap

| Feature | HashSet | HashMap |
|---|---|---|
| Stores | Elements | Key-value pairs |
| Duplicate keys/elements | No duplicate elements | No duplicate keys |
| Null | One null element | One null key + multiple null values |
| Internal | Uses HashMap | Hash table |
| Main methods | `add()`, `remove()`, `contains()` | `put()`, `get()`, `remove()` |

---

## ▸ HashSet vs LinkedHashSet vs TreeSet

| Feature | HashSet | LinkedHashSet | TreeSet |
|---|---|---|---|
| Unique | Yes | Yes | Yes |
| Order | None guaranteed | Insertion order | Sorted order |
| Average add | O(1) | O(1) | O(log n) |
| Null | One allowed | One allowed | Generally not allowed with natural ordering |
| Internal concept | Hashing | Hashing + linked structure | Tree |

### ▸ Easy memory trick

> **HashSet = unique + no order**

> **LinkedHashSet = unique + insertion order**

> **TreeSet = unique + sorted order**

---

## ▸ HashSet Constructors

Common constructors:

    HashSet<Integer> set1 = new HashSet<>();

    HashSet<Integer> set2 = new HashSet<>(20);

    HashSet<Integer> set3 = new HashSet<>(20, 0.75f);

    HashSet<Integer> set4 = new HashSet<>(anotherSet);

### ▸ Constructor meanings

`new HashSet<>()`

> Creates an empty HashSet with default capacity/load factor behavior.

`new HashSet<>(initialCapacity)`

> Creates a HashSet with the specified initial capacity.

`new HashSet<>(initialCapacity, loadFactor)`

> Creates a HashSet with specified initial capacity and load factor.

`new HashSet<>(collection)`

> Creates a HashSet containing the elements of the specified collection.

---

## ▸ HashSet Capacity vs Size

These are different concepts.

### ▸ Size

Number of elements currently stored:

    set.size()

### ▸ Capacity

Number of buckets available in the underlying hash table.

You generally do not access the internal HashSet capacity directly through the public API.

### ▸ Interview point

> **size = number of stored elements; capacity = available hash-table bucket capacity.**

---

## ▸ Fail-Fast Iterator

HashSet iterators are generally **fail-fast**.

If the HashSet is structurally modified after creating an iterator, except through the iterator's own `remove()`, the iterator may throw:

    ConcurrentModificationException

Example:

    Iterator<Integer> iterator = set.iterator();

    set.add(100);

    iterator.next();

This may throw:

    ConcurrentModificationException

### ▸ Important

Fail-fast behavior is a best-effort mechanism, not a synchronization guarantee.

---

## ▸ HashSet and Concurrent Modification

For concurrent access, do not assume HashSet's fail-fast iterator makes it thread-safe.

Fail-fast behavior is designed to detect certain structural modifications during iteration.

It does not provide thread safety.

---

## ▸ HashSet and `Objects.hash()`

When implementing `hashCode()` for custom objects, `Objects.hash()` can be used.

Example:

    @Override
    public int hashCode() {
        return Objects.hash(id, name);
    }

Import:

    import java.util.Objects;

---

## ▸ HashSet with String

`String` already correctly overrides:

    equals()
    hashCode()

Therefore:

    HashSet<String> set = new HashSet<>();

    set.add(new String("Java"));
    set.add(new String("Java"));

Only one element is stored because the two String objects are logically equal.

---

## ▸ HashSet with Integer

Wrapper classes such as `Integer` also implement logical equality and appropriate hashing.

Example:

    HashSet<Integer> set = new HashSet<>();

    set.add(10);
    set.add(10);

Only one `10` is stored.

---

## ▸ Important HashSet Internal Flow

Remember this flow for interviews:

    add(element)
          ↓
    hashCode()
          ↓
    hash calculation
          ↓
    bucket/index
          ↓
    existing elements?
          ↓
       equals()
       ↙     ↘
    equal   different
      ↓         ↓
    reject      add

### ▸ One-line memory trick

> **HashSet = hashCode() to find the bucket + equals() to confirm uniqueness.**

---

## ▸ Important HashSet Interview Traps

### ▸ Trap 1: Does HashSet allow duplicates?

No.

---

### ▸ Trap 2: Does HashSet allow null?

Yes, one null.

---

### ▸ Trap 3: Does HashSet maintain insertion order?

No.

---

### ▸ Trap 4: Does HashSet sort elements?

No.

---

### ▸ Trap 5: Is HashSet thread-safe?

No.

---

### ▸ Trap 6: What does HashSet use internally?

HashMap.

---

### ▸ Trap 7: What does HashSet use to detect duplicates?

`hashCode()` and `equals()`.

---

### ▸ Trap 8: Does same hashCode() mean objects are equal?

No.

---

### ▸ Trap 9: If equals() is overridden, should hashCode() also be overridden?

Yes.

---

### ▸ Trap 10: What does add() return for a duplicate?

`false`.

---

### ▸ Trap 11: Does HashSet have get(index)?

No.

---

### ▸ Trap 12: Can mutable objects cause problems in HashSet?

Yes, if fields used by `equals()`/`hashCode()` are changed after insertion.

---

### ▸ Trap 13: Is HashSet always O(1)?

No.

> Average add/remove/contains is O(1); worst-case behavior depends on collisions and implementation.

---

### ▸ Trap 14: Is fail-fast iteration thread-safe?

No.

---

## ▸ Interview Q&A

### ▸ Q1. What is HashSet?

> HashSet is a Set implementation that stores unique elements using hashing.

---

### ▸ Q2. How does HashSet maintain uniqueness?

> It uses `hashCode()` to locate the bucket and `equals()` to determine whether an equal element already exists.

---

### ▸ Q3. What does HashSet use internally?

> HashSet internally uses a HashMap.

---

### ▸ Q4. Why does HashSet use HashMap?

> HashMap already provides efficient hash-based storage, collision handling, and key uniqueness.

---

### ▸ Q5. Does HashSet allow duplicates?

> No, duplicate elements are not stored.

---

### ▸ Q6. Does HashSet allow null?

> Yes, it allows one null element.

---

### ▸ Q7. Does HashSet maintain insertion order?

> No, HashSet does not guarantee insertion order.

---

### ▸ Q8. Which Set should be used if insertion order is required?

> `LinkedHashSet`.

---

### ▸ Q9. Which Set should be used if sorted order is required?

> `TreeSet`.

---

### ▸ Q10. Is HashSet thread-safe?

> No, HashSet is not synchronized.

---

### ▸ Q11. What is the average time complexity of HashSet add(), remove(), and contains()?

> O(1) average.

---

### ▸ Q12. What happens when a duplicate is added?

> `add()` returns `false` and the set remains unchanged.

---

### ▸ Q13. What is a hash collision?

> A hash collision occurs when different objects map to the same hash bucket.

---

### ▸ Q14. Can two different objects have the same hashCode()?

> Yes, different objects can have the same hash code.

---

### ▸ Q15. If two objects have the same hashCode(), are they equal?

> No. Same hash code does not guarantee equality.

---

### ▸ Q16. What is the contract between equals() and hashCode()?

> If two objects are equal according to `equals()`, they must have the same `hashCode()`.

---

### ▸ Q17. Why must equals() and hashCode() be overridden together?

> Because hash-based collections depend on both methods to correctly locate and identify logically equal objects.

---

### ▸ Q18. What happens if equals() is overridden but hashCode() is not?

> Equal objects can end up in different buckets, causing incorrect duplicate detection.

---

### ▸ Q19. What happens if hashCode() is overridden but equals() is not?

> Objects with the same hash can still be considered different because equality may remain based on Object's implementation.

---

### ▸ Q20. Can HashSet access elements using an index?

> No, HashSet does not provide index-based access.

---

### ▸ Q21. Why is HashSet lookup O(1) average?

> Hashing allows the collection to directly locate the likely bucket instead of scanning all elements.

---

### ▸ Q22. What happens internally when add() is called?

> HashSet calculates the element's hash, identifies a bucket, checks existing entries using equality, and adds the element if no equal element exists.

---

### ▸ Q23. What happens when HashSet becomes full?

> When the load-factor threshold is reached, the underlying hash table can resize and redistribute entries.

---

### ▸ Q24. What is the default load factor of the underlying HashMap?

> `0.75`.

---

### ▸ Q25. Why does HashSet use a load factor?

> It balances memory usage against the number of collisions and lookup performance.

---

### ▸ Q26. Can HashSet store mutable objects?

> Yes, but changing fields involved in `equals()` or `hashCode()` after insertion can make the object difficult to find or remove.

---

### ▸ Q27. What is HashSet's clone behavior?

> `clone()` creates a shallow copy of the HashSet.

---

### ▸ Q28. What is the difference between HashSet and ArrayList?

> HashSet stores unique elements with hash-based lookup, while ArrayList allows duplicates and provides index-based access.

---

### ▸ Q29. What is the difference between HashSet and HashMap?

> HashSet stores unique elements, while HashMap stores key-value pairs; HashSet internally uses a HashMap.

---

### ▸ Q30. What is the difference between HashSet and LinkedHashSet?

> HashSet does not guarantee order, while LinkedHashSet maintains insertion order.

---

### ▸ Q31. What is the difference between HashSet and TreeSet?

> HashSet uses hashing with no guaranteed ordering, while TreeSet maintains sorted order using a tree-based structure.

---

### ▸ Q32. What happens if you modify a HashSet while iterating?

> Structural modification can cause `ConcurrentModificationException`; use the iterator's `remove()` when removal is needed during iteration.

---

## ▸ Most Important Internal Concept

For interviews, remember:

    HashSet
       ↓
    HashMap
       ↓
    hashCode()
       ↓
    bucket
       ↓
    equals()
       ↓
    unique element

This is the **core HashSet concept**.

---

## ▸ One-Liner Approach

> **For HashSet, hashCode() finds the bucket and equals() checks whether the element already exists.**

---

## ▸ Quick Comparison

| Collection | Duplicates | Order | Sorted | Average Lookup |
|---|---|---|---|---|
| `ArrayList` | Yes | Insertion | No | O(n) |
| `HashSet` | No | No guarantee | No | O(1) |
| `LinkedHashSet` | No | Insertion | No | O(1) |
| `TreeSet` | No | Sorted | Yes | O(log n) |

---

## ▸ Must Remember

- `HashSet` stores **unique elements**.
- `HashSet` implements `Set`.
- `HashSet` internally uses **HashMap**.
- Elements are stored as **HashMap keys**.
- HashSet uses **hashCode() + equals()**.
- `hashCode()` helps locate the bucket.
- `equals()` confirms equality.
- Same hash code does **not** mean equal objects.
- Equal objects **must** have the same hash code.
- `HashSet` allows **one null**.
- `HashSet` does **not guarantee insertion order**.
- `HashSet` does **not sort** elements.
- `HashSet` does **not provide index-based access**.
- `HashSet` is **not thread-safe**.
- Average `add()`, `remove()`, and `contains()` are **O(1)**.
- Hash collisions can occur.
- High collision buckets can be treeified in modern HashMap implementations.
- Default HashMap load factor is **0.75**.
- HashSet can resize when the threshold is reached.
- Avoid modifying fields used by `equals()`/`hashCode()` while an object is stored in a HashSet.
- `clone()` creates a **shallow copy**.
- Use `LinkedHashSet` when insertion order is required.
- Use `TreeSet` when sorted order is required.

---

## ▸ Final Interview Definition

> **HashSet is a hash-based Set implementation that stores unique elements, uses a HashMap internally, and relies on hashCode() and equals() for efficient duplicate detection.**