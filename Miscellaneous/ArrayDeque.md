
## 1. Overview

`ArrayDeque` is a resizable-array implementation of the `Deque` interface.

It can be used as:

- Queue → FIFO
- Stack → LIFO
- Deque → insertion/removal from both ends

### 2. Basic Example

    Deque<Integer> deque = new ArrayDeque<>();

    deque.addFirst(10);
    deque.addLast(20);

    System.out.println(deque);

Output:

    [10, 20]

---

## 3. Important Characteristics

| Feature | ArrayDeque |
|---|---|
| Implementation | Resizable circular array |
| Interface | `Deque` |
| Maintains insertion order | Yes |
| Allows duplicates | Yes |
| Allows `null` | No |
| Thread-safe | No |
| Random access | No |
| FIFO support | Yes |
| LIFO support | Yes |
| Double-ended operations | Yes |
| Dynamically grows | Yes |
| Iterator | Yes |
| Descending iterator | Yes |
| Cloneable | Yes |

---

# 4. Creating ArrayDeque

### Using Deque reference

    Deque<Integer> deque = new ArrayDeque<>();

This is generally preferred because we program to the interface.

### Using ArrayDeque reference

    ArrayDeque<Integer> deque = new ArrayDeque<>();

### With Initial Capacity

    Deque<Integer> deque = new ArrayDeque<>(20);

The initial capacity is not a fixed maximum size.

The deque can grow when more elements are added.

### From Another Collection

    List<Integer> list = List.of(10, 20, 30);

    Deque<Integer> deque = new ArrayDeque<>(list);

    System.out.println(deque);

Output:

    [10, 20, 30]

---

# 5. Why ArrayDeque?

`ArrayDeque` is generally preferred over:

- `Stack` for stack operations
- `LinkedList` for normal queue/deque operations

Reasons:

- Uses an array instead of individual linked nodes
- Lower memory overhead
- Better cache locality
- Efficient insertion/removal from both ends

### Interview Answer

> `ArrayDeque` is generally preferred for stack and deque operations because its array-based implementation has lower overhead and better cache locality than linked-node implementations.

---

# 6. ArrayDeque as a Queue

A Queue follows:

> FIFO — First In, First Out

Example:

    Deque<Integer> queue = new ArrayDeque<>();

    queue.offer(10);
    queue.offer(20);
    queue.offer(30);

    System.out.println(queue);

Output:

    [10, 20, 30]

Remove from the front:

    int value = queue.poll();

    System.out.println(value);

Output:

    10

Remaining:

    [20, 30]

### Queue Methods

    queue.offer(10);  // Add at rear
    queue.poll();     // Remove from front
    queue.peek();     // View front

### One-Line Approach

> Add elements at the rear and remove elements from the front to implement FIFO.

---

# 7. ArrayDeque as a Stack

A Stack follows:

> LIFO — Last In, First Out

Example:

    Deque<Integer> stack = new ArrayDeque<>();

    stack.push(10);
    stack.push(20);
    stack.push(30);

    System.out.println(stack);

Logical stack:

    30 ← top
    20
    10

Remove the top element:

    int value = stack.pop();

    System.out.println(value);

Output:

    30

### Stack Methods

    stack.push(10);  // Add to top
    stack.pop();     // Remove from top
    stack.peek();    // View top

### One-Line Approach

> Push and pop from the same end to implement LIFO.

---

# 8. Why Use ArrayDeque Instead of Stack?

Java's `Stack` class is a legacy class that extends `Vector`.

For normal stack usage, prefer:

    Deque<Integer> stack = new ArrayDeque<>();

instead of:

    Stack<Integer> stack = new Stack<>();

### Important Difference

| Feature | ArrayDeque | Stack |
|---|---|---|
| Modern choice | Yes | No |
| Implementation | Resizable array | Vector |
| Thread-safe | No | Yes |
| Synchronization | No | Synchronized |
| Allows null | No | Yes |
| Stack operations | Yes | Yes |

### Interview Answer

