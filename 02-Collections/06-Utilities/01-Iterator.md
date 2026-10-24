# Iterator

▸ Overview

`Iterator` is an interface in `java.util` used to traverse elements of a collection one by one.

It provides a common way to iterate over collections without exposing their internal data structure.

    import java.util.*;

    List<String> names = new ArrayList<>();
    names.add("John");
    names.add("Alice");
    names.add("Bob");

    Iterator<String> iterator = names.iterator();

    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }

Output:

    John
    Alice
    Bob

Interview Definition:

> Iterator is an interface used to sequentially traverse elements of a collection without exposing the collection's internal implementation.

---

▸ Iterator Interface

The main methods of `Iterator` are:

| Method | Purpose |
|---|---|
| `hasNext()` | Checks whether another element exists |
| `next()` | Returns the next element and moves forward |
| `remove()` | Removes the last element returned by `next()` |
| `forEachRemaining()` | Processes all remaining elements |

---

▸ hasNext()

Checks whether another element is available.

    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }

Important:

- Does not move the iterator.
- Returns `true` if another element exists.
- Returns `false` when iteration is complete.

---

▸ next()

Returns the next element and moves the iterator forward.

    Iterator<String> iterator = names.iterator();

    System.out.println(iterator.next());
    System.out.println(iterator.next());

Important:

- Returns the current next element.
- Advances the iterator.
- Throws `NoSuchElementException` if there are no more elements.

Example:

    List<Integer> list = List.of(10, 20);

    Iterator<Integer> iterator = list.iterator();

    iterator.next();  // 10
    iterator.next();  // 20
    iterator.next();  // NoSuchElementException

---

▸ remove()

Removes the element that was most recently returned by `next()`.

    List<Integer> numbers = new ArrayList<>(
        List.of(10, 20, 30)
    );

    Iterator<Integer> iterator = numbers.iterator();

    while (iterator.hasNext()) {
        Integer value = iterator.next();

        if (value == 20) {
            iterator.remove();
        }
    }

Result:

    [10, 30]

Important:

`iterator.remove()` is the safe way to remove the currently iterated element.

---

▸ remove() Rules

`remove()` must be called after `next()`.

Valid:

    iterator.next();
    iterator.remove();

Invalid:

    iterator.remove();

This can throw:

    IllegalStateException

Calling `remove()` twice after one `next()` is also invalid.

    iterator.next();
    iterator.remove();
    iterator.remove();  // IllegalStateException

Interview Point:

> `Iterator.remove()` removes the last element returned by `next()` and can be safely used during iteration.

---

▸ forEachRemaining()

Processes all elements that have not yet been visited.

    Iterator<String> iterator = names.iterator();

    iterator.next();

    iterator.forEachRemaining(
        name -> System.out.println(name)
    );

If the iterator has already visited one element, `forEachRemaining()` processes only the remaining elements.

---

▸ Iterator State

An Iterator internally maintains a traversal position.

Example:

    List<Integer> numbers = List.of(10, 20, 30);

Initially:

    Iterator
       |
       v
    [10, 20, 30]

After:

    iterator.next();

The iterator has passed `10`.

Conceptually:

    [10, 20, 30]
         ^
         |
      iterator

After another `next()`:

    [10, 20, 30]
             ^
             |
          iterator

Important:

`hasNext()` checks availability but does not move the iterator.

`next()` returns the element and advances the iterator.

---

▸ Iterable vs Iterator

These two are commonly confused.

### Iterable

`Iterable` represents an object that can provide an iterator.

    public interface Iterable<T> {
        Iterator<T> iterator();
    }

### Iterator

`Iterator` performs the actual traversal.

    Iterator<Integer> iterator = list.iterator();

Simple difference:

> Iterable provides an Iterator; Iterator performs the traversal.

Example:

    List<Integer> list = new ArrayList<>();

    Iterator<Integer> iterator = list.iterator();

Here:

    List -> Iterable
    iterator -> Iterator

---

▸ Enhanced For Loop and Iterator

The enhanced `for` loop works conceptually using `Iterable` and `Iterator`.

Code:

    for (String name : names) {
        System.out.println(name);
    }

Conceptually similar to:

    Iterator<String> iterator = names.iterator();

    while (iterator.hasNext()) {
        String name = iterator.next();
        System.out.println(name);
    }

Important:

The compiler handles the iterator creation for an enhanced `for` loop over an `Iterable`.

