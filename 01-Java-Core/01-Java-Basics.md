# 01. Java Basics

> The foundation of Java. This note covers the language concepts that are most frequently asked in Java interviews.

**Prerequisites:** None  
**Next Note:** [OOP](02-OOP.md)

---

# What is Java? ⭐

Java is a **high-level, object-oriented, platform-independent** programming language. It follows the principle of **WORA (Write Once, Run Anywhere)** by compiling code into **bytecode**, which runs on the JVM.

### Why Java is platform independent

Source Code (`.java`) → Compiler (`javac`) → Bytecode (`.class`) → JVM → Machine Code

**Interview Answer:** Java is platform independent because the compiler generates **bytecode**, and every operating system has its own JVM that executes that bytecode.

---

# JDK vs JRE vs JVM ⭐⭐⭐

| Term | Purpose |
|------|---------|
| **JDK** | Development kit containing compiler + JRE + tools |
| **JRE** | Runtime environment to run Java programs |
| **JVM** | Executes bytecode and manages memory |

### Easy way to remember

- **JDK** → Write + Compile + Run
- **JRE** → Run only
- **JVM** → Actually executes the program

---

# First Java Program

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello World");
    }
}
```

### Breakdown

| Keyword | Meaning |
|----------|---------|
| `public` | Accessible from anywhere |
| `class` | Blueprint of objects |
| `static` | Can be called without creating an object |
| `void` | Returns nothing |
| `main()` | Entry point of program |
| `String[] args` | Command-line arguments |

> ⭐ **Frequently Asked:** Why is `main()` static?

**Answer:** Because JVM calls `main()` before creating any object.

---

# Variables & Data Types ⭐⭐

## Primitive Data Types

| Type | Size | Example |
|------|------|---------|
| `byte` | 1 byte | 100 |
| `short` | 2 bytes | 2000 |
| `int` | 4 bytes | 25 |
| `long` | 8 bytes | 100L |
| `float` | 4 bytes | 10.5f |
| `double` | 8 bytes | 10.5 |
| `char` | 2 bytes | 'A' |
| `boolean` | 1 bit* | true |

> `String` is **not** primitive. It is a class.

## Declaration

```java
int age = 21;
double salary = 50000;
char grade = 'A';
boolean isActive = true;
```

---

# Primitive vs Non-Primitive ⭐⭐⭐

| Primitive | Non-Primitive |
|-----------|---------------|
| Stores value | Stores reference |
| Fixed size | Dynamic |
| Lowercase | Capitalized class names |
| `int` | `String` |
| `double` | `ArrayList` |

Example:

```java
int a = 10;
String name = "John";
```

> ⭐ **Interview:** `String` is an object, while `int` is a primitive.

---

# Type Casting ⭐⭐

## Implicit (Widening)

Small → Large

```java
int a = 10;
double b = a;
```

## Explicit (Narrowing)

Large → Small

```java
double x = 10.8;
int y = (int) x;
```

Output:

```text
y = 10
```

---

# Operators

## Arithmetic

```java
+  -  *  /  %
```

## Relational

```java
==  !=  >  <  >=  <=
```

## Logical ⭐

```java
&&
||
!
```

Example:

```java
if(age > 18 && isActive){
    System.out.println("Allowed");
}
```

---

# Increment Operators ⭐⭐

```java
int a = 5;

System.out.println(a++); // 5
System.out.println(a);   // 6

System.out.println(++a); // 7
```

| Operator | Meaning |
|----------|---------|
| `a++` | Use then increase |
| `++a` | Increase then use |

This is one of the most common Java MCQ topics.

---

# if-else & switch

## if

```java
if(score >= 40){
    System.out.println("Pass");
}else{
    System.out.println("Fail");
}
```

## Switch

```java
switch(day){
    case 1:
        System.out.println("Monday");
        break;
    default:
        System.out.println("Invalid");
}
```

Java 14+ also supports:

```java
String result = switch(day){
    case 1 -> "Monday";
    default -> "Invalid";
};
```

---

# Loops ⭐⭐

## for

```java
for(int i=0;i<5;i++){
    System.out.println(i);
}
```

## while

```java
while(n > 0){
    n--;
}
```

## do-while

```java
do{
    System.out.println("Runs once");
}while(false);
```

### for vs while

| for | while |
|------|-------|
| Known iterations | Unknown iterations |
| Counter based | Condition based |

---

# Arrays Basics ⭐⭐

An array stores **multiple values of the same data type** in contiguous memory.

## 1. Declare only

```java
int[] arr;
```

or

```java
int arr[];
```

> `int[] arr` is the recommended Java style.

---

## 2. Declare + Allocate Memory

```java
int[] arr = new int[5];
```

Default values:

| Type | Default Value |
|------|---------------|
| `int` | `0` |
| `double` | `0.0` |
| `boolean` | `false` |
| `char` | `'\u0000'` |
| Objects | `null` |

---

## 3. Declare + Initialize

```java
int[] arr = {1, 2, 3, 4, 5};
```

The compiler automatically creates the array.

---

## 4. Dynamic Initialization

Useful when assigning later.

```java
int[] arr;

