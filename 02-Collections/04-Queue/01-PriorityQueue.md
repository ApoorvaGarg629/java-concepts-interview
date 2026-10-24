# PriorityQueue in Java

▸ What is PriorityQueue?

`PriorityQueue` is a queue implementation in Java where elements are processed according to their **priority**, not insertion order.

It is available in:

    java.util.PriorityQueue

By default, it is a **min-heap**, so the smallest element has the highest priority.

Interview one-liner:

> PriorityQueue is a heap-based queue where the highest-priority element is always available at the head.

---

▸ Basic Example

    PriorityQueue<Integer> pq = new PriorityQueue<>();

    pq.add(30);
    pq.add(10);
    pq.add(20);

    System.out.println(pq.peek());

Output:

    10

The smallest element is at the head.

---

▸ Important: PriorityQueue Does NOT Maintain Sorted Order

This is a very common interview question.

Consider:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>();

    pq.add(30);
    pq.add(10);
    pq.add(20);
    pq.add(5);

The internal structure is a heap, not a fully sorted list.

So:

    System.out.println(pq);

may produce something like:

    [5, 10, 20, 30]

But you should NOT depend on the printed order as sorted order.

More importantly, iteration does not guarantee sorted traversal.

To process elements according to priority:

    while (!pq.isEmpty()) {
        System.out.println(pq.poll());
    }

Output:

    5
    10
    20
    30

Interview one-liner:

> PriorityQueue guarantees priority at the head, not sorted order during iteration.

---

▸ Internal Data Structure

PriorityQueue is implemented using a **binary heap**.

By default:

    Min-Heap

Internally, the heap is stored in an array.

Example:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>();

    pq.add(10);
    pq.add(20);
    pq.add(5);
    pq.add(30);

Conceptually:

              5
             / \
           20   10
          /
        30

The exact internal array representation may be:

    [5, 20, 10, 30]

The smallest element is always at index `0`.

---

▸ Why Array is Used Internally?

A binary heap is a complete binary tree.

A complete binary tree can be efficiently represented using an array without storing explicit left/right node references.

For an element at index `i`:

    Parent:
    (i - 1) / 2

    Left child:
    2 * i + 1

    Right child:
    2 * i + 2

Example:

    Index:   0   1   2   3   4   5
    Value:   5  10  20  30  40  50

For index `1`:

    Parent = (1 - 1) / 2 = 0

For index `1`:

    Left child = 2 * 1 + 1 = 3

    Right child = 2 * 1 + 2 = 4

---

▸ Heap Property

For a min-heap:

    Parent <= Children

Example:

              5
             / \
           10   20
          / \
        30   40

Every parent is smaller than or equal to its children.

Important:

> A heap is NOT a completely sorted binary tree.

Only the heap property is guaranteed.

---

▸ Min-Heap

Java's default PriorityQueue is a min-heap.

    PriorityQueue<Integer> pq =
        new PriorityQueue<>();

    pq.add(50);
    pq.add(10);
    pq.add(30);
    pq.add(5);

    pq.poll();

Result:

    5

Next:

    pq.poll();

Result:

    10

---

▸ Max-Heap

To create a max-heap:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>(Comparator.reverseOrder());

Now the largest element has the highest priority.

    pq.add(50);
    pq.add(10);
    pq.add(30);
    pq.add(5);

    System.out.println(pq.peek());

Output:

    50

Processing:

    while (!pq.isEmpty()) {
        System.out.println(pq.poll());
    }

Output:

    50
    30
    10
    5

Interview one-liner:

> Use `Comparator.reverseOrder()` to convert Java's default min-heap PriorityQueue into a max-heap.

---

▸ PriorityQueue with Custom Objects

Suppose:

    class Student {
        String name;
        int marks;

        Student(String name, int marks) {
            this.name = name;
            this.marks = marks;
        }
    }

Create a PriorityQueue ordered by marks:

    PriorityQueue<Student> pq =
        new PriorityQueue<>(
            Comparator.comparingInt(s -> s.marks)
        );

Add:

    pq.add(new Student("John", 80));
    pq.add(new Student("Alice", 95));
    pq.add(new Student("Bob", 70));

Now:

    pq.poll();

returns:

    Bob 70

Next:

    Alice 95
    John 80

---

▸ Custom Max-Heap

To process highest marks first:

    PriorityQueue<Student> pq =
        new PriorityQueue<>(
            Comparator.comparingInt(
                (Student s) -> s.marks
            ).reversed()
        );