---

▸ Iterator with ArrayList

    List<Integer> list = new ArrayList<>(
        List.of(10, 20, 30)
    );

    Iterator<Integer> iterator = list.iterator();

    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }

---

▸ Iterator with LinkedList

    LinkedList<Integer> list = new LinkedList<>();

    list.add(10);
    list.add(20);
    list.add(30);

    Iterator<Integer> iterator = list.iterator();

    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }

---

▸ Iterator with HashSet

    Set<Integer> set = new HashSet<>();

    set.add(10);
    set.add(20);
    set.add(30);

    Iterator<Integer> iterator = set.iterator();

    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }

Important:

HashSet does not guarantee iteration order.

---

▸ Iterator with HashMap

`Map` itself does not directly implement `Iterable`.

Therefore, we iterate over its views:

    map.keySet().iterator();

    map.values().iterator();

    map.entrySet().iterator();

Example:

    Map<Integer, String> map = new HashMap<>();

    map.put(1, "A");
    map.put(2, "B");

    Iterator<Map.Entry<Integer, String>> iterator =
        map.entrySet().iterator();

    while (iterator.hasNext()) {
        Map.Entry<Integer, String> entry = iterator.next();

        System.out.println(
            entry.getKey() + " " + entry.getValue()
        );
    }

Interview Point:

> HashMap is not directly Iterable, so we iterate through `keySet()`, `values()`, or `entrySet()`.

---

▸ Iterator with ArrayDeque

    ArrayDeque<Integer> deque = new ArrayDeque<>();

    deque.add(10);
    deque.add(20);
    deque.add(30);

    Iterator<Integer> iterator = deque.iterator();

    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }

---

▸ Iterator with PriorityQueue

    PriorityQueue<Integer> queue =
        new PriorityQueue<>();

    queue.add(30);
    queue.add(10);
    queue.add(20);

    Iterator<Integer> iterator = queue.iterator();

    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }

Important Interview Trap:

> `PriorityQueue`'s iterator does NOT guarantee priority order.

If you need elements in priority order, use:

    while (!queue.isEmpty()) {
        System.out.println(queue.poll());
    }

`poll()` removes the highest-priority element according to the queue's ordering.

---

▸ Structural Modification

A structural modification changes the size or structure of a collection.

Examples:

    list.add(40);
    list.remove(20);
    list.clear();

For an `ArrayList`:

    list.set(0, 100);

`set()` changes an existing value but does not change the size, so it is generally not considered a structural modification.

---

▸ Fail-Fast Iterator

A fail-fast iterator detects certain structural modifications made to a collection outside the iterator while iteration is in progress.

It may throw:

    ConcurrentModificationException

Example:

    List<Integer> list = new ArrayList<>(
        List.of(10, 20, 30)
    );

    Iterator<Integer> iterator = list.iterator();

    while (iterator.hasNext()) {
        Integer value = iterator.next();

        if (value == 20) {
            list.remove(value);
        }
    }

This can result in:

    ConcurrentModificationException

Why?

The collection was structurally modified directly while the iterator was traversing it.

---

▸ How Fail-Fast Works

Many standard collection implementations maintain a modification count conceptually.

For example:

    modCount

When the iterator is created, it conceptually remembers:

    expectedModCount = modCount

During iteration, it can check whether:

    modCount != expectedModCount

If the modification count differs, the iterator may throw:

    ConcurrentModificationException

Conceptually:

    Collection
        |
        | modification
        v
    modCount changes

    Iterator
        |
        | detects mismatch
        v
    ConcurrentModificationException

Important:

Fail-fast behavior is generally implemented as a best-effort detection mechanism.

It should NOT be described as a guaranteed mechanism for detecting every concurrent modification.

Interview One-Liner:

> A fail-fast iterator detects unexpected structural modification during iteration and may throw `ConcurrentModificationException`.

---

▸ Collections Commonly Associated with Fail-Fast Iterators

Common examples include iterators from:

- ArrayList
- LinkedList
- HashSet
- LinkedHashSet
- HashMap
- LinkedHashMap
- TreeSet
- TreeMap
- PriorityQueue

Important:

The exact behavior depends on the collection implementation and iterator.

Do not say:

> Every Java iterator is fail-fast.

That is incorrect.

---

▸ Correct Way to Remove During Iteration

