# Comparator in Java

▸ What is Comparator?

`Comparator` is a Java interface used to define **custom ordering** for objects.

It belongs to:

    java.util.Comparator

Main method:

    int compare(T o1, T o2);

Return value:

    Negative value  → o1 comes before o2
    0               → o1 and o2 are considered equal for ordering
    Positive value  → o1 comes after o2

Interview one-liner:

> Comparator is used to define custom sorting logic outside the class being sorted.

---

▸ Basic Example

    import java.util.*;

    List<Integer> numbers = Arrays.asList(5, 2, 8, 1, 3);

    numbers.sort(new Comparator<Integer>() {
        @Override
        public int compare(Integer a, Integer b) {
            return Integer.compare(a, b);
        }
    });

Result:

    [1, 2, 3, 5, 8]

---

▸ Descending Order

    numbers.sort(new Comparator<Integer>() {
        @Override
        public int compare(Integer a, Integer b) {
            return Integer.compare(b, a);
        }
    });

Result:

    [8, 5, 3, 2, 1]

Simpler using `reversed()`:

    numbers.sort(Comparator.naturalOrder().reversed());

---

▸ Comparator with Custom Objects

Suppose we have:

    class Student {
        String name;
        int age;

        Student(String name, int age) {
            this.name = name;
            this.age = age;
        }
    }

Sort students by age:

    List<Student> students = new ArrayList<>();

    students.add(new Student("John", 25));
    students.add(new Student("Alice", 20));
    students.add(new Student("Bob", 23));

    students.sort(Comparator.comparingInt(s -> s.age));

Result:

    Alice 20
    Bob   23
    John  25

---

▸ Comparator.comparing()

For object fields:

    students.sort(Comparator.comparing(s -> s.name));

Sorts students by name in ascending order.

For descending order:

    students.sort(Comparator.comparing(s -> s.name).reversed());

---

▸ comparingInt()

For primitive `int` fields:

    students.sort(Comparator.comparingInt(s -> s.age));

This is generally preferred over:

    Comparator.comparing(s -> s.age)

because `comparingInt()` is specifically designed for `int` values and avoids unnecessary boxing.

Similarly:

    Comparator.comparingLong(...)
    Comparator.comparingDouble(...)

---

▸ Lambda Expression

Because `Comparator` is a functional interface, it can be implemented using a lambda.

Instead of:

    students.sort(new Comparator<Student>() {
        @Override
        public int compare(Student s1, Student s2) {
            return Integer.compare(s1.age, s2.age);
        }
    });

Use:

    students.sort((s1, s2) -> Integer.compare(s1.age, s2.age));

Or:

    students.sort(Comparator.comparingInt(s -> s.age));

Interview preference:

> `Comparator.comparingInt()` / `Comparator.comparing()` is usually the cleanest approach for field-based sorting.

---

▸ Why Comparator is a Functional Interface

`Comparator` has one abstract method:

    int compare(T o1, T o2);

Therefore, it can be used with:

    Lambda expressions
    Method references
    Anonymous classes

Example:

    Comparator<Integer> comp = (a, b) -> Integer.compare(a, b);

---

▸ Comparator with Method Reference

Suppose:

    class Student {
        String name;

        String getName() {
            return name;
        }
    }

We can sort using:

    students.sort(Comparator.comparing(Student::getName));

This is equivalent to:

    students.sort(Comparator.comparing(s -> s.getName()));

---

▸ Multiple-Level Sorting

Comparator allows sorting by multiple fields.

Example:

    students.sort(
        Comparator.comparingInt((Student s) -> s.age)
                  .thenComparing(s -> s.name)
    );

Meaning:

    1. Sort by age
    2. If ages are equal, sort by name

Example:

    John  20
    Alice 20
    Bob   23

After sorting:

    Alice 20
    John  20
    Bob   23

---

▸ thenComparing()

`thenComparing()` is used when the previous comparison produces `0`.

Example:

    students.sort(
        Comparator.comparingInt((Student s) -> s.age)
                  .thenComparing(s -> s.name)
    );

