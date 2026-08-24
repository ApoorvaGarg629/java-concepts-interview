
> Arrays are the foundation of data structures in Java. Interview questions focus on declaration, memory layout, traversal, copying, passing to methods, multidimensional arrays, and the `Arrays` utility class.

Next: [Generics](05-Generics.md)

---

# What is an Array? ⭐⭐⭐

An array is a **fixed-size collection of elements of the same data type** stored in **contiguous memory**.

```java
int[] arr = {10, 20, 30, 40};
```

### Characteristics

- Fixed size
- Same data type
- Zero-based indexing
- Stored in contiguous memory
- Fast random access → **O(1)**

---

# How Arrays are Stored in Memory ⭐⭐⭐

```java
int[] arr = {10, 20, 30};
```

| Memory | Stores |
|---------|--------|
| Stack | Reference `arr` |
| Heap | Array object `[10,20,30]` |

> **Interview:** Arrays are objects in Java and are always allocated on the **Heap**.

---

# Array Declaration (All Ways) ⭐⭐⭐

## 1. Declaration Only

```java
int[] arr;
```

or

```java
int arr[];
```

> Prefer `int[] arr` (Java standard).

---

## 2. Declaration + Memory Allocation

```java
int[] arr = new int[5];
```

Creates an array of size 5.

### Default Values

| Type | Default |
|------|----------|
| int | 0 |
| long | 0 |
| double | 0.0 |
| boolean | false |
| char | `'\u0000'` |
| Object | null |

---

## 3. Declaration + Initialization

```java
int[] arr = {1, 2, 3, 4};
```

Most commonly used.

---

## 4. Dynamic Initialization

Useful when assigning later.

```java
int[] arr;

arr = new int[]{1,2,3};
```

---

## 5. Multi-dimensional Array

```java
int[][] matrix = new int[2][3];
```

Initialize directly:

```java
int[][] matrix = {
    {1,2,3},
    {4,5,6}
};
```

---

## 6. Jagged Array

Each row can have different lengths.

```java
int[][] arr = new int[3][];

arr[0] = new int[2];
arr[1] = new int[4];
arr[2] = new int[1];
```

---

# Accessing Elements ⭐⭐

```java
int[] arr = {5,10,15};

System.out.println(arr[0]);     // 5
System.out.println(arr[2]);     // 15
```

Length:

```java
arr.length
```

> `length` is a **property**, not a method.

---

# Traversing Arrays ⭐⭐⭐

## 1. Traditional for Loop

```java
for(int i=0;i<arr.length;i++){
    System.out.println(arr[i]);
}
```

Best when index is needed.

---

## 2. Enhanced for-each Loop

```java
for(int num : arr){
    System.out.println(num);
}
```

Best for read-only traversal.

Cannot modify the original array using `num`.

---

## 3. While Loop

```java
int i = 0;

while(i < arr.length){
    System.out.println(arr[i]);
    i++;
}
```

---

# Array Input ⭐⭐

```java
Scanner sc = new Scanner(System.in);

int n = sc.nextInt();

int[] arr = new int[n];

for(int i=0;i<n;i++){
    arr[i] = sc.nextInt();
}
```

---

# Passing Arrays to Methods ⭐⭐⭐

Arrays are passed **by value of reference**.

```java
void update(int[] arr){
    arr[0] = 100;
}
```

```java
int[] nums = {1,2,3};

update(nums);

System.out.println(nums[0]);
```

Output:

```text
100
```

### Why?

Java copies the **reference**, not the entire array.

---

# Returning Arrays ⭐⭐

```java
public int[] createArray(){

    return new int[]{1,2,3};
}
```

Usage:

```java
int[] arr = createArray();
```

---

# Copying Arrays ⭐⭐⭐

## 1. Reference Copy

```java
int[] a = {1,2,3};

int[] b = a;
```

Both refer to the same array.

Changing one affects both.

---

## 2. `clone()`

```java
int[] b = a.clone();
```

Creates a new array.

---

## 3. `Arrays.copyOf()`

```java
int[] copy =
    Arrays.copyOf(a, a.length);
```

Can also change size.

```java
Arrays.copyOf(a, 5);
```

Output:

```text
[1,2,3,0,0]
```

---

## 4. `System.arraycopy()`

```java
System.arraycopy(a, 0, b, 0, 3);
```

Fastest built-in copy method.

### Comparison

| Method | New Array |
|---------|-----------|
| `=` | ❌ |
| `clone()` | ✅ |
| `Arrays.copyOf()` | ✅ |
| `System.arraycopy()` | ❌ (copies into existing array) |

