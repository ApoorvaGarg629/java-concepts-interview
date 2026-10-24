# 01. Collections Framework Overview

> The Java Collections Framework (JCF) is a unified architecture of **interfaces, implementations, and utility algorithms** used to store and manipulate groups of objects efficiently.

**Source:** Oracle Java Collections Framework

---

# Folder Structure

```text
02-Collections/
│
├── 01-Collections-Overview.md
│
├── 02-List/
│   ├── 01-ArrayList.md
│   ├── 02-LinkedList.md
│   └── 03-Stack.md
│
├── 03-Set/
│   ├── 01-HashSet.md
│   ├── 02-LinkedHashSet.md
│   ├── 03-TreeSet.md
│   └── 04-EnumSet.md
│
├── 04-Queue/
│   ├── 01-PriorityQueue.md
│   ├── 02-ArrayDeque.md
│   └── 03-Deque.md
│
├── 05-Map/
│   ├── 01-HashMap.md
│   ├── 02-LinkedHashMap.md
│   ├── 03-TreeMap.md
│   ├── 04-Hashtable.md
│   ├── 05-ConcurrentHashMap.md
│   └── 06-EnumMap.md
│
├── 06-Iterators/
│   ├── 01-Iterator.md
│   ├── 02-ListIterator.md
│   └── 03-FailFast-vs-FailSafe.md
│
└── 07-Utilities/
    ├── 01-Collections-Class.md
    ├── 02-Comparable.md
    └── 03-Comparator.md
```

---

# What is the Collections Framework? ⭐⭐⭐

The Collections Framework is a **set of interfaces, classes, and algorithms** that provides ready-made data structures like Lists, Sets, Queues, and Maps. It reduces development effort by providing reusable and optimized implementations. <Cite refs={["turn0search0","turn0search3"]}/>

### It consists of three parts

| Part | Example |
|------|---------|
| Interfaces | List, Set, Queue, Map |
| Implementations | ArrayList, HashSet, HashMap |
| Algorithms | sort(), binarySearch(), reverse() |

---

# Collections Hierarchy ⭐⭐⭐

```text
Iterable
    │
Collection (Interface)
├── List
│   ├── ArrayList
│   ├── LinkedList
│   ├── Vector
│   └── Stack
│
├── Set
│   ├── HashSet
│   ├── LinkedHashSet
│   ├── TreeSet
│   └── EnumSet
│
└── Queue
    ├── PriorityQueue
    ├── Deque
    └── ArrayDeque

Map (Separate Interface)
├── HashMap
├── LinkedHashMap
├── TreeMap
├── Hashtable
├── ConcurrentHashMap
└── EnumMap
```

> **Interview:** `Map` is **part of the Collections Framework**, but it **does not extend the Collection interface**. <Cite refs={["turn0search1","turn0search2"]}/>

---

# Collection vs Collections ⭐⭐⭐

This is one of the most frequently asked interview questions.

| Collection | Collections |
|------------|-------------|
| Interface | Utility class |
| Stores a group of objects | Provides utility methods |
| Extended by List, Set, Queue | Contains static methods |
| `add()`, `remove()` | `sort()`, `reverse()`, `shuffle()` |

### Collection (Interface)

```java
Collection<String> names = new ArrayList<>();

names.add("Rahul");
names.add("Amit");
```

### Collections (Utility Class)

```java
Collections.sort(list);

Collections.reverse(list);

Collections.shuffle(list);
```

> **Memory Tip:** `Collection` = Data Structure, `Collections` = Helper Class.

---

# Iterable Interface ⭐⭐

`Iterable` is the **topmost interface** in the hierarchy. It enables the enhanced for-each loop by providing the `iterator()` method. <Cite ref=turn0search2/>

```java
for(String name : names){
    System.out.println(name);
}
```

---

# Core Collection Interfaces ⭐⭐⭐

### List

- Ordered
- Allows duplicates
- Index-based access

Examples:

- [ArrayList](02-List/01-ArrayList.md)
- [LinkedList](02-List/02-LinkedList.md)
- [Vector](02-List/03-Vector.md)
- [Stack](02-List/04-Stack.md)

---

### Set

- Unique elements
- No duplicates
- Mostly unordered

Examples:

- [HashSet](03-Set/01-HashSet.md)
- [LinkedHashSet](03-Set/02-LinkedHashSet.md)
- [TreeSet](03-Set/03-TreeSet.md)
- [EnumSet](03-Set/04-EnumSet.md)

---

### Queue

- FIFO by default
- Used for scheduling and processing

Examples:

- [PriorityQueue](04-Queue/01-PriorityQueue.md)
- [ArrayDeque](04-Queue/02-ArrayDeque.md)
- [Deque](04-Queue/03-Deque.md)

---

### Map

Stores **key-value pairs**.

Examples:

- [HashMap](05-Map/01-HashMap.md)
- [LinkedHashMap](05-Map/02-LinkedHashMap.md)
- [TreeMap](05-Map/03-TreeMap.md)
- [Hashtable](05-Map/04-Hashtable.md)
- [ConcurrentHashMap](05-Map/05-ConcurrentHashMap.md)

> `Map` does not inherit from `Collection`.

---

# Which Collection Should I Choose? ⭐⭐⭐

