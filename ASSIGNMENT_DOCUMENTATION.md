# Assignment 3 - Complete Documentation

**Student Name**: عبدالله الحسين 
**Student ID**: 445052833 
**Date Submitted**: 5\2
---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: `[YourStudentID]_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - 5\1 9pm
**What I implemented**: 
use reentrantlock to protect critical section
**Challenges encountered**: 
shared counters
**How I solved it**: 
defined a static final reentranlock
**Testing approach**: 
verified the final statistics in the console
**Time spent**: 
45 mins
---

### Entry 2 - 5\2 6am
**What I implemented**:protect the executionlog 

**Challenges encountered**: 
the arraylist is not thread safe
**How I solved it**: 
wrapped the operation within the lock 
**Testing approach**: 
check the size of the executionlog in the output and compare it with the total
**Time spent**: 
20mins
---

### Entry 3 - 5/2 8am
**What I implemented**: 
added samaphore to control cpu accsess and implement try catch finally 
**Challenges encountered**: 
posability of deadlock
**How I solved it**: 
used acquire in the start and release in finally
**Testing approach**: 
monitored the console output to confirm that onlu one process executes at a time
**Time spent**: 
90 mins
---

### Entry 4 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

---

### Entry 5 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:

[Your answer here - 4-6 sentences with code examples]
Race Condition in Shared Counters (contextSwitchCount, completedProcessCount, totalWaitingTime):
​The Problem: These variables were being updated by multiple threads at the same time.
​Why it is a problem: A simple line like count++ looks easy, but for the CPU, it's actually three separate steps: (1) read the value, (2) add one to it, (3) save it back. If two threads try to do this at the exact same moment they both read the same old value, increment it independently, and save it. So, one update gets "lost" in the process.
​The Result: The final statistics like the total waiting time were just wrong because some updates didn't make it to the final count.
Race Condition in executionLog (ArrayList):
​The Problem: I was using a standard ArrayList to keep track of the logs but I found that ArrayList isn't "thread-safe."
​Why it is a problem: When multiple threads try to add (.add()) a log entry at the same time, they get in each other's way while trying to resize or update the internal list structure.
​The Result: The program would sometimes crash with a ConcurrentModificationException  or some logs would simply disappear.
I used a ReentrantLock. For anything shared—like those counters or the list—I wrapped the update code inside lock.lock() and finally { lock.unlock(); }. This basically forces the threads to wait for their turn.
---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?
**Your Answer**:
ReentrantLock: Only one thread can have the key at a time. If another thread wants to enter, it has to wait until the first one leaves and returns the key.
​Semaphore: it like a bouncer at a club with a limited number of spots. It doesn't necessarily mean just one person can enter; you can set it to allow a certain number of threads (permits) to enter at the same time. If the limit is reached, others have to wait.
I used ReentrantLock in SharedResources for the counters (contextSwitchCount, etc.) and the executionLog.
​ ,Because I needed to make sure that these specific resources are modified by only one thread at a time to prevent any "Race Conditions" or data corruption.
​I used Semaphore in the Process class to control the CPU access.
​Even though I set the permit to 1 (like a lock), the main reason for using a Semaphore here is to control the flow of execution and limit how many processes are actively running on the "CPU" at once. It’s better for managing shared resources where you might eventually want to allow more than one process to run simultaneously 
[Your answer here - explain your implementation choices]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:
two or more process are stuck forever because each one is waiting for a resource that the other one is holding. None of them can move, and the program just freezes.
Resource Ordering: it assign a specific order to the resources (like always locking A before B). This prevents the "circular wait" condition where threads keep waiting for each other in a loop.
​Using finally blocks: By ensur that every lock or semaphore acquired is always released in a finally block, we guarantee that no thread holds onto a resource indefinitely, even if an error occurs
In the project, I primarily focused on the finally block technique. I realized that if a thread gets interrupted (e.g., InterruptedException) while holding the cpuSemaphore, it could crash the whole simulation and leave the semaphore locked forever. So, I wrapped my acquire() call in a try block and placed the release() call inside a finally block. This way, no matter what happens (if it finishes normally or if it crashes), the semaphore is guaranteed to be released, which keeps the simulation running smoothly without any deadlocks.
[Your answer here - reference try-finally blocks, lock ordering, etc.]

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

[Your answer here - explain coarse-grained vs fine-grained locking, independence of counters, concurrency implications. Show understanding of when to use each approach. 5-8 sentences expected.]

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 

**Why they need protection**: 

**Synchronization mechanism used**: 

**Code snippet**:
```java
// Paste your implementation here
```

**Justification**: 

---

### Critical Section #2: Execution Log

**What resource**: 

**Why it needs protection**: 

**Synchronization mechanism used**: 

**Code snippet**:
```java
// Paste your implementation here
```

**Justification**: 

---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 

**Number of permits and why**: 

**Where implemented**: 

**Code snippet**:
```java
// Paste your implementation here
```

**Effect on program behavior**: 

---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
```

**Results**: 
(Show that running multiple times produces consistent, correct results)

**Why synchronization is necessary**: 
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.)

**Conclusion**: 

---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: 

**Results**: 

**What this proves**: 

---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: 

**Actual values**: 

**Analysis**: 

---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]

**Purpose**: 

**Results**: 

**What I learned**: 

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
