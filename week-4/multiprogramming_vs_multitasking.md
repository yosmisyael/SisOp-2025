Name: Misyael Yosevian Wiarda

NRP : 3124500039

Class: 1 D3 IT B

Dosen Pengajar: Dr. Ferry Astika Saputra ST, M.Sc

# Differences between Multiprogramming and Multitasking (Timesharing)

## Multiprogramming: Efficiency Through Batch Processing

### Core Idea:
Multiprogramming is designed to maximize CPU utilization by keeping it busy. It operates on the principle that a single user's job often involves waiting for I/O operations (like reading from a disk). During these wait times, the CPU would otherwise be idle.   

To prevent this, multiprogramming loads multiple jobs into memory. When one job waits, the operating system (OS) switches to another job, ensuring the CPU is always working.   


### Batch System Context:
The provided text links multiprogramming to "Batch systems." This is key. Batch systems typically process jobs in a non-interactive, sequential manner.
Jobs are submitted in batches, and the OS executes them one after another, switching between them when necessary.

Example:
Imagine a scenario where a university runs payroll, generates student reports, and compiles programming assignments.
In a multiprogramming batch system, these jobs would be submitted as a batch. The OS starts the payroll job. When it needs to read data from a disk, the OS switches to the student report generation job. If that job also encounters an I/O wait, the OS switches to the compilation job. The user does not interact with the running programs. They simply submit their programs and retrieve the results later. The goal is to keep the CPU busy, not to provide interactive feedback.   

## Multitasking (Timesharing): Interactive Computing

### Core Idea:
Multitasking, also known as timesharing, is an extension of multiprogramming that allows users to interact with their programs while they are running.
The CPU switches between jobs so rapidly that users perceive that multiple programs are running simultaneously.
This interactivity is crucial for modern computing.
### Key Features:
- Interactive Response: The OS aims for response times of less than one second, creating a smooth user experience.
Processes: Each user's running program is represented as a "process" in memory.
- CPU Scheduling: When multiple processes are ready to run, the OS uses CPU scheduling algorithms to determine which process gets CPU time.   
- Swapping and Virtual Memory: If all processes cannot fit into physical memory, the OS uses swapping (moving processes between memory and disk) or virtual memory (allowing processes to execute even if they are not entirely in memory).

Example:
Consider a user working on a computer running a web browser, a word processor, and a music player.
In a multitasking system, the user can type in the word processor, browse the web, and listen to music simultaneously. The OS rapidly switches between these applications, giving each a small slice of CPU time.   
The user perceives that all applications are running concurrently. If the user opens too many programs, and not all of the program data will fit into ram, then the operating system will use virtual memory.

## Key Differences Summarized:
### Interaction:
- Multiprogramming: Non-interactive (batch processing).
- Multitasking: Interactive (timesharing).   
### Goal:
- Multiprogramming: Maximize CPU utilization.
- Multitasking: Provide interactive computing and user experience.
### Response Time:
- Multiprogramming: Response time is not a primary concern.
- Multitasking: Requires fast response times (typically < 1 second).
### User Experience:
- Multiprogramming: No user interaction during execution.
- Multitasking: Users directly interact with running programs.
