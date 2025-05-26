Name: Misyael Yosevian Wiarda

NRP : 3124500039

Class: 1 D3 IT B

Dosen Pengajar: Dr. Ferry Astika Saputra ST, M.Sc

# The Differences betweeen Virtualization and Containerization

## Virtualization

### Core Concept:
Virtualization creates a complete virtual copy of a physical computer, including the operating system, kernel, and hardware. A hypervisor (software) manages these virtual machines (VMs), allowing multiple VMs to run on a single physical server. Each VM operates independently, with its own dedicated resources.

### How it Operates:
The hypervisor allocates physical resources (CPU, RAM, storage) to each VM. Each VM runs its own full operating system, which can be different from the host OS. This provides strong isolation, as one VM's issues won't affect others.

### Example:
Running Windows and Linux simultaneously on a Mac using VMware or VirtualBox. Cloud providers like AWS and Azure using VMs to provide virtual servers.

### Key Differences:
- Full OS isolation.
- Higher resource overhead.
- Slower startup times.

## Containerization

### Core Concept:
Containerization packages an application and its dependencies into a container, which shares the host OS kernel.   
Containers are lightweight and portable, as they don't include a full OS.   
A container runtime (like Docker) manages the containers.   

### How it Operates:
Containers run as isolated processes on the host OS.   
They share the host OS kernel, which reduces resource overhead.   
Containers are much faster to start and stop than VMs.   

### Example:
Running multiple instances of a web application in Docker containers.   
Deploying microservices in a Kubernetes environment.   

### Key Differences:
OS kernel sharing.
Lower resource overhead.
Faster startup times.   
Core Differences that Make Them Behave Differently:

### OS Level:
- Virtualization: Each VM has its own OS.   
- Containerization: Containers share the host OS kernel.

### Resource Usage:
- Virtualization: High resource usage due to full OS copies.
- Containerization: Low resource usage due to kernel sharing.   

### Isolation:
- Virtualization: Strong isolation between VMs.   
- Containerization: Process-level isolation within the host OS.
