# Algorithms and Data Structures

## 1. What is an Algorithm?

An algorithm is a sequence of steps used to solve a problem or perform a specific task.

For example, to find the largest number in an array:

```c
int max = numbers[0];

for (int i = 1; i < 5; i++) {
    if (numbers[i] > max) {
        max = numbers[i];
    }
}
```

Here, the **array** is the data structure, while the operation performed on it is an **algorithm**.

---

## 2. What is a Data Structure and Why Do We Use It?

Data structures determine how data is organized in memory and how it can be accessed.

Different data structures exist because each one provides advantages for different types of operations.

When choosing a data structure, we usually consider operations such as:

| Operation | Description |
|---|---|
| Access | Accessing an element |
| Search | Searching for an element |
| Insert | Adding a new element |
| Delete | Removing an element |

For example, in an array, we can directly access an element if we know its index. In a Linked List, we may need to follow the nodes one by one to reach the same element.

---

## 3. Big-O Notation

**Big-O notation** describes how the running cost of an algorithm grows as the input size increases.

Instead of focusing only on how many seconds an algorithm takes to run, we look at how the number of operations changes when the amount of data increases.

### O(1) – Constant Time

The operation takes approximately the same amount of work regardless of the input size.

```c
printf("%d", array[3]);
```

Accessing an array element by its index:

```text
O(1)
```

### O(n) – Linear Time

The number of operations increases approximately at the same rate as the input size.

```c
for (int i = 0; i < n; i++) {
    printf("%d\n", array[i]);
}
```

The time complexity of this loop is:

```text
O(n)
```

### O(n²) – Quadratic Time

This is commonly seen with nested loops.

```c
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        printf("%d %d\n", i, j);
    }
}
```

Complexity:

```text
O(n²)
```

### O(log n) – Logarithmic Time

This can occur when a significant part of the problem is eliminated at each step.

One of the most common examples is **Binary Search**.

```text
[1, 4, 7, 10, 15, 20, 25, 30]
              ↑
        Start from the middle
```

Depending on the value we are searching for, half of the remaining elements can be eliminated at each step.

```text
Binary Search → O(log n)
```

In general:

```text
O(1) < O(log n) < O(n) < O(n log n) < O(n²)
```

---

## 4. Array

An Array is one of the basic data structures used to store elements in an ordered structure.

```c
int numbers[5] = {10, 20, 30, 40, 50};
```

Structure:

```text
[10][20][30][40][50]
 0   1   2   3   4
```

An element can be accessed directly using its index:

```c
numbers[3]
```

Result:

```text
40
```

Therefore, accessing an element by index takes:

```text
O(1)
```

However, when searching for a specific value, we may need to check the entire array in the worst case:

```text
Search → O(n)
```

---

## 5. Linked List

A Linked List consists of connected **nodes**.

Each node stores its own data and the address of the next node.

```text
[10 | •] → [20 | •] → [30 | •] → NULL
```

A simple node in C can be defined as:

```c
struct Node {
    int data;
    struct Node *next;
};
```

The `next` pointer stores the address of the next node.

Unlike an array, reaching a specific element may require following the nodes one by one.

```text
[10] → [20] → [30]
```

Therefore, accessing an arbitrary element can take:

```text
O(n)
```

---

## 6. Stack

A Stack works according to the **LIFO (Last In, First Out)** principle.

This means that the last element added is the first element removed.

```text
    [C] ← Removed first
    [B]
    [A]
```

The main Stack operations are:

| Operation | Function |
|---|---|
| `push` | Adds an element to the Stack |
| `pop` | Removes the top element |
| `peek` | Returns the top element without removing it |

One common use of a Stack is managing **function calls**.

For example:

```text
main()
  ↓
functionA()
  ↓
functionB()
```

The call stack can be represented as:

```text
functionB
functionA
main
```

Since `functionB` was added last, it finishes and is removed from the Stack first.

---

## 7. Queue

A Queue works according to the **FIFO (First In, First Out)** principle.

This means that the first element added is the first element removed.

```text
A → B → C → D
```

Since `A` was added first, it will also be removed first.

The main Queue operations are:

| Operation | Function |
|---|---|
| `enqueue` | Adds an element to the Queue |
| `dequeue` | Removes the first element |

A Queue can be compared to a real-life waiting line:

```text
First In → First Out
```

---

## 8. Stack vs Queue

| | Stack | Queue |
|---|---|---|
| Principle | LIFO | FIFO |
| Adding | Push | Enqueue |
| Removing | Pop | Dequeue |
| Example | Stack of plates | Waiting line |

---

## 9. HashMap

A HashMap stores data as **key-value pairs**.

For example:

```text
"Simay" → 95
"Alex"  → 80
"John"  → 72
```

Here, `"Simay"` is the **key**, while `95` is the **value** associated with that key.

A HashMap uses a **hash function** to determine where the data should be stored.

```text
Key
 ↓
Hash Function
 ↓
Bucket / Index
 ↓
Value
```

This allows the program to locate data quickly instead of checking every element one by one.

On average:

```text
Search → O(1)
Insert → O(1)
Delete → O(1)
```

However, situations such as hash collisions can affect performance, so these are **average-case complexities**.

---

## 10. Comparison of Data Structures

| Data Structure | Main Principle | Example Use |
|---|---|---|
| Array | Index-based access | Student grades |
| Linked List | Connected nodes | Dynamically linked data |
| Stack | LIFO | Undo operations, function calls |
| Queue | FIFO | Task and process queues |
| HashMap | Key → Value | Finding a user by username |

Basic Big-O comparison:

| Data Structure | Access | Search | Insert | Delete |
|---|---:|---:|---:|---:|
| Array | `O(1)` | `O(n)` | `O(n)` | `O(n)` |
| Linked List | `O(n)` | `O(n)` | `O(1)*` | `O(1)*` |
| Stack | `O(n)` | `O(n)` | `O(1)` | `O(1)` |
| Queue | `O(n)` | `O(n)` | `O(1)` | `O(1)` |
| HashMap | — | `O(1)` avg. | `O(1)` avg. | `O(1)` avg. |

\* In a Linked List, insertion or deletion can be `O(1)` if the required node or position is already known. If the node must first be found, the search cost also needs to be considered.

---

## 11. General Logic

The main purpose of algorithms and data structures is to store data appropriately and perform operations on that data as efficiently as possible.

The general process can be represented as:

```text
Problem
   ↓
Data to be stored
   ↓
Choose an appropriate data structure
   ↓
Apply an algorithm
   ↓
Evaluate performance using Big-O
```

Therefore, **data structures, algorithms, and Big-O** are not completely separate concepts. They are different parts of designing an efficient solution to a problem.