---
toc: true
layout: post
description: An introduction to process synchronization
categories: [Operating Systems]
title: Process Synchronization
---
### The Critical Section Problem

The critical section contain variables shared among different processes. The values of these may change
depending on the order in which in is accessed by various processes. This can result in corruption of data. Thus, only one process should
be allowed to enter the critical section at a time.

The solutions to this critical section problem need to satisfy the following three conditions:

1. Mutual Exclusion: Only one process enters the critical section at a time.
2. Progress: If no processes are executing in the critical section, and there are other processes waiting, the selection of the next process
cannot be postponed indefinitely.
3. Bounded Waiting: There must be a bound on the number of processes that are allowed to go in the critical section (maybe because of higher priority) before a process that has requested to enter the critical section. This is to ensure there is no starvation.

In this post, we talk about methods used in synchronization of threads utilizing different cores in the same CPU. Process synchronization in distributed system uses the same underlying principles, but is a little different. That's for another post.

### Hardware Solutions

These involves changing making changes to the hardware environment the threads are running in. After this eases the burden of the programmer to some extent.

#### TestAndSet

Although not truly a hardware solution, TestAndSet keeps track of a `lock` variable. Before entering the critical section, the process checks if the
section is locked. If not locked, it proceeds, else, it waits.

As you can imagine, this satisfies mutual exclusion and progress, but isn't bounded. The resources can be locked for a long long time. If the are
unlocked preemptively, there's no saying if the data will be corrupted.

#### Disable Interrupts

In uni-processor systems, A naive approach is to just disable interrupts. Without interrupts, two processes will never enter the critical section. Since the processes are executed in a non-preemptive environment now, it can lead to a host of problems like Convoy Effect, starvation, etc.

### Software Solutions

In these solutions the programmer handles which process enters the critical section and how the process leaves it.

#### Turn Method

Just have a turn variable that specifies which process should enter the critical section, looping over all processes.

```c
// code of process 0
while (true) {
    while (turn != 0);
    [CRITICAL SECTION]
    turn = 1;
    [EXIT SECTION]
}
```

```c
// code of process 1
while (true) {
    while (turn != 1);
    [CRITICAL SECTION]
    turn = 0;
    [EXIT SECTION]
}
```

This ensures mutual exclusion, but not progress, as there is no rule that takes in account waiting processes to be selected immediately.

How about a boolean to know which process wants to enter the critical section?

```c
// code of process 0
while (true) {
    wantsToEnter[0] = true;
    while (wantsToEnter[1] != 0);
    [CRITICAL SECTION]
    wantesToEnter[0] = false;
    [EXIT SECTION]
}
```

```c
// code of process 1
while (true) {
    wantsToEnter[1] = true;
    while (wantsToEnter[0]);
    [CRITICAL SECTION]
    wantesToEnter[1] = false;
    [EXIT SECTION]
}
```

#### Limitations of Turn Method

This is both mutually exclusive and ensures progress. But an edge-case would be, if the processes context switch at line `wantsToEnter[i] = true`
both the process will be marked interested in entering. This will cause a deadlock.

#### Peterson's Algorithm

Peterson's solution combines the above two approaches. There are two shared variables:

1. `bool interestedInEntering[N]` where N is the total number of processes.
2. `int turn` where turn is the process selected to enter.

Here's the pseudo-code:

```c
// code of process 0
while (true) {
    wantsToEnter[0] = true;
    turn = 1;
    while (turn == 1 && wantsToEnter[1]);
    [CRITICAL SECTION]
    turn = 0;
    wantsToEnter[0] = false;
    [EXIT SECTION]
}
```

```c
// code of process 1
while (true) {
    wantsToEnter[1] = true;
    turn = 0;
    while (turn == 0 && wantsToEnter[0]);
    [CRITICAL SECTION]
    wantsToEnter[1] = false;
    [EXIT SECTION]
}
```

#### Limitations of Peterson's Algorithm

This solution satisfies all three above mentioned properties, but it can't scale to more than two processes since branching will need to be added
for each process.

#### Semaphores

Semaphores was proposed by Dijkstra in 1965. It is a variable shared between threads governing which process will enter the critical condition.
Semaphores can be of two types. These are implemented primarily using two atomic functions. A sleep/wait function, and a wake-up/signal function. The wait functions makes new processes wait, and push them to a process queue. Signal function process dequeue waiting processes, signaling them.

An operation in called atomic if it executes as a single step relative to other threads. This is easier said than done (for example, a single 64-bit assignment when compiled using 32-bit executes as two instructions.)

1. Binary Semaphore (Mutex lock) - It's a lock that we put on the shared resource before using it, and release the lock after using it. No other thread can access
the resource when the lock is set.

2. Counting Semaphores - These locks can take a range of values and as you an imagine, are used to put lock on resources with multiple instances.

```c
struct semaphore {
    int value; // n is the number of instances of the resource
    // q contains all Process Control Blocks (PCBs)
    // corresponding to processes got blocked
    // while performing down operation.
    Queue<process> q;
 
}

void Wait(semaphore s) {
    s.value = s.value - 1;
    if (s.value < 0) {
        // here p is a process which is currently executing
        q.push(p);
        block();
    }
    else
        return;
}

void Signal(semaphore s) {
    s.value = s.value + 1;
    if (s.value <= 0) {
        // remove process p from queue
        Process p=q.pop();
        wakeup(p);
    }
    else
        return;
}

struct semaphore s;
while (true) {
    Wait(s);
    [CRITICAL SECTION]
    Signal(s)
    [EXIT SECTION]
}
```

#### Limitations of Semaphore

1. Priority inversion: This should be handled with passing the processes in a priority queue, but that leads to unbounded waiting and in turn doesn't satisfy the critical section
problem.
2. Deadlock: Deadlock can occur when a process, after finishing it's critical section execution, is trying to wake up a another process which isn't asleep.
3. Busy waiting: The condition `while (s.value == 0);` is pooled repeatedly. The process is not doing anything useful in this time.

Note: This information can be found of other CS portals. This is a half-assed post I intend to come back to later with some insights.
