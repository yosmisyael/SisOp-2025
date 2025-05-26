# Laporan Sistem Operasi: Algoritma Penjadwalan SJF tanpa Arrival Time Non-Preemptif
**Nama Anggota Kelompok:**
1. Misyael Yosevian Wiarda (3124500039)
2. Fahroldhi Sukirno (3124500046)
3. Erick Haidar Rahmat (3124500047)

Dosen Pengajar: Dr. Ferry Astika Saputra ST, M.Sc

---

## 1. Pendahuluan  
Pekerjaan Terpendek Pertama (SJF) adalah algoritma dimana proses yang memiliki waktu eksekusi terkecil dipilih untuk eksekusi berikutnya. Metode penjadwalan ini bisa bersifat preemptive atau non-preemptive. Ini secara signifikan mengurangi waktu tunggu rata-rata untuk proses lain yang menunggu eksekusi. Bentuk lengkap SJF adalah Pekerjaan Terpendek Pertama.
1. **SJF Non-Preemptif**.  
2. **SJF Preemptive**.  

---

## 2. Metrik Penjadwalan
- **Burst Time (BT)**: Waktu yang dibutuhkan proses untuk menyelesaikan eksekusi.
- **Completion Time (CT)**: Waktu proses selesai dieksekusi.  
- **Turnaround Time (TAT)**: Waktu total yang dihabiskan proses pada sistem, dihitung dengan `Completion Time (CT) - Arrival Time (AT)`.
- **Waiting Time (WT)**: Waktu tunggu total yang diperlukan proses dalam sistem, dihitung dengan`Turnaround Time (TAT) - Burst Time (BT)`.  
- **Response Time (RT)**: Waktu pertama proses mendapat CPU.  

---
## 3. Analisis Program yang Dipakai
Kode program C ini mengimplementasikan algoritma penjadwalan CPU Shortest Job First (SJF) non-preemptive yang mempertimbangkan waktu kedatangan (arrival time) setiap proses. Program ini menggunakan sebuah struktur data untuk menyimpan informasi tiap proses, meliputi nomor proses, waktu kedatangan, waktu burst (lama eksekusi), serta ruang untuk hasil perhitungan seperti waktu penyelesaian (completion time), waktu perputaran (turn-around time), dan waktu tunggu (waiting time).
Setelah membaca jumlah proses, program akan meminta input waktu kedatangan dan waktu burst untuk setiap proses. Langkah pertama adalah mengurutkan proses-proses berdasarkan waktu kedatangan secara menaik, memastikan proses yang datang lebih awal dipertimbangkan lebih dulu.
Selanjutnya, program mensimulasikan eksekusi proses satu per satu secara non-preemptive. Pada setiap waktu tertentu, program memilih proses dengan waktu burst terkecil dari kumpulan proses yang sudah tiba namun belum selesai. Jika tidak ada proses yang tersedia untuk dieksekusi pada waktu tersebut, waktu akan bertambah satu satuan hingga ada proses yang bisa dijalankan.
Ketika sebuah proses dipilih, waktu penyelesaian dihitung secara kumulatif dengan menambahkan waktu burst ke waktu saat ini. Kemudian, waktu perputaran dihitung sebagai selisih antara waktu penyelesaian dan waktu kedatangan, dan waktu tunggu dihitung sebagai selisih antara waktu perputaran dan waktu burst.
Setelah semua proses selesai dieksekusi, program menghitung rata-rata waktu perputaran dan rata-rata waktu tunggu dari seluruh proses. Terakhir, hasil perhitungan untuk setiap proses ditampilkan dalam bentuk tabel yang mencakup waktu kedatangan, burst time, completion time, turn-around time, dan waiting time, disertai dengan rata-rata waktu perputaran dan waktu tunggu di bagian akhir.

### 3.1 Program SJF tanpa Arrival Time (
```c
#include<stdio.h>
struct proc
{
    int no,bt,ct,tat,wt;
};
struct proc read(int i)
{
    struct proc p;
    printf("\nProcess No: %d\n",i);
    p.no=i;
    printf("Enter Burst Time: ");
    scanf("%d",&p.bt);
    return p;
}
int main()
{
    struct proc p[10],tmp;
    float avgtat=0,avgwt=0;
    int n,ct=0;
    printf("<--SJF Scheduling Algorithm Without Arrival Time (Non-Preemptive)-->\n");
    printf("Enter Number of Processes: ");
    scanf("%d",&n);
    for(int i=0;i<n;i++)
        p[i]=read(i+1);
    for(int i=0;i<n-1;i++)
        for(int j=0;j<n-i-1;j++)    
            if(p[j].bt>p[j+1].bt)
            {
				tmp=p[j];
				p[j]=p[j+1];
				p[j+1]=tmp;
            }
    printf("\nProcessNo\tBT\tCT\tTAT\tWT\tRT\n");
    for(int i=0;i<n;i++)
    {
        ct+=p[i].bt;
		p[i].ct=p[i].tat=ct;
		avgtat+=p[i].tat;
        p[i].wt=p[i].tat-p[i].bt;
        avgwt+=p[i].wt;
        printf("P%d\t\t%d\t%d\t%d\t%d\t%d\n",p[i].no,p[i].bt,p[i].ct,p[i].tat,p[i].wt,p[i].wt);
    }
    avgtat/=n,avgwt/=n;
    printf("\nAverage TurnAroundTime=%f\nAverage WaitingTime=%f",avgtat,avgwt);
}
```
Kode program C ini berfungsi untuk mendemonstrasikan dan menghitung metrik kinerja dari algoritma penjadwalan CPU Shortest Job First (SJF) non-preemptive dalam skenario di mana waktu kedatangan proses tidak diperhitungkan atau dianggap seragam (misalnya, semua proses tiba bersamaan pada waktu 0). SJF non-preemptive adalah algoritma penjadwalan di mana proses dengan waktu burst (lama eksekusi) paling kecil dieksekusi terlebih dahulu, dan proses yang sudah mulai dieksekusi tidak akan diganggu hingga selesai.

