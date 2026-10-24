# Stack

## ▸ What is Stack?

`Stack` is a **LIFO (Last-In-First-Out)** data structure where the element inserted last is removed first.

---

## ▸ Important Stack Methods

### ▸ push()

Adds an element to the top of the stack.

    stack.push(10);
    stack.push(20);

    // [10, 20]

Time complexity: **O(1) amortized**.

---

### ▸ pop()

Removes and returns the top element.

    Stack<Integer> stack = new Stack<>();

    stack.push(10);
    stack.push(20);

    int value = stack.pop();

    // value = 20
    // stack = [10]

If the stack is empty:

    stack.pop();

throws:

    EmptyStackException

---

### ▸ peek()

Returns the top element without removing it.

    stack.push(10);
    stack.push(20);

    System.out.println(stack.peek());

    // 20

If the stack is empty, `peek()` throws:

    EmptyStackException

---

### ▸ empty()

Checks whether the stack is empty.

    stack.empty();

Returns:

    true
    false

Modern alternative:

    stack.isEmpty();

---

### ▸ search()

Returns the **1-based position from the top** of the element.

    Stack<Integer> stack = new Stack<>();

    stack.push(10);
    stack.push(20);
    stack.push(30);

    System.out.println(stack.search(30)); // 1
    System.out.println(stack.search(20)); // 2
    System.out.println(stack.search(10)); // 3

If the element does not exist:

    stack.search(50); // -1

#### ▸ Interview trap

`search()` does **not** return the zero-based array/list index.

It counts from the **top starting at 1**.

---

## ▸ Stack Methods Quick Table

| Method | Purpose | Removes element? |
|---|---|---|
| `push()` | Add to top | No |
| `pop()` | Remove top | Yes |
| `peek()` | View top | No |
| `empty()` | Check empty | No |
| `isEmpty()` | Check empty | No |
| `search()` | Find position from top | No |

### ▸ Easy way to remember

> **push = add, pop = remove, peek = see**

---

### ▸ Stack Allows Duplicates
### ▸ Stack Allows null

### ▸ Interview point

> **Stack allows null because it inherits Vector's behavior, whereas ArrayDeque rejects null to avoid ambiguity with methods such as peek() and poll().**

### ▸ Stack is Synchronized

`Stack` is synchronized because it extends `Vector`, whose methods are synchronized.

---

## ▸ Why Stack is Considered Legacy

`Stack` is an older Java collection class.

It extends `Vector` and exposes many inherited `List` operations that are not natural for a stack.

For example:

    stack.add(10);
    stack.add(20);

You can also access elements by index:

    stack.get(0);

This means the class does not strictly enforce stack-only behavior.

### ▸ Modern recommendation

For a normal stack, prefer:

    Deque<Integer> stack = new ArrayDeque<>();

Then use:

    stack.push(10);
    stack.push(20);
    stack.pop();
    stack.peek();

### ▸ Interview answer

> **Stack is legacy; `Deque` with `ArrayDeque` is generally preferred for stack implementation in modern Java.**

---

## ▸ Stack vs ArrayDeque

| Feature | Stack | ArrayDeque |
|---|---|---|
| LIFO operations | Yes | Yes |
| Legacy | Yes | No |
| Implements | `List` through `Vector` | `Deque` |
| Thread-safe | Yes | No |
| Synchronized | Yes | No |
| Allows null | Yes | No |
| Allows duplicates | Yes | Yes |
| Random access | Yes, inherited from `Vector` | No `get(index)` |
| Modern recommendation | No | Yes |
| Stack usage | `push/pop/peek` | `push/pop/peek` |

### ▸ Why ArrayDeque is preferred?

> **ArrayDeque is preferred because it is a modern Deque implementation optimized for end operations without the unnecessary synchronization overhead of Stack.**

---

## ▸ Why Synchronization Is Not Always an Advantage

`Stack` is synchronized:

    Stack<Integer> stack = new Stack<>();

This provides thread safety but also introduces synchronization overhead.

If only one thread uses the stack, that synchronization is usually unnecessary.

`ArrayDeque` is not synchronized:

    Deque<Integer> stack = new ArrayDeque<>();

Therefore it is generally faster for normal single-threaded use.

### ▸ Interview one-liner

> **Stack's synchronization is useful for thread safety but unnecessary overhead when the stack is used by only one thread.**

---

## ▸ What If Thread Safety Is Required?

Do not automatically choose `Stack` just because it is synchronized.

The appropriate collection depends on the requirement.

For a synchronized deque:

    Deque<Integer> stack =
        Collections.synchronizedDeque(new ArrayDeque<>());

For concurrent non-blocking deque operations:

    ConcurrentLinkedDeque<Integer> deque =
        new ConcurrentLinkedDeque<>();

---

## ▸ Stack and Random Access

