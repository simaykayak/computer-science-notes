# Concurrency and Parallel Programming

Modern computers can perform many tasks during the same period of time. Programs may download data, respond to user input, process files, and perform calculations without waiting for every operation to finish one by one.

Two important concepts that make this possible are **concurrency** and **parallelism**.

However, when multiple tasks access the same data, new problems can appear. One of the most important is the **race condition**.

To control access to shared resources safely, programmers use synchronization mechanisms such as **mutexes, locks, and semaphores**.

---

## 1. Sequential Execution

The simplest execution model is **sequential execution**.

Tasks are executed one after another.

```text
Task A ────────>
                Task B ────────>
                                Task C ────────>
```

Task B starts after Task A finishes, and Task C starts after Task B finishes.

For example:

```text
1. Read file
2. Process data
3. Save result
```

This model is simple, but it may not use modern hardware efficiently.

---

## 2. Concurrency

**Concurrency** means that multiple tasks make progress during overlapping periods of time.

The tasks do not necessarily execute at exactly the same instant.

For example:

```text
Time ─────────────────────────────>

Task A: ████      ████
Task B:     ████      ████
```

The system switches between the tasks.

Conceptually:

```text
Task A
   ↓
Task B
   ↓
Task A
   ↓
Task B
```

Both tasks are active during the same general period.

A single CPU core can therefore support concurrency by rapidly switching between tasks.

This is often called **context switching**.

---

## 3. Parallelism

**Parallelism** means that multiple tasks are actually executing at the same time.

This normally requires multiple CPU cores or processing units.

For example:

```text
Time ─────────────────────────────>

Core 1: Task A ████████████████

Core 2: Task B ████████████████
```

Task A and Task B are physically executing simultaneously.

For example, if a computer has multiple CPU cores:

```text
CPU

Core 1 ─── Task A
Core 2 ─── Task B
Core 3 ─── Task C
Core 4 ─── Task D
```

different tasks may run at the same time.

---

## 4. Concurrency vs Parallelism

Although these terms are related, they do not mean the same thing.

| Concurrency | Parallelism |
|---|---|
| Multiple tasks make progress during overlapping time | Multiple tasks execute at the same instant |
| Can happen on one CPU core | Usually requires multiple processing units |
| Often involves switching between tasks | Tasks physically run simultaneously |
| Focuses on managing multiple tasks | Focuses on executing multiple tasks at once |

A simple way to remember the difference is:

```text
Concurrency:

A → B → A → B → A → B


Parallelism:

A → → → → → →
B → → → → → →
```

Concurrency is about **dealing with multiple tasks at once**.

Parallelism is about **doing multiple tasks at once**.

A concurrent program may also execute in parallel when multiple CPU cores are available.

---

## 5. Processes and Threads

Concurrency is often implemented using **processes** or **threads**.

A **process** is an instance of a running program.

For example:

```text
Operating System
│
├── Browser Process
├── Music Process
└── Code Editor Process
```

A process can contain one or more **threads**.

A thread is an execution path inside a process.

```text
Application Process
│
├── Thread 1
├── Thread 2
└── Thread 3
```

Threads belonging to the same process can usually access shared memory.

This makes communication efficient, but it also creates synchronization problems.

---

## 6. Shared Data

Consider two threads:

```text
Thread A ─────┐
              │
              ▼
          Shared Data
              ▲
              │
Thread B ─────┘
```

Both threads can access the same variable.

For example:

```java
int counter = 0;
```

Suppose both threads execute:

```java
counter++;
```

It may look like a simple operation.

However, conceptually it involves multiple steps:

```text
1. Read counter
2. Add 1
3. Write result
```

This becomes dangerous when multiple threads perform these operations at the same time.

---

## 7. Race Condition

A **race condition** occurs when the result of a program depends on the timing or ordering of concurrent operations.

Suppose:

```text
counter = 5
```

Two threads both want to increment it.

We expect:

```text
5 + 1 + 1 = 7
```

But the following execution is possible:

```text
Thread A reads counter → 5

Thread B reads counter → 5

Thread A calculates → 6

Thread B calculates → 6

Thread A writes → 6

Thread B writes → 6
```

The final value becomes:

```text
counter = 6
```

instead of:

```text
counter = 7
```

One update was effectively lost.

This is a **race condition**.

---

## 8. Why Race Conditions Are Dangerous

Race conditions can be difficult to find because they may not happen every time.

For example, the program might work correctly during one execution:

```text
Run 1 → Correct
```

but fail during another:

```text
Run 2 → Incorrect
```

The result depends on thread scheduling and timing.

This makes concurrency bugs especially difficult to reproduce and debug.

