# 03. Strings

> Strings are one of the most frequently asked Java interview topics. Questions usually revolve around immutability, the String Pool, memory management, comparison methods, and mutable alternatives like `StringBuilder`.

**Previous:** [OOP](02-OOP.md)  
**Next:** [Arrays](04-Arrays.md)

---

# What is a String? ⭐⭐⭐

A **String** is an immutable sequence of characters represented by the `String` class.

```java
String name = "Rahul";
```

- `String` is **not** a primitive data type.
- It is an object of `java.lang.String`.
- Strings are immutable, meaning they cannot be modified after creation.

---

# Ways to Create Strings ⭐⭐⭐

## 1. String Literal (Uses String Pool)

```java
String s1 = "Java";
String s2 = "Java";
```

Both `s1` and `s2` point to the **same object** in the String Pool.

## 2. Using `new` Keyword

```java
String s3 = new String("Java");
```

This creates a **new object in the Heap**, even if `"Java"` already exists in the pool.

### Interview Difference

| Literal                        | `new String()`       |
| ------------------------------ | -------------------- |
| Uses String Pool               | Creates Heap object  |
| Memory efficient               | Always new object    |
| Reuses existing string literal | Doesn't reuse object |

---

# String Constant Pool (SCP) ⭐⭐⭐

The **String Pool** is a special memory area inside the Heap that stores unique string literals.

```java
String a = "Hello";
String b = "Hello";
String c = new String("Hello");
```

### Memory

- `a` → Pool
- `b` → Same Pool Object
- `c` → New Heap Object

### Why use String Pool?

- Reduces duplicate objects
- Saves memory
- Improves performance

> **Interview:** String literals are stored in the String Constant Pool.

---

# Why Strings are [Immutable](Miscellaneous/Immutable-Class.md)
⭐⭐⭐

Once created, a string cannot be changed.

```java
String s = "Java";

s.concat(" Programming");

System.out.println(s);
```

Output:

```text
Java
```

Because `concat()` creates a **new string** instead of modifying the original.

## Why did Java make Strings immutable?

- Security (URLs, file paths)
- Thread safety
- String Pool optimization
- HashMap key reliability

> **Interview:** Immutability makes Strings thread-safe and allows safe reuse in the String Pool.

---

# String Memory Example ⭐⭐⭐

```java
String s1 = "Java";
String s2 = "Java";

s1 = s1 + "8";
```

Result:

- `"Java"` remains unchanged.
- `"Java8"` becomes a new object.
- `s2` still points to `"Java"`.

---

# String Comparison ⭐⭐⭐

## 1. `==`

Compares **references**.

```java
String a = "Java";
String b = "Java";

System.out.println(a == b);
```

Output:

```text
true
```

## 2. `equals()`

Compares **content**.

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a.equals(b));
```

Output:

```text
true
```

## 3. `equalsIgnoreCase()`

```java
"java".equalsIgnoreCase("JAVA");
```

Output:

```text
true
```

### Interview Table

| Method | Compares |
|---------|----------|
| `==` | Reference |
| `equals()` | Content |
| `equalsIgnoreCase()` | Content (Ignore case) |

---

# Common String Methods ⭐⭐⭐

## Length

```java
String s = "Hello";

s.length();
```

## Character Access

```java
s.charAt(1);
```

Output:

```text
e
```

## Substring

```java
String s = "Programming";

s.substring(3);
```

Output:

```text
gramming
```

```java
s.substring(3, 7);
```

Output:

```text
gram
```

## Contains

```java
s.contains("gram");
```

## Starts / Ends

```java
s.startsWith("Pro");
s.endsWith("ing");
```

## Index

```java
s.indexOf("g");
s.lastIndexOf("g");
```

---

# Concatenation ⭐⭐

## Using `+`

```java
String name = "Rahul";

String result = name + " Kumar";
```

## Using `concat()`

```java
String s = "Java";

