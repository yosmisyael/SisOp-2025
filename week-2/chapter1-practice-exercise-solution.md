# Chapter 1 Practice Exercise
Name: Misyael Yosevian Wiarda

NRP: 3124500039

Class: 1 D3 IT B

Dosen Pengajar: Dr. Ferry Astika Saputra ST, M.Sc

### 1.1 What are the three main purposes of an operating system?

**Answer:**

The three main purposes of operating system are:
- It manage computer's resources, distributing them fairly and efficiently to provide environment for program execution in efficient way.
- It ensures all program get resource they need by allocating the resource fairly and efficiently as required to perform required task.
-  Act as control through supervision of user program execution to minimalize error and manage operation of input output devices.
---
### 1.2 We have stressed the need for an operating system to make efcient use of the computing hardware. When is it appropriate for the operating system to forsake this principle and to “waste” resources? Why is such a system not really wasteful? 

**Answer:**

There are situations where it may be acceptable for the system to prioritize other factors over resource efficiency. For example, in single-user systems, the focus should be on enhancing the user experience. A graphical user interface (GUI) may consume extra CPU power, but its main purpose is to improve how the user interacts with the system, which justifies this use of resources. Therefore, it’s not truly wasteful.

---
### 1.3 What is the main dificulty that a programmer must overcome in writing an operating system for a real-time environment?

**Answer:**

The primary challenge for a programmer when developing an operating system for a real-time environment is ensuring that the system operates within strict time limits. If a task isn't completed within the required timeframe, it could lead to a failure of the entire system. As a result, the programmer must design the scheduling methods carefully to ensure that response times always stay within the specified time constraints.

---
### 1.4 Keeping in mind the various denitions of operating system, consider whether the operating system should include applications such as web browsers and mail programs. Argue both that it should and that it should not, and support your answers.

**Answer:**

By including applications like web browsers and email programs within the operating system could allow these applications to better utilize kernel features, potentially improving their performance compared to standalone versions. However, there are several reasons against embedding such applications directly in the operating system: 
1. These programs are applications, not essential parts of the OS, 
2. The security risks of running them within the kernel may outweigh any performance benefits.
3. Adding these applications can result in a more bloated operating system.

---
### 1.5 How does the distinction between kernel mode and user mode function as a rudimentary form of protection (security)?

**Answer:**

The separation between kernel mode and user mode acts as a basic form of protection by restricting access to critical system functions. Certain instructions can only be executed in kernel mode, and hardware devices or interrupts can only be accessed or modified when the system is in kernel mode. As a result, the CPU has limited functionality in user mode, helping to safeguard essential system resources and prevent unauthorized access or misuse.

---
### 1.6 Which of the following instructions should be privileged?

    a. Set value of timer.
    b. Read the clock.
    c. Clear memory.
    d. Issue a trap instruction.
    e. Turn off interrupts.
    f. Modify entries in device-status table.
    g. Switch from user to kernel mode.
    h. Access I/O device.

**Answer:**

The operations that should be privileged are: setting the timer value, clearing memory, disabling interrupts, modifying the device-status table, and accessing I/O devices. The remaining operations, such as reading the clock, issuing a trap instruction, and switching from user mode to kernel mode, can be performed in user mode.

---
### 1.7 Some early computers protected the operating system by placing it in a memory partition that could not be modied by either the user job or the operating system itself. Describe two difculties that you think could arise with such a scheme.

**Answer:**

Two potential issues with this approach are: First, the data needed by the operating system, such as passwords, access controls, and accounting information, would have to be stored in or passed through unprotected memory, making it vulnerable to unauthorized access. Second, the system might face difficulties in managing dynamic interactions or updates between protected and unprotected memory areas, leading to performance or security challenges.

--- 
### 1.8 Some CPUs provide for more than two modes of operation. What are two possible uses of these multiple modes?

**Answer:**

Most systems use only user and kernel modes, but some CPUs offer multiple modes of operation. These additional modes can be used for more detailed security management. For example, instead of just distinguishing between user and kernel modes, different types of user modes could be created, allowing users in the same group to execute each other’s code. The system could switch to a specific mode when a user in the group runs code, enabling mutual access to each other’s code. Another potential use is within kernel code itself. For instance, a particular mode could be dedicated to running USB device drivers, allowing these drivers to operate without fully entering kernel mode, effectively creating a hybrid user/kernel mode for device management.

---
### 1.9 Timers could be used to compute the current time. Provide a short description of how this could be accomplished.

**Answer:**

To calculate the current time using timer interrupts, a program could set a timer to trigger at a specific future time and then enter a sleep state. When the timer interrupt occurs, the program wakes up and updates its internal counter, which tracks how many interrupts have occurred. It would then repeat this process, continually resetting the timer and updating its state each time an interrupt is received. This way, the program can keep track of time based on the number of interrupts.

---
### 1.10 Give two reasons why caches are useful. What problems do they solve? What problems do they cause? If a cache can be made as large as the device for which it is caching (for instance, a cache as large as a disk), why not make it that large and eliminate the device?

**Answer:**

Caches are beneficial when two components that exchange data operate at different speeds. They help solve this issue by acting as an intermediate buffer of speed, allowing faster devices to access data without waiting for slower ones. However, maintaining data consistency between the cache and the original component is crucial. If data changes in the component and is also in the cache, the cache must be updated. This becomes particularly challenging in multiprocessor systems where multiple processes might access the same data.

While it’s theoretically possible to replace a component with a cache of the same size (e.g., using a cache as large as a disk), this only works if: (a) the cache can retain data in the same way the component does (e.g., if the component can hold data without power, the cache must do the same), and (b) the cost is justifiable, since faster storage tends to be more expensive.

--- 
### 1.11 Distinguish between the client–server and peer-to-peer models of distributed systems.

**Answer:**

In the client-server model, there is a clear distinction between the roles of the client and the server. The client requests services, while the server provides them. In contrast, the peer-to-peer model does not have such rigid roles. In this model, all nodes are considered equals (peers) and can act as both clients and servers. A node can request services from another peer or provide services to other peers in the system.

For example, in a system where nodes share cooking recipes, under the client-server model, all recipes would be stored on a central server. Clients would need to request a recipe from this server. On the other hand, in a peer-to-peer model, a node could ask other peer nodes for the recipe. Any peer that has the recipe could provide it to the requesting node, and each peer could act as both a client (requesting recipes) and a server (providing recipes).
