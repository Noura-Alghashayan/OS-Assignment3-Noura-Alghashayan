# Assignment 3 - Complete Documentation

**Student Name**: Noura Thamer Alghashayan   
**Student ID**: 445051981 
**Date Submitted**: [Submission Date]

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

### Entry 1 -  May 1, 2026 - 2:02 PM
**What I implemented**: 
I began by going over the assignment specifications and the starting code. I performed the initial commit and updated the student ID in SchedulerSimulationSync.java to my real student ID.

**Challenges encountered**: 
The first difficulty was figuring out which factors would be impacted by race conditions and where the shared resources were.

**How I solved it**: 
I located the common variables, such as contextSwitchCount, completedProcessCount, totalWaitingTime , and executionLog , within the SharedResources class.

**Testing approach**: 
After modifying the student ID, I ran the application to confirm that the code was still appropriately built and executed.

**Time spent**: 
25 minutes
---

### Entry 2 -  May 1, 2026 - 2:25 PM
**What I implemented**: 
I included the imports that are needed for both Semaphore and ReentrantLock. In order to safeguard shared counter variables, I then built a counterLock.

**Challenges encountered**: 
I needed to know why a race scenario may still occur when basic variables like contextSwitchCount++ are incremented.

**How I solved it**: 
I used counterLock.lock() and counterLock.unlock() inside of try-finally blocks to safeguard the counter update activities.

**Testing approach**: 
I ran the software and verified that the synchronization statistics were still accurately reported in the output.

**Time spent**: 
 9 minutes
---

### Entry 3 - May 1, 2026 - 2:34 PM
**What I implemented**: 
To safeguard the shared executionLog ArrayList, I introduced a separate logLock.

**Challenges encountered**: 
The executionLog is not thread-safe because it is a ArrayList. When many threads write to it simultaneously,
 a ConcurrentModificationException or inconsistent log entries may result.

**How I solved it**: 
To ensure that only one thread changes the list at a time, I used a different ReentrantLock called logLock  around executionLog.add(message).

**Testing approach**: 
I ran the application several times to make sure the execution log summary showed up without any errors.

**Time spent**: 
 14 minutes
---

### Entry 4 - May 1, 2026 - 2:48 PM
**What I implemented**: 
To manage CPU access, I created a binary semaphore called cpuSemaphore with a single permit.

**Challenges encountered**: 
Using cpuSemaphore.acquire() to handle InterruptedException was the primary challenge.

**How I solved it**: 
To prevent deadlock, I released the semaphore inside a finally block and used a try-catch block for the acquire().

**Testing approach**: 
I ran the software and confirmed that it finished correctly and that each process ran one at a time.

**Time spent**: 
20 minutes
---

### Entry 5 - May 1, 2026 - 3:08 PM
**What I implemented**: 
In addition to adding semaphore protection to runToCompletion(), I examined the repository visibility, commit history, and final output.

**Challenges encountered**: 
In addition to the standard run() function, I had to ensure that the semaphore protected the final process execution.

**How I solved it**: 
At the start of runToCompletion(), I inserted cpuSemaphore.acquire(), which I then released in a finally block.

**Testing approach**: 
I ran the program's final version to make sure it printed the statistics, finished all processes, and didn't display synchronization issues.

**Time spent**: 
 42 minutes
---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:
There are two race conditions in the original code. The first one occurs in the shared counter variables such as contextSwitchCount, completedProcessCount, and totalWaitingTime. In the original code, operations like:

contextSwitchCount++;
totalWaitingTime += time;

were executed by multiple threads without synchronization. These operations are not atomic, so if two threads update the same variable at the same time, one update may be lost, leading to incorrect final results.

The second race condition occurs in the shared executionLog, which is implemented as an ArrayList. In the original code, the following line was not protected:

executionLog.add(message);