> `ArrayDeque` is generally preferred over `Stack` because 
- `Stack` is a legacy synchronized class, while `ArrayDeque` is a modern implementation.
- `Stack` is synchronized and thread-safe, but this adds overhead; `ArrayDeque` is unsynchronized and faster for normal single-threaded use.
- `Stack` allows `null`, but `ArrayDeque` rejects `null` so methods like `peek()`/`poll()` can return `null` when deque is empty.

---

# 9. ArrayDeque as a Deque

Deque means:

> Double Ended Queue

It allows insertion and removal from both ends.

Example:

    Deque<Integer> deque = new ArrayDeque<>();

    deque.addFirst(20);
    deque.addLast(30);
    deque.addFirst(10);
    deque.addLast(40);

    System.out.println(deque);

Output:

    [10, 20, 30, 40]

---

# 10. Adding Elements

## add()

Adds an element at the rear.

    deque.add(10);

For a deque, this is equivalent to:

    deque.addLast(10);

---

## offer()

Adds an element at the rear.

    deque.offer(10);

---

## addFirst()

Adds an element at the front.

    deque.addFirst(10);

Example:

    Deque<Integer> deque = new ArrayDeque<>();

    deque.add(20);
    deque.addFirst(10);

    System.out.println(deque);

Output:

    [10, 20]

---

## addLast()

Adds an element at the rear.

    deque.addLast(30);

---

## offerFirst()

Adds an element at the front.

    deque.offerFirst(10);

---

## offerLast()

Adds an element at the rear.

    deque.offerLast(30);

---

# 11. Removing Elements

## remove()

Removes the first element.

    deque.remove();

Throws `NoSuchElementException` if the deque is empty.

---

## poll()

Removes the first element.

    deque.poll();

Returns `null` if the deque is empty.

---

## removeFirst()

Removes the first element.

    deque.removeFirst();

---

## removeLast()

Removes the last element.

    deque.removeLast();

---

## pollFirst()

Removes the first element.

    deque.pollFirst();

---

## pollLast()

Removes the last element.

    deque.pollLast();

---

# 12. Viewing Elements

## element()

Returns the first element without removing it.

    deque.element();

Throws `NoSuchElementException` if empty.

---

## peek()

Returns the first element without removing it.

    deque.peek();

Returns `null` if empty.

---

## getFirst()

Returns the first element.

    deque.getFirst();

Throws `NoSuchElementException` if empty.

---

## getLast()

Returns the last element.

    deque.getLast();

Throws `NoSuchElementException` if empty.

---

## peekFirst()

Returns the first element.

    deque.peekFirst();

Returns `null` if empty.

---

## peekLast()

Returns the last element.

    deque.peekLast();

Returns `null` if empty.

---

# 13. Important Method Differences

## add() vs offer()

Both normally add an element to the rear.

| Method | Failure behavior |
|---|---|
| `add()` | Throws exception |
| `offer()` | Returns `false` |

For `ArrayDeque`, the deque normally grows dynamically, so these methods generally succeed.

### Remember

> `add()` uses exception-based failure handling, while `offer()` uses a return value.

---

## remove() vs poll()

| Method | Empty deque |
|---|---|
| `remove()` | Throws `NoSuchElementException` |
| `poll()` | Returns `null` |

### Easy Way to Remember

    remove → exception
    poll   → null

---

## element() vs peek()

| Method | Empty deque |
|---|---|
| `element()` | Throws `NoSuchElementException` |
| `peek()` | Returns `null` |

### Easy Way to Remember

    element → exception
    peek    → null

---

## addFirst() vs offerFirst()

| Method | Failure behavior |
|---|---|
| `addFirst()` | Throws exception |
| `offerFirst()` | Returns `false` |

---

## removeFirst() vs pollFirst()

| Method | Empty deque |
|---|---|
| `removeFirst()` | Throws `NoSuchElementException` |
| `pollFirst()` | Returns `null` |

---

## getFirst() vs peekFirst()

| Method | Empty deque |
|---|---|
| `getFirst()` | Throws `NoSuchElementException` |
| `peekFirst()` | Returns `null` |

---

# 14. Complete Method Cheat Sheet

