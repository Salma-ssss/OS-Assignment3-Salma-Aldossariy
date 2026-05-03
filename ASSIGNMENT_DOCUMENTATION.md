# Assignment 3 - Complete Documentation

**Student Name**: [Salma Aldossariy]  
**Student ID**: [445052168]  
**Date Submitted**: [2026/5/3]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: `[445052168]_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [May 3, 2026, 6:00 AM]
**What I implemented**: 
I started by understanding the assignment requirements and analyzing the provided code. I identified shared resources such as contextSwitchCount, completedProcessCount, totalWaiting Time, and executionLog
**Challenges encountered**: 
it was initially difficult to identify where race conditions might occur
**How I solved it**: 
I reviewed Chapters 3,5 from Operating System Concepts and focused on critical sections
**Testing approach**: 
I ran the program without synchronization and observed behavior
**Time spent**: 1 hour

---

### Entry 2 - [May 3, 2026, 8:00 AM]
**What I implemented**: 
implemented ReentrantLock to protect shared counters
**Challenges encountered**: 
Understanding where to place lock() and unlock() correctly
**How I solved it**: 
used try-finally to guarantee unlocking
**Testing approach**: 
tested multiple runs and verified counters were consistent
**Time spent**: 2 hours

---

### Entry 3 - [May 3, 2026, 10:00 AM]
**What I implemented**: 
I added synchronization for executionLog
**Challenges encountered**: 
Understanding why ArrayList is not thread-safe
**How I solved it**: 
Protected it using the same lock
**Testing approach**: 
Ensured no ConcurrentModificationException occurs
**Time spent**: 1 hour

---

### Entry 4 - [May 3, 2026, 11:00 AM]
**What I implemented**: 
added a semaphore to control CPU access
**Challenges encountered**: 
Understanding how a semaphore differs from a lock
**How I solved it**: 
used a binary semaphore (1 permit)
**Testing approach**: 
Verified: Only one process executes at a time
**Time spent**: 2 hours

---

### Entry 5 - [May 3, 2026, 1:00 PM]
**What I implemented**: 
Final testing and validation
**Challenges encountered**: 
Ensuring consistent output across runs
**How I solved it**: 
Repeated execution multiple times
**Testing approach**: 
Compared outputs and verified correctness
**Time spent**: 3 hours

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:
The first race condition occurs in the shared variable contextSwitchCount, where multiple threads increment the value concurrently using contextSwitchCount++. Since this operation is not atomic, threads may overwrite each other's updates, leading to incorrect counts.

The second race condition occurs in executionLog, which is implemented using ArrayList. ArrayList is not thread-safe, so concurrent modifications can lead to inconsistent data or runtime exceptions.

These race conditions can cause incorrect statistics and corrupted logs, affecting the correctness of the
simulation

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:
ReentrantLock is used to provide mutual exclusion, ensuring that only one thread can access a critical section at a time. In this assignment, it was used to protect shared variables such as counters and execution logs

A semaphore, on the other hand, is used to control access to a limited number of resources. In this assignment, a binary semaphore (1 permit) was used to simulate CPU access, ensuring that only one process executes at a time.

Thus, locks were used for data protection, while semaphores were used for resource management.


---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:
A deadlock is a situation where two or more threads are waiting indefinitely for resources held by each other.

One prevention technique is using try-finally blocks to ensure that locks are always released, even if an exception occurs. Another technique is avoiding nested locks and maintaining a consistent locking order.

In this assignment, try-finally was used to guarantee that locks and semaphores are released properly.
Preventing deadlocks

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:
I used a single lock (coarse-grained locking) to protect all shared counters. This approach simplifies implementation and reduces the risk of deadlocks.

The trade-off is reduced concurrency compared to fine-grained locking, where separate locks could allow multiple threads to update different counters simultaneously.

However, given the simplicity of the assignment and the small number of shared variables, coarse-grained locking is more suitable and easier to maintain

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 
contextSwitchCount, completedProcessCount, totalWaiting Time
**Why they need protection**: 
They are shared among multiple threads and updated concurrently
**Synchronization mechanism used**: 
ReentrantLock
**Code snippet**:
```java
// Paste your implementation here
lock.lock();
try { 
    contextSwitchCount++; 
} finally { 
   lock.unlock();
}
```
**Justification**: 
Ensures mutual exclusion and prevents race conditions
---

### Critical Section #2: Execution Log

**What resource**: 
executionLog (ArrayList)
**Why it needs protection**: 
ArrayList is not thread-safe
**Synchronization mechanism used**: 
v
**Code snippet**:
```java
// Paste your implementation here
lock.lock();
try{
executionLog.add(message);
} finally {
lock.unlock();
}
```

**Justification**: 
Prevents data corruption and exceptions
---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 
Control CPU access
**Number of permits and why**: 
(simulate single CPU) 1
**Where implemented**: 
Inside run() method
**Code snippet**:
```java
// Paste your implementation here
SharedResources.cpuSemaphore.acquire();
...
SharedResources.cpuSemaphore.release();
```

**Effect on program behavior**: 
Ensures only one process executes at a time
---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
# Compile the progress
Java Schedulersimulationsync.java

# Run the program multiple times (at least 5 times)
Java SchedulersimulationSync
Java Schedulersimulationsync
Java SchedulerSimulationSync
Java SchedulersimulationSync
Java SchedulersimulationSync
```

