# Memory and Performance Basics

Understanding how a program uses memory is important for writing efficient and reliable software. Two of the most important memory areas are the **stack** and the **heap**.

Memory management also includes concepts such as **garbage collection** and **memory leaks**. These concepts help explain why some programs become slow, consume too much memory, or eventually crash.

---

## 1. Program Memory

When a program runs, it needs memory to store different kinds of information.

For example, a program may need memory for:

- local variables
- function calls
- objects
- arrays
- dynamically allocated data

Two important areas used for this purpose are:

```text
Program Memory
│
├── Stack
│   ├── Function calls
│   ├── Local variables
│   └── Temporary data
│
└── Heap
    ├── Objects
    ├── Dynamic data
    └── Data with flexible lifetime
```

The stack and heap have different purposes and different ways of managing memory.

---

## 2. Stack Memory

The **stack** is a memory area mainly used for function or method calls and their local data.

When a function is called, a new section called a **stack frame** is created.

A stack frame usually contains information such as:

- local variables
- function parameters
- return information

For example:

```c
void calculate() {
    int x = 5;
    int y = 10;
}
```

While `calculate()` is running, its local variables belong to its stack frame.

A simplified representation is:

```text
STACK

┌─────────────────┐
│ calculate()     │
│ x = 5           │
│ y = 10          │
└─────────────────┘
```

When the function finishes, its stack frame is automatically removed.

This means that stack memory management is mostly automatic.

---

## 3. How the Stack Works

The stack follows the principle:

**LIFO — Last In, First Out**

This means that the last function called is the first one to finish and leave the stack.

For example:

```text
main()
  ↓
functionA()
  ↓
functionB()
```

The stack could look like this:

```text
┌─────────────────┐
│ functionB()     │ ← removed first
├─────────────────┤
│ functionA()     │
├─────────────────┤
│ main()          │
└─────────────────┘
```

When `functionB()` finishes, its frame is removed.

Then `functionA()` continues.

---

## 4. Stack Overflow

The stack has a limited size.

If too many function calls are created without returning, the stack can run out of space.

A common example is incorrect recursion:

```c
void hello() {
    hello();
}
```

This function continuously calls itself.

The stack becomes:

```text
hello()
hello()
hello()
hello()
hello()
...
```

Eventually there is no space left for another stack frame.

This causes a:

**Stack Overflow**

Therefore, recursion must always have a condition that eventually stops the recursive calls.

---

## 5. Heap Memory

The **heap** is another memory area used during program execution.

Unlike the stack, the heap is mainly used for data whose size or lifetime may not be known in advance.

For example, in Java:

```java
Person person = new Person();
```

The `Person` object is created on the heap.

A simplified representation is:

```text
STACK                    HEAP

person ────────────────> Person object
```

The variable `person` contains a **reference** to the object.

The actual object exists on the heap.

This distinction is important:

```text
Reference                  Object
   │                          │
   │                          │
STACK ────────────────────> HEAP
```

---

## 6. Dynamic Memory Allocation

Heap memory is commonly associated with **dynamic memory allocation**.

Dynamic allocation means that memory is requested while the program is running.

In C:

```c
int *number = malloc(sizeof(int));
```

In C++:

```cpp
int *number = new int;
```

In Java:

```java
Person person = new Person();
```

These operations allocate memory dynamically.

However, different programming languages handle the release of this memory differently.

In C:

```c
free(number);
```

In C++:

```cpp
delete number;
```

The programmer is responsible for releasing dynamically allocated memory.

Java works differently because it uses **Garbage Collection**.

---

## 7. Stack vs Heap

The main differences can be summarized as follows:

| Stack | Heap |
|---|---|
| Used mainly for function calls and local data | Used mainly for dynamically created objects/data |
| Memory is managed automatically | Memory management depends on the language |
| Usually faster | Usually more expensive to allocate |
| Limited in size | Usually much larger |
| Uses stack frames | Uses dynamic allocation |
| Data often has a short lifetime | Data can have a longer or flexible lifetime |

A simple example:

```java
void example() {

    int age = 21;

    Person person = new Person();
}
```

A simplified memory representation is:

```text
STACK                         HEAP

age = 21

person ─────────────────────> Person object
```

The local variable `age` belongs to the current stack frame.

The reference `person` is also associated with the method's local data, while the object created with `new Person()` exists on the heap.

---

## 8. Garbage Collection

Languages such as Java use **Garbage Collection (GC)** to automatically manage heap memory.

The Garbage Collector searches for objects that are no longer reachable by the running program.

Consider:

```java
Person person = new Person();

person = null;
```

Initially:

```text
STACK                    HEAP

person ────────────────> Person object
```

After:

```java
person = null;
```

we have:

```text
STACK                    HEAP

person = null            Person object
                              ↑
                       no reference
```

The object is no longer reachable through `person`.

It becomes **eligible for garbage collection**.

At some later point, the Garbage Collector may reclaim the memory used by that object.

---

## 9. Reachable and Unreachable Objects

Garbage Collection is strongly related to the idea of **reachability**.

An object is **reachable** when the program can still access it through a chain of references.

For example:

```text
root
 │
 ▼
Object A
 │
 ▼
Object B
```

Both objects are reachable.

But consider:

```text
root ──────> Object A


             Object B
```

If nothing references `Object B`, it is unreachable.

The Garbage Collector can eventually reclaim its memory.

Therefore, a simplified GC idea is:

```text
Can the program still reach this object?

        │
   ┌────┴────┐
   │         │
  YES        NO
   │         │
 Keep      Garbage
object     candidate
```

Real garbage collectors use more sophisticated algorithms, but reachability is the key concept.

---