| Operation | Front | Rear |
|---|---|---|
| Insert | `addFirst()` | `addLast()` |
| Insert safely | `offerFirst()` | `offerLast()` |
| Remove | `removeFirst()` | `removeLast()` |
| Remove safely | `pollFirst()` | `pollLast()` |
| View | `getFirst()` | `getLast()` |
| View safely | `peekFirst()` | `peekLast()` |


---

# 16. Does ArrayDeque Allow null?

No.
### Interview Answer

> `ArrayDeque` does not permit null elements.

A useful reason is that `null` is used internally to represent unused/empty positions, so allowing null elements would create ambiguity.

---

# 17. Does ArrayDeque Allow Duplicates?

Yes.

Example:

    Deque<Integer> deque = new ArrayDeque<>();

    deque.add(10);
    deque.add(10);
    deque.add(20);

    System.out.println(deque);

Output:

    [10, 10, 20]

---

# 18. Does ArrayDeque Maintain Insertion Order?

Yes.

Normal iteration traverses elements from the front toward the rear.

Example:

    Deque<Integer> deque = new ArrayDeque<>();

    deque.add(10);
    deque.add(20);
    deque.add(30);

    for (Integer value : deque) {
        System.out.println(value);
    }

Output:

    10
    20
    30

---

# 19. Does ArrayDeque Support Random Access?

No.

You cannot access elements using an index like an `ArrayList`.

This is not supported:

    deque.get(2);

### Interview Answer

> `ArrayDeque` is designed for efficient operations at both ends, not index-based random access.

---

# 20. Internal Working

`ArrayDeque` is implemented using a resizable circular array.

Conceptually:

    Front                     Rear
      ↓                         ↓
    [10] [20] [30] [40] [50]

The internal array is treated as circular.

Conceptually:

    [0] [1] [2] [3] [4]
     ↑               ↓
     └───────────────┘

When the front or rear reaches an array boundary, the logical position can wrap around to the other side.

This allows the deque to reuse available space efficiently.

---

# 21. Why Circular Array?

Consider:

    [10][20][30][40][50]
     ↑
    front

If `10` is removed, the remaining elements do not need to be shifted like they would be in a normal array-based list.

Instead, the logical front moves forward.

Similarly, when space becomes available at the beginning, the rear can wrap around and use that space.

### Interview Answer

> A circular array allows `ArrayDeque` to efficiently reuse empty positions at both ends without shifting all elements.

---

# 22. Does ArrayDeque Use Linked Nodes?

No.

`ArrayDeque` uses a resizable array, while `LinkedList` uses linked nodes.

### Interview Answer

> `ArrayDeque` is array-based, whereas `LinkedList` is node-based.

---

# 23. Time Complexity

| Operation | Complexity |
|---|---|
| `addFirst()` | O(1) amortized |
| `addLast()` | O(1) amortized |
| `offerFirst()` | O(1) amortized |
| `offerLast()` | O(1) amortized |
| `removeFirst()` | O(1) |
| `removeLast()` | O(1) |
| `pollFirst()` | O(1) |
| `pollLast()` | O(1) |
| `peekFirst()` | O(1) |
| `peekLast()` | O(1) |
| `push()` | O(1) amortized |
| `pop()` | O(1) |
| `contains()` | O(n) |
| `remove(Object)` | O(n) |
| `clear()` | O(n) |

### Important

Insertion at either end is O(1) amortized because resizing can occasionally take O(n), but over a sequence of operations the average cost is O(1).

---

# 24. ArrayDeque vs ArrayList

| Feature | ArrayDeque | ArrayList |
|---|---|---|
| Implementation | Resizable circular array | Resizable array |
| Index access | No | Yes |
| Add at end | Efficient | Efficient |
| Remove from front | Efficient | O(n) |
| Add/remove both ends | Efficient | Not optimized |
| Queue | Yes | No |
| Stack | Yes | Not directly |
| Null | Not allowed | Allowed |
| Duplicates | Allowed | Allowed |

### Key Difference

> Use `ArrayList` when you need index-based access; use `ArrayDeque` when you need efficient operations at the ends.

---

# 25. ArrayDeque vs LinkedList