The name comparison is used only when:

    s1.age == s2.age

Interview one-liner:

> `thenComparing()` provides a secondary sorting criterion when the primary comparison considers two objects equal.

---

▸ Multiple-Level Sorting with Different Orders

Example:

Sort by:

    Age ascending
    Name descending

Code:

    students.sort(
        Comparator.comparingInt((Student s) -> s.age)
                  .thenComparing(
                      s -> s.name,
                      Comparator.reverseOrder()
                  )
    );

---

▸ Comparator.reversed()

Reverses an existing comparator.

Example:

    Comparator<Integer> ascending =
        Integer::compare;

    Comparator<Integer> descending =
        ascending.reversed();

Or:

    students.sort(
        Comparator.comparingInt((Student s) -> s.age).reversed()
    );

---

▸ Comparator.naturalOrder()

Returns a comparator that uses the natural ordering of objects.

Example:

    List<Integer> numbers = Arrays.asList(5, 2, 8, 1);

    numbers.sort(Comparator.naturalOrder());

Result:

    [1, 2, 5, 8]

For descending order:

    numbers.sort(Comparator.reverseOrder());

---

▸ Comparator.nullsFirst()

Allows `null` values and places them before non-null values.

Example:

    List<String> names =
        Arrays.asList("John", null, "Alice", "Bob");

    names.sort(Comparator.nullsFirst(Comparator.naturalOrder()));

Result:

    [null, Alice, Bob, John]

---

▸ Comparator.nullsLast()

Places `null` values after non-null values.

    names.sort(
        Comparator.nullsLast(Comparator.naturalOrder())
    );

Result:

    [Alice, Bob, John, null]

---

▸ Comparing Strings

Ascending:

    names.sort(Comparator.naturalOrder());

Descending:

    names.sort(Comparator.reverseOrder());

Using a field:

    students.sort(Comparator.comparing(Student::getName));

---

▸ String Comparison

Do not compare Strings using:

    s1.name - s2.name

Instead use:

    s1.name.compareTo(s2.name)

Example:

    students.sort((s1, s2) ->
        s1.name.compareTo(s2.name)
    );

Better:

    students.sort(Comparator.comparing(Student::getName));

---

▸ Comparator with TreeSet

Comparator is especially important with sorted collections such as `TreeSet`.

Example:

    TreeSet<Integer> set =
        new TreeSet<>((a, b) -> Integer.compare(b, a));

    set.add(10);
    set.add(5);
    set.add(20);

Result:

    [20, 10, 5]

The Comparator determines the ordering.

---

▸ Comparator with TreeMap

Comparator can also define the ordering of keys in a `TreeMap`.

Example:

    TreeMap<Integer, String> map =
        new TreeMap<>(Comparator.reverseOrder());

    map.put(10, "A");
    map.put(5, "B");
    map.put(20, "C");

Result:

    {20=C, 10=A, 5=B}

---

▸ Very Important TreeSet / TreeMap Trap

For sorted collections, `compare()` returning `0` means the keys/elements are considered equivalent for ordering.

Example:

    Comparator<Student> comp =
        Comparator.comparingInt(s -> s.age);

    TreeSet<Student> set = new TreeSet<>(comp);

If two students have the same age:

    Student("John", 20)
    Student("Alice", 20)

Then:

    comp.compare(student1, student2) == 0

So `TreeSet` may treat them as the same element even though they have different names.

Interview one-liner:

> TreeSet and TreeMap use `compareTo()` / `Comparator` for ordering and equivalence, so returning `0` can cause elements or keys to be treated as duplicates.

---

▸ Comparator vs Comparable

| Feature | Comparable | Comparator |
|---|---|---|
| Package | `java.lang` | `java.util` |
| Main method | `compareTo()` | `compare()` |
| Ordering | Natural ordering | Custom ordering |
| Logic location | Inside the class | Outside the class |
| Number of orderings | Usually one natural ordering | Multiple possible orderings |
| Lambda friendly | No | Yes |
| Used by TreeSet | Yes | Yes |
| Used by TreeMap | Yes | Yes |
| Used by sorting | Yes | Yes |

