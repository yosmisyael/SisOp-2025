Name: Misyael Yosevian Wiarda

NRP: 3124500039

Class: 1 D3 IT B

# Thread Program Analysis
Program: [print123thread.c](https://github.com/ferryastika/SisOp-24/blob/main/print123thread.c)
## 1. Include Headers:
```c
#include <stdio.h>
#include <pthread.h>
```
Baris pertama terdapat pustaka `stdio.h` untuk fungsi input/output standar seperti printf. Pada baris selanjutnya terdapat pustaka `pthread.h` yang diimpor untuk fungsi-fungsi terkait POSIX threads (pthreads), seperti membuat thread, mutex, dan condition variable.

## 2. Deklarasi Condition Variables:
```c
pthread_cond_t cond1 = PTHREAD_COND_INITIALIZER;
pthread_cond_t cond2 = PTHREAD_COND_INITIALIZER;
pthread_cond_t cond3 = PTHREAD_COND_INITIALIZER;
```
Condition variable adalah mekanisme sinkronisasi yang memungkinkan thread untuk menunggu (tidur) sampai suatu kondisi tertentu terpenuhi. Di sini, ada tiga condition variable, kemungkinan besar satu untuk setiap thread agar bisa menunggu gilirannya. `PTHREAD_COND_INITIALIZER` adalah cara statis untuk menginisialisasi condition variable.

## 3. Deklarasi Mutex:
```c
pthread_mutex_t lock = PTHREAD_MUTEX_INITIALIZER;
```
Mutex (Mutual Exclusion) adalah mekanisme locking yang digunakan untuk melindungi shared resource (sumber daya bersama) dari akses serentak oleh banyak thread (race condition). Hanya satu thread yang bisa "mengunci" (lock) mutex pada satu waktu. Thread lain yang mencoba mengunci mutex yang sama akan diblokir (menunggu) sampai thread yang memegang kunci membukanya (unlock). `PTHREAD_MUTEX_INITIALIZER` adalah cara statis untuk menginisialisasi mutex.

## 4. Variabel Global `done`:
```c
int done = 1;
```
Ini adalah variabel global yang berfungsi sebagai penanda atau flag untuk menentukan thread mana yang seharusnya berjalan (mencetak angka) selanjutnya. Nilai awalnya 1, menandakan thread yang bertugas mencetak angka 1 harus mulai lebih dulu. Variabel ini adalah shared resource yang dilindungi oleh lock.

## 5. Fungsi Thread foo:
```c
void *foo(void *n)
```
Ini adalah fungsi yang akan dijalankan oleh setiap thread yang dibuat. Fungsi ini menerima satu argumen `void *n`, yang dalam kasus ini akan berisi alamat dari angka integer (1, 2, atau 3) yang menjadi tanggung jawab thread tersebut.
```c
while(1): 
```
Loop tak terbatas agar thread terus mencoba mencetak angkanya secara berulang.
```c
pthread_mutex_lock(&lock);
```
Sebelum mengakses atau memodifikasi variabel done atau mencetak ke layar (yang juga bisa dianggap shared resource), thread harus mendapatkan kunci (lock) pada mutex. Ini memastikan hanya satu thread yang berada di dalam critical section ini pada satu waktu.
```c
if (done != (int)*(int*)n)
```
- `(int)*(int*)n`: Mengambil nilai integer dari pointer n. Pointer void *n di-cast menjadi pointer ke integer (int *), lalu di-dereference (*) untuk mendapatkan nilai integer-nya (1, 2, atau 3).
- Pengecekan ini bertanya: "Apakah giliran saat ini (done) tidak sama dengan nomor yang seharusnya dicetak oleh thread ini (n)?"

Jika Bukan Gilirannya:
- `pthread_cond_wait(&condX, &lock);`: Jika done tidak sesuai dengan n, artinya ini bukan giliran thread ini. Thread akan menunggu pada condition variable yang sesuai (cond1 jika n=1, cond2 jika n=2, cond3 jika n=3).
- Fungsi `pthread_cond_wait` secara atomik melakukan dua hal:
    1. Melepaskan (unlock) mutex (lock). Ini penting agar thread lain (yang gilirannya memang tiba) bisa masuk ke critical section.
    2. Membuat thread saat ini tidur (menunggu sinyal).

- Ketika thread lain memberikan sinyal pada condition variable ini, thread ini akan bangun dan secara otomatis mencoba mendapatkan kembali mutex (lock) sebelum melanjutkan eksekusi (kembali ke awal loop if).

Jika Memang Gilirannya:
- `printf("%d ", *(int*)n);`: Jika `done == n`, thread mencetak nomornya.
- Menjadwalkan Thread Berikutnya: Logika if-else if berikutnya menentukan giliran selanjutnya:
    - Jika done adalah 3, giliran berikutnya adalah 1 (done = 1;).
    - Jika done adalah 1, giliran berikutnya adalah 2 (done = 2;).
    - Jika done adalah 2, giliran berikutnya adalah 3 (done = 3;).
- `pthread_cond_signal(&condX);`: Memberikan sinyal (signal) ke condition variable yang sesuai dengan thread berikutnya. Ini akan membangunkan thread yang sedang menunggu di `pthread_cond_wait` pada condX tersebut. Misalnya, jika done baru saja diubah menjadi 2, maka `pthread_cond_signal(&cond2)` akan membangunkan thread yang bertanggung jawab mencetak angka 2.

- `pthread_mutex_unlock(&lock);`: Setelah selesai dengan critical section (mengecek done, mencetak, mengubah done, dan memberi sinyal), thread melepaskan kunci mutex, memungkinkan thread lain (biasanya yang baru saja diberi sinyal) untuk masuk.

## 6. Fungsi main:

```c
pthread_t tid1, tid2, tid3;
```
Mendeklarasikan variabel untuk menyimpan ID dari ketiga thread.
```c
int n1 = 1, n2 = 2, n3 = 3;
```
Variabel integer yang akan dilewatkan ke masing-masing thread.
```c
pthread_create(&tid1, NULL, foo, (void *)&n1);
```
Membuat thread pertama.
-` &tid1`: Alamat untuk menyimpan ID thread.
- `NULL`: Atribut thread (menggunakan default).
- `foo`: Fungsi yang akan dijalankan oleh thread.
- `(void *)&n1`: Argumen yang dilewatkan ke foo. Kita melewatkan alamat dari n1 dan meng-cast-nya ke `void *`.
Hal yang sama dilakukan untuk `tid2` dengan n2 dan `tid3` dengan n3.
```c 
while(1); 
```
Loop tak terbatas di main. Ini penting karena jika main selesai (keluar), seluruh proses akan berhenti, termasuk semua thread yang dibuatnya, bahkan jika mereka masih berjalan di while(1) mereka sendiri. Loop ini menjaga agar proses tetap hidup.

## Kesimpulan:

1. main membuat tiga thread, masing-masing ditugaskan angka 1, 2, dan 3.
2. Variabel `done` awalnya bernilai 1.
3. Semua thread memulai fungsi foo dan mencoba mendapatkan lock.
4. Thread 1 kemungkinan besar mendapatkan lock pertama, melihat `done == 1`, mencetak "1", mengubah done menjadi 2, memberi sinyal ke cond2, dan melepas lock.
5. Thread 2 (yang mungkin sedang menunggu di `pthread_cond_wait(&cond2, &lock)` atau baru mencoba mendapatkan lock) sekarang bisa mendapatkan lock, melihat `done == 2`, mencetak "2", mengubah done menjadi 3, memberi sinyal ke cond3, dan melepas lock.
6. Thread 3 mendapatkan lock, melihat `done == 3`, mencetak "3", mengubah done menjadi 1, memberi sinyal ke cond1, dan melepas lock.
7. Thread 1 mendapatkan lock lagi, melihat `done == 1`, mencetak "1", dan seterusnya.

Hasilnya adalah output `"1 2 3 1 2 3 1 2 3 ..."` yang dicetak ke layar secara terus-menerus, dengan urutan yang diatur oleh koordinasi antara mutex, condition variable, dan variabel `done`.