Race conditions can cause:

- incorrect calculations
- corrupted data
- inconsistent application state
- unexpected behavior
- crashes

---

## 9. Critical Section

A **critical section** is a part of the program that accesses shared data and therefore must be protected from unsafe simultaneous access.

For example:

```java
counter++;
```

If multiple threads modify `counter`, this operation may need protection.

Conceptually:

```text
Thread A
    │
    ▼
┌─────────────────┐
│ Critical Section│
│   counter++     │
└─────────────────┘
    ▲
    │
Thread B
```

We often want only one thread to execute this critical section at a time.

This is where synchronization mechanisms become important.

---

## 10. Locks

A **lock** is a synchronization mechanism used to control access to shared resources.

The basic idea is:

```text
Thread wants resource
        ↓
Acquire lock
        ↓
Access resource
        ↓
Release lock
```

If another thread already owns the lock:

```text
Thread A → LOCK → Critical Section

Thread B → waits...
```

After Thread A releases the lock:

```text
Thread A → releases LOCK

Thread B → acquires LOCK
           ↓
      Critical Section
```

This prevents multiple threads from modifying protected shared data at the same time.

---

## 11. Mutex

**Mutex** stands for:

**Mutual Exclusion**

A mutex allows only one thread at a time to enter a protected critical section.

Consider:

```text
             MUTEX
               │
        ┌──────┴──────┐
        │             │
     Thread A      Thread B
```

If Thread A acquires the mutex:

```text
Thread A → [LOCKED] → Shared Resource

Thread B → WAIT
```

After Thread A releases it:

```text
Thread A → release

Thread B → acquire → Shared Resource
```

The purpose is to guarantee **mutual exclusion**.

In other words:

> Only one thread can own the mutex and access the protected section at a time.

---

## 12. Mutex Example

Imagine a bank account:

```text
Balance = 1000
```

Two threads try to withdraw money.

Without synchronization:

```text
Thread A ─────┐
              ├──> Balance
Thread B ─────┘
```

both may read and modify the balance at the same time.

With a mutex:

```text
Thread A
   ↓
Acquire Mutex
   ↓
Read Balance
   ↓
Modify Balance
   ↓
Write Balance
   ↓
Release Mutex
```

Thread B must wait until the mutex becomes available.

This protects the shared state from conflicting modifications.

---

## 13. Lock vs Mutex

The terms **lock** and **mutex** are closely related and are sometimes used informally in similar ways.

A **lock** is the broader concept of controlling access to a resource.

A **mutex** is a synchronization mechanism specifically designed for mutual exclusion.

Conceptually:

```text
Lock
 │
 └── controls access

Mutex
 │
 └── ensures one owner at a time
```

The exact terminology and implementation depend on the programming language and operating system.

---

## 14. Semaphore

A **semaphore** is another synchronization mechanism.

Unlike a mutex, a semaphore can allow a specific number of threads to access a resource.

Think of a semaphore as a counter.

Suppose:

```text
Semaphore = 3
```

This means up to three tasks may access the resource.

```text
Thread A → allowed
Thread B → allowed
Thread C → allowed
Thread D → WAIT
```

When one thread leaves:

```text
Thread A → leaves

Thread D → allowed
```

The semaphore tracks how many available permits remain.

---

## 15. Semaphore Example

Imagine a parking lot with three spaces.

```text
Parking Lot

[Car A] [Car B] [Car C]

Available spaces = 0
```

Another car arrives:

```text
Car D → WAIT
```

When Car B leaves:

```text
Parking Lot

[Car A] [EMPTY] [Car C]
```

Car D can enter.

A semaphore works similarly.

```text
Semaphore = number of available permits
```

This makes semaphores useful when a limited number of tasks are allowed to use a resource simultaneously.

---

## 16. Mutex vs Semaphore

The main difference is the number of tasks allowed to access a resource.

| Mutex | Semaphore |
|---|---|
| Usually allows one owner at a time | Can allow multiple concurrent accesses |
| Used for mutual exclusion | Used for controlling access to limited resources |
| Protects critical sections | Maintains a number of permits |
| Locked / unlocked concept | Counter / permit concept |

A simple visualization:

```text
MUTEX

Resource
   ↑
only 1 thread


SEMAPHORE = 3

Resource
 ↑   ↑   ↑
T1  T2  T3
```

---

## 17. Binary Semaphore

A semaphore can also have only two states:

```text
0
or
1
```

This is called a **binary semaphore**.

It may look similar to a mutex because only one task can proceed at a time.

However, mutexes and binary semaphores can have different ownership and signaling semantics depending on the system.

Therefore, they should not always be treated as identical concepts.