Easy way to remember:

    Comparable  → "I know how to compare myself."
    Comparator  → "I know how to compare two objects."

Interview one-liner:

> Comparable defines natural ordering inside the class, while Comparator defines custom ordering outside the class.

---

▸ Example: Comparable vs Comparator

Comparable:

    class Student implements Comparable<Student> {

        int age;

        @Override
        public int compareTo(Student other) {
            return Integer.compare(this.age, other.age);
        }
    }

Now:

    Collections.sort(students);

uses the student's natural ordering.

Comparator:

    students.sort(
        Comparator.comparingInt(s -> s.age)
    );

The sorting logic is provided externally.

---

▸ Can a Class Have Multiple Comparators?

Yes.

This is one of the biggest advantages of `Comparator`.

Example:

Sort by age:

    students.sort(
        Comparator.comparingInt(s -> s.age)
    );

Sort by name:

    students.sort(
        Comparator.comparing(Student::getName)
    );

Sort by age descending:

    students.sort(
        Comparator.comparingInt((Student s) -> s.age).reversed()
    );

The same `Student` class can therefore have multiple sorting strategies.

---

▸ Comparator Does Not Modify the Class

One advantage of Comparator is that you can sort a class even if you cannot modify its source code.

Example:

    class Employee {
        String name;
        int salary;
    }

Even if `Employee` does not implement `Comparable`, we can write:

    employees.sort(
        Comparator.comparingInt(e -> e.salary)
    );

Interview one-liner:

> Comparator is useful when the class cannot be modified or when multiple different sorting orders are required.

---

▸ Comparator.comparing()

General syntax:

    Comparator.comparing(Function<T, U> keyExtractor)

Example:

    Comparator<Student> byName =
        Comparator.comparing(Student::getName);

Then:

    students.sort(byName);

---

▸ Comparator.comparingInt()

Syntax:

    Comparator.comparingInt(ToIntFunction<T> keyExtractor)

Example:

    Comparator<Student> byAge =
        Comparator.comparingInt(Student::getAge);

Then:

    students.sort(byAge);

---

▸ Comparing Multiple Fields

Example:

    Comparator<Student> comparator =
        Comparator.comparing(Student::getName)
                  .thenComparingInt(Student::getAge);

Then:

    students.sort(comparator);

Sorting order:

    1. Name
    2. Age when names are equal

---

▸ Comparator.comparing() with Descending Field

Example:

    students.sort(
        Comparator.comparing(Student::getName)
                  .reversed()
    );

For only the secondary field:

    students.sort(
        Comparator.comparing(Student::getName)
                  .thenComparing(
                      Student::getAge,
                      Comparator.reverseOrder()
                  )
    );

---

▸ Avoid Subtraction in Comparator

Avoid:

    (a, b) -> a - b

Although it looks simple, it can cause integer overflow.

Example:

    Integer.MAX_VALUE - (-1)

can overflow.

Prefer:

    (a, b) -> Integer.compare(a, b)

For long:

    Long.compare(a, b)

For double:

    Double.compare(a, b)

Interview one-liner:

> Use `Integer.compare()` instead of subtraction because subtraction can overflow.

---

▸ Comparator Contract

A Comparator should follow a consistent ordering.

Important properties:

    1. Sign consistency
    2. Transitivity
    3. Antisymmetry-like comparison behavior

For objects `a`, `b`, and `c`:

If:

    compare(a, b) < 0

then `a` should come before `b`.

If:

    compare(a, b) == 0

then they are considered equivalent for ordering.

If:

    compare(a, b) > 0

then `a` should come after `b`.

Ordering should also be transitive.

Example:

    a < b
    b < c

should imply:

    a < c

---

▸ Comparator and equals()

Ideally:

    compare(a, b) == 0

should be consistent with:

    a.equals(b)

But this is not strictly required by the Comparator interface.

If they are inconsistent, sorted collections can behave differently from HashSet/HashMap.

Example:

    compare(a, b) == 0
    a.equals(b) == false

