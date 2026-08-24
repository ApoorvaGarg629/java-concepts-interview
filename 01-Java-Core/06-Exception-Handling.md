# 06. Exception Handling

> Exception Handling is one of the most asked Java Core topics. Interviewers focus on exception hierarchy, checked vs unchecked exceptions, try-with-resources, custom exceptions, overriding rules, and finally block edge cases.

**Previous:** [Generics](05-Generics.md)  

---
# What is Exception Handling? ⭐⭐⭐

Exception handling is a mechanism to **handle runtime errors gracefully** so that the program can continue or terminate in a controlled manner.

Without exception handling:

```java
int a = 10;
int b = 0;

System.out.println(a / b);
```

Output:

```text
ArithmeticException
```

With exception handling:

```java
try{
    System.out.println(a / b);
}catch(ArithmeticException e){
    System.out.println("Cannot divide by zero");
}
```

---

# Exception Hierarchy ⭐⭐⭐

```text
Object
   │
Throwable
├── Error
│     ├── StackOverflowError
│     └── OutOfMemoryError
│
└── Exception
      ├── RuntimeException
      │     ├── NullPointerException
      │     ├── ArithmeticException
      │     ├── IndexOutOfBoundsException
      │     └── IllegalArgumentException
      │
      ├── IOException
      ├── SQLException
      └── ClassNotFoundException
```

### Interview Rule

- **Error** → JVM problems, usually not handled.
- **Exception** → Application problems, should be handled.

---

# Error vs Exception ⭐⭐⭐

| Error | Exception |
|--------|-----------|
| JVM issue | Application issue |
| Usually unrecoverable | Recoverable |
| Not handled | Can be handled |
| `OutOfMemoryError` | `IOException` |

Example:

```java
StackOverflowError
OutOfMemoryError
```

Don't catch `Error` in normal applications.

---

# Checked vs Unchecked Exceptions ⭐⭐⭐

## Checked Exception (compile time)

The compiler will not allow the program to compile until the exception is handled.

```java
FileReader file = new FileReader("data.txt");
```

❌ Compile error

Correct:

```java
try {
    FileReader file = new FileReader("data.txt");
} catch (IOException e) {
    e.printStackTrace();
}
```

Examples:

- IOException
- SQLException
- ClassNotFoundException

---

## Unchecked Exception

The program compiles successfully but fails during execution.

```java
String s = null;

System.out.println(s.length());
```

Output:

```text
NullPointerException
```

Examples:

- NullPointerException
- ArithmeticException
- ArrayIndexOutOfBoundsException
- IllegalArgumentException

### Comparison

| Compile-Time Exception                        | Runtime Exception                   |
| --------------------------------------------- | ----------------------------------- |
| Detected during compilation                   | Occurs while the program is running |
| Also called **Checked Exception**             | Also called **Unchecked Exception** |
| Must be handled using `try-catch` or `throws` | Handling is optional                |
| Extends `Exception`                           | Extends `RuntimeException`          |

> **Interview:** RuntimeException and its subclasses are unchecked exceptions.

---

# try-catch ⭐⭐⭐

Basic syntax:

```java
try{
    int ans = 10 / 0;
}catch(ArithmeticException e){
    System.out.println("Error");
}
```

Only the matching catch block executes.

---

# Multiple Catch ⭐⭐

```java
try{

}catch(ArithmeticException e){

}catch(NullPointerException e){

}catch(Exception e){

}
```

### Important Rule

Always place **child exception first**.

❌ Invalid

```java
catch(Exception e){}

catch(IOException e){}
```

Reason: Parent already catches child.

---

# Multi-Catch (Java 7) ⭐⭐

Handle multiple exceptions together.

```java
try{

}catch(IOException | SQLException e){
    System.out.println("Handled");
}
```

Useful when handling logic is identical.

---

# finally Block ⭐⭐⭐

`finally` always executes whether an exception occurs or not.

```java
try{
    System.out.println("Try");
}catch(Exception e){

}finally{
    System.out.println("Finally");
}
```

