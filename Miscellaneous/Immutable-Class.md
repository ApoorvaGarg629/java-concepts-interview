# Immutable Class

> An immutable class is a class whose objects **cannot be modified after they are created**.

**Parent Note:** [Strings](../03-Strings.md)

---

# What is an Immutable Class? ⭐⭐⭐

An immutable class does not allow its state (field values) to change after object creation.

```java
String name = "Java";

name.concat(" 21");

System.out.println(name); // Java
```

`String` is the best example of an immutable class.

> **Interview:** Immutable objects are thread-safe because their state never changes.

---

# How to Create an Immutable Class ⭐⭐⭐

There are **5 rules** to make a class immutable.

## Rule 1: Make the class `final`

This prevents inheritance.

```java
public final class Employee {

}
```

Why?

If another class extends it, it could add setter methods or override behavior, breaking immutability.

---

## Rule 2: Make all fields `private final`

```java
public final class Employee {

    private final int id;
    private final String name;
}
```

- `private` → No direct access
- `final` → Can be assigned only once

---

## Rule 3: Initialize fields through constructor

```java
public final class Employee {

    private final int id;
    private final String name;

    public Employee(int id, String name){
        this.id = id;
        this.name = name;
    }
}
```

Once assigned, they cannot be changed.

---

## Rule 4: Provide only getters

```java
public int getId(){
    return id;
}

public String getName(){
    return name;
}
```

Notice there are **no setters**.

### Why are getters allowed on final fields?

A getter **only returns the value**; it does not modify it.

```java
Employee e = new Employee(1, "Rahul");

System.out.println(e.getName());
```

This is completely safe.

---

## Rule 5: Defensively copy mutable objects ⭐⭐⭐

If a field is mutable (like `Date`, `ArrayList`, or arrays), never store its reference directly.

### ❌ Wrong

```java
public final class Student {

    private final List<String> subjects;

    public Student(List<String> subjects){
        this.subjects = subjects;
    }

    public List<String> getSubjects(){
        return subjects;
    }
}
```

Problem:

```java
List<String> list = new ArrayList<>();
list.add("Math");

Student s = new Student(list);

list.add("Science");

System.out.println(s.getSubjects());
```

Output:

```text
[Math, Science]
```

The object changed from outside!

---

### ✅ Correct (Defensive Copy)

```java
public final class Student {

    private final List<String> subjects;

    public Student(List<String> subjects){
        this.subjects = new ArrayList<>(subjects);
    }

    public List<String> getSubjects(){
        return new ArrayList<>(subjects);
    }
}
```

Now external changes do not affect the object.

> **Interview:** Always create defensive copies for mutable fields in both constructor and getter.

---

# Complete Immutable Class ⭐⭐⭐

```java
import java.util.*;

public final class Employee {

    private final int id;
    private final String name;
    private final List<String> skills;

    public Employee(int id, String name, List<String> skills){
        this.id = id;
        this.name = name;
        this.skills = new ArrayList<>(skills);
    }

    public int getId(){
        return id;
    }

    public String getName(){
        return name;
    }

    public List<String> getSkills(){
        return new ArrayList<>(skills);
    }
}
```

This class is fully immutable.

---

# Mutable vs Immutable Fields ⭐⭐⭐

| Immutable | Mutable |
|-----------|---------|
| String | ArrayList |
| Integer | Date |
| Long | HashMap |
| Double | Array |
| LocalDate | StringBuilder |

Immutable fields are safe to return directly.

Mutable fields require defensive copying.

---

# Why No Setter? ⭐⭐⭐

### Immutable Class

```java
public final class User {

    private final String name;

    public User(String name){
        this.name = name;
    }

    public String getName(){
        return name;
    }
}
```

Trying to write:

```java
user.setName("Amit");
```

This won't even compile because no setter exists.

---

# Positive Interview Scenario ⭐⭐⭐

### Is this immutable?

```java
public final class Book {

    private final String title;

    public Book(String title){
        this.title = title;
    }

    public String getTitle(){
        return title;
    }
}
```

✅ **Answer:** Yes.

Reason:

- Final class
- Final field
- Constructor initialization
- No setter
- `String` itself is immutable

---

# Negative Interview Scenario 1 ⭐⭐⭐

```java
public final class Student {

    private final List<String> subjects;

    public Student(List<String> subjects){
        this.subjects = subjects;
    }

    public List<String> getSubjects(){
        return subjects;
    }
}
```

**Question:** Is this immutable?

❌ **Answer:** No.

Reason:

- `List` is mutable.
- Constructor stores external reference.
- Getter exposes internal reference.

---

# Negative Interview Scenario 2 ⭐⭐⭐

```java
public class Employee {

    private final String name;

    public Employee(String name){
        this.name = name;
    }
}
```

Looks immutable?

❌ **Answer:** No.

Reason:

The class is **not final**. A subclass could introduce mutable state or expose modification behavior.

---

# Negative Interview Scenario 3 ⭐⭐

```java
public final class Demo {

    private final int[] arr;

    public Demo(int[] arr){
        this.arr = arr;
    }
}
```

Problem:

```java
int[] nums = {1,2,3};

Demo d = new Demo(nums);

nums[0] = 100;
```

Internal array becomes:

```text
[100, 2, 3]
```

### Correct

```java
this.arr = arr.clone();
```

Getter:

```java
public int[] getArr(){
    return arr.clone();
}
```

---

# Why String is Immutable but StringBuilder Isn't? ⭐⭐

| String | StringBuilder |
|---------|---------------|
| Immutable | Mutable |
| `final` class | Not immutable |
| Creates new object | Modifies same object |
| Thread-safe | Not thread-safe |

Example:

```java
String s = "Java";
s.concat(" 21");
```

Original remains unchanged.

```java
StringBuilder sb = new StringBuilder("Java");
sb.append(" 21");
```

Same object gets modified.

---

# Benefits of Immutable Classes ⭐⭐⭐

- Thread-safe
- Easy to cache
- Safe HashMap keys
- Predictable behavior
- No synchronization needed

---

# Most Asked Interview Questions ⭐⭐⭐

### Why is String immutable?

Because it is a `final` class with private final state, and every modification creates a new object.

---

### Can immutable classes have getters?

**Yes.** Getters are safe because they only read values.

---

### Can immutable classes have setters?

**No.** Setters change state, which violates immutability.

---

### Why do mutable fields require defensive copying?

Otherwise, external code can modify the object's internal state through the same reference.
### Rule to remember
- Immutable elements (String, Integer, LocalDate) → Shallow defensive copy is enough.
- Mutable elements (Date, ArrayList, custom mutable objects) → Deep defensive copy is required.
---

### Is `final` alone enough to make a class immutable?

**No.**

You also need:

- Final class
- Private final fields
- No setters
- Constructor initialization
- Defensive copies for mutable fields

---

# Revision Checklist

- [ ] 5 Rules of Immutable Class
- [ ] `private final` fields
- [ ] Constructor initialization
- [ ] Getters without setters
- [ ] Defensive copy in constructor
- [ ] Defensive copy in getter
- [ ] Mutable vs Immutable fields
- [ ] Positive & Negative interview scenarios