| Feature | ArrayDeque | LinkedList |
|---|---|---|
| Implementation | Resizable circular array | Doubly linked list |
| Implements Deque | Yes | Yes |
| Allows null | No | Yes |
| Random access | No | Yes, but O(n) |
| Memory overhead | Lower | Higher |
| Cache locality | Better | Worse |
| Queue operations | Efficient | Efficient |
| Deque operations | Efficient | Efficient |
| Typical Deque choice | Preferred | Usually not preferred |

### Interview Answer

> `ArrayDeque` is generally preferred over `LinkedList` for deque operations because it has lower per-element overhead and better cache locality.

---

# 26. ArrayDeque vs Stack

| Feature | ArrayDeque | Stack |
|---|---|---|
| Modern choice | Yes | No |
| Implementation | Resizable array | Vector |
| Thread-safe | No | Yes |
| Synchronization | No | Synchronized |
| Allows null | No | Yes |
| Stack operations | Yes | Yes |

### Interview Answer

> Prefer `ArrayDeque` over the legacy `Stack` class when thread safety is not required.

---

# 27. ArrayDeque vs ConcurrentLinkedDeque

`ArrayDeque` is not thread-safe.

    Deque<Integer> deque = new ArrayDeque<>();

If multiple threads modify it concurrently, external synchronization is required.

For a concurrent non-blocking deque:

    Deque<Integer> deque = new ConcurrentLinkedDeque<>();

Import:

    import java.util.concurrent.ConcurrentLinkedDeque;

### Interview Answer

> Use `ArrayDeque` for non-concurrent use and `ConcurrentLinkedDeque` when lock-free concurrent access is required.

---

# 28. Synchronizing ArrayDeque

An `ArrayDeque` can be wrapped using:

    Deque<Integer> deque =
            Collections.synchronizedDeque(new ArrayDeque<>());

Import:

    import java.util.Collections;

However, for concurrent applications, choose the collection based on the exact requirement.

Examples:

- `ArrayDeque` → non-thread-safe deque
- `ConcurrentLinkedDeque` → concurrent non-blocking deque
- `BlockingDeque` implementations → blocking producer/consumer scenarios

---

# 29. Iterator

`ArrayDeque` provides an iterator.

Example:

    Deque<Integer> deque = new ArrayDeque<>();

    deque.add(10);
    deque.add(20);
    deque.add(30);

    Iterator<Integer> iterator = deque.iterator();

    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }

Output:

    10
    20
    30

Normal iterator traverses:

    front → rear

---

# 30. Descending Iterator

`ArrayDeque` provides:

    descendingIterator()

Example:

    Deque<Integer> deque = new ArrayDeque<>();

    deque.add(10);
    deque.add(20);
    deque.add(30);

    Iterator<Integer> iterator = deque.descendingIterator();

    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }

Output:

    30
    20
    10

The descending iterator traverses:

    rear → front

---

# 31. Fail-Fast Iterator

`ArrayDeque` iterators are fail-fast on a best-effort basis.

Example:

    Deque<Integer> deque = new ArrayDeque<>();

    deque.add(10);
    deque.add(20);
    deque.add(30);

    for (Integer value : deque) {
        deque.add(40);
    }

This can result in:

    ConcurrentModificationException

### Important

Fail-fast behavior should not be relied upon for program correctness.

---

# 32. Clone

`ArrayDeque` implements `Cloneable`.

Example:

    ArrayDeque<Integer> deque1 = new ArrayDeque<>();

    deque1.add(10);
    deque1.add(20);

    ArrayDeque<Integer> deque2 = deque1.clone();

`clone()` creates a shallow copy.

The two deque objects are different:

    deque1 != deque2

But if the deque contains objects, their references are copied.

Example:

    ArrayDeque<Student> deque2 = deque1.clone();

Conceptually:

    deque1 ──→ Student A
           ──→ Student B

    deque2 ──→ Student A
           ──→ Student B

The two deques are different objects, but they contain references to the same `Student` objects.

### Interview Answer

> `ArrayDeque.clone()` creates a shallow copy of the deque.

---

# 33. Initial Capacity

You can provide an initial capacity:

    Deque<Integer> deque = new ArrayDeque<>(20);

This does not mean the deque can contain only 20 elements.

Example:

    Deque<Integer> deque = new ArrayDeque<>(2);

    deque.add(10);
    deque.add(20);
    deque.add(30);
    deque.add(40);

