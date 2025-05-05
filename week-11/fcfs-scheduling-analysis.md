# Laporan Sistem Operasi: Algoritma Penjadwalan FCFS  
**Nama Anggota Kelompok:**
1. Misyael Yosevian Wiarda (3124500039)
2. Fahroldhi Sukirno (3124500046)
3. Erick Haidar Rahmat (3124500047)

---

## 1. Pendahuluan  
First-Come, First-Served (FCFS) adalah algoritma penjadwalan **non-preemptive**, yang artinya begitu suatu proses mulai dijalankan, proses tersebut berjalan hingga selesai tanpa gangguan (interupsi). Proses-proeses akan dijalankan berdasarkan urutan kedatangan pada ready queue. Laporan ini membahas dua skenario implementasi FCFS:  
1. **Dengan Waktu Kedatangan (Arrival Time)**.  
2. **Tanpa Waktu Kedatangan** (semua proses dianggap tiba pada waktu `0`).  

---

## 2. Metrik Penjadwalan
- **Arrival Time (AT)**: Waktu kedatangan proses pada ready queue.
- **Completion Time (CT)**: Waktu proses selesai dieksekusi.  
- **Turnaround Time (TAT)**: Waktu total yang dihabiskan proses pada sistem, dihitung dengan `Completion Time (CT) - Arrival Time (AT)`.
- **Waiting Time (WT)**: Waktu tunggu total yang diperlukan proses dalam sistem, dihitung dengan`Turnaround Time (TAT) - Burst Time (BT)`.  
- **Response Time (RT)**: Waktu pertama proses mendapat CPU.  

---
## 3. Analisis Program yang Dipakai
### 3.1 Program FCFS dengan Arrival Time
```c
#include<stdio.h>
struct proc
{
    int no,at,bt,ct,tat,wt;
};
struct proc read(int i)
{
    struct proc p;
    printf("\nProcess No: %d\n",i);
    p.no=i;
    printf("Enter Arrival Time: ");
    scanf("%d",&p.at);
    printf("Enter Burst Time: ");
    scanf("%d",&p.bt);
    return p;
}
int main()
{
    struct proc p[10],tmp;
    float avgtat=0,avgwt=0;
    int n,ct=0;
    printf("<--FCFS Scheduling Algorithm (Non-Preemptive)-->\n");
    printf("Enter Number of Processes: ");
    scanf("%d",&n);
    for(int i=0;i<n;i++)
        p[i]=read(i+1);
    for(int i=0;i<n-1;i++)
        for(int j=0;j<n-i-1;j++)    
            if(p[j].at>p[j+1].at)
            {
            tmp=p[j];
            p[j]=p[j+1];
            p[j+1]=tmp;
            }
    printf("\nProcessNo\tAT\tBT\tCT\tTAT\tWT\tRT\n");
    for(int i=0;i<n;i++)
    {
        ct+=p[i].bt;
		p[i].ct=ct;
        p[i].tat=p[i].ct-p[i].at;
        avgtat+=p[i].tat;
        p[i].wt=p[i].tat-p[i].bt;
        avgwt+=p[i].wt;
        printf("P%d\t\t%d\t%d\t%d\t%d\t%d\t%d\n",p[i].no,p[i].at,p[i].bt,p[i].ct,p[i].tat,p[i].wt,p[i].wt);
    }
    avgtat/=n,avgwt/=n;
    printf("\nAverage TurnAroundTime=%f\nAverage WaitingTime=%f",avgtat,avgwt);
}
```
Kode program C ini mengimplementasikan algoritma penjadwalan CPU First-Come, First-Served (FCFS) non-preemptive yang mempertimbangkan waktu kedatangan (arrival time) setiap proses. Program ini menggunakan sebuah struktur data untuk menyimpan informasi tiap proses, meliputi nomor proses, waktu kedatangan, waktu burst (lama eksekusi), serta ruang untuk hasil perhitungan seperti waktu penyelesaian, waktu perputaran, dan waktu tunggu. Setelah membaca jumlah proses, program akan meminta input waktu kedatangan dan waktu burst untuk setiap proses. Langkah kunci dalam implementasi ini adalah pengurutan (sorting) proses-proses berdasarkan waktu kedatangan secara menaik, memastikan proses yang tiba lebih awal diproses terlebih dahulu sesuai prinsip FCFS. Setelah terurut, program mensimulasikan eksekusi secara sekuensial, menghitung waktu penyelesaian (completion time) secara kumulatif berdasarkan waktu burst, lalu menghitung waktu perputaran (turn-around time) sebagai selisih antara waktu penyelesaian dan waktu kedatangan. Waktu tunggu (waiting time) kemudian dihitung sebagai selisih antara waktu perputaran dan waktu burst. Terakhir, program menghitung rata-rata waktu perputaran dan waktu tunggu dari semua proses, lalu menampilkan detail hasil perhitungan untuk setiap proses serta nilai rata-rata keseluruhan.
### 3.2 Program FCFS tanpa Arrival Time
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
    printf("<--FCFS Scheduling Algorithm Without Arrival Time (Non-Preemptive)-->\n");
    printf("Enter Number of Processes: ");
    scanf("%d",&n);
    for(int i=0;i<n;i++)
        p[i]=read(i+1);
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
Kode program C ini berfungsi untuk mendemonstrasikan dan menghitung metrik kinerja dari algoritma penjadwalan CPU First-Come, First-Served (FCFS) dalam skenario di mana waktu kedatangan (arrival time) proses tidak diperhitungkan atau dianggap seragam (misalnya, semua proses tiba secara bersamaan pada waktu 0). Algoritma FCFS adalah metode non-preemptive, artinya sekali proses mulai dieksekusi, ia akan berjalan sampai selesai tanpa interupsi.