Now:

    pq.poll();

returns:

    Alice 95

---

▸ PriorityQueue with Multiple Conditions

Suppose we want:

    1. Higher marks first
    2. If marks are equal, name ascending

Use:

    PriorityQueue<Student> pq =
        new PriorityQueue<>(
            Comparator.comparingInt(
                (Student s) -> s.marks
            ).reversed()
            .thenComparing(s -> s.name)
        );

This gives:

    Alice 95
    Bob   90
    John  90
    David 80

---

▸ PriorityQueue Constructors

Default:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>();

With initial capacity:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>(20);

With Comparator:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>(
            Comparator.reverseOrder()
        );

With capacity and Comparator:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>(
            20,
            Comparator.reverseOrder()
        );

From another collection:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>(collection);

---

▸ Important Methods

Common methods:

    add()
    offer()
    peek()
    poll()
    remove()
    element()
    isEmpty()
    size()
    contains()
    clear()
    iterator()

---

▸ add()

Adds an element to the PriorityQueue.

    pq.add(10);

Returns:

    true

If insertion cannot be performed, `add()` may throw an exception according to the Queue contract.

For PriorityQueue, normal insertion succeeds unless a runtime issue such as an invalid/null element occurs.

---

▸ offer()

Also inserts an element.

    pq.offer(10);

For PriorityQueue:

    add()
    offer()

both normally return `true`.

General Queue difference:

> `offer()` is preferred when you want Queue-style insertion semantics.

---

▸ peek()

Returns the head element without removing it.

    PriorityQueue<Integer> pq =
        new PriorityQueue<>();

    pq.add(30);
    pq.add(10);
    pq.add(20);

    System.out.println(pq.peek());

Output:

    10

The queue remains unchanged.

---

▸ poll()

Returns and removes the head element.

    System.out.println(pq.poll());

Output:

    10

If the queue becomes:

    [20, 30]

then:

    pq.poll();

returns:

    20

If the queue is empty:

    pq.poll();

returns:

    null

---

▸ remove()

`remove()` removes the head element when called without an argument.

    pq.remove();

If the queue is empty:

    NoSuchElementException

There is also:

    pq.remove(element);

which removes a specific element if found.

---

▸ element()

Returns the head without removing it.

    pq.element();

Difference:

    peek()   → returns null if empty
    element() → throws NoSuchElementException if empty

---

▸ peek() vs poll()

    peek()
        |
        +-- returns head
        +-- does not remove
        +-- null if empty

    poll()
        |
        +-- returns head
        +-- removes head
        +-- null if empty

Interview one-liner:

> `peek()` reads the highest-priority element, while `poll()` reads and removes it.

---

▸ poll() vs remove()

    poll()
        → removes head
        → returns null if empty

    remove()
        → removes head
        → throws NoSuchElementException if empty

---

▸ element() vs peek()

    element()
        → returns head
        → throws NoSuchElementException if empty

    peek()
        → returns head
        → returns null if empty

---

▸ Does PriorityQueue Allow Duplicates?

Yes.

Example:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>();

    pq.add(10);
    pq.add(10);
    pq.add(5);

The queue contains:

    5
    10
    10

Duplicates are allowed.

---

▸ Does PriorityQueue Allow null?

No.

Example:

    pq.add(null);

throws:

    NullPointerException

Interview one-liner:

> PriorityQueue does not allow null elements.

---

▸ Does PriorityQueue Allow Multiple Equal Elements?

Yes.

Two elements can have the same priority.

For example:

    10
    10
    10

All can exist in the PriorityQueue.

If the comparator returns `0` for two objects, that does NOT mean PriorityQueue automatically removes one of them.

This is different from how TreeSet treats comparison-equivalent elements.

---

▸ Is PriorityQueue Thread-Safe?

No.

`PriorityQueue` is not synchronized and is not thread-safe.

For concurrent priority-based processing, use an appropriate concurrent implementation such as:

    PriorityBlockingQueue

---

▸ PriorityBlockingQueue

`PriorityBlockingQueue` is a concurrent implementation of a priority queue.

Example:

    PriorityBlockingQueue<Integer> pq =
        new PriorityBlockingQueue<>();

It is useful when multiple threads need to safely produce and consume priority-ordered elements.

Important:

> PriorityBlockingQueue is thread-safe, while PriorityQueue is not.

---

▸ PriorityQueue vs PriorityBlockingQueue

