# Assignment 3 - Complete Documentation

**Student Name**: Nada Rashed Alhuthayli 

**Student ID**: 445052014

**Date Submitted**: 29/4/2026

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]
https://drive.google.com/file/d/1IlCCugwa81HVcL6SO0Ng67_f9hdtRZWe/view?usp=sharing


**Video filename**: `445052014_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [28/4/2026, 11:00 PM]
**What I implemented**: 

Started the project by forking the repository and setting up the environment in VS Code. I also updated the studentID variable in SchedulerSimulationSync.java and made the first commit. 

**Challenges encountered**: 

Ensuring the GitHub repository was set to public to comply with the assignment requirements. 

**How I solved it**: 

Followed the "Danger Zone" settings in GitHub to change visibility to public.

**Testing approach**: 

Verified the repository link in an incognito window to ensure it is accessible.

**Time spent**: 

30 minutes.

---

### Entry 2 - [29/4/2026, 12:00 AM]
**What I implemented**: 

Implemented Task 1 and Task 2 by adding ReentrantLock to protect shared variables like counter and the execution log ArrayList.

**Challenges encountered**: 

Preventing potential deadlocks and handling ConcurrentModificationException in the list. 

**How I solved it**: 

Used try-finally blocks to ensure that the lock is always released in the finally section, even if an exception occurs.

**Testing approach**: 

Ran the simulation multiple times to verify that the final counts and logs were consistent across different runs.

**Time spent**: 

1.5 hours

---

### Entry 3 - [29/4/2026, 2;00 AM]
**What I implemented**: 

Completed Task 3 by adding a Semaphore with one permit to control CPU access and recorded the video demonstration.

**Challenges encountered**: 

Explaining the technical difference between a mutex lock and a binary semaphore during the video walkthrough.

**How I solved it**: 

Reviewed the README.md theory section to clearly articulate how the semaphore manages concurrent access.

**Testing approach**: 

Performed a final check of the commit history to ensure there are at least 4 meaningful commits.

**Time spent**: 

2 hours

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
In the original code, two primary race conditions exist:

• Statistic Counters: The shared resources affected are variables like contextSwitchCount and completedProcessCount. Concurrent access is a problem because increment operations are not atomic, meaning two threads could read the same value and overwrite each other's updates. This results in incorrect final totals for the simulation statistics.  

• Execution Log: The shared resource is the ArrayList used to store the execution history. When multiple threads try to add data to the list simultaneously, it can trigger a ConcurrentModificationException or lead to corrupted data entries. 


[Your answer here - 4-6 sentences with code examples]

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

• The Difference: A ReentrantLock is a mutual exclusion mechanism that allows only one thread to access a resource at a time, whereas a Semaphore manages a set of permits to control how many threads can access a resource concurrently.  

• Implementation: I used ReentrantLock for Tasks 1 and 2 to ensure exclusive access to the counters and the log list, preventing data corruption. I used a Semaphore with one permit (binary semaphore) for Task 3 to specifically control and synchronize concurrent CPU access between threads.

[Your answer here - explain your implementation choices]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

• Definition: A deadlock is a situation where two or more threads are blocked forever, each waiting for a resource held by the other.  

• Prevention Techniques: Two key techniques are Lock Ordering (always acquiring locks in a consistent predefined order) and using try-finally blocks.  

• My Implementation: I prevented deadlocks by placing the unlock() method inside a finally block. This ensures that even if an error occurs during execution, the lock is guaranteed to be released, allowing other threads to proceed. 

[Your answer here - reference try-finally blocks, lock ordering, etc.]

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

For Task 1, I chose a Coarse-grained locking approach by using a single lock to protect all three counters.

• Rationale: This choice was made to simplify the implementation and minimize the risk of complex deadlocks that can occur with multiple locks.  

• Trade-offs: While coarse-grained locking is easier to manage, it can limit performance because threads must wait for the lock even if they are updating different, independent counters. Fine-grained locking (one lock per counter) would allow more threads to work simultaneously.  

• Concurrency: Since the three counters are independent, a Fine-grained approach would provide better concurrency because it allows multiple threads to update different statistics at the same time without blocking each other.

[Your answer here - explain coarse-grained vs fine-grained locking, independence of counters, concurrency implications. Show understanding of when to use each approach. 5-8 sentences expected.]

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 

contextSwitchCount, completedProcessCount, and totalWaitingTime.

**Why they need protection**: 

These variables are shared resources accessed by multiple threads. Without protection, a Race Condition could occur where multiple threads try to update the counters simultaneously, leading to inconsistent or incorrect data.

**Synchronization mechanism used**: 

ReentrantLock (Fine-grained locking).

**Code snippet**:

public static final ReentrantLock contextSwitchLock = new ReentrantLock();
// ...
public static void incrementContextSwitch() {
    contextSwitchLock.lock();
    try {
        contextSwitchCount++;
    } finally {
        contextSwitchLock.unlock();
    }
}


**Justification**: 

The ReentrantLock ensures Mutual Exclusion, meaning only one thread can modify a specific counter at a time. Using separate locks for different counters (fine-grained) allows threads to update unrelated variables concurrently, improving performance.

---

### Critical Section #2: Execution Log

**What resource**: 

executionLog (an ArrayList of Strings).

**Why it needs protection**: 

The ArrayList class in Java is not thread-safe. If multiple process threads attempt to add logs at the same time, it can lead to data corruption or the application crashing.

**Synchronization mechanism used**: 

ReentrantLock.

**Code snippet**:

public static ReentrantLock logLock = new ReentrantLock();
// ...
public static void logExecution(String message) {
    logLock.lock();
    try {
        executionLog.add(message);
    } finally {
        logLock.unlock();
    }
}


**Justification**: 

Applying a lock before adding to the list ensures that log entries are synchronized, maintaining the integrity of the execution history.

---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 

To manage access to the CPU and ensure that the execution of processes is controlled.

**Number of permits and why**: 

1 permit. It acts as a Binary Semaphore to ensure that only one process can be "executing" on the CPU at any given time.

**Where implemented**: 

It is implemented in the run() and runToCompletion() methods of the Process class.

**Code snippet**:

public static final Semaphore cpuSemaphore = new Semaphore(1);
// ...
public void run() {
    try {
        SharedResources.cpuSemaphore.acquire();
        try {
            // Execution logic here...
        } finally {
            SharedResources.cpuSemaphore.release();
        }
    } catch (InterruptedException e) { ... }
}


**Effect on program behavior**: 

It prevents overlapping output in the console (like the progress bars) and ensures that the simulation correctly mimics a single-core CPU scheduler where only one task runs at a time.

---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running the program multiple times to verify that shared counters and logs remain consistent across different executions.


**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
```

