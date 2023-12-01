---
date : 2023-03-11 21:29 Sat
alias : []
---

---

## Computer-System Operation

I/O devices and the CPU can execute concurrently
+ By `device controller` (has local buffer)
+ What is interrupt.....
+ TBD

## Common Functions of Interrupts
+ Transfers control to  the `interrupt service routine(ISR)`  through `interrupt vector`, which contains the *addresses of all the service routines*
+ must save address of the interrupted instruction
+ trap, exception is a `software-generated` interrupt (not system)

## Interrupt Handling
+ Preserves the `state of CPU` through registers and the program counter
+ polling
+ vectored interrupt system

## I/O Structure
+ Control reutrns to user program `only upon I/O completion`
	+ Wait instruction idles CPU `until the next interrupt`
	+ Wait loop (contention for memory access)
	+ At most one I/O request is outstanding `at a time`, no simultaneous
+ Control retuns to usre program `without waiting for I/O completion`
	+ `System call : request to the OS to allow user to wait for I/O completion`
	+ `Device-status table`

## Storage Structure

## Direct Memory Access Structure

+ Device controller transfers blocks of data from buffer storage directly to main memory `without CPU intervention`
+ Only one interrupt is generated per block, rather than the one interrupt per byte

## Computer-System Architecture

Single general-purpose processor 、 Multiprocessors 
+ Increased throughput
+ Economy of scale
+ Increased reliablity
+ Types:
	+ Asymmetric Multiprocessing : `each processor is assigned a specific task`
	+ Symmetric Multiprocessing : `each processor performs all tasks`

Clustered System
+ Storage-area network
+ Types:
	+ Asymmetric clustering
	+ Symmetric clustering
+ High-performance computing

## 1.4 Operating-System Operations

Bootstrap program
+ stored in `ROM` `EPROM`  (?), known as firmware, loads OS kernal and statrs execution
+ Kernal starts `system daemons(services provided outside of the kernal)`

Kernel interrupt driven
 + Hardware interrupt
 + Software interrupt ( exception or trap)

### Multiprogramming and Multitasking
PPT P.20

### Dual-mode and Multimode Operation

+ Dual-mode
	+ User mode
	+ Kernal mode
	+ Mode bit
	+ Transition from user to kernal mode
		+ Timer to prevent infinite loop/process hogging resources
+ Multi-mode 
	+ virutal machine manager(VMM) for guest VM

## 1.5 Resoruce Management

### Process Management
+ Twy kinds of process
	+ Single-threaded process
		+ has `one program counter` to specifying location of next instruction
	+ Multi-threaded process
		+ has one program counter per thread

### Memory Management

+ Determines  `what` is in memory and `when`
+ Allocating and deallocation memory
+ Keeping track of which parts of memory are currently being used and by whom
+ Deciding which process and data to move into and out of memory

### File-system Management

### Mass-Storage Management

Mounting and unmounting(??)


## 1.6 Protection and Security

+ Protection 
+ Security - defense of the system against internal and external attacks
	+ DDOS
	+ worms
	+ viruses
	+ identity theft
+ Privilege
	+ User ID
	+ Group ID
	+ Privilege escalation

### 1.7 Virtualization

Allow operating systems to run applications within other OS
+ Emulation
	+ Generally slowest method
	+ Interpretation
+ Virtualization
	+ Consider VMware running WinXP guests, each running applications, all on native WinXPhostOS

	+ Machine-level virtualization - VMM(Virtual Machine Manager/Monitor) or `Hypervisor` provides virtualization services

	+ `Operating-system-level virtualization containerization` (??)


TBD: EPROM, MOUNT, container