| Feature | PriorityQueue | PriorityBlockingQueue |
|---|---|---|
| Thread-safe | No | Yes |
| Blocking operations | No | Yes |
| Priority ordering | Yes | Yes |
| Package | `java.util` | `java.util.concurrent` |
| Null elements | No | No |

---

▸ Time Complexity

For a PriorityQueue with `n` elements:

    add()       → O(log n)
    offer()     → O(log n)
    poll()      → O(log n)
    remove()    → O(log n)
    peek()      → O(1)
    element()   → O(1)
    size()      → O(1)
    contains()  → O(n)
    remove(x)   → O(n)
    clear()     → O(n)

Why is `add()` O(log n)?

Because the new element may need to move upward through the heap.

Why is `poll()` O(log n)?

Because after removing the root, another element may need to move downward to restore the heap property.

---

▸ How add() Works Internally

Suppose the min-heap is:

              10
             /  \
           20    30
          /
        40

Array:

    [10, 20, 30, 40]

Now add:

    5

Initially it is placed at the end:

    [10, 20, 30, 40, 5]

Now compare `5` with its parent:

    Parent = 20

Since:

    5 < 20

swap them:

    [10, 5, 30, 40, 20]

Now compare `5` with its new parent:

    Parent = 10

Since:

    5 < 10

swap:

    [5, 10, 30, 40, 20]

Heap property is restored.

This process is called:

    Sift Up
    or
    Bubble Up

---

▸ How poll() Works Internally

Suppose:

              5
             / \
           10   20
          /
        30

Array:

    [5, 10, 20, 30]

Remove the root:

    5

Move the last element to the root:

    [30, 10, 20]

Now compare `30` with its children.

Smallest child:

    10

Since:

    30 > 10

swap:

    [10, 30, 20]

Heap property is restored.

This process is called:

    Sift Down
    or
    Heapify Down

---

▸ Why is peek() O(1)?

The highest-priority element is always stored at the root.

The root is stored at:

    index 0

Therefore:

    peek() → O(1)

---

▸ Why is contains() O(n)?

A heap only guarantees parent-child priority relationships.

It does NOT provide sorted ordering that would allow binary search.

Therefore, finding an arbitrary element may require checking many elements.

    contains(x) → O(n)

---

▸ Building a PriorityQueue from a Collection

Example:

    List<Integer> list =
        Arrays.asList(50, 10, 30, 20);

    PriorityQueue<Integer> pq =
        new PriorityQueue<>(list);

The elements are organized into a heap.

You can then process them:

    while (!pq.isEmpty()) {
        System.out.println(pq.poll());
    }

Output:

    10
    20
    30
    50

---

▸ PriorityQueue and Comparable

If no Comparator is supplied, elements use their **natural ordering**.

For example:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>();

Integer already implements Comparable.

Therefore:

    1 < 2 < 3 < 4

The smallest value has the highest priority.

---

▸ PriorityQueue and Comparator

If a Comparator is provided, the Comparator determines priority.

Example:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>(
            Comparator.reverseOrder()
        );

Now:

    10 > 5 > 2

The largest value has the highest priority.

Interview one-liner:

> PriorityQueue uses natural ordering when no Comparator is provided and uses the supplied Comparator when one is provided.

---

▸ PriorityQueue and Comparable vs Comparator

Comparable:

    class Student
        implements Comparable<Student> {

        @Override
        public int compareTo(Student other) {
            return Integer.compare(
                this.marks,
                other.marks
            );
        }
    }

Then:

    PriorityQueue<Student> pq =
        new PriorityQueue<>();

Comparator:

    PriorityQueue<Student> pq =
        new PriorityQueue<>(
            Comparator.comparingInt(
                Student::getMarks
            )
        );

The second approach does not require modifying the Student class.

---

▸ PriorityQueue Iteration

Example:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>();

    pq.add(30);
    pq.add(10);
    pq.add(20);

    for (Integer value : pq) {
        System.out.println(value);
    }

The output is NOT guaranteed to be:

    10
    20
    30

The iterator does not provide priority-ordered traversal.

To process elements by priority:

    while (!pq.isEmpty()) {
        System.out.println(pq.poll());
    }

Interview trap:

> Never use PriorityQueue's iterator when you need elements in priority order.

---

▸ Converting PriorityQueue to Array

Example:

    Object[] array = pq.toArray();

The resulting array is not guaranteed to be sorted.

If sorted output is required, repeatedly use:

    poll()