s = s.concat(" 21");
```

Output:

```text
Java 21
```

---

# StringBuilder ⭐⭐⭐

`StringBuilder` is a **mutable** sequence of characters.

```java
StringBuilder sb = new StringBuilder("Java");

sb.append(" 21");

System.out.println(sb);
```

Output:

```text
Java 21
```

## Common Methods

```java
sb.append("A");
sb.insert(1, "X");
sb.delete(1,3);
sb.reverse();
sb.replace(0,2,"Hi");
```

---

# StringBuffer ⭐⭐

`StringBuffer` is similar to `StringBuilder` but is **thread-safe**.

```java
StringBuffer sb = new StringBuffer();

sb.append("Java");
```

---

# String vs StringBuilder vs StringBuffer ⭐⭐⭐

| Feature | String | StringBuilder | StringBuffer |
|---------|--------|--------------|--------------|
| Mutable | ❌ | ✅ | ✅ |
| Thread-safe | Yes (Immutable) | ❌ | ✅ |
| Performance | Slow | Fastest | Slower |
| Use Case | Read-only text | Single-thread modifications | Multi-thread modifications |

> **Interview:** Use `StringBuilder` for frequent string modifications.

---

# StringBuilder vs `+` Operator ⭐⭐⭐

Using `+` inside loops creates multiple objects.

```java
String s = "";

for(int i=0;i<3;i++){
    s += i;
}
```

Creates:

```text
""
"0"
"01"
"012"
```

Using `StringBuilder`:

```java
StringBuilder sb = new StringBuilder();

for(int i=0;i<3;i++){
    sb.append(i);
}
```

Only one mutable object is used.

---

# `intern()` Method ⭐⭐⭐

`intern()` moves or returns a string from the String Pool.

```java
String s1 = new String("Java");
String s2 = s1.intern();

String s3 = "Java";

System.out.println(s2 == s3);
```

Output:

```text
true
```

### Interview

`intern()` returns the pooled reference of a string.

---

# Empty vs Blank ⭐⭐

```java
String s1 = "";
String s2 = "   ";
```

| Method | Result |
|---------|--------|
| `s1.isEmpty()` | `true` |
| `s2.isEmpty()` | `false` |
| `s2.isBlank()` | `true` |

`isBlank()` was introduced in **Java 11**.

---

# String to Primitive Conversion ⭐⭐

## String → int

```java
int num = Integer.parseInt("123");
```

## int → String

```java
String s = String.valueOf(123);
```

## char → int

```java
char ch = '7';

int num = ch - '0';
```

Output:

```text
7
```

---

# Split & Join ⭐⭐

## Split

```java
String s = "A,B,C";

String[] arr = s.split(",");
```

## Join

```java
String result = String.join("-", "A", "B", "C");
```

Output:

```text
A-B-C
```

---

# String Pool Interview Programs ⭐⭐⭐

## Case 1

```java
String s1 = "Java";
String s2 = "Java";

System.out.println(s1 == s2);
```

✅ `true`

---

## Case 2

```java
String s1 = new String("Java");
String s2 = new String("Java");

System.out.println(s1 == s2);
```

❌ `false`

---

## Case 3

```java
String s1 = "Java";
String s2 = new String("Java");

System.out.println(s1.equals(s2));
```

✅ `true`

---

# Frequently Asked Interview Questions ⭐⭐⭐

### Why is String immutable?

To provide thread safety, enable String Pool optimization, improve security, and make hash-based collections reliable.

### Difference between `==` and `equals()`?

- `==` compares object references.
- `equals()` compares actual content.

### What is the String Pool?

A special area in Heap that stores unique string literals to avoid duplicate objects.

### Difference between StringBuilder and StringBuffer?

- `StringBuilder` → Faster, not synchronized.
- `StringBuffer` → Thread-safe, synchronized.

### When should you use StringBuilder?

Whenever strings are modified repeatedly, especially inside loops.

### Are Strings Stored in Stack Memory? ⭐⭐

**Answer:** No. String objects are always stored in the **Heap** (String Pool is part of the Heap). Only the **reference variable** is stored on the Stack when it is a local variable.

```java
public static void main(String[] args) {
    String s = "Java";
}
```

| Memory | Stores |
|---------|--------|
| Stack | Reference `s` |
| Heap (String Pool) | `"Java"` object |

> **Interview:** Stack stores the reference, Heap stores the String object.


### How is String Immutable Internally? ⭐⭐⭐

Java makes `String` immutable using three principles:

1. `String` is a **final class**.
2. Its internal character array is **private and final**.
3. All modifying methods create a **new String object**.

### Simplified Internal Structure

```java
public final class String {
    private final byte[] value;
}
```

### Example

```java
String s1 = "Java";

