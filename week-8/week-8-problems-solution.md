Nama: Misyael Yosevian Wiarda

NRP: 3124500039

Kelas: 1 D3 IT B

Dosen Pengajar: Dr. Ferry Astika Saputra ST, M.Sc

--- 
# Tugas Pertemuan Minggu ke-8
## 1. Jelaskan dalam 2 Paragraf Disertai dengan Gambar Tentang Konsep Single Thread dan Multithread!

### Solusi:
#### Single Thread
Konsep single-thread mengacu pada model eksekusi di mana suatu proses atau aplikasi hanya menjalankan satu urutan instruksi (disebut thread) pada satu waktu. Tugas-tugas diproses secara berurutan, satu demi satu. Jika satu tugas sedang aktif atau dalam kondisi menunggu (misalnya, menunggu operasi input/output selesai), maka tugas-tugas lain dalam antrean harus menunggu hingga tugas tersebut selesai atau memberikan kesempatan. Analogi sederhananya adalah seperti loket kasir tunggal yang melayani antrean pelanggan; setiap pelanggan harus menunggu giliran hingga pelanggan di depannya selesai dilayani sepenuhnya. Model ini lebih mudah dikelola dari segi pemrograman tetapi bisa menjadi kurang efisien dan responsif jika ada tugas yang memakan waktu lama atau terblokir, karena akan menahan seluruh alur eksekusi.

#### Multi Thread
Sebaliknya, konsep multi-thread memungkinkan sebuah proses atau aplikasi untuk menjalankan beberapa thread secara bersamaan (atau setidaknya secara konkuren, bergantian dalam irisan waktu yang sangat cepat pada prosesor single-core, atau benar-benar paralel pada prosesor multi-core). Dengan multi-threading, tugas besar dapat dipecah menjadi sub-tugas yang lebih kecil yang masing-masing dijalankan oleh thread terpisah. Jika satu thread terhenti karena menunggu sesuatu (seperti mengunduh data), thread lain dapat terus berjalan mengerjakan bagian tugas lainnya. Ini diibaratkan seperti memiliki beberapa loket kasir yang melayani pelanggan secara bersamaan, sehingga keseluruhan proses pelayanan menjadi lebih cepat dan efisien. Pendekatan ini meningkatkan kinerja dan responsivitas aplikasi, terutama untuk tugas-tugas yang melibatkan banyak operasi I/O atau komputasi paralel, meskipun kompleksitas pemrogramannya lebih tinggi karena perlu mengelola sinkronisasi antar-thread.
#### Visualisasi
- Single Thread