**Results**: 
(Show that running multiple times produces consistent, correct results)
Total Context Switches: 35
Total Completed Processes: 18
Total Waiting Time: 1264760ms
Average Waiting Time: 70264ms

═══ Process Summary Table ═══
Process    Priority     Burst Time   Waiting Time
────────────────────────────────────────────────
P1         1            3234         20          
P2         1            3119         3253        
P3         3            8092         99157       
P4         1            9966         99258       
P5         3            3653         14516       
P6         4            7817         72751       
P7         2            9473         101235      
P8         5            5405         80668       
P9         4            3940         30414       
P10        3            9883         102714      
P11        5            6747         86136       
P12        5            7202         88919       
P13        3            2030         46506       
P14        4            5315         92132       
P15        2            3812         52622       
P16        4            6941         93455       
P17        3            6670         96403       
P18        2            9236         104601      

═══ Execution Log Summary ═══
Total log entries: 70

**Why synchronization is necessary**: 
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.)
Without synchronization, race conditions could occur in shared variables such as contextSwitchCount, completedProcessCount, and totalWaitingTime. For example, multiple threads incrementing contextSwitchCount simultaneously could result in lost updates, producing incorrect values. Similarly, concurrent access to executionLog (ArrayList) could cause data corruption or runtime exceptions. Synchronization ensures mutual exclusion, meaning only one thread can modify shared resources at a time, thus preserving data integrity

**Conclusion**: 
The consistent results across multiple executions confirm that synchronization mechanisms (ReentrantLock Land Semaphore) were correctly implemented and effectively eliminated race conditions
---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: 
executed the program multiple times while focusing on operations involving the shared executionLog list.

Since multiple threads attempt to write to this list, it is a potential source of concurrency issues. I verified whether any runtime exceptions occurred during execution
**Results**: 
No ConcurrentModificationException or any other runtime exception was observed during any execution
of the program
**What this proves**: 
This confirms that the executionLog is properly synchronized using ReentrantLock. The lock ensures that only one thread modifies the list at a time, preventing concurrent modification issues and maintaining data 
.consistency
---

### Test 3: Correctness Verification
**What I tested**: 
Verifying that the final computed values ​​(such as total context switches, completed processes, and waiting time) are correct and logically consistent.
**Expected values**: 
-The number of completed processes should equal the total number of created processes
-Context switches should reflect the number of scheduling operations
-Total waiting time should be positive and reasonable
-Average waiting time should be correctly calculated
**Actual values**: 
Total Context Switches: 35
Total Completed Processes: 18
Total Waiting Time: 1264760ms
Average Waiting Time: 70264ms
**Analysis**: 
The actual values ​​match the expected behavior of a Round Robin scheduler. All processes were completed successfully, and no incorrect values ​​were observed. The statistics are consistent with the execution flow, confirming that synchronization did not interfere with correctness but instead ensured accurate computation
---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]
Running the program with different randomly generated values ​​for time quantum and number of processes
(based on different student IDs)

**Purpose**: 
To verify that the synchronization mechanisms work correctly under different workloads and execution conditions
**Results**: 
program behaved correctly in all scenarios. Regardless of the number of processes or time quantum, the output remained consistent and no race conditions or errors occurred
**What I learned**: 
Synchronization mechanisms such as locks and semaphores ensure stability and correctness regardless of system load. This demonstrates their importance in real-world concurrent systems
---

## Part 5: Reflection and Learning

### What I learned about synchronization:

[6-8 sentences about key concepts, challenges, insights]

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: 

**Example 2**: 

---

### How I would explain synchronization to others:

[Explain to someone who just finished Assignment 1 - use simple terms and analogies]

---

## Part 6: GitHub Repository Information

**Repository URL**: 

**Number of commits**: 

**Commit messages**: 
1. 
2. 
3. 
4. 

---

## Summary

**Total time spent on assignment**: 

**Key takeaways**: 
1. 
2. 
3. 

**Most challenging aspect**: 

**What I'm most proud of**: 

---

**End of Documentation**
