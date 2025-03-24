****Name: Misyael Yosevian Wiarda
NRP: 3124500039
Class: 1 D3 IT B

# IOPS/FLOPS Benchmark Reports

![Benchmark Result](https://raw.githubusercontent.com/yosmisyael/SisOp-2025/refs/heads/main/week-5/iops-flops-benchmark.jpghttps://github.com/yosmisyael/SisOp-2025/blob/main/week-5/iops-flops-benchmark.jpg "Benchmark Result")

- [IOPS/FLOPS Benchmark Reports]()
  * [1. IOPS](#1-iops)
    + [1.1 Benchmark Result](#11-benchmark-result)
    + [1.2 Benchmark Explanation](#12-benchmark-explanation)
      - [1.2.1. Main Function (main)](#121-main-function-main)
      - [1.2.2. IOPS Calculation Thread (calculateIOPS64)](#122-iops-calculation-thread-calculateiops64)
      - [1.2.3. Benchmark Initialization and Reporting (initIOPS64)](#123-benchmark-initialization-and-reporting-initiops64)
  * [2. FLOPS ](#2-flops)
    + [2.1 Benchmark Result](#21-benchmark-result)
    + [2.1.1 Command:](#211-command)
    + [2.1.2 Result:](#212-result)
    + [2.1.3 Interpretation](#213-interpretation)
    + [2.2 Benchmark Explanation](#22-benchmark-explanation)
      - [2.2.1. Floating-Point Calculation Thread (calculateFLOPS64) - Detailed Logic:](#221-floating-point-calculation-thread-calculateflops64-detailed-logic)
      - [2.2.2. Benchmark Initialization and Reporting (initFLOPS64) - Detailed Logic:](#222-benchmark-initialization-and-reporting-initflops64-detailed-logic)

<!-- TOC --><a name="1-iops"></a>

## 1. IOPS

<!-- TOC --><a name="11-benchmark-result"></a>

### 1.1 Benchmark Result

Command:

```shell
./iops64 8
```

Result:

```shell
Benchmarking for 64 Bit Integer operations per second
1| Tr 1: 2391874706 Tr 2: 2412399054 Tr 3: 2418575302 Tr 4: 2413187400 Tr 5: 2402978838 Tr 6: 2421890822 Tr 7: 2395598400 Tr 8: 2414157668 IOPS = 19270662190
Maximum CPU Throughput: 19.270662 Gigaiops.
Maximum Single Core Throughput: 2.421891 Gigaiops.
```

This benchmark assessed the 64-bit integer operations per second (IOPS) on an 8-core system, revealing a total CPU throughput of approximately 19.27 Gigaiops. Individual core performance varied, with the highest single-core throughput reaching about 2.42 Gigaiops. These results indicate the system's overall capacity to handle integer-based computations, as well as the peak performance achievable by a single core, providing a comprehensive view of the CPU's integer processing capabilities.

<!-- TOC --><a name="12-benchmark-explanation"></a>

### 1.2 Benchmark Explanation

<!-- TOC --><a name="121-main-function-main"></a>

#### 1.2.1. Main Function (main)

- **Argument Handling:**
  It checks if any command-line arguments are provided. If no arguments are given, it prompts the user to enter the number of CPU cores to use for the benchmark. If an argument is provided, it converts it to an integer (n) using atoi, representing the number of cores.
- **Benchmark Initialization:**
  It prints a message indicating the start of the 64-bit integer IOPS benchmark. It calls the initIOPS64 function, passing the number of CPU cores (n) as an argument.

<!-- TOC --><a name="122-iops-calculation-thread-calculateiops64"></a>

#### 1.2.2. IOPS Calculation Thread (calculateIOPS64)

- **Integer Operations:**
  This function is designed to run in a separate thread. It declares a large number of long long int variables and initializes them with large integer values. It enters an infinite while loop, performing a series of complex integer multiplications and additions on these variables. This is the core of the benchmark, designed to heavily utilize the CPU's integer arithmetic capabilities. It does the same calculation many times, in order to heavily stress the CPU.
- **Counting Operations:**
  A variable noOfOperations keeps track of the total number of operations performed. The loopOperations variable is used to set the amount of operations that are done in each loop. It updates the value pointed to by the arg pointer (which is a unsigned long long) with the current noOfOperations value. This allows the main thread to retrieve the operation count.

<!-- TOC --><a name="123-benchmark-initialization-and-reporting-initiops64"></a>

#### 1.2.3. Benchmark Initialization and Reporting (initIOPS64)

- **Thread Creation:**
  It takes the number of CPU cores (POOL) as input. It creates POOL number of threads, each running the calculateIOPS64 function.
  Each thread is passed a unique pointer to an element in the tFlops array, where it will store its operation count.
- **Measurement Period:**
  It sleeps for 60 seconds (sleep(60)), allowing the threads to perform their calculations. It then collects the number of operations performed by each thread, and stores them in the iopsLog 2 dimensional array.
- **Thread Cancellation:**
  After the measurement period, it cancels all the threads using pthread_cancel.
- **Result Reporting:**
  It opens a file named "iopsBench.txt" in append mode ("a"). It prints and writes the benchmark results to both the console and the file. For each thread, it calculates the IOPS (operations per second) by dividing the total operations by 60. It calculates the total IOPS for all threads combined. It also calculates the highest IOPS for any single thread, which is the single core maximum. It prints the maximum CPU throughput (total IOPS) and the maximum single-core throughput, converted to Gigaiops (billions of operations per second). It closes the file.
- **Output Interpretation:**
  The output shows the IOPS achieved by each individual thread (core) and the total IOPS for all cores combined. "IOPS" represents the number of 64-bit integer operations performed per second. "Maximum CPU Throughput" is the total IOPS of all cores combined. This value reflects the overall integer processing power of the system. "Maximum Single Core Throughput" is the highest IOPS achieved by any single core. This indicates the best-case integer performance of a single core on the CPU. The output is also written to a file iopsBench.txt, along with the date and time. The Gigaiops conversion is done by dividing the result of the IOPS calculation by 1,000,000,000. The code essentially tests the speed of integer math operations on the CPU.

<!-- TOC --><a name="2-flops"></a>

## 2. FLOPS

<!-- TOC --><a name="21-benchmark-result"></a>

### 2.1 Benchmark Result

<!-- TOC --><a name="211-command"></a>

### 2.1.1 Command:

```shell
./flops64 8
```

<!-- TOC --><a name="212-result"></a>

### 2.1.2 Result:

```shell
Benchmarking for 64 Bit Floating point operations per second
1| Tr 1: 2226363828 Tr 2: 2223968292 Tr 3: 2224483638 Tr 4: 2236111358 Tr 5: 2228431842 Tr 6: 2217805616 Tr 7: 2211725828 Tr 8: 2231984040 FLOPS = 17800874442
Maximum CPU Throughput: 17.800875 Gigaflops.
Maximum Single Core Throughput: 2.236111 Gigaflops.
```

<!-- TOC --><a name="213-interpretation"></a>

### 2.1.3 Interpretation

This benchmark measured the 64-bit floating-point operations per second (FLOPS) on an 8-core system, demonstrating a total CPU throughput of approximately 17.80 Gigaflops. Individual core performance fluctuated slightly, with the maximum single-core throughput reaching about 2.24 Gigaflops. These results provide insight into the system's ability to handle floating-point computations, showcasing both the aggregate performance across all cores and the peak performance of a single core.

<!-- TOC --><a name="22-benchmark-explanation"></a>

### 2.2 Benchmark Explanation

<!-- TOC --><a name="221-floating-point-calculation-thread-calculateflops64-detailed-logic"></a>

#### 2.2.1. Floating-Point Calculation Thread (calculateFLOPS64) - Detailed Logic:

- **Initialization of Operation Counters:**
  unsigned long long noOfOperations = 0;: This variable is initialized to zero and will accumulate the total count of floating-point operations performed by the thread. Unsigned long long loopOperations = 30 * 52;: This variable is set to a constant value, representing the number of floating-point operations implicitly performed within each iteration of the main while loop. The constant values are used to set the amount of math done per loop.
- **Initialization of Floating-Point Variables:**
  A large number of double variables (a, b, c, d, e, etc.) are initialized with specific floating-point values. These values are used as operands in the floating-point calculations.
- **Infinite Calculation Loop (while (1)):**
  The thread enters an infinite loop, continuously performing the floating-point calculations. Complex Floating-Point Expression: The core of the benchmark is a very long and complex floating-point expression that involves numerous multiplications and additions. This expression is designed to heavily utilize the CPU's floating-point unit. Repeated Calculation: The complex expression is repeated multiple times within the loop to increase the workload and stress the CPU.
- **Operation Count Update:**
  *((unsigned long long *)arg) = noOfOperations += loopOperations;: This line is crucial for reporting the progress. noOfOperations += loopOperations;: The loopOperations value is added to noOfOperations, effectively counting the floating-point operations performed in the current loop iteration.
  *((unsigned long long *)arg) = ...;: The updated noOfOperations value is then cast to an unsigned long long pointer and stored in the memory location pointed to by the arg parameter. This arg pointer was passed to the thread when it was created, and it points to a location within the tFlops array in the initFLOPS64 function. This allows the main thread to access the operation count from each worker thread.

<!-- TOC --><a name="222-benchmark-initialization-and-reporting-initflops64-detailed-logic"></a>

#### 2.2.2. Benchmark Initialization and Reporting (initFLOPS64) - Detailed Logic:

- **Thread Creation:**
  pthread_t memThreads[POOL];: An array of pthread_t variables is declared to hold the thread IDs.
  unsigned long long tFlops[POOL];: An array is created to store the total floating-point operation counts from each thread.pthread_create(&memThreads[r], NULL, calculateFLOPS64, &tFlops[r]);: Inside a loop, POOL number of threads are created. Each thread is created to execute the calculateFLOPS64 function. The &tFlops[r] address is passed as the arg parameter to each thread. This is how each thread gets a unique memory location to store its operation count.
- **Measurement Period:**
  sleep(60);: The main thread pauses execution for 60 seconds, allowing the worker threads to perform their calculations. flopsLog[r][i] = tFlops[r];: After the sleep period, the main thread reads the operation counts from the tFlops array and stores them into the flopsLog array. This collects the data from each thread.
- **Thread Cancellation:**
  pthread_cancel(memThreads[i]);: The main thread cancels all worker threads to ensure they stop running after the measurement period.
- **Result Reporting:**
  The code then calculates the FLOPS for each core and the total FLOPS, and then the maximum single core and total FLOPS. The results are printed to the console and written to a file named "flopsBench.txt". The results are converted to Gigaflops before they are printed.

## 1. IOPS

### 1.1 Benchmark Result

Command:

```shell
./iops64 8
```

Result:

```shell
Benchmarking for 64 Bit Integer operations per second
1| Tr 1: 2391874706 Tr 2: 2412399054 Tr 3: 2418575302 Tr 4: 2413187400 Tr 5: 2402978838 Tr 6: 2421890822 Tr 7: 2395598400 Tr 8: 2414157668 IOPS = 19270662190
Maximum CPU Throughput: 19.270662 Gigaiops.
Maximum Single Core Throughput: 2.421891 Gigaiops.
```

This benchmark assessed the 64-bit integer operations per second (IOPS) on an 8-core system, revealing a total CPU throughput of approximately 19.27 Gigaiops. Individual core performance varied, with the highest single-core throughput reaching about 2.42 Gigaiops. These results indicate the system's overall capacity to handle integer-based computations, as well as the peak performance achievable by a single core, providing a comprehensive view of the CPU's integer processing capabilities.

### 1.2 Benchmark Explanation

#### 1.2.1. Main Function (main)

- **Argument Handling:**
  It checks if any command-line arguments are provided. If no arguments are given, it prompts the user to enter the number of CPU cores to use for the benchmark. If an argument is provided, it converts it to an integer (n) using atoi, representing the number of cores.
- **Benchmark Initialization:**
  It prints a message indicating the start of the 64-bit integer IOPS benchmark. It calls the initIOPS64 function, passing the number of CPU cores (n) as an argument.

#### 1.2.2. IOPS Calculation Thread (calculateIOPS64)

- **Integer Operations:**
  This function is designed to run in a separate thread. It declares a large number of long long int variables and initializes them with large integer values. It enters an infinite while loop, performing a series of complex integer multiplications and additions on these variables. This is the core of the benchmark, designed to heavily utilize the CPU's integer arithmetic capabilities. It does the same calculation many times, in order to heavily stress the CPU.
- **Counting Operations:**
  A variable noOfOperations keeps track of the total number of operations performed. The loopOperations variable is used to set the amount of operations that are done in each loop. It updates the value pointed to by the arg pointer (which is a unsigned long long) with the current noOfOperations value. This allows the main thread to retrieve the operation count.

#### 1.2.3. Benchmark Initialization and Reporting (initIOPS64)

- **Thread Creation:**
  It takes the number of CPU cores (POOL) as input. It creates POOL number of threads, each running the calculateIOPS64 function.
  Each thread is passed a unique pointer to an element in the tFlops array, where it will store its operation count.
- **Measurement Period:**
  It sleeps for 60 seconds (sleep(60)), allowing the threads to perform their calculations. It then collects the number of operations performed by each thread, and stores them in the iopsLog 2 dimensional array.
- **Thread Cancellation:**
  After the measurement period, it cancels all the threads using pthread_cancel.
- **Result Reporting:**
  It opens a file named "iopsBench.txt" in append mode ("a"). It prints and writes the benchmark results to both the console and the file. For each thread, it calculates the IOPS (operations per second) by dividing the total operations by 60. It calculates the total IOPS for all threads combined. It also calculates the highest IOPS for any single thread, which is the single core maximum. It prints the maximum CPU throughput (total IOPS) and the maximum single-core throughput, converted to Gigaiops (billions of operations per second). It closes the file.
- **Output Interpretation:**
  The output shows the IOPS achieved by each individual thread (core) and the total IOPS for all cores combined. "IOPS" represents the number of 64-bit integer operations performed per second. "Maximum CPU Throughput" is the total IOPS of all cores combined. This value reflects the overall integer processing power of the system. "Maximum Single Core Throughput" is the highest IOPS achieved by any single core. This indicates the best-case integer performance of a single core on the CPU. The output is also written to a file iopsBench.txt, along with the date and time. The Gigaiops conversion is done by dividing the result of the IOPS calculation by 1,000,000,000. The code essentially tests the speed of integer math operations on the CPU.

## 2 FLOPS

### 2.1 Benchmark Result

### 2.1.1 Command:

```shell
./flops64 8
```

### 2.1.2 Result:

```shell
Benchmarking for 64 Bit Floating point operations per second
1| Tr 1: 2226363828 Tr 2: 2223968292 Tr 3: 2224483638 Tr 4: 2236111358 Tr 5: 2228431842 Tr 6: 2217805616 Tr 7: 2211725828 Tr 8: 2231984040 FLOPS = 17800874442
Maximum CPU Throughput: 17.800875 Gigaflops.
Maximum Single Core Throughput: 2.236111 Gigaflops.
```

### 2.1.3 Interpretation

This benchmark measured the 64-bit floating-point operations per second (FLOPS) on an 8-core system, demonstrating a total CPU throughput of approximately 17.80 Gigaflops. Individual core performance fluctuated slightly, with the maximum single-core throughput reaching about 2.24 Gigaflops. These results provide insight into the system's ability to handle floating-point computations, showcasing both the aggregate performance across all cores and the peak performance of a single core.

### 2.2 Benchmark Explanation

#### 2.2.1. Floating-Point Calculation Thread (calculateFLOPS64) - Detailed Logic:

- **Initialization of Operation Counters:**
  unsigned long long noOfOperations = 0;: This variable is initialized to zero and will accumulate the total count of floating-point operations performed by the thread. Unsigned long long loopOperations = 30 * 52;: This variable is set to a constant value, representing the number of floating-point operations implicitly performed within each iteration of the main while loop. The constant values are used to set the amount of math done per loop.
- **Initialization of Floating-Point Variables:**
  A large number of double variables (a, b, c, d, e, etc.) are initialized with specific floating-point values. These values are used as operands in the floating-point calculations.
- **Infinite Calculation Loop (while (1)):**
  The thread enters an infinite loop, continuously performing the floating-point calculations. Complex Floating-Point Expression: The core of the benchmark is a very long and complex floating-point expression that involves numerous multiplications and additions. This expression is designed to heavily utilize the CPU's floating-point unit. Repeated Calculation: The complex expression is repeated multiple times within the loop to increase the workload and stress the CPU.
- **Operation Count Update:**
  *((unsigned long long *)arg) = noOfOperations += loopOperations;: This line is crucial for reporting the progress. noOfOperations += loopOperations;: The loopOperations value is added to noOfOperations, effectively counting the floating-point operations performed in the current loop iteration.
  *((unsigned long long *)arg) = ...;: The updated noOfOperations value is then cast to an unsigned long long pointer and stored in the memory location pointed to by the arg parameter. This arg pointer was passed to the thread when it was created, and it points to a location within the tFlops array in the initFLOPS64 function. This allows the main thread to access the operation count from each worker thread.

#### 2.2.2. Benchmark Initialization and Reporting (initFLOPS64) - Detailed Logic:

- **Thread Creation:**
  pthread_t memThreads[POOL];: An array of pthread_t variables is declared to hold the thread IDs.
  unsigned long long tFlops[POOL];: An array is created to store the total floating-point operation counts from each thread.pthread_create(&memThreads[r], NULL, calculateFLOPS64, &tFlops[r]);: Inside a loop, POOL number of threads are created. Each thread is created to execute the calculateFLOPS64 function. The &tFlops[r] address is passed as the arg parameter to each thread. This is how each thread gets a unique memory location to store its operation count.
- **Measurement Period:**
  sleep(60);: The main thread pauses execution for 60 seconds, allowing the worker threads to perform their calculations. flopsLog[r][i] = tFlops[r];: After the sleep period, the main thread reads the operation counts from the tFlops array and stores them into the flopsLog array. This collects the data from each thread.
- **Thread Cancellation:**
  pthread_cancel(memThreads[i]);: The main thread cancels all worker threads to ensure they stop running after the measurement period.
- **Result Reporting:**
  The code then calculates the FLOPS for each core and the total FLOPS, and then the maximum single core and total FLOPS. The results are printed to the console and written to a file named "flopsBench.txt". The results are converted to Gigaflops before they are printed.