Since ArrayList is not thread-safe, multiple threads adding elements at the same time can cause inconsistent data or even runtime errors like ConcurrentModificationException.

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:
A ReentrantLock is  used to safeguard crucial areas where shared data is altered. It guarantees that a single thread can only access that region at a time. ReentrantLock was utilized in my implementation to safeguard the executionLog and shared counter variables, allowing for safe modifications to these shared resources.

A semaphore is used to manage who has access to a resource. In my code, a single CPU was represented by a semaphore with one permit. This mimics the operation of a real CPU by limiting the number of processes that can run simultaneously. To manage CPU access, I employed cpuSemaphore.acquire() prior to execution and cpuSemaphore.release() following execution.

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:
When two or more threads are waiting for one another to release resources and none of them can proceed with execution, this is known as deadlock. Consequently, the program ceases to advance.

Always releasing locks and resources inside a finally block is one way to avoid deadlock. In order to ensure that ReentrantLock and Semaphore are always released, even in the event of an error, I made sure to release both inside finally blocks.

Keeping critical sections short is another tactic. I limited the use of locks in my solution to quick tasks like writing a log entry or updating counters. This lessens the likelihood of blocking other threads and the amount of time a thread retains a lock.

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:
The three shared counter variables, for Task 1 contextSwitchCount, completedProcessCount, and totalWaitingTime—were protected by a single lock called counterLock. Since multiple shared variables are protected by a single lock, this locking strategy is coarse-grained.

I went with this strategy since it is more straightforward, manageable, and appropriate for this task. Because only one counter update may occur at a time, coarse-grained locking has the drawback of potentially lowering concurrency. Because the counters are independent, fine-grained locking, which employs a different lock for each counter, can offer greater concurrency. However, utilizing a single lock is obvious, secure, and sufficient for this simulation because these counter actions are so brief.

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 
contextSwitchCount, completedProcessCount, and totalWaitingTime.

**Why they need protection**: 
These variables are shared between multiple process threads. If more than one thread updates them at the same time, the final values may become incorrect because operations like incrementing or adding are not atomic.

**Synchronization mechanism used**: 
I used a ReentrantLock called counterLock.

**Code snippet**:
```java
public static final ReentrantLock counterLock = new ReentrantLock();

public static void incrementContextSwitch() {
    counterLock.lock();
    try {
        contextSwitchCount++;
    } finally {
        counterLock.unlock();
    }
}

public static void incrementCompletedProcess() {
    counterLock.lock();
    try {
        completedProcessCount++;
    } finally {
        counterLock.unlock();
    }
}

public static void addWaitingTime(long time) {
    counterLock.lock();
    try {
        totalWaitingTime += time;
    } finally {
        counterLock.unlock();
    }
}
```

**Justification**: 
The counterLock ensures that only one thread can update the shared counter variables at a time. This prevents lost updates and keeps the final synchronization statistics accurate.

---

### Critical Section #2: Execution Log

**What resource**: 
executionLog (ArrayList)

**Why it needs protection**: 
The executionLog is shared between multiple threads, and ArrayList is not thread-safe. If multiple threads add elements at the same time, it can lead to inconsistent data or runtime errors such as ConcurrentModificationException.

**Synchronization mechanism used**: 
I used a separate ReentrantLock called logLock.

**Code snippet**:
```java
public static final ReentrantLock logLock = new ReentrantLock();

public static void logExecution(String message) {  
    logLock.lock();  
    try {  
        executionLog.add(message);  
    } finally {  
        logLock.unlock();  
    }  
}
```

**Justification**: 
Using a separate lock for the execution log ensures that only one thread can modify the log at a time. This prevents data corruption and allows safe logging of process execution steps.

---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 
The purpose of the semaphore is to control access to the simulated CPU. It makes sure that only one process can execute on the CPU at a time.


**Number of permits and why**: 
I used one permit because the simulation represents a single CPU. Using one permit makes the semaphore work like a binary semaphore.

**Where implemented**: 
The semaphore was declared in the SharedResources class and used in both run() and runToCompletion().