![Single Thread Visualization](https://raw.githubusercontent.com/yosmisyael/SisOp-2025/refs/heads/main/embeds/single-thread.jpg)
- Multi Thread

![Multi Thread Visualization](https://raw.githubusercontent.com/yosmisyael/SisOp-2025/refs/heads/main/embeds/multi-thread.jpg)
#### Kesimpulan
| Fitur                       | Visualisasi Single-Thread (`single-thread.jpg`)                                                                 | Visualisasi Multi-Thread (`multi-thread.jpg`)                                                                                                      |
| :-------------------------- | :-------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Jumlah Proses** | 1 (Satu Proses)                                                                                                 | 1 (Satu Proses)                                                                                                                                    |
| **Sumber Daya Bersama** | `code`, `data`, `files`<br>(Dimiliki oleh proses)                                                                | `code`, `data`, `files`<br>(Dimiliki oleh proses, digunakan bersama oleh semua *thread*)                                                            |
| **Sumber Daya Spesifik Thread** | **Hanya 1 set** yang terdiri dari:<br>- `registers`<br>- `stack`<br>- `PC`                                      | **Banyak set** (gambar menunjukkan 3 set), masing-masing terdiri dari:<br>- `registers`<br>- `stack`<br>- `PC`<br>(Satu set unik untuk setiap *thread*) |
| **Jumlah Alur Eksekusi** | **1** (Digambarkan dengan satu garis bergelombang)                                                              | **Banyak** (Digambarkan dengan 3 garis bergelombang), satu alur untuk setiap *thread* |
| **Model Eksekusi Tersirat** | Sekuensial (Sequential)                                                                                         | Konkuren / Paralel (Concurrent / Parallel)                                                                                                       |
| **Label Visualisasi** | `single-threaded process`                                                                                       | `multithreaded process`                                                                                                                            |
---

## 2. Kerjakan Programming Exercise a. Penerapan thread pada contoh SumTask.java b. penerapan Thread di Linux (thrd-posix.c) dan penerapan thread di Microsoft Windows (thrd-win32.c) . Beri penjelasan dalam bentuk esay. Gunakan Link https://github.com/ferryastika/osc10e/tree/master/ch4

### Solusi:
### a. Penerapan Thread pada `SumTask.java`
Kode program `Sumtask.java`:
```java
/**
 * Fork/join parallelism in Java
 *
 * Figure 4.18
 *
 * @author Gagne, Galvin, Silberschatz
 * Operating System Concepts  - Tenth Edition
 * Copyright John Wiley & Sons - 2018
 */

import java.util.concurrent.*;

public class SumTask extends RecursiveTask<Integer>
{
    static final int SIZE = 10000;
    static final int THRESHOLD = 1000;

    private int begin;
    private int end;
    private int[] array;

    public SumTask(int begin, int end, int[] array) {
        this.begin = begin;
        this.end = end;
        this.array = array;
    }

    protected Integer compute() {
        if (end - begin < THRESHOLD) {
            // conquer stage 
            int sum = 0;
            for (int i = begin; i <= end; i++)
                sum += array[i];

            return sum;
        }
        else {
            // divide stage 
            int mid = begin + (end - begin) / 2;
            
            SumTask leftTask = new SumTask(begin, mid, array);
            SumTask rightTask = new SumTask(mid + 1, end, array);

            leftTask.fork();
            rightTask.fork();

            return rightTask.join() + leftTask.join();
        }
    }

	public static void main(String[] args) {
		ForkJoinPool pool = new ForkJoinPool();
		int[] array = new int[SIZE];

		// create SIZE random integers between 0 and 9
		java.util.Random rand = new java.util.Random();

		for (int i = 0; i < SIZE; i++) {
			array[i] = rand.nextInt(10);
		}		
		
		// use fork-join parallelism to sum the array
		SumTask task = new SumTask(0, SIZE-1, array);

		int sum = pool.invoke(task);

		System.out.println("The sum is " + sum);
	}
}
```
#### Tujuan Program
Kode ini mendemonstrasikan penggunaan Fork/Join Framework di Java untuk melakukan perhitungan jumlah (sum) elemen dalam sebuah array secara paralel. Kerangka kerja Fork/Join dirancang untuk mempercepat pekerjaan yang dapat dipecah menjadi sub-tugas yang lebih kecil (divide and conquer) dan dijalankan secara paralel, terutama pada sistem dengan prosesor multi-core. Berikut ini adalah hasil output ketika kode tersebut dijalankan.

Kelas SumTask mewarisi RecursiveTask, yang dirancang untuk tugas yang dapat dipecah dan menghasilkan nilai kembalian (dalam hal ini, jumlah integer). Metode compute() adalah inti logikanya: ia memeriksa apakah rentang array yang ditangani lebih kecil dari THRESHOLD; jika ya, penjumlahan dilakukan secara sekuensial (tahap conquer). Jika tidak, tugas dibagi menjadi dua sub-tugas (leftTask dan rightTask), masing-masing dijadwalkan untuk eksekusi asinkron menggunakan fork(), dan hasilnya kemudian ditunggu serta digabungkan menggunakan join() (tahap divide dan join). Metode main() bertugas menginisialisasi array berukuran 10.000 elemen dengan angka acak (0-9), membuat ForkJoinPool sebagai pengelola thread, membuat tugas SumTask awal untuk seluruh array, dan memulai keseluruhan proses komputasi paralel dengan pool.invoke().
#### Output:
![SumTask Output](https://raw.githubusercontent.com/yosmisyael/SisOp-2025/refs/heads/main/embeds/output_sumtask.jpg)
Output yang terlihat pada gambar (output_sumtask.jpg) menampilkan hasil eksekusi program tersebut dari sebuah terminal. Perintah java SumTask digunakan untuk menjalankan program Java yang telah dikompilasi. Baris berikutnya, The sum is 44373, adalah keluaran standar (standard output) yang dicetak oleh pernyataan System.out.println("The sum is " + sum); di akhir metode main(). Angka 44373 ini merepresentasikan hasil akhir dari proses penjumlahan paralel seluruh elemen array (yang berjumlah 10.000 dan berisi angka acak antara 0 dan 9) yang berhasil dihitung oleh Fork/Join framework pada saat eksekusi tersebut. Karena array diisi dengan angka acak, menjalankan program ini lagi kemungkinan besar akan menghasilkan nilai jumlah yang berbeda.


### b. Penerapan Thread di Linux
Kode Program thrd-posix.c:
```c
/**
 * A pthread program illustrating how to
 * create a simple thread and some of the pthread API
 * This program implements the summation function where
 * the summation operation is run as a separate thread.
 *
 * Most Unix/Linux/OS X users
 * gcc thrd.c -lpthread
 *
 * Figure 4.11
 *
 * @author Gagne, Galvin, Silberschatz
 * Operating System Concepts  - Tenth Edition
 * Copyright John Wiley & Sons - 2018
 */

#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>

int sum; /* this data is shared by the thread(s) */

void *runner(void *param); /* the thread */

int main(int argc, char *argv[])
{
pthread_t tid; /* the thread identifier */
pthread_attr_t attr; /* set of attributes for the thread */

if (argc != 2) {
	fprintf(stderr,"usage: a.out <integer value>\n");
	/*exit(1);*/
	return -1;
}

if (atoi(argv[1]) < 0) {
	fprintf(stderr,"Argument %d must be non-negative\n",atoi(argv[1]));
	/*exit(1);*/
	return -1;
}

/* get the default attributes */
pthread_attr_init(&attr);

/* create the thread */
pthread_create(&tid,&attr,runner,argv[1]);

/* now wait for the thread to exit */
pthread_join(tid,NULL);

printf("sum = %d\n",sum);
}

/**
 * The thread will begin control in this function
 */
void *runner(void *param) 
{
int i, upper = atoi(param);
sum = 0;

	if (upper > 0) {
		for (i = 1; i <= upper; i++)
			sum += i;
	}

	pthread_exit(0);
}
```
#### Tujuan Program
Kode C ini bertujuan untuk menghitung jumlah deret bilangan asli dari 1 hingga N, di mana N adalah angka yang diberikan oleh pengguna saat program dijalankan. Keunikan program ini adalah proses penjumlahan tersebut tidak dilakukan langsung di alur utama program (main), melainkan didelegasikan ke sebuah thread terpisah yang dibuat menggunakan Pthreads API. Fungsi main pertama-tama memeriksa apakah pengguna memberikan tepat satu argumen angka non-negatif. Jika valid, ia menginisialisasi atribut thread dengan nilai default (pthread_attr_init) dan kemudian menciptakan thread baru (pthread_create). Thread baru ini akan menjalankan fungsi runner, dan nilai N (dalam bentuk string argv[1]) diteruskan sebagai parameter ke fungsi tersebut. Setelah membuat thread, fungsi main akan menunggu (pthread_join) sampai thread runner selesai menjalankan tugasnya. Sementara itu, fungsi runner menerima parameter N, mengubahnya menjadi integer, lalu menghitung jumlah dari 1 hingga N. Hasil penjumlahan disimpan dalam variabel global sum, yang bisa diakses baik oleh runner maupun main. Setelah selesai menghitung, runner keluar (pthread_exit). Ketika pthread_join di main selesai (menandakan runner sudah tuntas), main akan mencetak nilai akhir yang tersimpan di variabel global sum.
#### Output
![Posix Thread Output](https://raw.githubusercontent.com/yosmisyael/SisOp-2025/refs/heads/main/embeds/output_thrdposix.jpg)
Gambar output (output_thrdposix.jpg) menunjukkan interaksi pengguna dengan program ini di terminal. Pertama, dilakukan proses kompilasi program dengan perintah `gcc thrd-posix.c -o thrdposix`. Setelah program terkompilasi, selanjutnya program dijalankan dengan perintah `./thrdposix 13`, yang berarti menjalankan program thrdposix yang baru saja dibuat, dengan memberikan angka 13 sebagai argumen baris perintah (command-line argument). Angka 13 ini diterima oleh fungsi main sebagai argv[1] dan diteruskan ke thread runner sebagai batas atas penjumlahan (N=13). Output baris terakhir, sum = 91, adalah output yang dicetak oleh fungsi main setelah thread runner menyelesaikan perhitungannya dan main selesai menunggu. Nilai 91 ini adalah hasil penjumlahan bilangan dari 1 hingga 13 (1 + 2 + 3 + ... + 13 = 91), yang membuktikan bahwa thread runner berhasil melakukan kalkulasi dan menyimpannya di variabel global sum sebelum main mencetaknya.

### c. Penerapan Thread di Windows
Kode Program thrd_win32.c
```c
/**
 * This program creates a separate thread using the CreateThread() system call.
 *
 * Figure 4.13
 *
 * @author Gagne, Galvin, Silberschatz
 * Operating System Concepts  - Tenth Edition
 * Copyright John Wiley & Sons - 2018
 */

#include <stdio.h>
#include <windows.h>


DWORD Sum; /* data is shared by the thread(s) */

/* the thread runs in this separate function */
DWORD WINAPI Summation(PVOID Param)
{
	DWORD Upper = *(DWORD *)Param;

	for (DWORD i = 0; i <= Upper; i++)
		Sum += i;



	return 0;
}


int main(int argc, char *argv[])
{
	DWORD ThreadId;
	HANDLE ThreadHandle;
	int Param;

	// do some basic error checking
	if (argc != 2) {
		fprintf(stderr,"An integer parameter is required\n");
		return -1;
	}

	Param = atoi(argv[1]);

	if (Param < 0) {
		fprintf(stderr, "an integer >= 0 is required \n");
		return -1;
	}

	// create the thread
	ThreadHandle = CreateThread(NULL, 0, Summation, &Param, 0, &ThreadId);

	if (ThreadHandle != NULL) {
		WaitForSingleObject(ThreadHandle, INFINITE);
		CloseHandle(ThreadHandle);
		printf("sum = %d\n",Sum);
	}
}
```
#### Tujuan Program
Tujuan utama dari program C ini adalah untuk mendemonstrasikan cara membuat dan mengelola thread terpisah dalam lingkungan sistem operasi Windows menggunakan Windows API. Secara spesifik, program ini menghitung jumlah total deret bilangan dari 0 hingga N (di mana N adalah angka yang diberikan pengguna melalui argumen baris perintah) dengan mendelegasikan tugas perhitungan tersebut ke thread sekunder, sementara thread utama menunggu hasilnya.

- Variabel Global Sum: Variabel DWORD Sum; dideklarasikan secara global agar nilainya dapat diakses dan dimodifikasi oleh kedua thread (utama dan sekunder). Ini berfungsi sebagai media komunikasi untuk hasil perhitungan.
- Fungsi Summation(PVOID Param): Ini adalah fungsi yang akan dieksekusi oleh thread sekunder. Tugasnya adalah:
	- Menerima parameter Param (yang merupakan alamat dari variabel N di main).
	- Mengambil nilai N dari alamat tersebut.
	- Melakukan loop untuk menjumlahkan semua bilangan bulat dari 0 hingga N.
	- Menyimpan hasil penjumlahan ke dalam variabel global Sum.
	- Mengembalikan nilai 0 untuk menandakan penyelesaian.
- Fungsi main(int argc, char *argv[]): Ini adalah thread utama dan titik masuk program. Tugasnya meliputi:
	- Memvalidasi input argumen baris perintah untuk memastikan pengguna memberikan satu angka N yang non-negatif.
	- Memanggil CreateThread(): Fungsi Windows API ini digunakan untuk membuat thread sekunder baru. CreateThread diberi tahu untuk menjalankan fungsi Summation, dan alamat dari variabel N (&Param) diteruskan sebagai argumen untuk Summation. Fungsi ini mengembalikan HANDLE (referensi) ke thread baru.
	- Memanggil WaitForSingleObject(): Setelah thread dibuat, thread utama menggunakan fungsi ini untuk berhenti sejenak dan menunggu hingga thread sekunder (Summation) selesai menjalankan tugasnya. INFINITE berarti menunggu tanpa batas waktu.
	- Memanggil CloseHandle(): Setelah thread sekunder selesai dan WaitForSingleObject kembali, fungsi ini dipanggil untuk membereskan dan melepaskan handle yang tadi digunakan untuk merujuk ke thread sekunder. Ini adalah praktik manajemen sumber daya yang baik.
	- Mencetak Hasil: Akhirnya, main mencetak nilai yang tersimpan di variabel global Sum ke layar.

#### Output
![output_winthread.jpg](https://raw.githubusercontent.com/yosmisyael/SisOp-2025/refs/heads/main/embeds/winthrd.PNG)

Gambar output (output_winthread.jpg) menunjukkan interaksi pengguna dengan program ini di terminal Windows (misalnya, Command Prompt atau PowerShell). Pertama, dilakukan proses kompilasi program C dengan perintah seperti gcc thrd-win32.c -o thrdwin.exe (menggunakan kompiler GCC) atau perintah serupa menggunakan kompiler lain seperti Microsoft Visual C++. Setelah program terkompilasi menjadi thrdwin.exe, selanjutnya program dijalankan dengan perintah ./thrdwin.exe 10, yang berarti menjalankan program nama_program.exe yang baru saja dibuat, dengan memberikan angka 10 sebagai argumen baris perintah. Angka 10 ini diterima oleh fungsi main sebagai argv[1], dikonversi menjadi integer dan disimpan dalam variabel Param. Alamat dari Param (&Param) kemudian diteruskan melalui CreateThread ke thread sekunder yang menjalankan fungsi Summation sebagai batas atas penjumlahan (N=10). Output baris terakhir, sum = 55, adalah output yang dicetak oleh fungsi main setelah thread Summation menyelesaikan perhitungannya (menyimpan hasil di variabel global Sum) dan main selesai menunggu thread tersebut menggunakan WaitForSingleObject. Nilai 55 ini adalah hasil penjumlahan bilangan dari 0 hingga 10 (0 + 1 + 2 + ... + 10 = 55), yang membuktikan bahwa thread Summation berhasil melakukan kalkulasi dan menyimpannya di variabel global Sum sebelum main mencetaknya.

---
## 3. Buat PPT Tentang Evolusi Teknologi *Processor* Intel dengan Menggunakan Referensi: *https://www.youtube.com/watch?v=PT787d9odKk*
### Solusi
Berikut ini adalah tautan ke file PPT tentang evolusi teknologi processor Intel berdasarkan referensi [video](https://www.youtube.com/watch?v=PT787d9odKk) dengan tambahan dari sumber artikel [Wikipedia](https://en.wikipedia.org/wiki/List_of_Intel_processors): 
1. [Evolusi Teknologi Processor Intel (format PPTX)](https://docs.google.com/presentation/d/1WdZte9zvL5qwzWQ9e5kj1_Nxja5JZc4g/edit?usp=drive_link&ouid=110405865782530381135&rtpof=true&sd=true)
2. [Evolusi Teknologi Processor Intel (format PDF)](https://drive.google.com/file/d/1rj2mr4z1aebNDD_W9Dw8C6D_szDxQ8Jb/view?usp=drive_link)

**NB**: Pada format PPTX terdapat mislokasi desain karena konversi ke format PPTX, versi lebih baik berformat PDF

---

## 4. Jawab Pertanyaan Exercise pada Chapter 4! (Practice Exercise)
### Solusi
### 4.1 Multithreading Example
**Question:** Provide three programming examples in which multithreading provides better performance than a single-threaded solution.

**Answer:**

1. Concurrent Request Handling: Web servers, for instance, can significantly improve their capacity and user experience by using a separate thread to handle each client connection or request. This allows the server to manage multiple users simultaneously, preventing one lengthy request from blocking others.
2. Parallel Task Execution: Applications performing intensive calculations that can be broken down, such as large-scale matrix multiplication or scientific simulations, gain performance from multithreading. Different threads can work on distinct parts of the problem concurrently, often on different processor cores, drastically reducing the total computation time compared to a sequential approach.
3. Responsive User Interfaces: Graphical applications, including integrated development environments (IDEs) or even simple text editors performing background spell-checks, utilize multithreading to maintain responsiveness. One thread can manage user interactions (like button clicks or typing), ensuring the interface doesn't freeze, while other threads perform potentially time-consuming tasks like computation, data processing, or background monitoring.

### 4.2 Speed Calculation with Amdahl's Law
**Question:** Using Amdahl’s Law, calculate the speedup gain of an application that has a 60 percent parallel component for (a) two processing cores and (b) four processing cores.

**Answer:**

Amdahl's Law is used to calculate the theoretical maximum speedup achievable by parallelizing a portion of a task. The formula is:
$$
S = \frac{1}{(1 - P) + \frac{P}{N}}
$$

Where:
* $S$ is the speedup factor.
* $P$ is the proportion of the application that can be parallelized.
* $(1 - P)$ is the proportion of the application that must remain serial.
* $N$ is the number of processing cores.

In this case, we are given:
* Parallel component ($P$) = 60% = 0.60
* Serial component ($1 - P$) = 1 - 0.60 = 0.40

Now let's calculate the speedup for each case:

**a. For two processing cores (N = 2):**

* Substitute the values into the formula:
    $`S = \frac{1}{(1 - 0.60) + \frac{0.60}{2}}`$
* Perform the calculation:
    $`S = \frac{1}{0.40 + 0.30}`$
    $`S = \frac{1}{0.70}`$
    $`S \approx 1.4286`$
* So, with two processing cores, we get a speedup of approximately **1.42 times**.

**b. For four processing cores (N = 4):**

* Substitute the values into the formula:
    $`S = \frac{1}{(1 - 0.60) + \frac{0.60}{4}}`$
* Perform the calculation:
    $`S = \frac{1}{0.40 + 0.15}`$
    $`S = \frac{1}{0.55}`$
    $`S \approx 1.8182`$
* So, with four processing cores, we get a speedup of approximately **1.82 times**.

### 4.3 Web Server Parellelism Classification
**Question:** Does the multithreaded web server described in Section 4.1 exhibit task or data parallelism?

**Answer:** Data parallelism. The key is that while all threads perform the identical task of processing a web request, they do so on separate and independent data – the unique requests they each receive.

### 4.4 User Thread and Kernel Thread Different
**Question:** What are two differences between user-level threads and kernel-level threads? Under what circumstances is one type better than the other?

**Answer:**

a.  The operating system kernel is unaware of user-level threads, while it has knowledge of and manages kernel-level threads.

b.  In system configurations where multiple user threads map to fewer kernel entities (many-to-one or many-to-many models), a thread library handles the scheduling of user threads. In contrast, the kernel is directly responsible for scheduling kernel-level threads.

c.  Unlike user threads, which are always tied to a specific process, kernel threads can exist independently. Generally, kernel threads require more system overhead to manage than user threads because they necessitate corresponding data structures within the kernel.

### 4.5 Context Switching of Kernel-Level Threads
**Question:** Describe the actions taken by a kernel to context-switch between kernellevel threads.

**Answer:** To perform a context switch between kernel threads, the kernel usually takes two main steps: first, it preserves the state of the CPU by saving the contents of its registers for the thread that is being switched out; second, it loads the previously saved register contents for the thread that is being switched in, allowing it to resume execution from where it left off.

### 4.6 Thread Creation Resource Usage
**Question:** What resources are used when a thread is created? How do they differ from those used when a process is created?

**Answer:** Creating a thread requires fewer resources than creating a process, reflecting the smaller nature of a thread. Process creation necessitates the allocation of a large data structure, the process control block (PCB), which contains essential information such as the memory map, open file descriptors, and environment settings. A significant portion of the time spent creating a process is typically dedicated to allocating and managing this memory map. Conversely, creating either type of thread, user or kernel, only requires the allocation of a compact data structure to hold its register set, stack, and priority.


### 4.7 Case Study
**Question:**  Assume that an operating system maps user-level threads to the kernel using the many-to-many model and that the mapping is done through LWPs. Furthermore, the system allows developers to create real-time threads for use in real-time systems. Is it necessary to bind a real-time thread to an LWP? Explain.

**Answer:** Yes, it is necessary to bind a real-time thread to a Lightweight Process (LWP) in this scenario. This is because precise timing is critical for real-time applications. If a thread designated as real-time is not bound to an LWP, it faces the risk of experiencing delays while waiting to be assigned an LWP before it can begin execution.

Consider a situation where a real-time thread is actively running, attached to an LWP, but then becomes blocked (for instance, due to performing I/O, being preempted by a higher-priority real-time thread, or waiting for a mutual exclusion lock). During this blocked state, the LWP it was using can be allocated to another thread. Consequently, when the real-time thread is subsequently scheduled to run again, it would first have to wait to be attached to an available LWP. By explicitly binding an LWP to a real-time thread, you guarantee that once the thread is scheduled by the system, it can commence execution with minimal, if any, delay.
