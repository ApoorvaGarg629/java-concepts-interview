# 05. Generics

> Generics provide **compile-time type safety** by allowing classes, interfaces, and methods to work with different data types without sacrificing type safety.

**Previous:** [Arrays](04-Arrays.md)  
**Next:** [Exception Handling](06-Exception-Handling.md)

---

# What are Generics? ⭐⭐⭐

Generics allow you to write **one reusable implementation** that can work with multiple data types.

Without generics:

```java
ArrayList list = new ArrayList();

list.add("Java");
list.add(10);
```

Problem: Any type can be added, leading to runtime errors.

With generics:

```java
ArrayList<String> list = new ArrayList<>();

list.add("Java");
// list.add(10); ❌ Compile-time error
```

> **Interview:** Generics provide **type safety** and eliminate explicit type casting.

---

# Why do we need Generics? ⭐⭐⭐

### Without Generics

```java
ArrayList list = new ArrayList();

list.add("Rahul");

String name = (String) list.get(0);
```

Requires manual casting.

### With Generics

```java
ArrayList<String> list = new ArrayList<>();

list.add("Rahul");

String name = list.get(0);
```

No casting required.

### Benefits

- Compile-time type checking
- Reusable code
- No explicit casting
- Better readability

---

# Generic Class ⭐⭐⭐

A class can accept a type parameter using `<T>`.

```java
class Box<T> {

    private T value;

    public void set(T value){
        this.value = value;
    }

    public T get(){
        return value;
    }
}
```

Usage:

```java
Box<String> box = new Box<>();

box.set("Java");

System.out.println(box.get());
```

Another type:

```java
Box<Integer> box = new Box<>();

box.set(100);
```

The same class works for multiple types.

---

# Generic Interface ⭐⭐

```java
interface Printer<T> {

    void print(T value);
}
```

Implementation:

```java
class StringPrinter implements Printer<String>{

    public void print(String value){
        System.out.println(value);
    }
}
```

---

# Generic Method ⭐⭐⭐

A method can have its own type parameter.

```java
class Utility {

    public static <T> void display(T value){
        System.out.println(value);
    }
}
```

Usage:

```java
Utility.display("Java");
Utility.display(10);
Utility.display(true);
```

> Notice `<T>` appears **before the return type**.

---

# Type Parameters ⭐⭐

Java uses common naming conventions.

| Symbol | Meaning |
|---------|----------|
| `T` | Type |
| `E` | Element |
| `K` | Key |
| `V` | Value |
| `N` | Number |

Example:

```java
Map<String, Integer> map = new HashMap<>();
```

- `K` → String
- `V` → Integer

---

# Multiple Generic Types ⭐⭐

```java
class Pair<K, V>{

    K key;
    V value;

    Pair(K key, V value){
        this.key = key;
        this.value = value;
    }
}
```

Usage:

```java
Pair<Integer, String> p =
    new Pair<>(1, "Rahul");
```

---

# Wildcards (`?`) ⭐⭐⭐

Wildcards are used when the exact type is unknown.

There are three types.

## 1. Unbounded Wildcard `<?>`

Accepts any type.

```java
public void print(List<?> list){

    for(Object obj : list){
        System.out.println(obj);
    }
}
```

Works with:

```java
List<String>
List<Integer>
List<Double>
```

Use when you only **read** values.

---

## 2. Upper Bounded Wildcard `<? extends T>` ⭐⭐⭐

Accepts **T and its subclasses**.

```java
public double sum(List<? extends Number> list){

    double total = 0;

    for(Number n : list){
        total += n.doubleValue();
    }

    return total;
}
```

Valid:

```java
List<Integer>
List<Double>
List<Float>
```

Invalid:

```java
List<String>
```

### Memory Trick

**Extends = Read**

You can safely read values as `Number`, but you **cannot add** new elements.

---

## 3. Lower Bounded Wildcard `<? super T>` ⭐⭐⭐

Accepts **T and its parent classes**.

```java
public void addNumbers(List<? super Integer> list){

    list.add(10);
    list.add(20);
}
```

Valid:

```java
List<Integer>
List<Number>
List<Object>
```

### Memory Trick

**Super = Write**

Used when adding elements.

---

# extends vs super ⭐⭐⭐

| Wildcard | Use |
|-----------|-----|
| `<? extends T>` | Read |
| `<? super T>` | Write |

### Easy Rule

> **PECS**

- **Producer → Extends**
- **Consumer → Super**

If a collection **produces** values → `extends`

If a collection **consumes** values → `super`

---

# Type Erasure ⭐⭐⭐

Generics exist only during **compilation**.

```java
List<String> list = new ArrayList<>();
```

After compilation, JVM sees:

```java
List list = new ArrayList();
```

This process is called **Type Erasure**.

### Why?

Java introduced generics in Java 5 while remaining compatible with older JVMs.

> **Interview:** Generics provide compile-time safety; runtime uses type erasure.

---

# Generic Restrictions ⭐⭐

## Cannot create Generic Arrays

❌ Invalid

```java
T[] arr = new T[10];
```

Reason: Type erasure.

---

## Cannot instantiate Type Parameter

❌ Invalid

```java
T obj = new T();
```

Use:

```java
Supplier<T>
Reflection
Factory Pattern
```

---

## Cannot use Primitive Types

❌ Invalid

```java
List<int> list;
```

✅ Correct

