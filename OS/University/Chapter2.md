---
date : 2023-03-12 11:00 Sun
alias : []
---

---

## 2.1 Operating System Services
+ User interface 
	+ Command-LIne
	+ GUI
	+ Touch-screen
	+ Batch(?)

## 2.2 User and Operating System Interface

### CLI
+ CLI or Command interpreter 
	+ allow direct command entry
	+ Sometimes implemented in kernel , sometimes by systems prgoram
	+ Sometimes multiple flavors implemented -- shells
	+ BASH

### GUI
+ User friendly desktop metaphor interface

## 2.3 System Calls
+ Programming interface to the services provided by the OS
	+ Win32 API for Windows
	+ POSIX API for POSIX-based systems
	+ Java API for the JVM

### System Call Parameter Passing
+ Three general methods used to pass parameters to the OS
	+ pass the parameters in registers
	+ Parameters stored in a `block` or `table` , in `memory` and address of block passed as a parameter in a register
	+ Paramters placed, or `pushed`, on to the `stack` by the `program` and `popped` off the stack by the opperating system 

*Block and stack methods do not limit the number or length of parameters being passed*

## 2.4 System Services
+ File manipulation
+ Status inforamtion sometimes stored in a file(`registry`) 
+ Programming language support
+ Program loading and execution
+ Communications
+ Background services(servies , subsystems, daemons)
+ APplication programs

## 2.5 Linkers and Loaders
+ Source code compiled into object files designed to be loaded into any physical memory location – `relocatable object file`
+ `Linker` combines these into single binary executable file
	+ Also brings in libraries
+ Program resides on `secondary storage` as binary executable
+ Must be brought into memory by `loader` to be executed
+ `Relocation` assigns final addresses to program parts and adjusts code and  data in program to match those addresses
+ Dynamically linked libraries (DLLs) are loaded `as needed`

## 2.6 Why Applications are Operating System Specific
+ Each operating system provides its own unique system calls
+ Apps can be multi-operating system
	+ Written in interpreted language
		+ Python
		+ Ruby
	+ Written in language that includes a VM
	+ Use standard language
+ ABI

### 2.7 Operating System Design and Implementation
+ Defineing goals and specifications
	+ `User` goals and `System` goals
		+ `User goals`  : operating system should be convenient to use , use, easy to learn, reliable, safe, and fast.
		+ `System goals` : operating system should be easy to design, implement, and maintain, as well as flexible, reliable, error-free, and efficient
+ Policy : `What` will be done
+ Mechanism : `How` to  do it?

## 2.8 Operating System Structure
+ General-purpose -- large program
+ Simple Structure -- MS-DOS
+ More complex -- UNIX
+ Layered -- and abstraction
+ Microkernel -- Mach

### Monolithic Structure -- Original Unix (單一型、集體型核心)
+ Orginal UNIX operating system limited by hardware functionality
+ Consists of two separable parts
	+ System programs
	+ The kernel
		+ Consists of everything `below the system-call interface and above the physical hardware`		
		+ Provides the file system, CPU scheduling, memory management, and other operating-system functions; a large number of functions for one level

## Linux System Structure
+ Monolithic plus `modular design`

## Layered Approach
+ OS is divided into a number of layers each built on top of lower layers
+ Layers are selected such that each uses functions and services of only lower-level layers

### Microkernels
+ Moves as much from the kernel into user space 
+ Mach example of mircorkernel
+ `Communication takes place between user modules using message passing`
+ Pros and cons
	+ Pros
		+ Easier to extend a microkernel
		+ Eaiser to port the operating system to  new architectures (less code is running in kernel)
		+ More reliable (less code is running in kernel mode)
		+ More secure
	+ Cons
		+ Performance overhead of user space to kernel space communication

### Modules
+ Loadable kernel modules(LKM)
	+ Object-oriented approach
	+ Each core component is separte
	+ Each talks to the others over konwn interfaces
	+ `Each is loadable as needed within in the kernel`

### Hybrid Systems
+ Combines multiple approaches to address performance, security, usability 
+ Monolithic + Modules/partial-Microkernel

TBD: ppt13, batch, aqua UI, cocoa programing environment

## 2.9 Building and Booting an Operating System
+ Operating systems generally designed to run on a class of systems with variety of peripherals(外設)

### Building and Booting Linux

### System Boot
>1. When power initialized on system, `execution starts at a fixed memory location` 
>2. Operating system must be made available to hardware so hardware can start it
> 3. `Small piece of code – bootstrap loader, BIOS, stored in ROM or EEPROM locates the kernel, loads it into memory, and starts it` 
>4. Sometimes two-step process where boot block at fixed location loaded by ROM code, which loads bootstrap loader from disk
>5. `Modern systems replace BIOS with Unified Extensible Firmware Interface (UEFI)`
>6. Common bootstrap loader, `GRUB`, allows selection of kernel from multiple disks, versions, kernel options
>7. Kernel loads and system is then running
>8. Boot loaders frequently allow various boot states,such as single user mode