Wrong:

    for (Integer value : list) {
        if (value == 20) {
            list.remove(value);
        }
    }

This can cause:

    ConcurrentModificationException

Correct:

    Iterator<Integer> iterator = list.iterator();

    while (iterator.hasNext()) {
        Integer value = iterator.next();

        if (value == 20) {
            iterator.remove();
        }
    }

Interview One-Liner:

> Use `Iterator.remove()` when you need to remove the current element during iteration.

---

▸ Fail-Safe Iterator

"Fail-safe iterator" is a commonly used interview term, but it is NOT an official Java interface or official iterator category.

It is commonly used to describe an iterator that can continue iteration without throwing `ConcurrentModificationException` when the underlying collection is modified, often because iteration works on a snapshot/copy.

Common examples:

- CopyOnWriteArrayList
- CopyOnWriteArraySet

Example:

    CopyOnWriteArrayList<Integer> list =
        new CopyOnWriteArrayList<>(
            List.of(10, 20, 30)
        );

    Iterator<Integer> iterator = list.iterator();

    list.add(40);

    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }

The iterator works on the snapshot that existed when the iterator was created.

Therefore, the newly added `40` is not seen by that iterator.

Important:

Do not say:

> All concurrent collections have fail-safe iterators.

That is incorrect.

Some concurrent collections use different semantics such as weakly consistent iteration.

---

▸ CopyOnWriteArrayList Iterator

`CopyOnWriteArrayList` is useful when reads are frequent and writes are relatively rare.

When a write occurs, a new internal array is created.

Its iterator operates over a snapshot of the array.

Important:

`CopyOnWriteArrayList` iterator does NOT support `remove()`.

Example:

    CopyOnWriteArrayList<Integer> list =
        new CopyOnWriteArrayList<>(
            List.of(10, 20, 30)
        );

    Iterator<Integer> iterator = list.iterator();

    iterator.next();

    iterator.remove();  // UnsupportedOperationException

Interview Point:

> `CopyOnWriteArrayList` provides snapshot-style iteration, so its iterator does not throw `ConcurrentModificationException` because of later modifications to the list.

---

▸ Weakly Consistent Iterator

Weakly consistent iterators are commonly associated with concurrent collections.

Examples:

- ConcurrentHashMap
- ConcurrentLinkedQueue
- ConcurrentLinkedDeque

They are designed to work safely while the collection is being modified concurrently.

Unlike snapshot iterators, they generally do NOT operate on a fixed copy.

They may reflect some modifications made after the iterator was created.

Therefore:

> Weakly consistent means the iterator can safely traverse while concurrent modifications happen, but it does not guarantee a fixed snapshot of the collection.

---

▸ ConcurrentHashMap Iterator

Example:

    ConcurrentHashMap<Integer, String> map =
        new ConcurrentHashMap<>();

    map.put(1, "A");
    map.put(2, "B");

    Iterator<Map.Entry<Integer, String>> iterator =
        map.entrySet().iterator();

    map.put(3, "C");

    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }

The iterator does not normally throw `ConcurrentModificationException` because of concurrent updates.

However, you should not assume that it must see every modification made after iterator creation.

Interview Point:

> ConcurrentHashMap iterators are weakly consistent, not fail-safe snapshot iterators.

---

▸ Fail-Fast vs Fail-Safe vs Weakly Consistent

| Feature | Fail-Fast | Fail-Safe (Informal) | Weakly Consistent |
|---|---|---|---|
| Concurrent modification | May throw CME | Allowed | Allowed |
| Snapshot/copy | No | Typically yes | No |
| Sees later updates | Not guaranteed | Usually no | May see some |
| `ConcurrentModificationException` | May occur | Typically no | Typically no |
| Official Java terminology | Commonly documented behavior | No | Yes |
| Example | ArrayList | CopyOnWriteArrayList | ConcurrentHashMap |
| Main idea | Detect modification | Iterate over snapshot/copy | Safely iterate during concurrent updates |

Memory Trick:

    Fail-Fast
    = detect modification and may fail

    Fail-Safe
    = commonly means iterate using snapshot/copy

    Weakly Consistent
    = concurrent updates allowed, iteration is not a fixed snapshot

---

▸ Fail-Fast vs Weakly Consistent

Example:

    ArrayList
        |
        | external structural modification
        v
    Iterator
        |
        v
    ConcurrentModificationException may occur