---

▸ PriorityQueue and Sorting

Suppose:

    [5, 2, 8, 1, 3]

Using PriorityQueue:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>();

    for (int x : nums) {
        pq.offer(x);
    }

    while (!pq.isEmpty()) {
        System.out.println(pq.poll());
    }

Output:

    1
    2
    3
    5
    8

This gives sorted output for a min-heap.

But building a PriorityQueue just to sort a complete collection is generally unnecessary when `Arrays.sort()` or `Collections.sort()` is available.

PriorityQueue is useful when you need to repeatedly access/remove the highest-priority element.

---

▸ Top K Problems

PriorityQueue is heavily used for Top K interview problems.

Example:

    Find the K largest elements.

A common approach is to maintain a min-heap of size `k`.

Example:

    nums = [3, 2, 1, 5, 6, 4]
    k = 2

Maintain:

    min-heap of size 2

Process:

    3 → [3]
    2 → [2, 3]
    1 → [2, 3] after removing 1
    5 → [3, 5]
    6 → [5, 6]
    4 → [5, 6] after removing 4

Final heap:

    [5, 6]

The smallest element inside the heap represents the cutoff for the current Top K.

Interview one-liner:

> For Top K largest, maintain a min-heap of size K and remove the smallest whenever the heap exceeds K.

---

▸ Top K Smallest

For Top K smallest, commonly use a max-heap of size `k`.

Example:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>(
            Comparator.reverseOrder()
        );

Maintain only the K smallest elements.

When:

    pq.size() > k

remove:

    pq.poll();

Because the max-heap puts the largest of the selected K elements at the head.

Interview one-liner:

> For Top K smallest, maintain a max-heap of size K and remove the largest whenever the heap exceeds K.

---

▸ Kth Largest Element

Common approach:

    1. Create a min-heap.
    2. Add every number.
    3. If heap size > k, remove the smallest.
    4. The heap root is the kth largest.

Code:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>();

    for (int num : nums) {
        pq.offer(num);

        if (pq.size() > k) {
            pq.poll();
        }
    }

    int kthLargest = pq.peek();

Time:

    O(n log k)

Space:

    O(k)

---

▸ Kth Smallest Element

Use a max-heap:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>(
            Comparator.reverseOrder()
        );

    for (int num : nums) {
        pq.offer(num);

        if (pq.size() > k) {
            pq.poll();
        }
    }

    int kthSmallest = pq.peek();

Time:

    O(n log k)

Space:

    O(k)

---

▸ Merge K Sorted Lists

PriorityQueue is commonly used to merge K sorted lists.

Basic idea:

    1. Put the first element from every list into a min-heap.
    2. Remove the smallest element.
    3. Add the next element from that same list.
    4. Repeat until the heap is empty.

Interview one-liner:

> Put the current smallest element from each list into a min-heap, repeatedly remove the minimum, and add the next element from the same list.

Complexity:

    O(N log K)

where:

    N = total number of elements
    K = number of lists

---

▸ Find Median from Data Stream

PriorityQueue is also used for maintaining the median dynamically.

Use two heaps:

    Max-heap → smaller half
    Min-heap → larger half

Conceptually:

             Median
               |
        ----------------
        |              |
    Max-Heap         Min-Heap
    smaller half     larger half

Maintain their sizes so they differ by at most one.

Median:

    Same size:
        average of both roots

    Different size:
        root of the larger heap

Interview one-liner:

> Maintain the lower half in a max-heap and the upper half in a min-heap, keeping their sizes balanced.

---

▸ Meeting Rooms / Scheduling Problems

PriorityQueue is useful when repeatedly selecting the earliest finishing task.

For example:

    PriorityQueue<Integer> minHeap =
        new PriorityQueue<>();

Store end times.

The smallest end time is always available through:

    peek()

This is useful in:

    Meeting Rooms II
    CPU scheduling
    Task scheduling
    Event processing

---

▸ Dijkstra's Algorithm

PriorityQueue is commonly used in Dijkstra's shortest-path algorithm.

Store:

    (distance, node)

using a min-heap.

Example:

    PriorityQueue<int[]> pq =
        new PriorityQueue<>(
            Comparator.comparingInt(a -> a[0])
        );

Here:

    a[0] = distance
    a[1] = node

The node with the smallest current distance is processed first.

---

▸ BFS with Priority

Normal BFS uses:

    Queue

Priority-based traversal can use:

    PriorityQueue

