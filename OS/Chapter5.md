---
date : 2023-04-10 Mon 16:13
alias : []
---

---

## CPU Scheduler

CPU scheduling decisions may take place when a process:
1. Switches from running to  waiting state
2. Switches from running to `ready` state
3. Switchs from waiting to ready
4. Terminates


1 and 4 are `nonpreemptive` , 2 and 3 are `preemtive`

## Dispatcher

+ Dispatch latency 
	+ Time spend for the dispatcher to stop one process adn start another running

## Scheduling Crieteria

+ CPU utilization
+ Throughtput
+ Turnaround time
+ Waiting time
+ Response time

## Scheduling Algorithms

+ First-come, First-served 
	+ `Convoy effect`
		+ short process behind long process
+ Shortest-Job-Frist
	+ nonpreemptive
	+ preemptive
		+ also called Shortest-Remaining-Time-First (SRTF)
	+ *pros*
		+ gives minimum average waiting time 
	+ *cons*
		+ hard to the length of the next CPU request

+ Round-Robin
	+ After this time has elapsed , the process is `preempted and added to the end of the ready queue`
	+ q
		+ q large -> FIO
		+ q small -> q 相對於context switch來說要大一點，不然成本很高
	+ *pros*
		+ better response than `SJF`
	+ *cons*
		+ `higher average turnaround time`
+ Prioriy 
	+ <font color="orange">smallest interger -> highest priority</font>
	+ preemptive
	+ nonpreemptive
	+ *cons*
		+ Starvation
			+ can be solved using `aging` - as time progresses increase the priority of the process
		
+ Mulitilevel Queue
	+ with priority scheduling , have `separate queues` for each priority
	+ schedule the process in the `hightest-priority` queue
+ Multilevel Feedback
	+ process move between the `various queues `

## Thread Scheduling

+ process-contention scope(PCS)
	+ user-space thread
	+ competition is `within the process`
+ system-contention scope(SCS)
	+ kernel-space thread
	+ competition is `among all thread in system`

## Multiple-Processor Scheduling

+ Multiprocess architecture
	+ Multicore CPU
	+ Multithreaded cores
	+ NUMA system
		+ assign memory close to the CPU the thread is running on
	+ Heterogeneous 

+ Symmetric multiprocessing(SMP)
	+ each processor is `self scheduling`
	+ threads in a `common ready queue`
	+ each processor may have it own `private queue`

+ Muticore Processors

+ Multithreaded Multicore System

+ Mutiple-Processor Scheduling - Load Balancing
	+ Load balancing
	+ Push migration
		+ `periodic` task checks load on each processor, `and if found pushes task from overloaded CPU to other CPUs`
	+ Pull migration
		+ `idle processors pulls waiting task` from busy processor

+ Muti-Processor Scheduling - Processor Affinity
	+ Soft affinity 
		+ OS attemps to keep thread running on the same processor , but `no guarantees`
	+ Hard affinity
		+ allwos a process to `specify a set of processors` it may run on


+ Real-Time CPU Scheduling
	+ Soft real-time systems
		+ Critical real-time tasks have the highest priority, but `no guarantee` as to when tasks will be scheduled
	+ Hard real-time systems
		+ task must be serviced by its deadline
	+ Event latency
		+ the amount of time that elapases from when an event occurs to when it is serviced
		+ Interrupt latency
		+ Dispatch latency
			+ conflicts
				+ 
			+ dispatch

+ Rate Montonic Scheduling
+ Earliest Deadline First Scheduling (EDF)
