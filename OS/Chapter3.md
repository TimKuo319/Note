---
date : 2023-04-09 Mon 15:25
alias : []
---

---

## Process parts in memory

+ Text section : usually used to store program code
+ Stack : containing temporary data such as fucntion parameter, return addresses, local variables....
+ Data section : containing global variables
+ Heap : containing memory dynamically allocated during run time

## Process state

+ New
+ Running
+ Waiting
+ Ready
+ Terminated

## Processs Control Block(PCB)
+ Process state
+ Program Counter
+ CPU registers -- content of all process-centric register
	+ Program Counter
	+ Stacke Pointer 
	+ Base Pointer
	+ General Purpose Registers
	+ Flags Register
+ CPU scheduling infromation
	+ Priority
	+ Scheduling queue pointers
+ Memory-management information
	+ memory allocated to the process , `base address`, `limit addresss`, `page table pointer`........
+ Accounting information
	+ CPU used
	+ clock time elapsed since start
	+ time limits
+ I/O status information
	+ I/O devices allocated to process
	+ list of open files


## Process creation

+ Resource sharing options
	+ Parent and children share all resources
	+ Children share subset of parent resources
	+ Parent and child share no resources
+ Execution optoins 
	+ Parent and children execute concurrently
	+ Parent waits until children terminate
+ Address space
	+ Child duplicate of parent
	+ Child has a program loaded into it

# Process termination

+ Terminates but no parent waiting , process is a zombie
+ If parent terminated without invoing wait() , process i an orphan

Main difference : 
+ zombie processes are caused by the `failure of the parent process to properly clean up after its child process`
+ orphan processes are caused by the `unexpected termination of the parent process.`


## Interprocess Communication

Cooperating processes need interprocess communication(IPC)

Two models of IPC
+ Shared memory
+ Message passing

### IPC - Message Passing

processes communicate with each other without resorting to shared variables

+ send
+ receive

## Synchronization
+ Blocking
	+ Blocking send -- the sender is blocked until the message is received
	+ Blocking receive -- the receiver is blocked until the message is avaliable
+ Non-blocking

`Different combinations possible!!`
`If both send and receive are blocking, we have a rendezvous`

## Buffering
Queue of message of attached to  the communication link
1. zero capacity - no mesage are queued , rendezvous
2. Bounded capacity - finite length of n message , sender must wait if link is full
3. Unbounded - infinie length

## Pipe

+ Ordinary pipes 
	+ cannot be accessed from outside the process
	+ Require parent-child relationship
	+ usually unidirectional
+ Named pipes
	+ can be accessed without a parent-child relationship
	+ bidirectional

## Remote Procedure Calls

+ stub