For example, some graph problems require processing the smallest-cost or highest-priority state first.

This is the core idea behind algorithms such as:

    Dijkstra
    Best-first search
    A* variants

---

▸ PriorityQueue vs Queue

| Feature | Queue | PriorityQueue |
|---|---|---|
| Processing order | Usually FIFO | Priority |
| Head | Oldest element | Highest-priority element |
| Default ordering | Insertion order | Natural ordering |
| Internal structure | Depends on implementation | Binary heap |
| Random access | No | No |
| Allows duplicates | Usually yes | Yes |
| Allows null | Depends on implementation | No |

---

▸ PriorityQueue vs ArrayDeque

| Feature | PriorityQueue | ArrayDeque |
|---|---|---|
| Purpose | Priority-based processing | Double-ended queue |
| Internal structure | Binary heap | Resizable circular array |
| Head | Highest-priority element | Front element |
| Tail operations | Not priority-based | Yes |
| Random access | No | No |
| Null | Not allowed | Not allowed |
| Thread-safe | No | No |

---

▸ PriorityQueue vs TreeSet

| Feature | PriorityQueue | TreeSet |
|---|---|---|
| Purpose | Priority-based processing | Sorted unique elements |
| Duplicates | Allowed | Not allowed |
| Internal structure | Binary heap | Red-Black Tree |
| Head | Highest priority | Smallest/largest depending on order |
| Full sorted iteration | No | Yes |
| peek/poll | Efficient | Similar navigation/removal operations |
| Ordering | Comparable/Comparator | Comparable/Comparator |

Important difference:

> PriorityQueue allows duplicates and only guarantees priority at the head, while TreeSet maintains unique elements in sorted order.

---

▸ PriorityQueue vs Heap

In Java, `PriorityQueue` is the standard library implementation of a priority queue using a heap-based structure.

Interview answer:

> A heap is the underlying data structure, while PriorityQueue is the Java collection class that provides the priority-queue API.

---

▸ PriorityQueue vs Stack

Stack follows:

    LIFO

PriorityQueue follows:

    Priority order

Example:

Stack:

    push 10
    push 20
    push 5

    pop() → 5

PriorityQueue:

    add 10
    add 20
    add 5

    poll() → 5

The results may sometimes look similar, but their rules are completely different.

---

▸ Important Internal Detail: Sift Up

When inserting:

    1. Place new element at the end.
    2. Compare with parent.
    3. Swap if heap property is violated.
    4. Continue until heap property is restored.

Complexity:

    O(log n)

---

▸ Important Internal Detail: Sift Down

When removing the root:

    1. Remove root.
    2. Move last element to root.
    3. Compare with children.
    4. Swap with the appropriate child.
    5. Continue until heap property is restored.

Complexity:

    O(log n)

---

▸ Why PriorityQueue Uses a Complete Binary Tree

A heap is a complete binary tree, meaning:

    All levels are completely filled except possibly the last,
    and the last level is filled from left to right.

This allows efficient array representation.

No explicit tree nodes are required.

---

▸ PriorityQueue Resizing

PriorityQueue uses an internal array that can grow when it becomes full.

Therefore, the queue is dynamically resizable.

The exact growth policy is an implementation detail and should not be relied upon in interview code.

Important concept:

> PriorityQueue is array-backed internally but dynamically resizable.

---

▸ PriorityQueue and Null Comparator

You can pass:

    null

as the Comparator.

Example:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>(null);

This means natural ordering is used.

Equivalent conceptually to:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>();

---

▸ What Happens If Elements Are Not Comparable?

If no Comparator is provided, elements must have a compatible natural ordering.

For example:

    PriorityQueue<Student> pq =
        new PriorityQueue<>();

If `Student` does not implement `Comparable<Student>`, inserting elements can result in:

    ClassCastException

If you provide a Comparator:

    PriorityQueue<Student> pq =
        new PriorityQueue<>(
            Comparator.comparingInt(Student::getAge)
        );

then `Student` does not need to implement Comparable.

---

▸ Important Comparator Trap

Suppose:

    PriorityQueue<Student> pq =
        new PriorityQueue<>(
            Comparator.comparingInt(Student::getAge)
        );

If two students have the same age, the Comparator returns:

    0

This does NOT mean one student is removed.

Both students can remain in the PriorityQueue.

This differs from:

    TreeSet

where comparison returning `0` can cause the second element to be considered a duplicate.

---

▸ Iterator.remove()