The deque dynamically grows.

### Interview Answer

> The constructor's initial capacity is not a fixed maximum capacity.

---

# 34. Size vs Capacity

### Size

Number of elements currently stored:

    deque.size();

Example:

    Deque<Integer> deque = new ArrayDeque<>();

    deque.add(10);
    deque.add(20);

    System.out.println(deque.size());

Output:

    2

### Capacity

The amount of internal storage allocated is an implementation detail and is different from `size()`.

---

# 35. Common Use Cases

## Stack

    Deque<Integer> stack = new ArrayDeque<>();

    stack.push(10);
    stack.push(20);
    stack.push(30);

    int value = stack.pop();

Commonly used for:

- DFS
- Parentheses matching
- Backtracking
- Expression evaluation
- Monotonic stack problems

---

## Queue

    Deque<Integer> queue = new ArrayDeque<>();

    queue.offer(10);
    queue.offer(20);
    queue.offer(30);

    int value = queue.poll();

Commonly used for:

- BFS
- Level-order traversal
- Task processing

---

## Sliding Window

`ArrayDeque` is commonly used for monotonic deque problems.

Example:

    Deque<Integer> deque = new ArrayDeque<>();

Common problems:

- Sliding Window Maximum
- Sliding Window Minimum
- Maintaining candidates in sorted order

---

# 36. ArrayDeque for BFS

Example:

    Deque<Integer> queue = new ArrayDeque<>();

    queue.offer(1);

    while (!queue.isEmpty()) {
        int node = queue.poll();

        // Process node
    }

### One-Line Approach

> Use `offer()` to add nodes at the rear and `poll()` to remove nodes from the front, giving FIFO behavior.

---

# 37. ArrayDeque for DFS

Example:

    Deque<Integer> stack = new ArrayDeque<>();

    stack.push(1);

    while (!stack.isEmpty()) {
        int node = stack.pop();

        // Process node
    }

### One-Line Approach

> Use `push()` and `pop()` on the same end to get LIFO behavior.

---

# 38. Common Interview Questions

## Q1. What is ArrayDeque?

> `ArrayDeque` is a resizable-array implementation of the `Deque` interface that supports efficient insertion and removal from both ends.

## Q2. Is ArrayDeque a Queue or Stack?

> It can act as both because it implements `Deque`.

## Q3. What is the internal data structure of ArrayDeque?

> A resizable circular array.

## Q4. Is ArrayDeque thread-safe?

> No, ArrayDeque is not thread-safe.

## Q5. Does ArrayDeque allow null?

> No, ArrayDeque does not allow null elements.

## Q6. Does ArrayDeque allow duplicates?

> Yes, duplicates are allowed.

## Q7. Does ArrayDeque maintain insertion order?

> Yes, normal iteration traverses elements from front to rear.

## Q8. Does ArrayDeque support random access?

> No, it does not provide index-based random access.

## Q9. Can ArrayDeque grow dynamically?

> Yes, it can grow beyond its initial capacity.

## Q10. Why is ArrayDeque called a circular array implementation?

> Because the logical front and rear can wrap around the physical array, allowing efficient reuse of available space.

## Q11. Why is ArrayDeque generally preferred over LinkedList?

> It has lower memory overhead and better cache locality because it uses an array instead of linked nodes.

## Q12. Why is ArrayDeque preferred over Stack?

> `Stack` is a legacy synchronized class, while `ArrayDeque` provides efficient stack operations without synchronization overhead when thread safety is unnecessary.

## Q13. What happens when ArrayDeque is empty and poll() is called?

> `poll()` returns `null`.

## Q14. What happens when ArrayDeque is empty and remove() is called?

> `remove()` throws `NoSuchElementException`.

## Q15. What happens when ArrayDeque is empty and peek() is called?

> `peek()` returns `null`.

## Q16. What happens when ArrayDeque is empty and element() is called?

> `element()` throws `NoSuchElementException`.

## Q17. Can multiple threads safely modify ArrayDeque?

> No. External synchronization or a concurrent collection is required.

## Q18. Which collection should be used for a concurrent deque?