For `ConcurrentHashMap`:

    ConcurrentHashMap
        |
        | concurrent modification
        v
    Iterator
        |
        v
    Continues safely
    May reflect some updates

Important Interview Point:

> Do not call ConcurrentHashMap's iterator "fail-safe" if you want to be technically precise; call it weakly consistent.

---

▸ ListIterator

`ListIterator` is a specialized iterator for `List`.

It extends `Iterator`.

    Iterator
       ^
       |
    ListIterator

It supports:

- Forward traversal
- Backward traversal
- Add
- Remove
- Update
- Index information

It is available only for Lists.

Example:

    List<String> names =
        new ArrayList<>(
            List.of("A", "B", "C")
        );

    ListIterator<String> iterator =
        names.listIterator();

    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }

---

▸ ListIterator Methods

| Method | Purpose |
|---|---|
| `hasNext()` | Checks for next element |
| `next()` | Returns next element |
| `hasPrevious()` | Checks for previous element |
| `previous()` | Returns previous element |
| `nextIndex()` | Returns index of next element |
| `previousIndex()` | Returns index of previous element |
| `remove()` | Removes last returned element |
| `set(E e)` | Replaces last returned element |
| `add(E e)` | Inserts an element |

---

▸ Forward Traversal with ListIterator

    List<Integer> numbers =
        new ArrayList<>(
            List.of(10, 20, 30)
        );

    ListIterator<Integer> iterator =
        numbers.listIterator();

    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }

Output:

    10
    20
    30

---

▸ Backward Traversal with ListIterator

    List<Integer> numbers =
        new ArrayList<>(
            List.of(10, 20, 30)
        );

    ListIterator<Integer> iterator =
        numbers.listIterator(numbers.size());

    while (iterator.hasPrevious()) {
        System.out.println(iterator.previous());
    }

Output:

    30
    20
    10

Important:

A normal `Iterator` only moves forward.

`ListIterator` can move both forward and backward.

---

▸ ListIterator set()

`set()` replaces the last element returned by `next()` or `previous()`.

    List<String> names =
        new ArrayList<>(
            List.of("John", "Bob", "Alice")
        );

    ListIterator<String> iterator =
        names.listIterator();

    while (iterator.hasNext()) {
        String name = iterator.next();

        if (name.equals("Bob")) {
            iterator.set("David");
        }
    }

Result:

    [John, David, Alice]

---

▸ ListIterator add()

`add()` inserts an element at the current iterator position.

    List<String> names =
        new ArrayList<>(
            List.of("John", "Alice")
        );

    ListIterator<String> iterator =
        names.listIterator();

    iterator.next();

    iterator.add("Bob");

Result:

    [John, Bob, Alice]

---

▸ Iterator vs ListIterator

| Feature | Iterator | ListIterator |
|---|---|---|
| Package | java.util | java.util |
| Works with | Collection types | List only |
| Forward traversal | Yes | Yes |
| Backward traversal | No | Yes |
| `remove()` | Yes | Yes |
| `set()` | No | Yes |
| `add()` | No | Yes |
| Index methods | No | Yes |
| Parent interface | None | Iterator |

Interview One-Liner:

> Iterator supports forward traversal, while ListIterator provides bidirectional traversal and modification operations specifically for Lists.

---

▸ Multiple Iterators

A collection can have multiple iterators.

Example:

    List<Integer> list =
        List.of(10, 20, 30);

    Iterator<Integer> iterator1 =
        list.iterator();

    Iterator<Integer> iterator2 =
        list.iterator();

Each iterator maintains its own traversal state.

    iterator1.next();
    iterator1.next();

    iterator2.next();

`iterator1` and `iterator2` can therefore be at different positions.

---

▸ Iterator and Concurrent Collections

Different concurrent collections provide different iterator guarantees.

### ConcurrentHashMap

Uses weakly consistent iterators.

### ConcurrentLinkedQueue

Uses weakly consistent iterators.

### ConcurrentLinkedDeque

Uses weakly consistent iterators.

### CopyOnWriteArrayList

Uses snapshot-style iteration.

Important:

> "Concurrent collection" does not automatically mean "fail-safe iterator."

Always check the iterator semantics of the specific collection.

---

▸ Iterator vs Enumeration

`Enumeration` is an older legacy interface.

`Iterator` is the modern replacement for most traversal needs.

