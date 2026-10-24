# Comparable

▸ What is Comparable?

`Comparable` is an interface used to define the **natural ordering** of objects.

It is present in:

    java.lang.Comparable

It contains one main method:

    int compareTo(T o);

**Interview one-liner:**

> Comparable is used to define the natural ordering of objects by implementing `compareTo()` inside the class itself.

▸ Syntax

    public interface Comparable<T> {
        int compareTo(T o);
    }

Example:

    class Student implements Comparable<Student> {

        int id;

        Student(int id) {
            this.id = id;
        }

        @Override
        public int compareTo(Student other) {
            return Integer.compare(this.id, other.id);
        }
    }

Now `Student` objects can be naturally ordered by `id`.

▸ Why Do We Need Comparable?

Suppose we have:

    class Student {

        int id;
        String name;
    }

Now Java does not automatically know how two `Student` objects should be ordered.

Should they be sorted by:

    id
    name
    age
    marks

We can define the default ordering by implementing:

    Comparable<Student>

and overriding:

    compareTo()`.

▸ Example

    class Student implements Comparable<Student> {

        int id;
        String name;

        Student(int id, String name) {
            this.id = id;
            this.name = name;
        }

        @Override
        public int compareTo(Student other) {
            return Integer.compare(this.id, other.id);
        }
    }

Now:

    Student s1 = new Student(10, "John");
    Student s2 = new Student(20, "David");

    System.out.println(s1.compareTo(s2));

Result:

    -1

because:

    10 < 20

▸ Meaning of compareTo()

`compareTo()` returns:

    Negative value
    → this object < other object

    0
    → this object == other according to ordering

    Positive value
    → this object > other object

The exact negative or positive number does not matter.

Only the sign matters.

▸ Easy Way to Remember

    compareTo() < 0
    → Current object comes before other

    compareTo() == 0
    → Same ordering position

    compareTo() > 0
    → Current object comes after other

▸ Example

    Student s1 = new Student(10, "John");
    Student s2 = new Student(20, "David");

    s1.compareTo(s2)

Since:

    10 < 20

result:

    Negative

And:

    s2.compareTo(s1)

result:

    Positive

And:

    s1.compareTo(s1)

result:

    0

▸ Natural Ordering

The ordering defined by `Comparable` is called **natural ordering**.

Examples:

    Integer
    String
    Double
    LocalDate

already implement Comparable.

Therefore:

    List<Integer> numbers =
        Arrays.asList(30, 10, 20);

    Collections.sort(numbers);

Result:

    [10, 20, 30]

`Integer` provides its natural ordering through `Comparable`.

▸ Comparable with String

`String` implements `Comparable<String>`.

Example:

    String s1 = "Apple";
    String s2 = "Banana";

    System.out.println(s1.compareTo(s2));

Result:

    Negative value

because `"Apple"` comes before `"Banana"` lexicographically.

▸ Comparable with Integer

    Integer a = 10;
    Integer b = 20;

    a.compareTo(b);

Result:

    Negative value

because:

    10 < 20

▸ Comparable with TreeSet

This is one of the most important interview connections.

If a class implements Comparable, a TreeSet can use its `compareTo()` method to maintain sorted order.

Example:

    class Student implements Comparable<Student> {

        int id;

        Student(int id) {
            this.id = id;
        }

        @Override
        public int compareTo(Student other) {
            return Integer.compare(this.id, other.id);
        }
    }

Then:

    TreeSet<Student> students =
        new TreeSet<>();

    students.add(new Student(30));
    students.add(new Student(10));
    students.add(new Student(20));

TreeSet uses:

    compareTo()

to maintain:

    10
    20
    30

▸ Comparable with TreeMap

Comparable can also be used by TreeMap when keys do not have an explicitly supplied Comparator.

Example:

    class Student implements Comparable<Student> {

        int id;

        @Override
        public int compareTo(Student other) {
            return Integer.compare(this.id, other.id);
        }
    }

Then:

    TreeMap<Student, String> map =
        new TreeMap<>();

TreeMap uses:

    compareTo()

to determine key ordering.

▸ Comparable with Collections.sort()

Example:

    class Student implements Comparable<Student> {

        int id;

        @Override
        public int compareTo(Student other) {
            return Integer.compare(this.id, other.id);
        }
    }

Then:

    List<Student> students =
        new ArrayList<>();

    students.add(new Student(30));
    students.add(new Student(10));
    students.add(new Student(20));

    Collections.sort(students);

The list is sorted using:

    compareTo()

▸ Comparable with List.sort()

Modern Java also allows:

    students.sort(null);

Passing `null` means use the elements' natural ordering.

This uses their `Comparable` implementation.

▸ Comparable vs Comparator

This is one of the most frequently asked interview questions.

    Comparable
    → Natural ordering
    → compareTo()
    → Implemented inside the class

    Comparator
    → Custom ordering
    → compare()
    → Separate class/lambda

Example:

    class Student implements Comparable<Student> {

        @Override
        public int compareTo(Student other) {
            return Integer.compare(this.id, other.id);
        }
    }

Natural ordering:

    Student by ID

Custom ordering:

    Comparator<Student> byName =
        Comparator.comparing(student -> student.name);

▸ Easy Difference

Remember:

    Comparable
    → "How should I naturally compare myself?"

    Comparator
    → "How should I compare these two objects for this particular use case?"

▸ Comparable Example

    class Student implements Comparable<Student> {

        int id;

        @Override
        public int compareTo(Student other) {
            return Integer.compare(this.id, other.id);
        }
    }

The Student class decides its natural ordering.

▸ Comparator Example

    Comparator<Student> byId =
        Comparator.comparingInt(student -> student.id);

    Comparator<Student> byName =
        Comparator.comparing(student -> student.name);

The ordering is defined outside the Student class.

▸ Can We Have Multiple Comparable Orderings?

No.

A class has one natural ordering through its `compareTo()` implementation.

For multiple different orderings, use `Comparator`.

Example:

    Natural ordering
    → Student by id

    Comparator
    → Student by name
    → Student by age
    → Student by marks

**Interview one-liner:**

> A class can have one natural ordering through Comparable, but multiple custom orderings can be defined using Comparator.

▸ Can Comparable Be Used with Lambda?

No.

`Comparable` is not a functional interface because it is not intended to be used as a lambda for defining arbitrary comparisons.

For example, you cannot normally replace:

    implements Comparable<Student>

with a lambda.

For lambda-based comparison, use:

    Comparator<Student>

Example:

    Comparator<Student> comparator =
        (s1, s2) -> Integer.compare(s1.id, s2.id);

▸ Why Does Comparator Work with Lambda?

`Comparator` is a functional interface because it has one abstract method:

    int compare(T o1, T o2);

Therefore:

    Comparator<Student> comparator =
        (s1, s2) -> Integer.compare(s1.id, s2.id);

is valid.

▸ compareTo() vs compare()

Comparable:

    s1.compareTo(s2)

Comparator:

    comparator.compare(s1, s2)

The difference is where the comparison logic is defined.

    Comparable
    → inside object/class

    Comparator
    → outside object/class

▸ How to Implement compareTo()

Prefer comparison methods rather than subtraction.

Good:

    @Override
    public int compareTo(Student other) {
        return Integer.compare(this.id, other.id);
    }

Avoid:

    return this.id - other.id;

Why?

Because subtraction can overflow.

Example:

    this.id = Integer.MAX_VALUE;
    other.id = -1;

Then:

    this.id - other.id

can overflow and produce an incorrect comparison result.

**Interview best practice:**

> Use `Integer.compare()`, `Long.compare()`, `Double.compare()`, etc., instead of subtracting values in `compareTo()`.

▸ Comparing Multiple Fields

Suppose Student has:

    id
    name
    age

We can define natural ordering using multiple fields.

Example:

    @Override
    public int compareTo(Student other) {

        int result =
            Integer.compare(this.id, other.id);

        if (result != 0) {
            return result;
        }

        return this.name.compareTo(other.name);
    }

This means:

    First compare id
    ↓
    If id is same
    ↓
    Compare name

▸ Comparator.comparing()

For custom ordering, Comparator provides a cleaner approach.

Example:

    Comparator<Student> comparator =
        Comparator.comparingInt(student -> student.id);

Then:

    students.sort(comparator);

▸ Comparable Contract

`compareTo()` should follow a consistent contract.

Important properties:

1. Negative when current object is smaller.
2. Zero when objects are equivalent according to ordering.
3. Positive when current object is greater.
4. Comparison should be consistent.
5. Comparison should be transitive.

▸ Signum Rule

If:

    x.compareTo(y)

is negative, then:

    y.compareTo(x)

should be positive.

If:

    x.compareTo(y) == 0

then:

    y.compareTo(x) == 0

This maintains consistent ordering.

▸ Transitivity

Suppose:

    A < B
    B < C

Then:

    A < C

The comparison logic should maintain this property.

Bad comparison logic can break sorted collections and sorting algorithms.

▸ Consistency with equals()

Ideally:

    x.compareTo(y) == 0

should be consistent with:

    x.equals(y)

But Java does not require this in every case.

The Comparable documentation recommends consistency with equals when possible.

**Important for TreeSet/TreeMap:**

If:

    compareTo() == 0

TreeSet/TreeMap can treat objects as equivalent for ordering purposes, even when:

    equals() == false

▸ TreeSet Example of compareTo() == 0

Suppose:

    class Student implements Comparable<Student> {

        int id;
        String name;

        @Override
        public int compareTo(Student other) {
            return Integer.compare(this.id, other.id);
        }
    }

Now:

    Student s1 =
        new Student(1, "John");

    Student s2 =
        new Student(1, "David");

If:

    s1.equals(s2)

is false,

but:

    s1.compareTo(s2) == 0

then TreeSet may store only one of them.

Why?

Because TreeSet uses comparison to determine ordering and element equivalence.

**Interview one-liner:**

> In TreeSet, `compareTo() == 0` means the elements are considered equivalent for Set ordering, even if `equals()` returns false.

▸ Comparable and equals()

A good implementation should ideally make:

    compareTo() == 0

consistent with:

    equals()

Example:

    @Override
    public boolean equals(Object obj) {
        ...
    }

    @Override
    public int hashCode() {
        ...
    }

    @Override
    public int compareTo(Student other) {
        ...
    }

The equality and ordering logic should represent the same logical identity whenever practical.

▸ Important Difference from hashCode()

Hash-based collections use:

    hashCode()
    equals()

Sorted collections use:

    compareTo()
    or Comparator.compare()

Remember:

    HashSet / HashMap
    → hashCode() + equals()

    TreeSet / TreeMap
    → compareTo() / Comparator

▸ What Happens If compareTo() Returns 0?

For sorting:

    a.compareTo(b) == 0

means they have the same ordering position.

For TreeSet:

    only one is retained

For TreeMap:

    they are treated as equivalent keys.

This is a very important interview point.

▸ Comparable and HashSet

HashSet does NOT use Comparable to determine duplicates.

Example:

    HashSet<Student>

uses:

    hashCode()
    equals()

TreeSet uses:

    compareTo()
    or Comparator

Therefore:

    HashSet
    → equals + hashCode

    TreeSet
    → compareTo / Comparator

▸ Comparable and PriorityQueue

PriorityQueue can also use natural ordering.

Example:

    PriorityQueue<Integer> queue =
        new PriorityQueue<>();

    queue.add(30);
    queue.add(10);
    queue.add(20);

The smallest element has priority:

    10

For custom objects:

    PriorityQueue<Student> queue =
        new PriorityQueue<>();

the Student class can implement Comparable.

▸ Comparable and Arrays.sort()

For objects implementing Comparable:

    Student[] students = ...;

    Arrays.sort(students);

can use their natural ordering.

Example:

    class Student implements Comparable<Student> {

        int id;

        @Override
        public int compareTo(Student other) {
            return Integer.compare(this.id, other.id);
        }
    }

Then:

    Arrays.sort(students);

sorts students using:

    compareTo()

▸ Comparable and Arrays.sort() with Comparator

You can also provide a custom Comparator:

    Arrays.sort(
        students,
        Comparator.comparingInt(student -> student.id)
    );

This uses Comparator instead of the natural ordering.

▸ When Should We Use Comparable?

Use Comparable when:

- The class has one clear natural ordering.
- You own the class.
- The natural ordering makes sense for most use cases.
- You want TreeSet/TreeMap/PriorityQueue/sorting to use that default ordering.

Example:

    Employee
    → natural ordering by employeeId

    Student
    → natural ordering by studentId

    Product
    → natural ordering by productId

▸ When Should We Use Comparator?

Use Comparator when:

- Multiple orderings are required.
- You don't control the class.
- The ordering is specific to one use case.
- You don't want to change the class itself.

Example:

    Student by id
    Student by name
    Student by marks

Use multiple Comparators.

▸ Example: Multiple Comparators

    Comparator<Student> byId =
        Comparator.comparingInt(student -> student.id);

    Comparator<Student> byName =
        Comparator.comparing(student -> student.name);

    Comparator<Student> byAge =
        Comparator.comparingInt(student -> student.age);

Now:

    students.sort(byId);

or:

    students.sort(byName);

or:

    students.sort(byAge);

The class can still have one natural ordering.

▸ Reverse Natural Ordering

If the natural ordering is ascending:

    students.sort(null);

You can reverse it:

    students.sort(
        Comparator.reverseOrder()
    );

This works when the elements implement Comparable.

▸ Comparable with Collections.min()

For a collection of Comparable objects:

    List<Integer> numbers =
        Arrays.asList(30, 10, 20);

    Integer min =
        Collections.min(numbers);

Result:

    10

The natural ordering is used.

▸ Comparable with Collections.max()

    Integer max =
        Collections.max(numbers);

Result:

    30

Natural ordering is used.

▸ Common Interview Question

**Q: What is Comparable?**

> Comparable is an interface used to define the natural ordering of objects through the `compareTo()` method.

▸ Common Interview Question

**Q: Which package contains Comparable?**

> `java.lang`, so it is automatically imported.

▸ Common Interview Question

**Q: Which method does Comparable contain?**

> `compareTo()`.

Signature:

    int compareTo(T o);

▸ Common Interview Question

**Q: What does compareTo() return?**

> Negative if the current object is smaller, zero if they are equivalent in ordering, and positive if the current object is greater.

▸ Common Interview Question

**Q: Where is Comparable implemented?**

> Inside the class whose objects need a natural ordering.

▸ Common Interview Question

**Q: What is natural ordering?**

> The default ordering defined by a class through its `Comparable` implementation.

▸ Common Interview Question

**Q: Comparable vs Comparator?**

> Comparable defines one natural ordering inside the class, while Comparator defines external/custom orderings.

▸ Common Interview Question

**Q: Can a class implement multiple Comparables?**

> No. A class can implement Comparable with one type and therefore define one natural ordering.

▸ Common Interview Question

**Q: Can a class have multiple Comparators?**

> Yes. Multiple Comparators can define different orderings for the same class.

▸ Common Interview Question

**Q: Why use Comparator instead of Comparable?**

> Use Comparator when you need multiple orderings or cannot modify the class.

▸ Common Interview Question

**Q: Which collection uses Comparable for natural ordering?**

> TreeSet, TreeMap keys, and PriorityQueue can use natural ordering when no Comparator is provided.

▸ Common Interview Question

**Q: Does HashSet use Comparable?**

> No. HashSet uses `hashCode()` and `equals()` for determining duplicates.

▸ Common Interview Question

**Q: Does TreeSet use equals() to determine duplicates?**

> TreeSet primarily uses `compareTo()` or `Comparator.compare()` for ordering and determining equivalence.

▸ Common Interview Question

**Q: What happens if compareTo() returns 0 for two different objects?**

> Sorted collections such as TreeSet can treat them as equivalent even if `equals()` returns false.

▸ Common Interview Question

**Q: Should compareTo() be consistent with equals()?**

> Ideally yes, because inconsistent ordering can cause surprising behavior in sorted collections.

▸ Common Interview Question

**Q: Why should we avoid `this.id - other.id` in compareTo()?**

> Because subtraction can overflow and produce an incorrect comparison result.

Prefer:

    Integer.compare(
        this.id,
        other.id
    );

▸ Common Interview Question

**Q: Can Comparable be implemented using a lambda?**

> No. Use Comparator for lambda-based custom comparisons.

▸ Common Interview Question

**Q: What is the difference between compareTo() and equals()?**

> `compareTo()` determines ordering, while `equals()` determines logical equality.

▸ Common Interview Question

**Q: What is the difference between compareTo() and hashCode()?**

> `compareTo()` is used for ordering, while `hashCode()` is used to support hash-based lookup and must be consistent with equals.

▸ Quick Comparison

| Feature | Comparable | Comparator |
|---|---|---|
| Purpose | Natural ordering | Custom ordering |
| Method | `compareTo()` | `compare()` |
| Location | Inside class | Outside class |
| Package | `java.lang` | `java.util` |
| Number of orderings | Usually one | Multiple |
| Lambda | No | Yes |
| Modifies class | Yes | No |
| TreeSet support | Yes | Yes |
| TreeMap support | Yes, for keys | Yes |
| PriorityQueue support | Yes | Yes |

▸ Comparable vs Comparator Example

    Comparable:

    class Student implements Comparable<Student> {

        int id;

        @Override
        public int compareTo(Student other) {
            return Integer.compare(this.id, other.id);
        }
    }

    Comparator:

    Comparator<Student> byName =
        Comparator.comparing(
            student -> student.name
        );

Think:

    Comparable
    → Student decides default ordering

    Comparator
    → Caller decides required ordering

▸ Most Important Connection

Remember this chain:

    Comparable
        ↓
    compareTo()
        ↓
    Natural Ordering
        ↓
    TreeSet / TreeMap / PriorityQueue / sorting

And:

    Comparator
        ↓
    compare()
        ↓
    Custom Ordering
        ↓
    TreeSet / TreeMap / PriorityQueue / sorting

▸ One-Liner to Remember

> **Comparable = define the object's natural ordering inside the class using `compareTo()`.**

▸ Final Interview Definition

> Comparable is a Java interface that allows a class to define its natural ordering by implementing the `compareTo()` method, which is used by sorting algorithms and sorted collections such as TreeSet and TreeMap when no Comparator is provided.