A `TreeSet` may keep only one of them, while a `HashSet` can keep both.

---

▸ Comparator and HashSet

`HashSet` does NOT use Comparator for uniqueness.

It uses:

    hashCode()
    equals()

Example:

    HashSet<Student> set

The Comparator is irrelevant to HashSet.

---

▸ Comparator and HashMap

`HashMap` does NOT use Comparator for normal key lookup.

It primarily uses:

    hashCode()
    equals()

Comparator is relevant to sorted maps such as:

    TreeMap

---

▸ Comparator and PriorityQueue

Comparator can define the ordering of a `PriorityQueue`.

Example:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>(Comparator.reverseOrder());

    pq.add(10);
    pq.add(5);
    pq.add(20);

The highest value gets the highest priority.

    pq.poll() → 20

Important:

> PriorityQueue's iterator does not guarantee sorted traversal; the Comparator determines priority/order of removal, not full iteration order.

---

▸ Comparator with Arrays.sort()

For object arrays:

    Student[] students = ...;

    Arrays.sort(
        students,
        Comparator.comparingInt(Student::getAge)
    );

For primitive arrays such as:

    int[]

Comparator cannot be used directly because primitive arrays are not arrays of objects.

---

▸ Comparator with Collections.sort()

Example:

    Collections.sort(
        students,
        Comparator.comparingInt(Student::getAge)
    );

Modern Java can also use:

    students.sort(
        Comparator.comparingInt(Student::getAge)
    );

---

▸ Comparator with List.sort()

`List.sort()` accepts a Comparator.

Example:

    students.sort(
        Comparator.comparing(Student::getName)
    );

To use natural ordering:

    students.sort(null);

`null` means use the list elements' natural ordering.

---

▸ Comparator with Streams

Comparator is commonly used with streams.

Example:

    students.stream()
        .sorted(Comparator.comparingInt(Student::getAge))
        .forEach(System.out::println);

Descending:

    students.stream()
        .sorted(
            Comparator.comparingInt(Student::getAge).reversed()
        )
        .forEach(System.out::println);

---

▸ Finding Maximum Using Comparator

Example:

    Student oldest =
        students.stream()
            .max(Comparator.comparingInt(Student::getAge))
            .orElse(null);

---

▸ Finding Minimum Using Comparator

    Student youngest =
        students.stream()
            .min(Comparator.comparingInt(Student::getAge))
            .orElse(null);

---

▸ Comparator.reversed() vs reverseOrder()

`Comparator.reverseOrder()`:

    Comparator<Integer> c =
        Comparator.reverseOrder();

Used for types that have natural ordering.

`reversed()`:

    Comparator<Integer> c =
        Comparator.naturalOrder().reversed();

Reverses an existing Comparator.

Easy difference:

> `reverseOrder()` creates a reverse natural-order comparator, while `reversed()` reverses an existing comparator.

---

▸ Custom Comparator Using Anonymous Class

Before lambdas:

    Comparator<Student> comparator =
        new Comparator<Student>() {

            @Override
            public int compare(Student s1, Student s2) {
                return Integer.compare(s1.age, s2.age);
            }
        };

    students.sort(comparator);

Modern Java usually prefers:

    students.sort(
        Comparator.comparingInt(s -> s.age)
    );

---

▸ Comparator as a Separate Class

You can create a dedicated Comparator.

    class AgeComparator implements Comparator<Student> {

        @Override
        public int compare(Student s1, Student s2) {
            return Integer.compare(s1.age, s2.age);
        }
    }

Usage:

    students.sort(new AgeComparator());

This approach is useful when the comparison logic is large or reused in many places.

---

▸ Comparator with Generics

Comparator is generic:

    Comparator<T>

Example:

    Comparator<String>

    Comparator<Integer>

    Comparator<Student>

The generic type represents the objects being compared.

---

▸ Comparator Interface Definition

Conceptually:

    @FunctionalInterface
    public interface Comparator<T> {

        int compare(T o1, T o2);

        // Other default/static methods
    }

The important abstract method is:

    compare(T o1, T o2)