Program dimulai dengan mendefinisikan sebuah struktur data untuk merepresentasikan setiap proses. Struktur ini menyimpan informasi seperti ID proses, waktu burst, serta tempat untuk menyimpan hasil perhitungan metrik kinerja, seperti waktu penyelesaian (Completion Time), waktu perputaran (Turn-Around Time), dan waktu tunggu (Waiting Time).

Setelah pengguna memasukkan jumlah proses yang akan dijadwalkan, program akan meminta input waktu burst untuk masing-masing proses. Karena waktu kedatangan tidak menjadi pertimbangan, input hanya difokuskan pada burst time. Proses-proses ini kemudian disimpan dalam sebuah array.

Langkah penting dalam implementasi SJF adalah melakukan pengurutan (sorting) proses-proses berdasarkan nilai burst time secara menaik. Hal ini dilakukan untuk memastikan proses dengan burst time terkecil akan dieksekusi lebih dahulu, sesuai dengan prinsip algoritma SJF.

Setelah proses diurutkan, program mensimulasikan eksekusi CPU secara sekuensial. Sebuah variabel pelacak waktu digunakan untuk menghitung waktu penyelesaian kumulatif dari setiap proses. Untuk setiap proses, waktu penyelesaian dihitung dengan menambahkan waktu burst proses tersebut ke total waktu sebelumnya. Karena waktu kedatangan dianggap 0 untuk semua proses, maka waktu perputaran (Turn-Around Time) adalah sama dengan waktu penyelesaian. Selanjutnya, waktu tunggu (Waiting Time) dihitung dengan mengurangkan burst time dari waktu perputaran.

Response Time dalam kasus ini juga sama dengan waktu tunggu, karena tidak ada penundaan atau interupsi sejak proses masuk ke antrian hingga dieksekusi (karena waktu kedatangan sama dan proses langsung dijadwalkan sesuai urutan burst time terkecil).

Setelah seluruh proses dieksekusi dan metrik dihitung, program menghitung rata-rata waktu perputaran dan rata-rata waktu tunggu dari semua proses. Hasil akhir ditampilkan dalam bentuk tabel yang mencakup burst time, completion time, turn-around time, dan waiting time untuk masing-masing proses, disertai nilai rata-rata dari metrik utama tersebut.

## 4. Analisis Skenario  

### 4.1 SJF Tanpa Arrival Time  
**Input:** **4 Proses** dengan *Burst Time* (semua tiba pada waktu `0`):

| Process | Burst Time |  
|---------|------------|  
| P1      | 7          |  
| P2      | 4          |  
| P3      | 1          |  
| P4      | 4          |  


**Hasil Eksekusi:**

![image](https://github.com/user-attachments/assets/7814645a-dfb0-49aa-b0b3-0674c105cc26)

| ProcessNo | BT | CT  | TAT | WT | RT |  
|-----------|----|-----|-----|----|----|  
| P3        | 1  | 1   | 1   | 0  | 0  |  
| P2        | 4  | 5   | 5   | 1  | 1  |  
| P4        | 4  | 9   | 9   | 5  | 5  |  
| P1        | 7  | 16  | 16  | 9  | 9  |  

**Perhitungan Rata-Rata:**  
- **Turnaround Time (TAT):**  7.75 seconds
- **Waiting Time (WT):**  3.75 seconds

**Gantt Chart:**  

![baru](https://private-user-images.githubusercontent.com/134139485/445097109-c7c84aee-df63-4fc1-b5dd-0f02d42865f8.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDc2NjM0OTMsIm5iZiI6MTc0NzY2MzE5MywicGF0aCI6Ii8xMzQxMzk0ODUvNDQ1MDk3MTA5LWM3Yzg0YWVlLWRmNjMtNGZjMS1iNWRkLTBmMDJkNDI4NjVmOC5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwNTE5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDUxOVQxMzU5NTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT00NGQwOWEzY2FmN2U4OTI5NWMyNjVlYTgwNWVjY2M0NjFlMzcxMWY5NDM3YWQ1YmFmZmYzN2MyNzdiYTQzZDZjJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.5JLhXT9N551uxOyiDjdy6PGmbF87IUucse6he-R_Uc0)

---

## 6. Kesimpulan  

1. **SJF Non-Preemptive tanpa Arrival Time:**:  
 - Lebih efisien dibanding FCFS dalam hal waktu tunggu dan waktu perputaran rata-rata.
 - Namun, kurang adil terhadap proses dengan burst time besar, karena dapat terus tertunda.
 - Tidak cocok untuk sistem yang membutuhkan prioritas real-time atau keadilan antar proses.

2. **Rekomendasi**:  
 - SJF Non-Preemptive cocok untuk sistem batch atau beban kerja yang diketahui sebelumnya.  
 - Tidak direkomendasikan untuk sistem interaktif atau dengan arrival time yang bervariasi, karena bisa menyebabkan starvation pada proses yang lama  .  

---  