## 10. Why Garbage Collection Can Affect Performance

Garbage Collection makes memory management easier for programmers, but it is not free.

The runtime must periodically:

- examine objects
- determine which objects are reachable
- reclaim unused memory
- sometimes reorganize memory

This consumes CPU time.

For example, a program that continuously creates unnecessary objects:

```java
for (int i = 0; i < 1000000; i++) {
    Person p = new Person();
}
```

creates many temporary objects.

Eventually many of them become unreachable.

The Garbage Collector must process these objects.

Therefore:

```text
Too many allocations
        ↓
More unused objects
        ↓
More GC work
        ↓
Possible performance overhead
```

This does not mean objects should be avoided. It means unnecessary allocations can become expensive in performance-sensitive programs.

---

## 11. Memory Leaks

A **memory leak** happens when memory is no longer useful to the program but cannot be properly reclaimed.

Over time:

```text
Memory usage

│                         █
│                     █████
│                 █████████
│             █████████████
│         █████████████████
│     █████████████████████
└────────────────────────────> Time
```

If memory usage continues growing, the program may eventually:

- become slower
- consume excessive RAM
- run out of memory
- crash

---

## 12. Memory Leaks in C and C++

In languages with manual memory management, forgetting to release allocated memory can create a memory leak.

Example in C:

```c
void example() {

    int *number = malloc(sizeof(int));

}
```

Memory was allocated with:

```c
malloc()
```

but it was never released with:

```c
free()
```

If this happens repeatedly, more and more memory remains allocated.

The correct version is:

```c
void example() {

    int *number = malloc(sizeof(int));

    free(number);
}
```

Similarly, C++ programs using `new` must manage the corresponding lifetime correctly, traditionally with `delete`.

Modern C++ usually prefers techniques such as **RAII** and smart pointers instead of manually pairing every `new` with `delete`.

---

## 13. Can Java Have Memory Leaks?

Yes.

Java has Garbage Collection, but memory leaks are still possible.

The Garbage Collector removes **unreachable objects**.

It cannot remove an object that is still reachable, even if the program no longer actually needs it.

For example:

```java
List<Person> people = new ArrayList<>();

while (true) {

    Person p = new Person();

    people.add(p);
}
```

Every new `Person` is added to the list.

Therefore:

```text
people
  │
  ├──> Person
  ├──> Person
  ├──> Person
  ├──> Person
  ├──> Person
  └──> ...
```

All objects remain reachable through `people`.

The Garbage Collector therefore cannot remove them.

If the list keeps growing unnecessarily, memory usage also keeps growing.

This is a form of memory leak.

The important rule is:

> Garbage Collection does not mean that memory leaks are impossible.

GC only removes objects that are no longer reachable.

---

## 14. Memory and Performance

Memory management can directly affect program performance.

Several common problems include:

### Too Many Allocations

Creating huge numbers of temporary objects can increase allocation and garbage collection work.

```text
Many allocations
      ↓
More memory usage
      ↓
More GC activity
      ↓
Possible slowdown
```

### Keeping Unnecessary References

If references to unused objects remain:

```text
Reference
    │
    ▼
Unused Object
```

the Garbage Collector may still consider the object reachable.

This prevents the memory from being reclaimed.

### Large Data Structures

Large arrays, lists, maps, and other structures can consume significant heap memory.

For example:

```java
int[] data = new int[100000000];
```

Programs should therefore avoid storing data that they do not actually need.

---

## 15. Stack and Heap Example

Consider:

```java
public static void main(String[] args) {

    int age = 21;

    Person person = new Person("Simay");

    printPerson(person);
}
```

A simplified representation could be:

```text
STACK                              HEAP

main()
┌─────────────────────┐
│ age = 21            │
│ person ─────────────┼──────────> Person
└─────────────────────┘             name = "Simay"


printPerson()
┌─────────────────────┐
│ person ──────────────┼───────────┘
└─────────────────────┘
```

Both method frames can contain references to the same object.

When `printPerson()` finishes, its stack frame disappears.

The object does not necessarily disappear because `main()` still has a reference to it.

This demonstrates an important difference between stack frames and heap objects.

---

## 16. Common Mistakes

### 1. Thinking that a reference is the object itself

```java
Person p = new Person();
```

`p` is a reference.

The `Person` object is a separate object in memory.

---

### 2. Thinking that Garbage Collection happens immediately

An unreachable object becomes **eligible** for garbage collection.

This does not mean that it is immediately deleted at that exact moment.

---

### 3. Thinking that Java cannot have memory leaks

Java can have memory leaks when unnecessary objects remain reachable.

---

### 4. Forgetting to release manually allocated memory

In languages such as C, forgetting:

```c
free(pointer);
```

after dynamically allocating memory can cause leaks.

---

### 5. Confusing stack overflow with heap exhaustion

They are different problems.

```text
Too many nested function calls
          ↓
     Stack Overflow
```

while excessive heap usage can lead to an **out-of-memory** condition.

---

## 17. Summary

The **stack** is primarily associated with function calls, stack frames, and local execution data. It is fast and automatically managed as functions enter and return.

The **heap** stores dynamically allocated data such as objects. Heap objects can have lifetimes independent of the function that originally created them.

Languages such as Java use **Garbage Collection** to identify objects that are no longer reachable and reclaim their memory.

However, Garbage Collection does not prevent every memory problem. Objects that remain reachable through unnecessary references cannot be collected and may cause **memory leaks**.

Understanding stack memory, heap memory, Garbage Collection, and memory leaks makes it easier to understand why a program consumes memory, why memory usage may continue increasing, and why memory management can influence application performance.

> **Note:** AI assistance was used to help create and format some of the diagrams in these notes.