**Results**: 

The final statistics (Total Completed Processes, Total Waiting Time) were consistent in every run, matching the number of processes created.


**Why synchronization is necessary**: 
synchronization, a Race Condition could occur where multiple threads update contextSwitchCount or totalWaitingTime at the same time, leading to "lost updates" and incorrect final totals.

**Conclusion**: 

The use of ReentrantLock ensures that only one thread can modify shared variables at a time, providing data integrity.

---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException when accessing the executionLog.

**Testing procedure**: 

Running the simulation with a large number of processes to ensure frequent simultaneous updates to the ArrayList.

**Results**: 

The program executed without any exceptions or crashes during log updates.

**What this proves**: 

This proves that logLock successfully protects the executionLog (which is an ArrayList and not thread-safe), preventing multiple threads from corrupting the list's internal structure during concurrent additions.

---

### Test 3: Correctness Verification
**What I tested**:Verifying that final summary values (completed processes, context switches) align with the logic of the scheduler.

**Expected values**: 

Completed Processes should exactly equal the number of processes (10-20 processes based on the Student ID).

**Actual values**: 

The "Total Completed Processes" in the output matched the initial number of processes in the simulation header.

**Analysis**: 

The synchronization logic is correct; no process was lost, and the cpuSemaphore properly managed the turn-taking of threads

