# Assignment 3 - Complete Documentation

**Student Name**: Rakan Almohsen
**Student ID**: 443050277
**Date Submitted**: May 2, 2026

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
>
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [PASTE GOOGLE DRIVE LINK HERE]

**Video filename**: `443050277_Assignment3_Synchronization.mp4`

**Verification**:

* [ ] Link is accessible (tested in incognito mode)
* [ ] Video is 3-5 minutes long
* [ ] Video shows code walkthrough and commits
* [ ] Video has clear audio
* [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

### Entry 1 - May 1, 6:00 PM

**What I implemented**:
Identified shared resources and analyzed race conditions in counters and execution log.

**Challenges encountered**:
Understanding where race conditions occur.

**How I solved it**:
Reviewed shared variables and traced thread execution.

**Testing approach**:
Ran the program multiple times and observed inconsistent results.

**Time spent**: 1 hour

---

### Entry 2 - May 1, 8:00 PM

**What I implemented**:
Added ReentrantLock for shared counters.

**Challenges encountered**:
Choosing between one lock or multiple locks.

**How I solved it**:
Used fine-grained locking (separate locks for each counter).

**Testing approach**:
Verified counters remain consistent across runs.

**Time spent**: 1.5 hours

---

### Entry 3 - May 2, 10:00 AM

**What I implemented**:
Protected execution log using ReentrantLock.

**Challenges encountered**:
Avoiding ConcurrentModificationException.

**How I solved it**:
Wrapped ArrayList access using logLock.

**Testing approach**:
Ran program multiple times to ensure stability.

**Time spent**: 1 hour

---

### Entry 4 - May 2, 1:00 PM

**What I implemented**:
Implemented Semaphore for CPU control.

**Challenges encountered**:
Correct placement of acquire() and release().

**How I solved it**:
Used try-finally in run() and runToCompletion().

**Testing approach**:
Observed that only one process runs at a time.

**Time spent**: 1 hour

---

### Entry 5 - May 2, 4:00 PM

**What I implemented**:
Final testing and debugging.

**Challenges encountered**:
Ensuring no deadlocks or race conditions.

**How I solved it**:
Used proper locking and finally blocks.

**Testing approach**:
Executed program multiple times.

**Time spent**: 1 hour

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions

A race condition occurs when multiple threads access shared data at the same time.
In this program, the first race condition was in `contextSwitchCount++`, where multiple threads could overwrite each other’s updates.

The second race condition was in `executionLog.add()`, where multiple threads modify the same ArrayList.

Concurrent access is problematic because operations like increment are not atomic.

This can lead to incorrect values or corrupted data.

---

### Question 2: Locks vs Semaphores

ReentrantLock ensures mutual exclusion, meaning only one thread can access a critical section.

Semaphore controls how many threads can access a resource simultaneously.

I used ReentrantLock to protect shared counters and execution log.

I used Semaphore to simulate CPU access, allowing only one process at a time.

---

### Question 3: Deadlock Prevention

Deadlock occurs when threads wait forever for each other’s resources.

Two prevention techniques:

1. Using try-finally to ensure locks are always released
2. Avoiding holding multiple locks simultaneously

In my code, I used finally blocks to guarantee release of locks and semaphores.

---

### Question 4: Lock Granularity Design Decision

I used **fine-grained locking** with separate locks for each counter.

This allows multiple threads to update different counters at the same time.

Coarse-grained locking uses a single lock, which reduces concurrency.

Fine-grained locking improves performance and efficiency.

Since the counters are independent, this approach provides better concurrency.

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**:
contextSwitchCount, completedProcessCount, totalWaitingTime

**Why they need protection**:
They are accessed and modified by multiple threads

**Synchronization mechanism used**:
ReentrantLock

**Code snippet**:

```java
contextLock.lock();
try {
    contextSwitchCount++;
} finally {
    contextLock.unlock();
}
```

**Justification**:
Prevents race conditions and ensures accurate values.

---

### Critical Section #2: Execution Log

**What resource**:
executionLog (ArrayList)

**Why it needs protection**:
Concurrent access can cause exceptions or corruption

**Synchronization mechanism used**:
ReentrantLock

**Code snippet**:

```java
logLock.lock();
try {
    executionLog.add(message);
} finally {
    logLock.unlock();
}
```

**Justification**:
Ensures safe access to shared list.

---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**:
Control access to CPU

**Number of permits and why**:
1 permit to simulate single CPU

**Where implemented**:
run() and runToCompletion()

**Code snippet**:

```java
cpuSemaphore.acquire();
try {
    // execution
} finally {
    cpuSemaphore.release();
}
```

**Effect on program behavior**:
Ensures only one process executes at a time.

---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check

**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**:

```bash
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
```

**Results**:
All runs produced consistent values

**Why synchronization is necessary**:
Without synchronization, race conditions could cause incorrect values

**Conclusion**:
Synchronization works correctly

---

### Test 2: Exception Testing

**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**:
Ran program multiple times

**Results**:
No exception occurred

**What this proves**:
Execution log is thread-safe

---

### Test 3: Correctness Verification

**What I tested**: Verifying correct final values

**Expected values**:
Correct counts for all processes

**Actual values**:
Matched expected

**Analysis**:
Program behaves correctly

---

### Test 4: Different Scenarios

**Scenario tested**: Different time quantum

**Purpose**: Test system behavior

**Results**: Program remained stable

**What I learned**: Synchronization works under different conditions

---

## Part 5: Reflection and Learning

### What I learned about synchronization:

Synchronization is important to protect shared resources.
Race conditions can cause incorrect results.
Locks ensure only one thread accesses data at a time.
Semaphores control resource usage.
Using try-finally prevents deadlocks.
Fine-grained locking improves performance.

---

### Real-world applications:

**Example 1**: Banking systems updating account balances

**Example 2**: Multi-user database systems

---

### How I would explain synchronization to others:

Synchronization is like organizing access to shared resources.
Locks act like keys allowing one thread at a time.
Semaphores limit how many threads can access a resource.

---

## Part 6: GitHub Repository Information

**Repository URL**: [https://github.com/rakanacc/OS-Assignment3-Rakan-Almohsen.git]

**Number of commits**: 1

**Commit messages**:

1. student ID change, added locks, added semaphore, completed documentation

---

## Summary

**Total time spent on assignment**: ~5 hours

**Key takeaways**:

1. Importance of synchronization
2. Preventing race conditions
3. Using locks and semaphores

**Most challenging aspect**:
Understanding race conditions

**What I'm most proud of**:
Successfully implementing synchronization

---