Because `Stack` extends `Vector`, it inherits indexed access.

Example:

    Stack<Integer> stack = new Stack<>();

    stack.push(10);
    stack.push(20);
    stack.push(30);

    System.out.println(stack.get(0)); // 10

This is possible because `Stack` is also a `List` through its inheritance hierarchy.

### ▸ Important interview point

A pure stack abstraction normally focuses on:

    push()
    pop()
    peek()

But Java's `Stack` exposes additional `List` operations because it extends `Vector`.

---

## ▸ Stack Can Be Used Like a List

Because of inheritance from `Vector`, you can do:

    stack.add(10);
    stack.add(20);

    stack.get(0);
    stack.set(0, 100);
    stack.remove(0);

This is one reason `Stack` is considered less desirable as a modern stack abstraction.

`Deque` provides a cleaner API for stack operations.

---

## ▸ Stack Capacity and Vector

Because `Stack` extends `Vector`, it inherits `Vector`'s capacity-related behavior.

Important methods include:

    capacity()
    ensureCapacity()
    trimToSize()

Example:

    Stack<Integer> stack = new Stack<>();

    System.out.println(stack.capacity());

The underlying storage grows as elements are added.

### ▸ Interview point

> **Stack inherits Vector's dynamic-array behavior and capacity management.**

---

## ▸ Stack Time Complexity

| Operation | Complexity |
|---|---:|
| `push()` | O(1) amortized |
| `pop()` | O(1) |
| `peek()` | O(1) |
| `empty()` | O(1) |
| `isEmpty()` | O(1) |
| `search()` | O(n) |
| `get(index)` | O(1) |

`search()` requires finding the element from the top, so it can take O(n).

---

## ▸ Internal Working

Since `Stack` extends `Vector`, its storage is based on a dynamically growing array.

Conceptually:

    Stack
       ↓
    Vector
       ↓
    Dynamic array

Elements are stored in sequence.

For example:

    stack.push(10);
    stack.push(20);
    stack.push(30);

Internally:

    [10, 20, 30]
             ↑
            TOP

`pop()` removes the top element:

    [10, 20]
          ↑
         TOP

---

## ▸ Stack Empty Behavior

`pop()` and `peek()` throw `EmptyStackException` when the stack is empty.

Example:

    Stack<Integer> stack = new Stack<>();

    stack.pop();

Result:

    java.util.EmptyStackException

Same for:

    stack.peek();

### ▸ Interview point

> **Stack's `pop()` and `peek()` throw `EmptyStackException` when no element is available.**

This differs from `Deque` methods:

    poll()
    peek()

which return `null` when the deque is empty.

---

## ▸ Stack vs Deque Empty Operations

| Stack | Deque |
|---|---|
| `pop()` | `pop()` |
| `peek()` | `peek()` |
| Empty `pop()` → `EmptyStackException` | Empty `pop()` → `NoSuchElementException` |
| Empty `peek()` → `EmptyStackException` | Empty `peek()` → `null` |

For `ArrayDeque`, `poll()` is also available:

    poll()

Empty deque:

    poll() → null

---

## ▸ Stack as a LIFO Structure

Stack operations:

    push()
    pop()
    peek()

Example:

    Stack<String> stack = new Stack<>();

    stack.push("A");
    stack.push("B");
    stack.push("C");

    stack.pop();  // C
    stack.pop();  // B
    stack.pop();  // A

This follows:

    Last In → First Out

---

## ▸ Common Stack Use Cases

Stacks are commonly used for:

- Parentheses matching
- Expression evaluation
- Expression conversion
- Function call management
- Recursion
- Undo/Redo
- Backtracking
- DFS
- Browser history
- Monotonic stack problems
- Syntax parsing

For coding interviews, stack is especially common in:

- Valid Parentheses
- Min Stack
- Daily Temperatures
- Next Greater Element
- Largest Rectangle in Histogram
- Evaluate Reverse Polish Notation
- Remove K Digits
- Simplify Path

---

## ▸ Stack and Recursion

Java's method calls are managed using the **call stack**.

Example:

    void methodA() {
        methodB();
    }

    void methodB() {
        methodC();
    }

    void methodC() {
    }

Conceptually:

    methodC()
    methodB()
    methodA()

The most recently called method completes first.

This follows the LIFO principle.

---

## ▸ Stack and DFS

DFS can be implemented using a stack.

Example:

    Stack<Integer> stack = new Stack<>();

    stack.push(start);

    while (!stack.isEmpty()) {
        int node = stack.pop();

        // process node

        // push neighboring nodes
    }

However, in modern Java, prefer:

    Deque<Integer> stack = new ArrayDeque<>();

---

## ▸ Stack and Parentheses Matching

A stack is useful because the most recently opened bracket must be closed first.