Because it has one abstract method, it is a functional interface.

---

▸ Important Comparator Methods

Common methods to remember:

    compare()
    comparing()
    comparingInt()
    comparingLong()
    comparingDouble()
    thenComparing()
    reversed()
    naturalOrder()
    reverseOrder()
    nullsFirst()
    nullsLast()

---

▸ Common Interview Questions

▸ What is Comparator?

> Comparator is an interface used to define custom ordering of objects outside the class.

▸ Which method does Comparator define?

> `compare(T o1, T o2)`.

▸ What does compare() return?

> Negative if the first object comes before the second, zero if they are equivalent for ordering, and positive if the first comes after the second.

▸ Is Comparator a functional interface?

> Yes, because it has one abstract method: `compare()`.

▸ Can Comparator be used with lambda expressions?

> Yes.

Example:

    Comparator<Integer> c =
        (a, b) -> Integer.compare(a, b);

▸ Why use Comparator instead of Comparable?

> Use Comparator when you need custom or multiple sorting orders, or when you cannot modify the class.

▸ Can one class have multiple Comparators?

> Yes, and this is one of the main advantages of Comparator.

▸ Can Comparator sort objects that do not implement Comparable?

> Yes.

▸ Does HashSet use Comparator?

> No. HashSet uses `hashCode()` and `equals()`.

▸ Does TreeSet use Comparator?

> Yes, if one is provided; otherwise it uses natural ordering.

▸ Does TreeMap use Comparator?

> Yes, if one is provided; otherwise it uses natural ordering of keys.

▸ What happens if Comparator returns 0 for two different objects in TreeSet?

> TreeSet treats them as equivalent for ordering and may store only one.

▸ Why should subtraction be avoided in compare()?

> Because subtraction can overflow; use `Integer.compare()` instead.

▸ What is thenComparing()?

> It adds a secondary sorting rule when the primary comparison returns 0.

▸ What is Comparator.comparing()?

> It creates a Comparator based on a key extracted from the object.

▸ What is Comparator.comparingInt()?

> It creates a Comparator based on an `int` key and avoids unnecessary boxing.

▸ What is nullsFirst()?

> It creates a Comparator that places null values before non-null values.

▸ What is nullsLast()?

> It creates a Comparator that places null values after non-null values.

---

▸ Most Important Interview Example

Question:

Sort employees first by salary ascending and then by name ascending.

Answer:

    employees.sort(
        Comparator.comparingInt(Employee::getSalary)
                  .thenComparing(Employee::getName)
    );

Descending salary:

    employees.sort(
        Comparator.comparingInt(Employee::getSalary)
                  .reversed()
                  .thenComparing(Employee::getName)
    );

---

▸ Quick Revision

    Comparator
        |
        +-- Custom ordering
        |
        +-- java.util
        |
        +-- compare(o1, o2)
        |
        +-- Functional interface
        |
        +-- Lambda friendly
        |
        +-- Multiple sorting strategies
        |
        +-- comparing()
        |
        +-- comparingInt()
        |
        +-- thenComparing()
        |
        +-- reversed()
        |
        +-- naturalOrder()
        |
        +-- reverseOrder()
        |
        +-- nullsFirst()
        |
        +-- nullsLast()
        |
        +-- Used by:
              |
              +-- Arrays.sort()
              +-- Collections.sort()
              +-- List.sort()
              +-- TreeSet
              +-- TreeMap
              +-- PriorityQueue
              +-- Stream.sorted()

---

▸ Comparable vs Comparator — Memory Trick

    Comparable  → compareTo() → inside class → natural ordering

    Comparator  → compare()   → outside class → custom ordering

Best interview one-liner:

> Comparable defines the natural ordering of an object inside its class, whereas Comparator defines custom ordering externally and allows multiple sorting strategies.

---

▸ Final Definition

`Comparator` is a functional interface in `java.util` that allows developers to define custom ordering between objects using `compare()`, and it supports multiple sorting strategies through methods such as `comparing()`, `thenComparing()`, and `reversed()`.