Output:

```text
Try
Finally
```

---

# Does finally always execute? ⭐⭐⭐

### Yes

```java
try{
    return;
}finally{
    System.out.println("Finally");
}
```

Output:

```text
Finally
```

### No (Rare Edge Cases)

`finally` may not execute when:

- JVM crashes
- `System.exit(0)` is called
- Power failure

```java
try{
    System.exit(0);
}finally{
    System.out.println("Never");
}
```

---

# return in try vs finally ⭐⭐⭐

```java
public static int test(){

    try{
        return 10;
    }finally{
        return 20;
    }
}
```

Output:

```text
20
```

> **Interview:** `finally` overrides the return value from `try`.

Avoid returning from `finally`.

---

# throw Keyword ⭐⭐⭐

Used to **explicitly throw** an exception.

```java
int age = 16;

if(age < 18){
    throw new IllegalArgumentException("Invalid age");
}
```

Use `throw` for business validations.

---

# throws Keyword ⭐⭐⭐

Used to declare that a method may throw an exception.

```java
public void read() throws IOException{

}
```

The caller must handle or rethrow it.

### Difference

| throw | throws |
|--------|---------|
| Throws object | Declares exception |
| Inside method | Method signature |
| One exception object | Multiple exceptions allowed |

---

# throw vs throws ⭐⭐⭐

```java
public void vote(int age){

    if(age < 18){
        throw new IllegalArgumentException();
    }
}
```

```java
public void readFile() throws IOException{

}
```

Easy memory trick:

- **throw** → Actually throws
- **throws** → Declares possibility

---

# Custom Exception ⭐⭐⭐

Create your own checked exception.

```java
class InvalidAgeException extends Exception{

    InvalidAgeException(String msg){
        super(msg);
    }
}
```

Use:

```java
if(age < 18){
    throw new InvalidAgeException("Age must be 18");
}
```

### Custom Unchecked Exception

```java
class InvalidAgeException
        extends RuntimeException{

    InvalidAgeException(String msg){
        super(msg);
    }
}
```

Choose RuntimeException for validation/business logic.

---

# Exception Propagation ⭐⭐⭐

Exceptions propagate up the call stack until handled.

```java
void c(){
    throw new RuntimeException();
}

void b(){
    c();
}

void a(){
    b();
}
```

If `a()` doesn't catch it, the program terminates.

---

# Method Overriding Rules ⭐⭐⭐

## Checked Exception

Child can throw:

- Same exception
- Child exception
- No exception

```java
class Parent{

    void read() throws IOException{}
}

class Child extends Parent{

    @Override
    void read() throws FileNotFoundException{}
}
```

Valid because FileNotFoundException is a subclass.

---

## Unchecked Exception

No restriction.

```java
class Parent{
    void test(){}
}

class Child extends Parent{
    void test() throws RuntimeException{}
}
```

Valid.

---

# Constructor Exception ⭐⭐

Constructors can throw exceptions.

```java
class Student{

    Student() throws IOException{

    }
}
```

Caller must handle it.

---

# try-with-resources ⭐⭐⭐

Automatically closes resources.

```java
try(FileReader file =
        new FileReader("a.txt")){

    // use file

}catch(IOException e){

}
```

No need to call:

```java
file.close();
```

### Why use it?

Prevents resource leaks.

Resources must implement `AutoCloseable`.

Examples:

- FileReader
- BufferedReader
- Scanner
- Connection

---

# AutoCloseable Interface ⭐⭐

Any class implementing `AutoCloseable` works with try-with-resources.

```java
class Demo implements AutoCloseable{

    public void close(){
        System.out.println("Closed");
    }
}
```

Usage:

```java
try(Demo d = new Demo()){

}
```

Output:

```text
Closed
```

---

# Common Runtime Exceptions ⭐⭐⭐

## NullPointerException

```java
String s = null;

s.length();
```

Avoid:

```java
Objects.requireNonNull(s);
```

---

## ArithmeticException