```java
List<Integer> list;
```

Generics work only with **reference types**.

---

# Generic Constructor ⭐

A constructor can also be generic.

```java
class Demo {

    <T> Demo(T value){
        System.out.println(value);
    }
}
```

Usage:

```java
new Demo("Java");
new Demo(100);
```

---

# Raw Type ⭐⭐

Using a generic class without specifying a type is called a **raw type**.

```java
ArrayList list = new ArrayList();

list.add("Java");
list.add(100);
```

Problems:

- No type safety
- Runtime casting
- Compiler warnings

Always prefer:

```java
ArrayList<String> list = new ArrayList<>();
```

---

# Diamond Operator `<>` ⭐⭐

Introduced in Java 7.

Instead of:

```java
Map<String, Integer> map =
    new HashMap<String, Integer>();
```

Write:

```java
Map<String, Integer> map =
    new HashMap<>();
```

The compiler automatically infers the types.

---

# Real Interview Examples ⭐⭐⭐

## Scenario 1

```java
List<?> list = List.of("A", "B");
```

Can you add?

```java
list.add("C");
```

❌ No.

Reason: Exact type is unknown.

---

## Scenario 2

```java
List<? extends Number> list =
    List.of(1,2,3);
```

Can you read?

```java
Number n = list.get(0);
```

✅ Yes.

Can you add?

```java
list.add(10);
```

❌ No.

---

## Scenario 3

```java
List<? super Integer> list =
    new ArrayList<Number>();

list.add(10);
```

Can you read as Integer?

```java
Integer n = list.get(0);
```

❌ No.

Only `Object` is guaranteed.

---

# Most Asked Interview Questions ⭐⭐⭐

### Why do we use Generics?

To provide compile-time type safety and eliminate explicit casting.

### What is Type Erasure?

The compiler removes generic type information before runtime for backward compatibility.

### Difference between `extends` and `super`?

- `extends` → Read (Producer)
- `super` → Write (Consumer)

### Why can't Generics use primitive types?

Because generics work only with **objects**, so wrapper classes are used instead.

### What is the Diamond Operator?

`<>` lets the compiler infer generic types, reducing boilerplate code.

---
---

# Interview Q&A ⭐⭐⭐

## Q1. Why do we declare `<T>` before the return type?

`T` is a **type parameter**, not the return type. It tells the compiler that the method can work with any type.

```java
public static <T> T identity(T value){
    return value;
}
```

Without declaring `<T>`, the compiler doesn't know what `T` is.

❌ Invalid

```java
public static T identity(T value) { }
```

---

## Q2. Does a generic method have to be `static`?

**No.** `static` and generics are unrelated.

### Instance generic method

```java
class Box {
    public <T> T get(T value){
        return value;
    }
}
```

### Static generic method

```java
class Utility {
    public static <T> T get(T value){
        return value;
    }
}
```

- `static` → Belongs to the class.
- `<T>` → Declares a method-level type parameter.

---

## Q3. Why can't we add to `List<? extends Number>`?

Because `?` means **unknown subtype of Number**.

```java
List<? extends Number> list = new ArrayList<Integer>();
```

The compiler treats it as:

> "A list of some unknown type extending Number."

It could be `Integer`, `Double`, or `Float`, so adding any value could break type safety.

```java
list.add(10);      // ❌
list.add(2.5);     // ❌
```

Reading is always safe.

```java
Number n = list.get(0);   // ✅
```

**Memory Rule:** `extends = Read`

---

## Q4. But the compiler knows it's `ArrayList<Integer>`, right?

Yes, it knows the **object type**, but it checks operations using the **reference type**.

```java
List<? extends Number> list = new ArrayList<Integer>();
```

The contract is `List<? extends Number>`, so the exact subtype is intentionally hidden.

| Object Type | Reference Type |
|-------------|----------------|
| `ArrayList<Integer>` | `List<? extends Number>` |

Java always follows the **left-hand side** for allowed operations.

---

## Q5. Is this the same?

```java
List<? extends Number> list = new ArrayList<Integer>();
```

```java
List<? extends Number> list = new ArrayList<>();
```

**No.**

The diamond operator infers the concrete type from the target type.

```java
new ArrayList<>()
```

becomes

```java
new ArrayList<Number>()
```

So:

| Code | Actual Object |
|------|---------------|
| `new ArrayList<Integer>()` | `ArrayList<Integer>` |
| `new ArrayList<>()` | `ArrayList<Number>` |

Both still behave the same because the reference type is `List<? extends Number>`.

---

## Q6. When should I use `extends` vs `super`?

Remember **PECS**:

| Wildcard | Think | Use |
|----------|-------|-----|
| `<? extends T>` | Producer | Read |
| `<? super T>` | Consumer | Write |

```java
List<? extends Number> nums;   // Read
List<? super Integer> ints;    // Write
```

> **Interview One-liner:** Producer Extends, Consumer Super.

---

# Revision Checklist

- [ ] Generic Class
- [ ] Generic Method
- [ ] Generic Interface
- [ ] Type Parameters (`T, E, K, V`)
- [ ] Wildcards `?`
- [ ] `extends` vs `super`
- [ ] PECS Rule
- [ ] Type Erasure
- [ ] Raw Types
- [ ] Diamond Operator

---

# What's Next

Continue with **[06. Exception Handling](06-Exception-Handling.md)**.