---

# Shallow vs Deep Copy ⭐⭐⭐

## Primitive Array

```java
int[] a = {1,2,3};

int[] b = a.clone();
```

Safe because primitives are copied.

---

## Object Array

```java
Student[] a = new Student[2];

Student[] b = a.clone();
```

Only references are copied.

Both arrays point to the same `Student` objects.

> **Interview:** `clone()` performs a **shallow copy** for object arrays.

---

# Multi-dimensional Arrays ⭐⭐⭐

```java
int[][] matrix = {
    {1,2},
    {3,4}
};
```

Access:

```java
matrix[1][0]
```

Output:

```text
3
```

Traversal:

```java
for(int i=0;i<matrix.length;i++){
    for(int j=0;j<matrix[i].length;j++){
        System.out.print(matrix[i][j]+" ");
    }
}
```

---

# Jagged vs Matrix ⭐⭐

### Matrix

```java
int[][] arr = new int[2][3];
```

Equal columns.

### Jagged

```java
int[][] arr = {
    {1},
    {2,3},
    {4,5,6}
};
```

Different column sizes.

---

# Arrays Utility Class ⭐⭐⭐

Import:

```java
import java.util.Arrays;
```

---

## Sort

```java
Arrays.sort(arr);
```

Ascending order.

---

## Binary Search

```java
Arrays.binarySearch(arr, 30);
```

Array must already be sorted.

Returns index.

---

## Fill

```java
Arrays.fill(arr, 5);
```

Output:

```text
[5,5,5,5]
```

---

## Equals

```java
Arrays.equals(a, b);
```

Compares contents.

Not references.

---

## Deep Equals

```java
Arrays.deepEquals(arr1, arr2);
```

Used for multi-dimensional arrays.

---

## toString

```java
System.out.println(Arrays.toString(arr));
```

Output:

```text
[1, 2, 3]
```

---

## deepToString

```java
System.out.println(Arrays.deepToString(matrix));
```

Output:

```text
[[1, 2], [3, 4]]
```

---

# Array Time Complexities ⭐⭐⭐

| Operation | Complexity |
|-----------|------------|
| Access | O(1) |
| Update | O(1) |
| Search | O(n) |
| Binary Search | O(log n) |
| Insert (middle) | O(n) |
| Delete | O(n) |

---

# Common Interview Edge Cases ⭐⭐⭐

## Empty Array

```java
int[] arr = new int[0];

System.out.println(arr.length);
```

Output:

```text
0
```

---

## Index Out of Bounds

```java
int[] arr = {1,2};

System.out.println(arr[5]);
```

Throws:

```text
ArrayIndexOutOfBoundsException
```

---

## Length vs Length()

```java
arr.length
```

Correct

```java
arr.length()
```

Incorrect

| Array | String |
|--------|---------|
| `length` | `length()` |

---

## Array of Objects

```java
String[] names = new String[3];

System.out.println(names[0]);
```

Output:

```text
null
```

Default value of object arrays is `null`.

---

## Can Array Size Change?

```java
int[] arr = new int[5];
```

No.

To increase size:

```java
arr = Arrays.copyOf(arr, 10);
```

A new array is created.

---

# Arrays vs ArrayList ⭐⭐⭐

| Array | ArrayList |
|--------|-----------|
| Fixed size | Dynamic size |
| Stores primitives | Stores objects |
| Faster | Slightly slower |
| `length` | `size()` |
| No built-in add/remove | Supports add/remove |

---

# Most Asked Interview Questions ⭐⭐⭐

### Why are arrays faster than ArrayList?

Arrays provide direct indexed access without resizing overhead.

### Where are arrays stored?

- Reference → Stack (local variable)
- Array object → Heap

### Difference between `clone()` and `=`?

- `=` copies the reference.
- `clone()` creates a new array.

### Difference between `Arrays.equals()` and `==`?

- `==` compares references.
- `Arrays.equals()` compares contents.

### Why is `length` not a method?

Because array size is a built-in property stored with the array object.

---

# Revision Checklist

- [ ] Array declaration methods
- [ ] Default values
- [ ] Memory representation
- [ ] Traversal techniques
- [ ] Passing arrays to methods
- [ ] Returning arrays
- [ ] Copying (`=`, `clone`, `copyOf`, `arraycopy`)
- [ ] Multi-dimensional arrays
- [ ] Jagged arrays
- [ ] Arrays utility methods
- [ ] Time complexities
- [ ] Arrays vs ArrayList