**Code snippet**:
```java
public static final Semaphore cpuSemaphore = new Semaphore(1);

boolean acquired = false;

try {
    try {
        SharedResources.cpuSemaphore.acquire();
        acquired = true;
    } catch (InterruptedException e) {
        e.printStackTrace();
    }

    // process execution code

} finally {
    if (acquired) {
        SharedResources.cpuSemaphore.release();
    }
}
```

**Effect on program behavior**: 
The semaphore prevents more than one process from entering the CPU execution section at the same time. This keeps the scheduler behavior consistent and avoids concurrent CPU access.
---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
javac SchedulerSimulationSync.java
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
```

**Results**: 
═══ Synchronization Statistics ═══
Total Context Switches: 30
Total Completed Processes: 13
Total Waiting Time: 892771ms
Average Waiting Time: 68674ms

═══ Process Summary Table ═══
Process    Priority     Burst Time   Waiting Time
────────────────────────────────────────────────
P1         2            2286         56          
P2         4            9010         81460       
P3         3            9850         82509       
P4         2            4167         55113       
P5         5            7755         55337       
P6         3            7467         59159       
P7         3            7120         62675       
P8         5            9432         84362       
P9         2            9159         85796       
P10        4            7606         74014       
P11        4            9448         86965       
P12        3            5872         81702       
P13        1            5799         83623       

═══ Execution Log Summary ═══
Total log entries: 60


**Why synchronization is necessary**: 
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.)

When several threads access shared resources like contextSwitchCount, completedProcessCount, totalWaitingTime, and executionLog, synchronization is required. In the absence of synchronization, simultaneous processes such as adding log messages or increasing counters could result in inconsistent log data, lost updates, or inaccurate statistics. While the Semaphore guarantees that only one process can access the simulated CPU at a time, the ReentrantLock safeguards the shared variables and execution log.

**Conclusion**: 
The consistency check verifies that the software operates properly and generates consistent outcomes. The completion message and final statistics demonstrate that the synchronization mechanisms are operating as intended.
---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: 
After using logLock to synchronize the executionLog, I ran the application several times. My main goal was to see if concurrent access to the ArrayList would result in any runtime exceptions.

**Results**: 
All of the program's executions were successful, and no ConcurrentModificationException was thrown. Total log entries: 60 was written at the end of the execution log summary, indicating that all logging procedures were successfully finished.

**What this proves**: 
This demonstrates how thread-safe access to the ArrayList is ensured by protecting executionLog.add(message) with ReentrantLock (logLock). It ensures that several threads can securely write to the shared log and avoids problems with concurrent modification.
---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: 
It was anticipated that every process would successfully finish execution and that the total number of finished processes would equal the total number of created processes. Additionally, the software should show a process summary table and accurate synchronization statistics.

**Actual values**: 
Total Context Switches: 30
Total Completed Processes: 13
Total Waiting Time: 892771ms
Average Waiting Time: 68674ms

**Analysis**: 
The fact that the actual values match the expected behavior indicates that the scheduler operated as intended and all processes were carried out successfully. The message "ALL PROCESSES COMPLETED" and accurate statistics demonstrate how synchronization techniques guaranteed precise execution and avoided race situations.
---

### Test 4: Different Scenarios
**Scenario tested**: 
Different burst times, priorities, and multiple processes (13 processes)

**Purpose**: 
This test was conducted to ensure that the synchronization solution functions properly with varying process durations and priorities, rather than just one straightforward scenario.

**Results**: 
13 processes were displayed in the report, each with a unique burst time and priority. While certain processes, like P1, could be completed in a single quantum, others required several turns and context switches . The final statistics were reported error-free and all processes were successfully finished by the scheduler.

**What I learned**: 
I discovered that since threads may access shared resources at different times, synchronization is crucial in various scheduling scenarios. Locks and semaphores aid in maintaining the program's consistency, organization, and safety even in situations when processes have disparate burst times.
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