```java
10 / 0;
```

---

## ArrayIndexOutOfBoundsException

```java
int[] arr = {1,2};

arr[5];
```

---

## NumberFormatException

```java
Integer.parseInt("ABC");
```

---

## IllegalArgumentException

```java
throw new IllegalArgumentException(
    "Invalid input"
);
```

Used for invalid method arguments.

---

# Common Checked Exceptions ⭐⭐

| Exception | Real-life Use |
|-----------|---------------|
| IOException | File handling |
| SQLException | Database |
| ClassNotFoundException | Reflection |
| InterruptedException | Multithreading |

---

# Exception Chaining ⭐⭐

Wrap one exception inside another.

```java
try{

}catch(IOException e){

    throw new RuntimeException(
        "File failed",
        e
    );
}
```

Useful for preserving root cause.

Retrieve original cause:

```java
e.getCause();
```

---

# Final vs finally vs finalize ⭐⭐⭐

| final | finally | finalize() |
|--------|----------|-----------|
| Keyword | Block | Method |
| Prevent modification | Cleanup block | GC callback |
| Variables/classes | Exception handling | Deprecated |

### `final`

```java
final int age = 20;
```

### `finally`

```java
try{

}finally{

}
```

### `finalize()`

Deprecated since Java 9+ and should not be used.

---

# Exception Handling Best Practices ⭐⭐⭐

### Catch specific exceptions

✅

```java
catch(IOException e){}
```

❌

```java
catch(Exception e){}
```

---

### Never ignore exceptions

❌

```java
catch(Exception e){}
```

Always log or rethrow.

---

### Don't return from finally

```java
finally{
    return 10;
}
```

Avoid because it hides exceptions and return values.

---

### Prefer try-with-resources

Instead of manually closing files.

---

# Real Interview Edge Cases ⭐⭐⭐

## Case 1

```java
try{
    throw new RuntimeException();
}catch(Exception e){
    System.out.println("Catch");
}finally{
    System.out.println("Finally");
}
```

Output:

```text
Catch
Finally
```

---

## Case 2

```java
try{
    return 1;
}finally{
    System.out.println("Cleanup");
}
```

Output:

```text
Cleanup
```

Method returns `1`.

---

## Case 3

```java
try{
    return 1;
}finally{
    return 2;
}
```

Output:

```text
2
```

`finally` overrides the return.

---

## Case 4

```java
try{
    System.exit(0);
}finally{
    System.out.println("Hello");
}
```

Output:

```text
Nothing
```

JVM terminates immediately.

---

## Case 5

```java
catch(IOException e){

}catch(Exception e){

}
```

✅ Valid

Reverse order is invalid.

---

# Most Asked Interview Questions ⭐⭐⭐

### Difference between Checked and Unchecked Exception?

Checked exceptions are verified at compile time; unchecked exceptions occur at runtime.

### Can we have try without catch?

**Yes**, with `finally`.

```java
try{

}finally{

}
```

### Can we have catch without try?

❌ No.

### Can finally exist without catch?

✅ Yes.

### Does finally always execute?

Yes, except JVM termination (`System.exit()`), crash, or power failure.

### Difference between throw and throws?

- `throw` → Actually throws an exception object.
- `throws` → Declares possible exceptions in the method signature.

### Why is finalize() deprecated?

Because garbage collection is unpredictable; use `AutoCloseable` and try-with-resources instead.

---

# Revision Checklist

- [ ] Exception hierarchy
- [ ] Error vs Exception
- [ ] Checked vs Unchecked
- [ ] try-catch-finally
- [ ] Multi-catch
- [ ] throw vs throws
- [ ] Custom exceptions
- [ ] Exception propagation
- [ ] Overriding rules
- [ ] try-with-resources
- [ ] AutoCloseable
- [ ] Exception chaining
- [ ] final vs finally vs finalize
- [ ] Real interview edge cases

---

# What's Next

Continue with **[07. Enums & Wrapper Classes](07-Enums-and-Wrappers.md)**.