| Feature | Enumeration | Iterator |
|---|---|---|
| Legacy | Yes | No |
| `hasNext()` | No | Yes |
| `nextElement()` | Yes | No |
| `remove()` | No | Yes |
| Used with modern collections | Less common | Yes |

`Enumeration` is mainly associated with legacy classes such as:

    Vector
    Hashtable

Interview One-Liner:

> Iterator is the modern replacement for Enumeration and additionally supports element removal.

---

▸ Iterator vs Enhanced For Loop

Enhanced for loop:

    for (Integer value : list) {
        System.out.println(value);
    }

Iterator:

    Iterator<Integer> iterator = list.iterator();

    while (iterator.hasNext()) {
        Integer value = iterator.next();

        System.out.println(value);
    }

Use an explicit Iterator when you need operations such as:

    iterator.remove();

---

▸ Common Interview Traps

### Trap 1: Does hasNext() move the iterator?

No.

    hasNext()
    = checks only

    next()
    = returns + moves

---

### Trap 2: What happens when next() is called after the end?

It throws:

    NoSuchElementException

---

### Trap 3: Can Iterator remove an element?

Yes.

    iterator.remove();

But it must follow a valid `next()` or `previous()` operation depending on the iterator.

---

### Trap 4: Can Iterator add an element?

No.

`Iterator` does not have `add()`.

`ListIterator` does.

---

### Trap 5: Can Iterator move backward?

No.

`ListIterator` can move backward.

---

### Trap 6: Is every Iterator fail-fast?

No.

Iterator behavior depends on the collection implementation.

---

### Trap 7: Does fail-fast always throw ConcurrentModificationException?

No.

Fail-fast behavior is generally best-effort.

---

### Trap 8: Is Fail-Safe an official Java term?

No.

It is a commonly used interview term.

---

### Trap 9: Is ConcurrentHashMap fail-safe?

Technically, its iterators are described as weakly consistent.

---

### Trap 10: Is CopyOnWriteArrayList iterator fail-fast?

No.

It uses snapshot-style iteration.

---

### Trap 11: Can CopyOnWriteArrayList iterator remove elements?

No.

Its iterator does not support `remove()`.

---

### Trap 12: Is PriorityQueue iterator sorted?

No.

PriorityQueue's iterator does not guarantee priority order.

Use:

    poll()

to repeatedly retrieve elements according to priority.

---

▸ Iterator Complexity

Iterator traversal is generally:

    O(n)

for visiting all elements.

Individual operation complexity depends on the underlying collection.

For example:

    ArrayList iterator.next()
    = O(1)

    LinkedList iterator.next()
    = O(1)

The iterator itself does not make the underlying collection's operations faster.

---

▸ Iterator Internal Working

Conceptually:

    Collection
        |
        | iterator()
        v
    Iterator
        |
        | hasNext()
        v
    Check next element
        |
        | next()
        v
    Return element
        |
        v
    Move traversal position

For fail-fast collections, the iterator may also maintain modification state:

    expectedModCount
            |
            v
    compare with collection's
    current modification count

Mismatch:

    ConcurrentModificationException

---

▸ Why Use Iterator?

Iterator provides:

1. Standard traversal mechanism.
2. Encapsulation of collection internals.
3. Safe removal through `iterator.remove()`.
4. Common traversal API across different collections.
5. Support for controlled iteration.
6. Specialized traversal through `ListIterator`.

---

▸ Iterator Important Classes

| Collection | Iterator Behavior |
|---|---|
| ArrayList | Commonly fail-fast |
| LinkedList | Commonly fail-fast |
| HashSet | Commonly fail-fast |
| LinkedHashSet | Commonly fail-fast |
| HashMap | Commonly fail-fast |
| LinkedHashMap | Commonly fail-fast |
| TreeSet | Commonly fail-fast |
| TreeMap | Commonly fail-fast |
| PriorityQueue | Commonly fail-fast; iterator not sorted |
| CopyOnWriteArrayList | Snapshot-style |
| CopyOnWriteArraySet | Snapshot-style |
| ConcurrentHashMap | Weakly consistent |
| ConcurrentLinkedQueue | Weakly consistent |
| ConcurrentLinkedDeque | Weakly consistent |

---

▸ Quick Comparison