| Requirement | Best Choice |
|------------|-------------|
| Ordered + Fast Access | ArrayList |
| Frequent Insert/Delete | LinkedList |
| Unique Elements | HashSet |
| Sorted Unique Elements | TreeSet |
| Key-Value Storage | HashMap |
| Sorted Keys | TreeMap |
| Thread-safe Map | ConcurrentHashMap |
| Priority Processing | PriorityQueue |

---

# List vs Set vs Queue vs Map ⭐⭐⭐

| Feature | List | Set | Queue | Map |
|----------|------|-----|-------|-----|
| Duplicates | ✅ | ❌ | ✅ | Keys ❌ |
| Order | ✅ | Depends | FIFO/Priority | Depends |
| Index Access | ✅ | ❌ | ❌ | ❌ |
| Key-Value | ❌ | ❌ | ❌ | ✅ |

---

# Common Methods of Collection Interface ⭐⭐

```java
Collection<String> list = new ArrayList<>();
```

| Method | Purpose |
|---------|----------|
| `add()` | Insert |
| `remove()` | Delete |
| `contains()` | Search |
| `size()` | Count elements |
| `isEmpty()` | Check empty |
| `clear()` | Remove all |
| `iterator()` | Traverse |

Example:

```java
list.add("Java");

list.remove("Java");

list.contains("Java");

list.size();
```

---

# Time Complexity Overview ⭐⭐⭐

| Collection | Search | Insert | Delete |
|------------|--------|--------|--------|
| ArrayList | O(n) | O(1)* | O(n) |
| LinkedList | O(n) | O(1) | O(1) |
| HashSet | O(1) | O(1) | O(1) |
| TreeSet | O(log n) | O(log n) | O(log n) |
| HashMap | O(1) | O(1) | O(1) |
| TreeMap | O(log n) | O(log n) | O(log n) |

> `*` ArrayList insertion at the end is amortized O(1).

---

# Interview Questions ⭐⭐⭐

### Is Map part of Collection?

**Answer:** `Map` is part of the **Collections Framework**, but it **does not implement the Collection interface**.

### Why does Collection extend Iterable?

To support iteration using `iterator()` and the enhanced for-each loop.

### Why should we program to interfaces?

```java
List<String> list = new ArrayList<>();
```

Instead of:

```java
ArrayList<String> list = new ArrayList<>();
```

This allows changing the implementation without changing the client code.

### Why use Collections class?

To perform common operations like sorting, reversing, shuffling, finding min/max, etc.

---
# What's Next

## List Implementations

- [ArrayList](02-Collections/02-List/01-ArrayList.md)
- [LinkedList](02-Collections/02-List/02-LinkedList.md)
- [Stack](02-Collections/02-List/04-Stack.md)

## Set Implementations

- [HashSet](02-Collections/03-Set/01-HashSet.md)
- [LinkedHashSet](02-Collections/03-Set/02-LinkedHashSet.md)
- [TreeSet](02-Collections/03-Set/03-TreeSet.md)
- [EnumSet](02-Collections/03-Set/04-EnumSet.md)

## Queue Implementations

- [PriorityQueue](02-Collections/04-Queue/01-PriorityQueue.md)
- [ArrayDeque](02-Collections/04-Queue/02-ArrayDeque.md)
- [Deque](02-Collections/04-Queue/03-Deque.md)

## Map Implementations

- [HashMap](02-Collections/05-Map/01-HashMap.md)
- [LinkedHashMap](02-Collections/05-Map/02-LinkedHashMap.md)
- [TreeMap](02-Collections/05-Map/03-TreeMap.md)
- [Hashtable](02-Collections/05-Map/04-Hashtable.md)
- [ConcurrentHashMap](02-Collections/05-Map/05-ConcurrentHashMap.md)
- [EnumMap](02-Collections/05-Map/06-EnumMap.md)

## Utilities


- [Iterator](02-Collections/06-Utilities/01-Iterator.md)
- [ListIterator](02-Collections/06-Utilities/02-ListIterator.md)
- [Fail Fast vs Fail Safe](02-Collections/06-Utilities/03-FailFast-vs-FailSafe.md)
- [Comparable](02-Collections/06-Utilities/04-Comparable.md)
- [Comparator](02-Collections/06-Utilities/05-Comparator.md)
- [Collections Utility Class](02-Collections/06-Utilities/06-Collections-Class.md)

## Internal Working

- [ArrayList Internal](02-Collections/09-Internal-Working/01-ArrayList-Internal.md)
- [HashMap Internal](02-Collections/09-Internal-Working/02-HashMap-Internal.md)
- [HashSet Internal](02-Collections/09-Internal-Working/03-HashSet-Internal.md)
- [LinkedHashMap Internal](02-Collections/09-Internal-Working/04-LinkedHashMap-Internal.md)
- [TreeMap Red-Black Tree](02-Collections/09-Internal-Working/05-TreeMap-Red-Black-Tree.md)
- [ConcurrentHashMap Internal](02-Collections/09-Internal-Working/06-ConcurrentHashMap-Internal.md)
- [PriorityQueue Binary Heap](02-Collections/09-Internal-Working/07-PriorityQueue-Heap.md)
---
# Revision Checklist

- [ ] Collections Framework definition
- [ ] Iterable → Collection hierarchy
- [ ] List vs Set vs Queue vs Map
- [ ] Collection vs Collections
- [ ] Common Collection methods
- [ ] Time complexity overview
- [ ] Which implementation to choose