> `ConcurrentLinkedDeque` is suitable when a non-blocking concurrent deque is required.

## Q19. Can ArrayDeque be used for BFS?

> Yes. Use `offer()` and `poll()` for FIFO behavior.

## Q20. Can ArrayDeque be used for DFS?

> Yes. Use `push()` and `pop()` for LIFO behavior.

## Q21. Is ArrayDeque faster than LinkedList?

> For many queue/deque workloads, yes, because its array-based implementation has lower overhead and better cache locality.

## Q22. Is ArrayDeque synchronized?

> No. ArrayDeque is not synchronized.

## Q23. Does ArrayDeque implement List?

> No. It implements `Deque`, which extends `Queue`.

## Q24. Can we access ArrayDeque elements using get(index)?

> No. ArrayDeque does not support index-based access.

## Q25. What type of copy does clone() create?

> `clone()` creates a shallow copy.

---

# 39. Important Interview Traps

### Trap 1: ArrayDeque allows null

Wrong:

> ArrayDeque allows null.

Correct:

> ArrayDeque does NOT allow null.

---

### Trap 2: ArrayDeque is thread-safe

Wrong:

> ArrayDeque is thread-safe.

Correct:

> ArrayDeque is NOT thread-safe.

---

### Trap 3: ArrayDeque is a linked list

Wrong:

> ArrayDeque uses linked nodes.

Correct:

> ArrayDeque uses a resizable circular array.

---

### Trap 4: ArrayDeque supports random access

Wrong:

    deque.get(2);

Correct:

> ArrayDeque does not support index-based access.

---

### Trap 5: Initial capacity is maximum capacity

Wrong:

> `new ArrayDeque<>(10)` means the deque can contain a maximum of 10 elements.

Correct:

> `10` is an initial capacity; the deque can dynamically grow.

---

### Trap 6: ArrayDeque is always faster

Do not say that it is always faster.

Better interview answer:

> `ArrayDeque` is generally more efficient for typical deque operations because of lower memory overhead and better cache locality, but performance depends on the workload.

---

# 40. Quick Revision

    ArrayDeque
    │
    ├── Implements Deque
    ├── Resizable circular array
    ├── Queue → FIFO
    ├── Stack → LIFO
    ├── Deque → both ends
    │
    ├── Queue
    │   ├── offer()
    │   ├── poll()
    │   └── peek()
    │
    ├── Stack
    │   ├── push()
    │   ├── pop()
    │   └── peek()
    │
    ├── Both ends
    │   ├── addFirst()
    │   ├── addLast()
    │   ├── pollFirst()
    │   └── pollLast()
    │
    ├── null → NOT allowed
    ├── duplicates → allowed
    ├── thread-safe → NO
    ├── random access → NO
    ├── dynamically grows
    ├── iterator → front to rear
    ├── descendingIterator → rear to front
    └── clone() → shallow copy

---

# 41. Must Remember for Interviews

1. `ArrayDeque` implements `Deque`.
2. It can work as both a Queue and Stack.
3. It uses a resizable circular array.
4. It supports efficient insertion/removal from both ends.
5. It does NOT allow null.
6. It allows duplicates.
7. It is NOT thread-safe.
8. It does NOT support random/index-based access.
9. It dynamically grows beyond its initial capacity.
10. Prefer `ArrayDeque` over legacy `Stack` for normal stack operations.
11. `poll()` returns `null` when empty.
12. `remove()` throws `NoSuchElementException` when empty.
13. `peek()` returns `null` when empty.
14. `element()` throws `NoSuchElementException` when empty.
15. `push()` / `pop()` are used for LIFO.
16. `offer()` / `poll()` are commonly used for FIFO.
17. `addFirst()` / `addLast()` insert at both ends.
18. `pollFirst()` / `pollLast()` remove from both ends.
19. `clone()` creates a shallow copy.
20. `ConcurrentLinkedDeque` should be considered when concurrent non-blocking deque access is required.

---

# 42. One-Line Interview Definition

> **ArrayDeque is a resizable circular-array implementation of the Deque interface that supports efficient FIFO, LIFO, and double-ended operations, but does not allow null elements and is not thread-safe.**