| Topic | Key Point |
|---|---|
| Iterator | Forward traversal |
| ListIterator | Bidirectional List traversal |
| `hasNext()` | Checks availability |
| `next()` | Returns and advances |
| `remove()` | Removes last returned element |
| `forEachRemaining()` | Processes remaining elements |
| Fail-Fast | May throw `ConcurrentModificationException` |
| Fail-Safe | Informal term, commonly snapshot/copy based |
| Weakly Consistent | Safe concurrent traversal without fixed snapshot |
| CopyOnWriteArrayList | Snapshot-style iterator |
| ConcurrentHashMap | Weakly consistent iterator |
| PriorityQueue iterator | Not sorted |

---

▸ Interview Questions

### What is Iterator?

> Iterator is a Java interface used to sequentially traverse elements of a collection.

### What is the difference between Iterator and Iterable?

> Iterable provides an Iterator, while Iterator performs the actual traversal.

### What is the difference between Iterator and ListIterator?

> Iterator supports forward traversal, while ListIterator supports bidirectional traversal and additional modification operations for Lists.

### What happens if next() is called when no element exists?

> `NoSuchElementException` is thrown.

### Can Iterator remove elements?

> Yes, using `iterator.remove()` after a valid `next()` call.

### Can Iterator add elements?

> No. `ListIterator` provides `add()`.

### What is a fail-fast iterator?

> A fail-fast iterator detects unexpected structural modification and may throw `ConcurrentModificationException`.

### Does fail-fast guarantee ConcurrentModificationException?

> No. Fail-fast behavior is generally best-effort.

### What is a fail-safe iterator?

> "Fail-safe" is an informal term commonly used for iterators that can continue despite modifications, often by iterating over a snapshot or copy.

### Give an example of snapshot-style iteration.

> `CopyOnWriteArrayList` and `CopyOnWriteArraySet`.

### What type of iterator does ConcurrentHashMap provide?

> Weakly consistent iterators.

### What is a weakly consistent iterator?

> It safely supports concurrent modification and may reflect some updates, but it does not provide a fixed snapshot.

### Why is CopyOnWriteArrayList useful?

> It is useful when reads are frequent and writes are relatively rare because readers can iterate over a stable snapshot.

### Can CopyOnWriteArrayList iterator remove elements?

> No. Its iterator does not support `remove()`.

### Is PriorityQueue iterator sorted?

> No. Its iterator does not guarantee priority order; use `poll()` for priority order.

### Can HashMap directly provide an Iterator?

> No. HashMap is not directly Iterable; use `keySet()`, `values()`, or `entrySet()`.

---

▸ Most Important Interview One-Liners

> Iterator = forward traversal of a collection.

> ListIterator = bidirectional traversal and modification support for Lists.

> `hasNext()` checks; `next()` returns and advances.

> `Iterator.remove()` safely removes the last element returned by `next()`.

> Fail-fast iterator may throw `ConcurrentModificationException` when the collection is structurally modified unexpectedly during iteration.

> Fail-fast is best-effort, not a guaranteed concurrency detection mechanism.

> "Fail-safe" is an informal interview term, commonly associated with snapshot/copy-based iteration.

> `CopyOnWriteArrayList` uses snapshot-style iteration.

> `ConcurrentHashMap` uses weakly consistent iterators.

> Do not call every concurrent iterator fail-safe; distinguish snapshot-style and weakly consistent behavior.

> `ListIterator` supports both forward and backward traversal.

> PriorityQueue's iterator does not guarantee sorted or priority order.

> For priority order from PriorityQueue, repeatedly use `poll()`.

---

▸ Final Revision Summary

    Iterator
        |
        +-- hasNext()
        +-- next()
        +-- remove()
        +-- forEachRemaining()
        |
        +-- ListIterator
        |      |
        |      +-- previous()
        |      +-- hasPrevious()
        |      +-- set()
        |      +-- add()
        |
        +-- Fail-Fast
        |      |
        |      +-- May throw CME
        |      +-- Best-effort detection
        |
        +-- Snapshot-style
        |      |
        |      +-- CopyOnWriteArrayList
        |      +-- CopyOnWriteArraySet
        |
        +-- Weakly Consistent
               |
               +-- ConcurrentHashMap
               +-- ConcurrentLinkedQueue
               +-- ConcurrentLinkedDeque

Final Interview Definition:

> Iterator is a Java interface for traversing collection elements sequentially; standard collections may provide fail-fast iterators, CopyOnWrite collections provide snapshot-style iteration, and concurrent collections such as ConcurrentHashMap provide weakly consistent iteration.