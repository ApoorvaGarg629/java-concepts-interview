# 02. LinkedList

> `LinkedList` is a `List` and `Deque` implementation based on a **doubly linked list**. It is useful when frequent insertion/deletion is required, especially at the beginning or end.

**Interfaces:** `List`, `Deque`  
**Internal Structure:** Doubly Linked List  
**Internal Working:** [LinkedList Internal](https://chatgpt.com/c/02-Collections/09-Internal-Working/02-LinkedList-Internal.md)

---

## 1. Basic Characteristics ⭐⭐⭐

- Maintains insertion order
    
- Allows duplicate elements
    
- Allows `null`
    
- Dynamic size
    
- Not synchronized
    
- Supports both `List` and `Deque` operations
    
- Does **not** provide fast random access
    

```java
List<String> list = new LinkedList<>();

list.add("Java");
list.add("Spring");
list.add("Java");
```

Output:

```text
[Java, Spring, Java]
```

---

## 2. LinkedList Structure ⭐⭐⭐

Each node contains:

```text
┌─────────┬─────────┬─────────┐
│  prev   │  data   │  next   │
└─────────┴─────────┴─────────┘
```

Example:

```text
null ← [A] ⇄ [B] ⇄ [C] → null
```

Each node maintains references to:

- Previous node
    
- Current element
    
- Next node
    

---

## 3. Creating LinkedList

```java
List<Integer> list = new LinkedList<>();
```

Or:

```java
LinkedList<Integer> list = new LinkedList<>();
```

You can also initialize from another collection:

```java
List<Integer> list =
        new LinkedList<>(Arrays.asList(1, 2, 3));
```

---

## 4. Adding Elements ⭐⭐⭐

### `add()`

```java
list.add(10);
list.add(20);
```

Adds at the end.

### `add(index, element)`

```java
list.add(1, 15);
```

Inserts at the specified position.

### `addFirst()`

```java
LinkedList<Integer> list = new LinkedList<>();

list.addFirst(10);
```

### `addLast()`

```java
list.addLast(20);
```

---

## 5. Accessing Elements ⭐⭐⭐

```java
list.get(2);
```

Unlike `ArrayList`, LinkedList does **not** provide O(1) random access.

To access an element at index `i`, it must traverse the nodes.

### Optimization

LinkedList can start from the **head or tail**, whichever is closer.

```text
Index near beginning → traverse from head
Index near end       → traverse from tail
```

Still **O(n)** in the general case.

---

## 6. Removing Elements ⭐⭐⭐

### By Index

```java
list.remove(2);
```

### By Object

```java
list.remove(Integer.valueOf(20));
```

### First Element

```java
list.removeFirst();
```

### Last Element

```java
list.removeLast();
```

### First/Last Without Exception

```java
list.pollFirst();
list.pollLast();
```

`pollFirst()` / `pollLast()` return `null` when the deque is empty.

---

## 7. `get()` vs `peek()` ⭐⭐

```java
list.getFirst();
list.peekFirst();
```

Difference:

|Method|Empty List|
|---|---|
|`getFirst()`|Throws `NoSuchElementException`|
|`peekFirst()`|Returns `null`|

Similarly:

```java
getLast()
peekLast()
```

---

## 8. LinkedList as a Deque ⭐⭐⭐

Because `LinkedList` implements `Deque`, it can be used as both a queue and a stack.

### Queue

```java
Deque<Integer> queue = new LinkedList<>();

queue.offer(10);
queue.offer(20);

queue.poll();
```

### Stack

```java
Deque<Integer> stack = new LinkedList<>();

stack.push(10);
stack.push(20);

stack.pop();
```

> **Interview:** [`ArrayDeque`](Miscellaneous/ArrayDeque.md) is generally preferred over `LinkedList` when you specifically need a stack or queue.

---

## 9. Traversal

### For-each

```java
for(String s : list) {
    System.out.println(s);
}
```

### Iterator

```java
Iterator<String> it = list.iterator();

while(it.hasNext()) {
    System.out.println(it.next());
}
```

### ListIterator

```java
ListIterator<String> it =
        list.listIterator();

while(it.hasNext()) {
    System.out.println(it.next());
}
```

`ListIterator` supports traversal in both directions.

---

## 10. Time Complexity ⭐⭐⭐

|Operation|LinkedList|
|---|--:|
|Add first|O(1)|
|Add last|O(1)|
|Remove first|O(1)|
|Remove last|O(1)|
|Get first|O(1)|
|Get last|O(1)|
|`get(index)`|O(n)|
|Search|O(n)|
|Add at known node|O(1)|
|Remove at known node|O(1)|

### Important Interview Point

Saying **"LinkedList insertion is always O(1)"** is incomplete.

```java
list.add(500, value);
```

Finding index `500` itself can take **O(n)**.

So insertion by index is generally **O(n)**.

---

## 11. ArrayList vs LinkedList ⭐⭐⭐

|Feature|ArrayList|LinkedList|
|---|---|---|
|Internal structure|Dynamic array|Doubly linked list|
|Random access|O(1)|O(n)|
|Add at end|O(1)*|O(1)|
|Add/remove first|O(n)|O(1)|
|Memory|Less|More|
|Cache locality|Better|Worse|
|Implements Deque|❌|✅|

`*` Amortized O(1)

### Which one should you choose?

**Mostly reading/accessing by index:**

```java
ArrayList
```

**Frequent insertion/removal at beginning/end:**

```java
LinkedList
```

> In real-world Java applications, `ArrayList` is usually the better default. Use `LinkedList` when its deque behavior or specific insertion/removal characteristics actually benefit your workload.

---

## 12. Why Does LinkedList Use More Memory? ⭐⭐

Each node stores:

```text
Previous reference
Element reference
Next reference
```

So compared with an array, LinkedList has additional memory overhead for node objects and references.

---

## 13. Null Values

LinkedList allows `null`.

```java
list.add(null);
```

Multiple `null` values are allowed.

---

## 14. Duplicate Values

Duplicates are allowed.

```java
list.add("Java");
list.add("Java");
```

Result:

```text
[Java, Java]
```

---

## 15. Thread Safety ⭐⭐⭐

`LinkedList` is **not synchronized**.

For a synchronized wrapper:

```java
List<String> list =
        Collections.synchronizedList(
                new LinkedList<>()
        );
```

For concurrent queue/deque requirements, consider concurrent collections such as:

```java
ConcurrentLinkedQueue
ConcurrentLinkedDeque
```

---

## 16. Copying LinkedList

### Reference Copy

```java
List<String> list2 = list1;
```

Both references point to the same LinkedList.

### Copy Constructor

```java
List<String> list2 =
        new LinkedList<>(list1);
```

Creates a new LinkedList with a **shallow copy** of the elements.

---

## 17. `clone()`

`LinkedList` provides a public `clone()` method.

```java
LinkedList<String> copy =
        (LinkedList<String>) list.clone();
```

This creates a new LinkedList, but the elements are copied as references.

Therefore, it is a **shallow copy**.

> Prefer the copy constructor when you want a clear, interface-friendly copy.

---

## 18. Common Interview Questions ⭐⭐⭐

### Why is LinkedList slow for `get(index)`?

Because it must traverse nodes to reach the requested index.

### Is LinkedList really O(1) for insertion?

Only when the insertion/removal position is already known through a node/iterator. Finding the position can take O(n).

### Why does LinkedList consume more memory than ArrayList?

Each node stores extra `prev` and `next` references in addition to the element.

### Is LinkedList synchronized?

No.

### Can LinkedList be used as a Stack?

Yes, because it implements `Deque`.

```java
Deque<Integer> stack = new LinkedList<>();
```

But `ArrayDeque` is generally preferred.

### Can LinkedList contain null?

Yes.

### Does LinkedList allow duplicates?

Yes.

---

## 19. Important Methods

```java
add()
addFirst()
addLast()

get()
getFirst()
getLast()

remove()
removeFirst()
removeLast()

offer()
offerFirst()
offerLast()

poll()
pollFirst()
pollLast()

peek()
peekFirst()
peekLast()

push()
pop()
```

---

## 20. Interview One-Liners ⭐⭐⭐

**LinkedList:**

> A doubly linked list implementation of `List` and `Deque` that provides efficient insertion/removal at the ends but slow random access.

**ArrayList vs LinkedList:**

> ArrayList is better for random access, while LinkedList is useful for frequent insertion/removal at the ends and deque operations.

**Why is `get()` O(n)?**

> LinkedList must traverse nodes from the head or tail to reach the requested index.

**Why more memory?**

> Each node stores the element plus `prev` and `next` references.

---

## Revision Checklist

-  Doubly linked list structure
    
-  `List` + `Deque` implementation
    
-  `addFirst()` / `addLast()`
    
-  `removeFirst()` / `removeLast()`
    
-  `get(index)` complexity
    
-  `getFirst()` vs `peekFirst()`
    
-  Queue and Stack using LinkedList
    
-  LinkedList vs ArrayList
    
-  Memory overhead
    
-  Thread safety
    
-  Shallow copy and `clone()`
    
-  Interview edge cases