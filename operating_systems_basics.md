## Operating System Basics (OS Basics)
The fundamental software that acts as a bridge between a computer's hardware and the user or applications. It's one of the first programs that runs when a computer starts up, and it's responsible for managing the processor, memory, storage devices, and input-output devices. Thanks to the operating system, multiple programs can run safely and efficiently at the same time.

## What is the Kernel?
The kernel is the most fundamental layer of the operating system that communicates directly with the hardware.
It manages hardware resources (CPU, memory, disk, devices) and provides applications with a safe, abstracted interface. User programs cannot access hardware directly; every request passes through the kernel via a system call.
The kernel's main responsibilities are:
* Managing CPU usage
* Managing memory
* Controlling the file system
* Managing Input/Output (I/O) operations
* Communicating with hardware devices
* Ensuring programs run safely
In short, the kernel can be thought of as the central manager that controls all of a computer's resources.

## Process vs. Thread
Process: A program that is currently running.
When a program is opened, the operating system creates a separate execution space for it. This space contains the program's memory, variables, and the system resources it uses.
* Running Google Chrome
* Opening Microsoft Word
Each process is independent of the others. If one process crashes, other processes are generally not affected.

## Thread
A process can contain multiple threads. Threads within the same process share memory and other resources.
For example, in a web browser:
* One thread might load the page.
* Another thread might play a video.
* Another thread might track user input.
This allows programs to perform many tasks at once, faster.

## Differences Between Process and Thread
Process
Has its own memory.
More expensive to create.
Runs independently of others.
Uses more system resources.
A crash generally doesn't affect other processes.

Thread
Shares memory with the same process.
Faster to create.
Runs together within the same process.
Uses fewer resources.
A serious error in one thread can affect other threads within the same process.

## How is Memory Management Done?
Memory management is how the operating system distributes RAM among programs in an organized and efficient manner.
When an application is run, the operating system allocates as much memory as it needs. When the program is closed, the memory used is freed up again, becoming available for other applications.
The operating system's main responsibilities in memory management are:
* Allocating memory to programs
* Reclaiming unused memory
* Preventing programs from accessing each other's memory
* Using Virtual Memory to treat storage space as temporary memory when RAM is insufficient
* Ensuring memory is used efficiently
Thanks to these mechanisms, many programs can run smoothly at the same time.

## What are CPU Schedulers?
A CPU scheduler is the kernel component that decides which process/thread will use the CPU, when, and for how long. The goal is to distribute the limited CPU resource among processes fairly and efficiently. Modern operating systems (Linux, Windows) generally use priority-based, preemptive schedulers.

Common CPU scheduling algorithms:
* FCFS (First Come First Served)
The process that arrives first is run first. Easy to implement, but long processes can make others wait.
* Round Robin (RR)
Each process is given a specific time slice. When a process's time is up, it moves to the back of the queue. Commonly preferred in multi-user operating systems.
* Priority Scheduling
Each process is assigned a priority value. Processes with higher priority run first.
* SJF (Shortest Job First)
The process with the shortest execution time runs first. Reduces average waiting time, but long processes may be delayed.