Program dimulai dengan mendefinisikan sebuah struktur data bernama proc untuk merepresentasikan setiap proses. Struktur ini menyimpan nomor proses, waktu yang dibutuhkan CPU untuk mengeksekusinya (Burst Time), serta ruang untuk menyimpan hasil perhitungan seperti Waktu Penyelesaian (Completion Time), Waktu Perputaran (Turn-Around Time), dan Waktu Tunggu (Waiting Time).

Setelah itu, program akan meminta pengguna untuk memasukkan jumlah proses yang akan dijadwalkan. Untuk setiap proses, program akan meminta input Burst Time-nya saja, karena waktu kedatangan tidak menjadi faktor dalam perhitungan ini. Proses-proses ini kemudian disimpan dalam sebuah array.

Inti dari simulasi FCFS dalam kode ini terletak pada loop yang memproses proses-proses sesuai dengan urutan saat data mereka diinput. Karena tidak ada waktu kedatangan yang berbeda atau proses sorting berdasarkan waktu kedatangan, urutan input ini menentukan urutan eksekusi.

Dalam loop perhitungan, program mensimulasikan eksekusi CPU secara sekuensial. Sebuah variabel pelacak waktu penyelesaian (completion time tracker, diwakili oleh ct) digunakan. Untuk setiap proses, Completion Time-nya dihitung dengan menambahkan Burst Time proses tersebut ke nilai ct saat ini (yang merupakan waktu selesainya proses sebelumnya). Ini mencerminkan bahwa proses berikutnya langsung dimulai setelah proses sebelumnya selesai.

Karena diasumsikan waktu kedatangan adalah 0 untuk semua proses (atau tidak relevan karena urutan ditentukan oleh input), Waktu Perputaran (Turn-Around Time) untuk setiap proses dihitung sebagai Completion Time dikurangi 0, sehingga TAT sama dengan CT. Selanjutnya, Waktu Tunggu (Waiting Time) dihitung dengan mengurangi Burst Time dari Turn-Around Time (WT = TAT - BT), yang mencerminkan berapa lama proses menunggu di antrian sebelum mulai dieksekusi. Waktu Balas (Response Time) juga dicetak dan pada kasus ini nilainya sama dengan Waiting Time.

Setelah menghitung nilai-nilai ini untuk setiap proses, program menghitung rata-rata Waktu Perputaran dan Waktu Tunggu dari semua proses. Terakhir, program akan menampilkan tabel yang merangkum Burst Time, Completion Time, Turn-Around Time, dan Waiting Time untuk setiap proses, diikuti dengan nilai rata-rata Turn-Around Time dan Waiting Time keseluruhan.