String s2 = s1.concat(" 21");

System.out.println(s1);   // Java
System.out.println(s2);   // Java 21
```

`concat()` does not modify `s1`; it creates a new object.

> **Interview:** String is immutable because it is `final`, its data is private, and modifications always create new objects.


### What Happens to Old String Objects? ⭐⭐⭐

Strings are never modified. A new object is created, and the old object becomes eligible for **Garbage Collection** if no reference points to it.

### Heap String Example

```java
String s = new String("Java");

s = new String("Python");
```

Result:

- `"Python"` → Referenced by `s`
- Old `"Java"` → Eligible for Garbage Collection

### Important

```java
String s = "Java";

s = s.concat(" 21");
```

The literal `"Java"` remains in the **String Pool** and can still be reused by other references.

> **Interview:** Old heap String objects become eligible for GC only when they become unreachable.


### `String s = "Java"` vs `new String("Java")` ⭐⭐⭐

### String Literal

```java
String s1 = "Java";
String s2 = "Java";
```

- Uses String Pool
- Reuses the same object

```java
s1 == s2      // true
```

### Using `new`

```java
String s1 = new String("Java");
String s2 = new String("Java");
```

- Creates separate Heap objects

```java
s1 == s2          // false
s1.equals(s2)     // true
```

### Comparison

| `"Java"` | `new String("Java")` |
|----------|----------------------|
| String Pool | Heap |
| Reuses object | Always creates new object |
| Memory efficient | Extra memory |

> **Interview:** Prefer string literals unless a separate object is explicitly required.


### How to Split a String on Multiple Spaces? ⭐⭐

Use the regular expression `\\s+`.

```java
String str = "Java   is    awesome";

String[] words = str.split("\\s+");
```

Output:

```text
[Java, is, awesome]
```

### Why `\\s+`?

| Regex | Meaning |
|--------|---------|
| `" "` | One space only |
| `"\\s"` | Single whitespace |
| `"\\s+"` | One or more whitespaces |

> **Interview:** `split("\\s+")` handles multiple spaces, tabs, and other whitespace characters.


### `trim()` vs `strip()` ⭐⭐

Both remove leading and trailing whitespace, but `strip()` supports **Unicode whitespace**.

### Normal Spaces

```java
String s = "   Java   ";

s.trim();
s.strip();
```

Both return:

```text
Java
```

### Unicode Space

```java
String s = "\u2003Java\u2003";
```

| Method | Result |
|---------|--------|
| `trim()` | Unicode space remains |
| `strip()` | `"Java"` |

### Comparison

| Method | Removes |
|---------|---------|
| `trim()` | Basic ASCII whitespace |
| `strip()` | Unicode + ASCII whitespace |
| `stripLeading()` | Leading only |
| `stripTrailing()` | Trailing only |

> **Interview:** Prefer `strip()` in Java 11+ because it correctly handles international Unicode whitespace.
---

# Revision Checklist

- [ ] String Literal vs `new String()`
- [ ] String Constant Pool
- [ ] Immutability
- [ ] `==` vs `equals()`
- [ ] `intern()`
- [ ] `substring()`, `charAt()`, `split()`
- [ ] StringBuilder
- [ ] StringBuffer
- [ ] `isEmpty()` vs `isBlank()`
- [ ] Primitive ↔ String conversion

---

# What's Next

Continue with **[04. Arrays](04-Arrays.md)**.