Example:

    { [ ( ) ] }

Processing:

    { → push
    [ → push
    ( → push
    ) → pop
    ] → pop
    } → pop

The LIFO behavior naturally matches nested brackets.

---

## ▸ Stack vs Queue

| Feature | Stack | Queue |
|---|---|---|
| Principle | LIFO | FIFO |
| Insert | Top | Rear |
| Remove | Top | Front |
| Common operations | `push/pop` | `offer/poll` |
| Example | Undo | Print queue |

### ▸ Easy memory trick

> **Stack = LIFO, Queue = FIFO**

---

## ▸ Stack vs LinkedList vs ArrayDeque

| Feature | Stack | LinkedList | ArrayDeque |
|---|---|---|---|
| Legacy | Yes | No | No |
| Stack operations | Yes | Yes | Yes |
| Queue operations | Limited | Yes | Yes |
| Deque operations | Not primary | Yes | Yes |
| Synchronized | Yes | No | No |
| Null allowed | Yes | Yes | No |
| Random access | Yes | Yes, O(n) | No `get(index)` |
| Preferred for stack | No | Possible | Yes |

For normal stack usage:

    Deque<Integer> stack = new ArrayDeque<>();

is generally preferred.

---

## ▸ Can Stack Store Objects?

Yes.

    Stack<Student> students = new Stack<>();

    students.push(new Student());
    students.push(new Student());

`Stack` is generic and can store any reference type.

---

## ▸ Can Stack Store Primitive Types?

Collections cannot directly store primitives.

This does not work:

    Stack<int> stack;

Use wrapper classes:

    Stack<Integer> stack;

Java automatically handles boxing/unboxing:

    stack.push(10);          // autoboxing
    int value = stack.pop(); // unboxing

---

## ▸ Stack with Strings

Example:

    Stack<String> stack = new Stack<>();

    stack.push("Java");
    stack.push("Python");
    stack.push("C++");

    System.out.println(stack.pop());

Output:

    C++

---

## ▸ Stack vs Collections.synchronizedList()

`Stack` is synchronized because it extends `Vector`.

A synchronized list can also be created using:

    List<Integer> list =
        Collections.synchronizedList(new ArrayList<>());

But this does not automatically make it a proper stack abstraction.

For stack behavior, prefer:

    Deque<Integer> stack = new ArrayDeque<>();

---

## ▸ Important Interview Traps

### ▸ Trap 1: Is Stack a modern collection?

No.

> `Stack` is a legacy class.

---

### ▸ Trap 2: Is Stack thread-safe?

Yes.

> `Stack` methods are synchronized through its inheritance from `Vector`.

---

### ▸ Trap 3: Does Stack allow null?

Yes.

> `Stack` allows `null`.

---

### ▸ Trap 4: Does ArrayDeque allow null?

No.

> `ArrayDeque` does not allow `null`.

---

### ▸ Trap 5: Does Stack allow duplicates?

Yes.

---

### ▸ Trap 6: What happens when `pop()` is called on an empty Stack?

`EmptyStackException` is thrown.

---

### ▸ Trap 7: What does `search()` return?

It returns the **1-based position from the top**.

---

### ▸ Trap 8: Why prefer ArrayDeque over Stack?

> `ArrayDeque` is modern, avoids unnecessary synchronization, and provides a cleaner Deque-based stack API.

---

### ▸ Trap 9: Is Stack internally an array?

Yes.

`Stack` extends `Vector`, and `Vector` uses a dynamically growing array internally.

---

### ▸ Trap 10: Can Stack perform random access?

Yes, because it inherits indexed methods such as:

    get(index)

from `Vector`/`List`.

---

## ▸ Interview Q&A

### ▸ Q1. What is Stack?

> Stack is a LIFO data structure where the last inserted element is removed first.

---

### ▸ Q2. Which class represents Stack in Java?

> `java.util.Stack`

---

### ▸ Q3. Is Stack a class or interface?

> `Stack` is a class.

---

### ▸ Q4. Which class does Stack extend?

> `Stack` extends `Vector`.

---

### ▸ Q5. Is Stack synchronized?

> Yes, because Stack inherits synchronization from Vector.

---

### ▸ Q6. Is Stack thread-safe?

> Yes, its individual operations are synchronized.

---

### ▸ Q7. Why is Stack considered legacy?

> Because it is based on the older Vector class and exposes unnecessary List operations; modern Java prefers Deque implementations such as ArrayDeque.

---

### ▸ Q8. What is preferred instead of Stack?

> `Deque` with `ArrayDeque` is generally preferred for stack operations.

Example:

    Deque<Integer> stack = new ArrayDeque<>();

---

### ▸ Q9. Why is ArrayDeque preferred over Stack?

> ArrayDeque provides a modern Deque API and avoids the synchronization overhead of Stack when thread safety is not required.

