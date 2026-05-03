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

[Your answer here - 4-6 sentences with code examples]

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

[Your answer here - explain your implementation choices]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

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