PriorityQueue's iterator supports removal of the element most recently returned by the iterator.

Example:

    Iterator<Integer> iterator =
        pq.iterator();

    while (iterator.hasNext()) {
        Integer value = iterator.next();

        if (value == 10) {
            iterator.remove();
        }
    }

As with other collection iterators, calling `iterator.remove()` twice without another `next()` causes:

    IllegalStateException

---

▸ Fail-Fast Iterator

PriorityQueue's iterator is fail-fast on a best-effort basis.

If the queue is structurally modified outside the iterator during iteration, a:

    ConcurrentModificationException

may occur.

Do not rely on fail-fast behavior for program correctness.

---

▸ PriorityQueue is Not a FIFO Queue

This is an important interview point.

Normal Queue:

    First inserted → generally first processed

PriorityQueue:

    Highest priority → first processed

Example:

    add(50)
    add(10)
    add(30)

poll():

    10

Even though `50` was inserted first.

---

▸ One-Liner Approach for PriorityQueue Problems

General:

> Use a heap to keep the most important element at the top so it can be accessed or removed efficiently.

Top K largest:

> Maintain a min-heap of size K and remove the smallest when the size exceeds K.

Top K smallest:

> Maintain a max-heap of size K and remove the largest when the size exceeds K.

Merge K sorted lists:

> Keep one current element from each list in a min-heap and repeatedly extract the smallest.

Median of stream:

> Use a max-heap for the smaller half and a min-heap for the larger half, keeping both halves balanced.

---

▸ Common Interview Questions

▸ What is PriorityQueue?

> PriorityQueue is a heap-based Queue implementation where the head is determined by priority rather than insertion order.

▸ What is the default PriorityQueue in Java?

> A min-heap using natural ordering.

▸ What is the internal data structure?

> A resizable array-based binary heap.

▸ Is PriorityQueue sorted?

> No. It only guarantees that the highest-priority element is at the head.

▸ Does PriorityQueue allow duplicates?

> Yes.

▸ Does PriorityQueue allow null?

> No.

▸ Is PriorityQueue thread-safe?

> No.

▸ What is the thread-safe alternative?

> `PriorityBlockingQueue`.

▸ What is the time complexity of `peek()`?

> O(1).

▸ What is the time complexity of `add()`?

> O(log n).

▸ What is the time complexity of `poll()`?

> O(log n).

▸ What is the time complexity of `contains()`?

> O(n).

▸ How do you create a max-heap?

> Use `new PriorityQueue<>(Comparator.reverseOrder())`.

▸ Can PriorityQueue store custom objects?

> Yes, using either Comparable or a Comparator.

▸ What happens if custom objects are neither Comparable nor supplied with a Comparator?

> A `ClassCastException` can occur when the queue needs to compare them.

▸ Does PriorityQueue's iterator return elements in sorted order?

> No.

▸ How do you get elements in priority order?

> Repeatedly call `poll()` until the queue becomes empty.

▸ Why is PriorityQueue useful for Top K problems?

> It maintains only the K most relevant elements while keeping insertion/removal at O(log K).

▸ What is the difference between PriorityQueue and TreeSet?

> PriorityQueue allows duplicates and guarantees priority only at the head, while TreeSet stores unique elements in sorted order.

---

▸ Quick Revision

    PriorityQueue
        |
        +-- java.util
        |
        +-- Heap-based
        |
        +-- Binary Heap
        |
        +-- Array-backed
        |
        +-- Default = Min-Heap
        |
        +-- Comparator.reverseOrder() = Max-Heap
        |
        +-- Duplicates allowed
        |
        +-- null not allowed
        |
        +-- Not thread-safe
        |
        +-- peek() = O(1)
        |
        +-- add()/offer() = O(log n)
        |
        +-- poll() = O(log n)
        |
        +-- contains() = O(n)
        |
        +-- Iterator NOT sorted
        |
        +-- poll() repeatedly = priority order
        |
        +-- Uses Comparable / Comparator
        |
        +-- Useful for:
              |
              +-- Top K
              +-- Kth largest/smallest
              +-- Merge K sorted lists
              +-- Median of data stream
              +-- Scheduling
              +-- Dijkstra
              +-- Best-first search

---

▸ Final Definition

`PriorityQueue` is a resizable, heap-based Queue implementation in Java that processes elements according to their priority. By default it is a min-heap using natural ordering, while a Comparator can be supplied to define custom priority ordering.