---

### Test 4: Different Scenarios
**Scenario tested**: Testing the scheduler with different timeQuantum values and a high number of processes.

**Purpose**: 

To observe how synchronization handles increased contention when many threads are waiting for the cpuSemaphore.

**Results**: 

Even with more processes, the cpuSemaphore maintained the "one-at-a-time" execution rule, and the logs remained orderly.

**What I learned**: 

Proper synchronization prevents "Interleaving" (tangled output) and ensures that even under heavy load, the shared resources remain accurate and reliable.

---

## Part 5: Reflection and Learning

### What I learned about synchronization:

Through this assignment, I learned that synchronization is essential when multiple threads share access to the same resources, such as the executionLog and global counters. I discovered that without mechanisms like ReentrantLock, a race condition can occur, causing threads to overwrite each other's data. Implementing fine-grained locking taught me how to protect specific variables without slowing down the entire system unnecessarily. I also gained experience using a Semaphore as a gatekeeper to ensure that only one process executes on the CPU at a time. Dealing with thread-safe issues in ArrayList highlighted the importance of protecting standard collections in a concurrent environment. One of the key challenges was ensuring that locks were always released in a finally block to prevent deadlocks. Overall, I now understand that synchronization provides the predictability and data integrity required for a functional operating system.

[6-8 sentences about key concepts, challenges, insights]

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: 

When two different ATMs try to withdraw money from the same bank account at the exact same time, synchronization is critical to ensure the balance is updated correctly and no one withdraws more than the available funds.

**Example 2**: 

When thousands of users try to book the last available seat on a flight simultaneously, synchronization ensures that the seat is only assigned to one person and that the "available seats" counter doesn't become negative.

---

### How I would explain synchronization to others:

Imagine a single-person bathroom in a busy coffee shop. The lock on the door is the synchronization mechanism. Even if ten people are waiting in line (the Ready Queue), only the person with the key (the Lock/Semaphore) can enter and use the facility (the CPU). While they are inside, no one else can enter, which prevents a "messy" situation where multiple people try to use the same space at once. Once they finish, they unlock the door and pass the key to the next person in line. In programming, synchronization is just that—a way to make threads wait their turn so they don't crash into each other while using shared data.

[Explain to someone who just finished Assignment 1 - use simple terms and analogies]

---

## Part 6: GitHub Repository Information

**Repository URL**: https://github.com/Nada-Rashed-445/OS-Assignment3-Nada-Rashed/blob/main/ASSIGNMENT_DOCUMENTATION.md

**Number of commits**: 9

**Commit messages**: 

1.set my student ID: 445052014 

2. defind ReentrantLock

3. Add a ReentrantLock(s)

4. Method to increment context switch counter

5. Method to increment completed process counter

6. Method to add waiting time

7. Method to add waiting time

8. try and Finally

9. Similar synchronization for method runToCompletion 

---

## Summary

**Total time spent on assignment**: 
5 hours 

**Key takeaways**: 
1. Understanding the critical importance of thread synchronization in preventing race conditions and data corruption.

2. Mastering the practical application of ReentrantLock and Semaphore to manage shared resources in Java.
   
3. Learning how to use try-finally blocks as a standard practice to ensure locks are always released and avoid system deadlocks.  

**Most challenging aspect**: 

Identifying all the hidden race conditions within the ArrayList and counters, and ensuring the synchronization didn't lead to performance bottlenecks or deadlocks. 

**What I'm most proud of**: 

Successfully creating a thread-safe CPU scheduler simulation that produces consistent, accurate results across multiple runs and documenting the process clearly in the development log. 

---

**End of Documentation**
