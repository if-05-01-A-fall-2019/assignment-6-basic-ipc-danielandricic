### Daniel Andricic 3AHIF

# Basics of Interprocess Communication

1.) Race Condition Describe with your own words: What is a race condition?
+ A race condition is when two processes constantly alternate for the computing time and together change their global variables, overwriting the entries from the process that finished first. The first process is the "loser", so to speak.
---
2.) Disabling Interrupts

i.) Why is it impossible to achieve Mutual Exclusion via disabling interrupts on a multi-core machine?
+ Because if you disable the interrupts in one Process and this one is taking forever to end his task,
the System will transmit the other Processes to the other Cores.
The other Cores of the Multi-Processor-System are able to access the resssources from the critical Process anytime.
That means if a task is taking forever, the system won't freeze, because it switches the Cores.

ii.) Why is it dangerous to give user processes the power to disable interrupts?
+ Because the user can't know exactly which processes the OS needs to perform its tasks.
If the user sets a process without interrupts, the computer will froze.

The danger exists that the computer freezes itself or loses cached data.

---
3.) Peterson's Solution

i.) Play through the two scenarios of the handout of Peterson's solution. Document how it works.
+ interest[0] = true and loser = 0, The Process 0 can start with his Task.
After he finishes his Task, he have to wait on Process 1 because loser = 1.
+ interest[1] = true and loser = 1, Process 1 starts his Task, after his Task he changes also loser, so loser = 0 again and he has to wait on Process 0.
+ interest[0] = false, Process 1 can finally enter the critical region.

ii.) Play through the scenario which makes the strict alternation approach fail. Document how it fails.
+ The first Process gets more computing time, because of it's priority, so he can iterate 8 instructions until the scheduler switches to the second Process.
The second Process can iterate 1 instruction, so the first Process can iterate 2 times through. Until he can't work after Process 2 gets his task done and it starts again from the beginning.
The Processes would slow down the Computer dramatically or freeze it.

iii.) What is the meaning of the variable loser in Peterson's solution? When does it have any effect?
+ The Variable loser is the processID from the second Process, because the first Process entered the critical region, so the second Process has to wait until he gets the OK of the Scheduler.

iv.) Extend the given functions such they can handle three processes.

```
int loser // shared variable
bool interested[3] // 3 Processes
int otherProcess1;
int otherProcess2;

void enter_region(int process) {
  if(process == 2) {
    otherProcess2 = 1;
    otherProcess1 = 0;
  }
  else if(process == 1) {
    otherProcess2 = 2;
    otherProcess1 = 0;
  }
  else {
    otherProcess1 = 1;
    otherProcess2 = 2;
  }
  interested[process] = true;
  loser = process;
  while(loser == process && (interest[otherProcess1] || interested[otherProcess2]));
}

void leave_region(int process) {
  interested[process] = false;
}

```
