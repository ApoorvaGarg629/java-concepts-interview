# 01. ArrayList

> `ArrayList` is the most commonly used implementation of the `List` interface. It provides **dynamic resizing, ordered storage, duplicate elements, and fast random access**.

**Interface:** `List<E>`  
**Internal Structure:** Dynamic Array  
**Internal Working:** [ArrayList Internal](../09-Internal-Working/01-ArrayList-Internal.md)

---

# What is ArrayList? ⭐⭐⭐

`ArrayList` is a **resizable array** that automatically grows when its capacity is exceeded. It maintains insertion order and allows duplicate elements. <Cite refs={["turn0search0","turn0search3"]}/>

```java
List<String> list = new ArrayList<>();

list.add("Java");
list.add("Python");
list.add("Java");
```

Output:

```text
[Java, Python, Java]
```

### Characteristics

- Ordered
- Duplicate elements allowed
- Index-based access
- Dynamic resizing
- Not synchronized

---

# ArrayList Hierarchy ⭐⭐

```text
Iterable
    │
Collection
    │
List
    │
ArrayList
```

```java
List<Integer> list = new ArrayList<>();
```

> **Interview:** Always program to the **List interface**, not the implementation.

---

# How to Create an ArrayList ⭐⭐

## Using List Interface (Recommended)

```java
List<String> list = new ArrayList<>();
```

## Direct Implementation

```java
ArrayList<String> list = new ArrayList<>();
```

### Initial Capacity

```java
ArrayList<Integer> list = new ArrayList<>(20);
```

Creates capacity for 20 elements.

> Capacity is **not** the same as size.

---

# Capacity vs Size ⭐⭐⭐

This is one of the most asked ArrayList questions.

```java
ArrayList<Integer> list = new ArrayList<>(10);
```

| Property | Value |
|----------|-------|
| Capacity | 10 |
| Size | 0 |

After:

```java
list.add(5);
```

| Property | Value |
|----------|-------|
| Capacity | 10 |
| Size | 1 |

### Memory Trick

- **Capacity** → Space available
- **Size** → Elements actually stored

---

# Common Methods ⭐⭐⭐

## add()

```java
list.add("Java");
```

Add at end.

### Add at Index

```java
list.add(1, "Python");
```

Shifts remaining elements.

---

## get()

```java
String value = list.get(0);
```

Time: **O(1)**

---

## set()

```java
list.set(0, "Spring");
```

Replaces an element.

---

## remove()

By index:

```java
list.remove(1);
```

By object:

```java
list.remove("Java");
```

> `remove(1)` removes by index, while `remove(Integer.valueOf(1))` removes the object.

---

## contains()

```java
list.contains("Java");
```

Returns boolean.

---

## size()

```java
list.size();
```

Number of elements.

---

## clear()

```java
list.clear();
```

Removes all elements.

---

## isEmpty()

```java
list.isEmpty();
```

---

# Traversing ArrayList ⭐⭐⭐

## Enhanced for Loop

```java
for(String s : list){
    System.out.println(s);
}
```

---

## Traditional Loop

```java
for(int i=0;i<list.size();i++){
    System.out.println(list.get(i));
}
```

Best when index is needed.

---

## Iterator

```java
Iterator<String> it = list.iterator();

while(it.hasNext()){
    System.out.println(it.next());
}
```

Useful for safe removal during iteration.

---

# Sorting ⭐⭐⭐

## Natural Order

```java
Collections.sort(list);
```

Ascending.

---

## Reverse Order

```java
Collections.sort(list, Collections.reverseOrder());
```

Descending.

---

## Using Comparator

```java
list.sort(Comparator.naturalOrder());
```

Modern Java approach.

---

# Copying ArrayList ⭐⭐⭐

## Reference Copy

```java
List<String> b = list;
```

Both refer to the same object.

---

## Copy Constructor (Recommended)

```java
List<String> copy = new ArrayList<>(list);
```

Creates a new list.

---

## clone()

```java
ArrayList<String> copy =
    (ArrayList<String>) list.clone();
```

Returns a **shallow copy**.

> `clone()` is available on `ArrayList`, **not** on the `List` interface.

---

# remove() Interview Trap ⭐⭐⭐