arr = new int[]{1, 2, 3, 4};
```

> This syntax is required outside the declaration.

---

## 5. Multi-Dimensional Array

```java
int[][] matrix = new int[3][4];
```

Initialize directly:

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6}
};
```

---

## 6. Jagged Array

Each row can have a different length.

```java
int[][] arr = new int[3][];

arr[0] = new int[2];
arr[1] = new int[4];
arr[2] = new int[1];
```

Example:

```text
[10 20]
[30 40 50 60]
[70]
```

---

## Access & Traversal

```java
int[] arr = {10, 20, 30};

System.out.println(arr[0]);      // 10
System.out.println(arr.length);  // 3
```

### Traditional for loop

```java
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}
```

### Enhanced for-each loop

```java
for (int num : arr) {
    System.out.println(num);
}
```

---

## Interview Notes ⭐

| Syntax | Use Case |
|---------|----------|
| `int[] arr;` | Declaration |
| `new int[5]` | Fixed-size array |
| `{1,2,3}` | Immediate initialization |
| `new int[]{1,2,3}` | Dynamic initialization |
| `int[][]` | Matrix |
| `new int[3][]` | Jagged array |

---

# Methods ⭐⭐

Syntax

```java
public int add(int a, int b){
    return a + b;
}
```

Calling

```java
int ans = add(2,3);
```

### Method Components

| Part | Example |
|------|---------|
| Access Modifier | `public` |
| Return Type | `int` |
| Name | `add` |
| Parameters | `(int a, int b)` |

---

# Command Line Arguments

```java
public static void main(String[] args){
    System.out.println(args[0]);
}
```

Run:

```text
java Main Hello
```

Output:

```text
Hello
```

---

# Wrapper Classes ⭐⭐

Every primitive has an object equivalent.

| Primitive | Wrapper |
|-----------|----------|
| int | Integer |
| char | Character |
| double | Double |
| boolean | Boolean |

Example:

```java
Integer num = 10;
int value = num;
```

### Autoboxing

```java
Integer x = 5;
```

### Unboxing

```java
int y = x;
```

> ⭐ Frequently asked in collections because collections store objects, not primitives.

---

# var Keyword (Java 10)

```java
var name = "Apoorva";
var age = 22;
```

Rules:

- Only for local variables
- Type is decided during compilation
- Cannot initialize with `null`

---

# Keywords Every Beginner Should Know

| Keyword | Purpose |
|----------|----------|
| `public` | Accessible everywhere |
| `private` | Accessible within class |
| `protected` | Package + subclass |
| `static` | Belongs to class |
| `final` | Cannot change |
| `this` | Current object |
| `new` | Creates object |
| `return` | Returns value |

---

# Common Interview Questions ⭐⭐⭐

## Why is Java platform independent?

Because Java compiles into **bytecode**, which is executed by the JVM available on different operating systems.

---

## Why is `main()` static?

JVM invokes `main()` directly without creating an object, so it must belong to the class.

---

## Difference between JDK, JRE and JVM

- **JDK:** Develop + Compile + Run
- **JRE:** Run programs
- **JVM:** Executes bytecode

---

## Primitive vs Wrapper Class

- Primitive stores actual value.
- Wrapper stores objects and is required in collections.

Example:

```java
List<Integer> list = new ArrayList<>();
```

You cannot write:

```java
List<int> list; // Invalid
```

---

# Revision Checklist

- [ ] JDK vs JRE vs JVM
- [ ] Platform Independence
- [ ] Primitive vs Non-Primitive
- [ ] Type Casting
- [ ] Operators (`++i` vs `i++`)
- [ ] Loops
- [ ] Arrays Basics
- [ ] Methods
- [ ] Wrapper Classes & Autoboxing
- [ ] Why `main()` is static

---

# What's Next

Continue with **[02. OOP](02-OOP.md)**.