Dengan demikian, kode ini secara efektif menggambarkan cara kerja FCFS dalam kasus paling sederhana di mana urutan eksekusi murni berdasarkan urutan penerimaan (atau ID) proses, dan menyediakan perhitungan metrik dasar untuk mengevaluasi kinerja penjadwal dalam skenario tersebut.

## 4. Analisis Skenario  

### 4.1 FCFS (Arrival Time)  
**Input:** **4 Proses** dengan *Arrival Time* dan *Burst Time*:  
  | Process | Arrival Time | Burst Time |  
  |---------|--------------|------------|  
  | P1      | 0            | 8          |  
  | P2      | 1            | 9          |  
  | P3      | 2            | 4          |  
  | P4      | 3            | 5          |  

**Hasil Eksekusi:**
![Hasil kalkulasi FCFS dengan *arrival time*](https://raw.githubusercontent.com/yosmisyael/SisOp-2025/refs/heads/main/embeds/fcfs.png)
| ProcessNo | AT | BT | CT  | TAT | WT | RT |  
|-----------|----|----|-----|-----|----|----|  
| P1        | 0  | 8  | 8   | 8   | 0  | 0  |  
| P2        | 1  | 9  | 17  | 16  | 7  | 7  |  
| P3        | 2  | 4  | 21  | 19  | 15 | 15 |  
| P4        | 3  | 5  | 26  | 23  | 18 | 18 |  

**Perhitungan Rata-Rata:** 
- **Turnaround Time (TAT):** 16.5 seconds
- **Waiting Time (WT):**  10.0 seconds


**Keterangan:**  
- Proses P1 langsung dieksekusi karena tiba pada waktu `0`.  
- Proses berikutnya (P2, P3, P4) harus menunggu hingga proses sebelumnya selesai, menyebabkan **waiting time tinggi** untuk proses yang tiba belakangan.  

---

### 4.2 FCFS Tanpa Arrival Time  
**Input:** **4 Proses** dengan *Burst Time* (semua tiba pada waktu `0`):

| Process | Burst Time |  
|---------|------------|  
| P1      | 8          |  
| P2      | 4          |  
| P3      | 9          |  
| P4      | 5          |  

**Hasil Eksekusi:**
![Hasil kalkulasi FCFS dengan *arrival time*](https://raw.githubusercontent.com/yosmisyael/SisOp-2025/refs/heads/main/embeds/fcfs-wo-arival.png)
| ProcessNo | BT | CT  | TAT | WT | RT |  
|-----------|----|-----|-----|----|----|  
| P1        | 8  | 8   | 8   | 0  | 0  |  
| P2        | 4  | 12  | 12  | 8  | 8  |  
| P3        | 9  | 21  | 21  | 12 | 12 |  
| P4        | 5  | 26  | 26  | 21 | 21 |  

**Perhitungan Rata-Rata:**  
- **Turnaround Time (TAT):**  16.75 seconds
- **Waiting Time (WT):**  10.25 seconds


**Keterangan:**  
- Urutan eksekusi murni berdasarkan antrian input.  
- **Convoy Effect**: Proses dengan *burst time* kecil (P2=4) terpaksa menunggu proses panjang sebelumnya (P1=8).  

---

## 5. Perbandingan Kedua Skenario  
| Parameter           | Dengan Arrival Time | Tanpa Arrival Time |  
|---------------------|---------------------|--------------------|  
| Rata-rata TAT       | 16.5               | 16.75             |  
| Rata-rata WT        | 10.0               | 10.25             |  
| Dampak Utama        | Proses belakangan tertunda | Convoy Effect      |  

---

## 6. Kesimpulan  
1. **FCFS dengan Arrival Time**:  
 - Lebih realistis untuk sistem dengan proses yang tiba secara asinkron.  
 - **Kelemahan**: Menyebabkan waiting time tinggi untuk proses yang tiba belakangan.  

2. **FCFS tanpa Arrival Time**:  
 - Sederhana, tetapi **tidak adil** untuk proses dengan burst time kecil.  
 - **Convoy Effect** mengurangi efisiensi sistem.  

3. **Rekomendasi**:  
 - FCFS cocok untuk sistem dengan beban ringan atau prioritas kesederhanaan.  
 - Tidak direkomendasikan untuk sistem yang membutuhkan respons cepat atau adil.  

---  