---

### ▸ Q10. Does Stack allow null?

> Yes, Stack allows null elements.

---

### ▸ Q11. Why does ArrayDeque not allow null?

> Because null can be used by methods such as `peek()` and `poll()` to indicate that the deque is empty, so allowing null would create ambiguity.

---

### ▸ Q12. What is the difference between `pop()` and `peek()`?

> `pop()` removes and returns the top element, while `peek()` only returns the top element.

---

### ▸ Q13. What happens when `pop()` is called on an empty Stack?

> `EmptyStackException` is thrown.

---

### ▸ Q14. What does `search()` do?

> It returns the 1-based position of an element measured from the top of the stack, or -1 if the element is not found.

---

### ▸ Q15. Does Stack allow duplicate elements?

> Yes, Stack allows duplicates.

---

### ▸ Q16. Can Stack access elements by index?

> Yes, because Stack inherits indexed access such as `get(index)` from Vector/List.

---

### ▸ Q17. Is Stack internally an array?

> Yes, indirectly; Stack extends Vector, which uses a dynamically growing array.

---

### ▸ Q18. What is the time complexity of push and pop?

> `push()` is O(1) amortized and `pop()` is O(1).

---

### ▸ Q19. What is the time complexity of search?

> O(n), because the stack may need to scan its elements.

---

### ▸ Q20. Can Stack be used for DFS?

> Yes, DFS can be implemented using a stack, although `Deque` with `ArrayDeque` is preferred in modern Java.

---

### ▸ Q21. Can Stack be used for parentheses matching?

> Yes, because nested brackets naturally follow the LIFO behavior of a stack.

---

### ▸ Q22. What is the difference between Stack and Queue?

> Stack follows LIFO, while Queue follows FIFO.

---

### ▸ Q23. Is synchronization always an advantage?

> No; synchronization provides thread safety but adds overhead when concurrent access is not required.

---

### ▸ Q24. If thread safety is required, should we always use Stack?

> No; choose the collection based on the concurrency requirement rather than using the legacy Stack by default.

---

## ▸ Modern Java Stack Recommendation

### ▸ Old approach

    Stack<Integer> stack = new Stack<>();

    stack.push(10);
    stack.push(20);
    stack.pop();

### ▸ Recommended approach

    Deque<Integer> stack = new ArrayDeque<>();

    stack.push(10);
    stack.push(20);
    stack.pop();

The second approach is preferred for normal stack usage.

---

## ▸ Stack Operation Cheat Sheet

    push(x)   → add x to top
    pop()     → remove and return top
    peek()    → return top without removing
    empty()   → check whether empty
    isEmpty() → check whether empty
    search(x) → 1-based position from top

---

## ▸ Stack vs ArrayDeque: Most Important Interview Points

Remember these:

- `Stack` is a **legacy class**.
- `Stack` extends **Vector**.
- `Stack` is **synchronized**.
- `Stack` is **thread-safe for individual operations**.
- `Stack` **allows null**.
- `Stack` **allows duplicates**.
- `Stack` supports **indexed access** because of Vector/List inheritance.
- `pop()` removes the top element.
- `peek()` only reads the top element.
- Empty `Stack.pop()`/`peek()` throws `EmptyStackException`.
- `search()` uses **1-based position from the top**.
- `ArrayDeque` is generally preferred for normal stack usage.
- `ArrayDeque` is **not synchronized**.
- `ArrayDeque` **does not allow null**.
- Use `ConcurrentLinkedDeque` or appropriate synchronization when concurrent deque behavior is required.

---

## ▸ One-Liner Revision

> **Stack is a legacy, synchronized LIFO class extending Vector; for modern Java stack usage, prefer Deque with ArrayDeque unless a specific concurrency requirement says otherwise.**

---

## ▸ Quick Revision Table

| Topic | Stack |
|---|---|
| Type | Class |
| Package | `java.util` |
| Principle | LIFO |
| Parent | Vector |
| Legacy | Yes |
| Synchronized | Yes |
| Thread-safe | Yes, for individual operations |
| Null | Allowed |
| Duplicates | Allowed |
| Indexed access | Yes |
| `push()` | Add to top |
| `pop()` | Remove top |
| `peek()` | View top |
| Empty `pop()` | `EmptyStackException` |
| `search()` | 1-based from top |
| Internal storage | Vector's dynamic array |
| Modern alternative | `Deque` + `ArrayDeque` |

---

## ▸ Must Remember

**Stack = Legacy + Vector + Synchronized + LIFO**

**ArrayDeque = Modern + Deque + Unsynchronized + LIFO/FIFO**

For interviews, the most important comparison is:

> **Stack is synchronized but legacy; ArrayDeque is unsynchronized but modern and generally preferred for normal stack usage.**