```java
ArrayList<Integer> list =
    new ArrayList<>();

list.add(10);
list.add(20);
list.add(30);

list.remove(1);
```

Output:

```text
[10, 30]
```

Removes by **index**.

To remove the value:

```java
list.remove(Integer.valueOf(20));
```

Output:

```text
[10, 30]
```

---

# Null Values ⭐⭐

`ArrayList` allows multiple null values.

```java
list.add(null);
list.add(null);
```

Valid.

---

# Time Complexity ⭐⭐⭐

| Operation | Complexity |
|-----------|------------|
| get | O(1) |
| set | O(1) |
| add(end) | O(1)* |
| add(index) | O(n) |
| remove(end) | O(1) |
| remove(index) | O(n) |
| contains | O(n) |

`*` Amortized O(1)

---

# ArrayList vs Array ⭐⭐⭐

| Array | ArrayList |
|--------|-----------|
| Fixed size | Dynamic |
| Primitive supported | Objects only |
| `length` | `size()` |
| Faster | Slight overhead |
| No add/remove | Supports add/remove |

---

# ArrayList vs LinkedList ⭐⭐⭐

| ArrayList | LinkedList |
|-----------|------------|
| Dynamic array | Doubly linked list |
| Fast random access | Slow random access |
| Slow middle insertion | Fast middle insertion |
| Less memory | More memory |

### When to choose?

| Scenario | Choose |
|----------|--------|
| Frequent reading | ArrayList |
| Frequent insertion/deletion | LinkedList |

---

# Thread Safety ⭐⭐⭐

`ArrayList` is **not synchronized**.

```java
List<String> list = new ArrayList<>();
```

If multiple threads modify it simultaneously, use:

```java
List<String> list =
    Collections.synchronizedList(new ArrayList<>());
```

or

```java
CopyOnWriteArrayList<String> list =
    new CopyOnWriteArrayList<>();
```

---

# Common Interview Edge Cases ⭐⭐⭐

## Can ArrayList store primitives?

❌ No

```java
ArrayList<int> list;
```

✅ Use wrapper classes.

```java
ArrayList<Integer> list;
```

---

## Does ArrayList allow duplicates?

```java
list.add("Java");
list.add("Java");
```

Yes.

---

## Does ArrayList maintain insertion order?

Yes.

```java
[Java, Python, Spring]
```

Order remains unchanged.

---

## Can ArrayList contain null?

Yes.

```java
list.add(null);
```

Valid.

---

## Is clone() available on List?

```java
List<String> list = new ArrayList<>();

list.clone();
```

❌ No.

Reason: `clone()` is defined in `ArrayList`, not in the `List` interface.

---

# Most Asked Interview Questions ⭐⭐⭐

### Why is ArrayList faster than LinkedList for `get()`?

Because it provides direct index access using an underlying array.

### Difference between capacity and size?

- Capacity → Allocated storage
- Size → Actual elements

### Is ArrayList synchronized?

No.

### Difference between `clone()` and copy constructor?

Both create a **new `ArrayList`** and create shallow copies, but the copy constructor works through the `List` interface and is generally preferred.

```java
ArrayList<Student> list1 = new ArrayList<>();

Student s = new Student("Rahul");
list1.add(s);

// Shallow copy using clone()
ArrayList<Student> list2 =
        (ArrayList<Student>) list1.clone();

// Shallow copy using copy constructor
List<Student> list3 =
        new ArrayList<>(list1);
```

### What happens?

```text
list1 ──► ArrayList #1 ──► Student("Rahul")
list2 ──► ArrayList #2 ──► same Student
list3 ──► ArrayList #3 ──► same Student
```

- `list1`, `list2`, and `list3` are **different ArrayList objects**.
    
- The `Student` object is **the same object** in all three lists.
    
- Only the **references to the elements** are copied.
    

### Why is `add(index)` O(n)?

Elements after the index must be shifted one position to the right.

---

# Revision Checklist

- [ ] ArrayList characteristics
- [ ] Capacity vs Size
- [ ] Common methods
- [ ] Traversal techniques
- [ ] remove() overload trap
- [ ] clone vs copy constructor
- [ ] Time complexities
- [ ] ArrayList vs LinkedList
- [ ] Thread safety
- [ ] Common interview questions