---

## 18. Deadlock

Synchronization mechanisms solve many concurrency problems, but incorrect locking can introduce another problem:

**Deadlock**

A deadlock happens when tasks wait for each other indefinitely.

Example:

```text
Thread A owns Lock 1
Thread B owns Lock 2
```

Then:

```text
Thread A waits for Lock 2
Thread B waits for Lock 1
```

We now have:

```text
Thread A
   │
   │ waits for
   ▼
 Lock 2
   ▲
   │ owns
Thread B


Thread B
   │
   │ waits for
   ▼
 Lock 1
   ▲
   │ owns
Thread A
```

Neither thread can continue.

The program becomes stuck.

This is a **deadlock**.

---

## 19. Why More Threads Do Not Always Mean More Speed

It may seem logical that:

```text
More Threads = Faster Program
```

but this is not always true.

Threads introduce overhead.

For example:

- creating and managing threads costs resources
- context switching takes time
- synchronization takes time
- threads may wait for locks
- multiple threads may compete for CPU time
- shared resources can become bottlenecks

Therefore:

```text
More threads
     ↓
More possible parallel work
     ↓
BUT ALSO
     ↓
More synchronization and scheduling overhead
```

The best number of threads depends on the workload and hardware.

---

## 20. CPU-Bound and I/O-Bound Tasks

Concurrency and parallelism can be useful for different types of workloads.

## CPU-Bound

A CPU-bound task spends most of its time performing calculations.

Examples:

- image processing
- video encoding
- mathematical calculations
- data compression

Parallelism can often improve performance because different CPU cores can perform calculations simultaneously.

```text
Core 1 → Calculation A
Core 2 → Calculation B
Core 3 → Calculation C
Core 4 → Calculation D
```

## I/O-Bound

An I/O-bound task spends much of its time waiting.

Examples:

- network requests
- reading files
- database queries
- waiting for user input

Concurrency can help because another task can make progress while one task is waiting.

```text
Task A → Network Request → WAIT
                            ↓
Task B → performs work
```

---

## 21. Concurrency Example

Imagine a server receiving requests from multiple users.

Sequential execution might look like:

```text
User A → Process → Finish

User B → Process → Finish

User C → Process → Finish
```

User B must wait for User A.

With concurrency:

```text
User A ────┐
User B ────┼──> Server
User C ────┘
```

The server can manage multiple requests during overlapping periods.

If the machine has multiple CPU cores, some of this work may also execute in parallel.

---

## 22. Synchronization

**Synchronization** means coordinating concurrent tasks so that shared resources are accessed safely and operations happen in a valid order.

Synchronization tools include:

```text
Synchronization
│
├── Locks
├── Mutexes
├── Semaphores
└── Other synchronization primitives
```

Their purpose is not simply to make programs faster.

Their main purpose is to make concurrent execution **correct and controlled**.

---

## 23. Common Mistakes

### 1. Thinking concurrency and parallelism are identical

They are related but different.

```text
Concurrency → overlapping progress

Parallelism → simultaneous execution
```

---

### 2. Assuming `counter++` is always safe

An operation that looks like one line of code may internally involve several steps.

Multiple threads can interfere with these steps.

---

### 3. Locking too little

If shared data is not properly protected, race conditions can occur.

---

### 4. Locking too much

Excessive locking can reduce performance because threads spend more time waiting.

---

### 5. Acquiring locks in inconsistent order

This can contribute to deadlocks.

For example:

```text
Thread A: Lock 1 → Lock 2

Thread B: Lock 2 → Lock 1
```

Each thread may end up waiting for the other.

---

### 6. Assuming more threads always improve performance

Too many threads can increase scheduling, context-switching, and synchronization overhead.

---

## 24. Summary

**Concurrency** means managing multiple tasks whose execution overlaps in time, while **parallelism** means executing multiple tasks simultaneously.

When concurrent tasks share data, their operations can interfere with each other. This can produce a **race condition**, where the result depends on the timing and ordering of execution.

A **critical section** contains operations on shared resources that may need synchronization.

**Locks and mutexes** can protect critical sections by restricting access. A mutex generally provides mutual exclusion so that only one thread owns the protected resource at a time.

A **semaphore** uses permits to control how many tasks may access a resource simultaneously.

Synchronization must be used carefully. Too little synchronization can produce race conditions, while incorrect or excessive synchronization can create deadlocks or reduce performance.

Understanding concurrency, parallelism, race conditions, mutexes, locks, and semaphores is essential for building software that can safely and efficiently use modern multi-core systems.

---

> **Note:** AI assistance was used to help create and format some of the diagrams in these notes.