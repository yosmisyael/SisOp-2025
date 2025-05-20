# Laporan Sistem Operasi: Algoritma Penjadwalan SJF Non-Preemptive dengan Arrival Time 

| Nama           | : | Misyael Yosevian Wiarda             |
| -------------- | - | ----------------------------------- |
| NRP            | : | 3124500039                          |
| Kelas          | : | 1 D3 IT B                           |
| Dosen Pengajar | : | Dr. Ferry Astika Saputra ST, M.Sc   |

---

## 1. Pendahuluan
Penjadwalan CPU merupakan salah satu komponen penting dalam sistem operasi modern yang bertanggung jawab mengatur alokasi sumber daya CPU kepada berbagai proses yang bersaing. Efisiensi penjadwalan secara signifikan memengaruhi kinerja keseluruhan sistem, termasuk waktu respons, throughput, dan utilisasi CPU. 

Shortest Job Firsts (SJF) sendiri merupakan salah satu algoritma penjadwalan yang bekerja dengan menjadwalkan proses yang memiliki waktu eksekusi terpendek terlebih dahulu. Pendekatan ini secara teoritis mampu memberikan waktu tunggu rata-rata minimum. Namun, dalam implementasi praktis, penentuan waktu eksekusi yang akurat di muka seringkali menjadi tantangan. Laporan ini akan menganalisis metode penjadwalan CPU SJF, khususnya dengan mempertimbangkan waktu kedatangan (arrival time) proses.
 
## 2. Metrik dalam Penjadwalan
- **Burst Time (BT)**: Waktu yang dibutuhkan proses untuk menyelesaikan eksekusi.
- **Completion Time (CT)**: Waktu proses selesai dieksekusi.  
- **Turnaround Time (TAT)**: Waktu total yang dihabiskan proses pada sistem, dihitung dengan `Completion Time (CT) - Arrival Time (AT)`.
- **Waiting Time (WT)**: Waktu tunggu total yang diperlukan proses dalam sistem, dihitung dengan`Turnaround Time (TAT) - Burst Time (BT)`.  
- **Response Time (RT)**: Waktu pertama proses mendapat CPU.  

## 3. Analisis Program yang Dipakai
### 3.1 Program Perhitungan Penjadwalan
```c
#include<stdio.h>
struct proc
{
    int no,at,bt,it,ct,tat,wt;
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
    int  n,j,min=0;
    float avgtat=0,avgwt=0;
    struct proc p[10],temp;
    printf("<--SJF Scheduling Algorithm (Non-Preemptive)-->\n");
    printf("Enter Number of Processes: ");
    scanf("%d",&n);
    for(int i=0;i<n;i++)
        p[i]=read(i+1);
    for(int i=0;i<n-1;i++)
        for(j=0;j<n-i-1;j++)    
            if(p[j].at>p[j+1].at)
            {
            temp=p[j];
            p[j]=p[j+1];
            p[j+1]=temp;
            }
    for(j=1;j<n&&p[j].at==p[0].at;j++)
        if(p[j].bt<p[min].bt)
            min=j;
    temp=p[0];
    p[0]=p[min];
    p[min]=temp;
    p[0].it=p[0].at;
    p[0].ct=p[0].it+p[0].bt;
    for(int i=1;i<n;i++)
    {
        for(j=i+1,min=i;j<n&&p[j].at<=p[i-1].ct;j++)
            if(p[j].bt<p[min].bt)
                min=j;
        temp=p[i];
        p[i]=p[min];
        p[min]=temp;
        if(p[i].at<=p[i-1].ct)
            p[i].it=p[i-1].ct;
        else
            p[i].it=p[i].at;
        p[i].ct=p[i].it+p[i].bt;
    }
    printf("\nProcess\t\tAT\tBT\tCT\tTAT\tWT\tRT\n");
    for(int i=0;i<n;i++)
    {
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
### 3.2 Penjelasan Kode
1. Struktur Data dan Masukan
    - struct proc: Menampung detail proses seperti waktu kedatangan (at), waktu burst (bt), waktu penyelesaian (ct), waktu penyelesaian (tat), waktu tunggu (wt), dll.
    - read () Fungsi: Mengambil input pengguna untuk waktu kedatangan dan waktu burst dari setiap proses.

2. Proses Penyortiran
    - Pengurutan Awal: Proses-proses diurutkan berdasarkan waktu kedatangan menggunakan pengurutan gelembung. Hal ini memastikan proses ditangani sesuai urutan kedatangannya.
    -Pemilihan Proses Pertama: Di antara proses-proses dengan waktu kedatangan paling awal, proses dengan waktu burst terpendek dipilih untuk dijalankan terlebih dahulu. Ini memastikan perilaku SJF di awal.

3. Logika Penjadwalan SJF
    Perhitungan Waktu Penyelesaian:
    - Proses pertama dimulai pada waktu kedatangannya dan berjalan tanpa gangguan.
        - Untuk proses selanjutnya, algoritme memeriksa:

            - Apakah proses telah tiba pada waktu penyelesaian proses sebelumnya (p[i].at <= p[i-1].ct). Jika ya, maka proses akan dimulai segera setelah proses sebelumnya.
            - Jika tidak, proses akan dimulai pada waktu kedatangannya sendiri.

    - Memilih Pekerjaan Terpendek: Untuk setiap proses baru, kode akan melihat ke depan untuk menemukan waktu burst terpendek di antara semua proses yang telah tiba pada waktu saat ini. Hal ini memastikan perilaku non-preemptive SJF.

4. Perhitungan Waktu
    - Waktu Penyelesaian (TAT): CT - AT (total waktu dari kedatangan hingga penyelesaian).

    - Waktu Tunggu (WT): TAT - BT (waktu yang dihabiskan untuk menunggu dalam antrian siap).

    - Waktu Tanggapan (RT): Sama dengan WT di sini, karena penjadwalan non-preemptive memulai proses hanya pada

## 4. Analisis Skenario Data Pengujian
Untuk menganalisis kinerja algoritma penjadwalan Shortest Job First (SJF) dengan mempertimbangkan waktu kedatangan (arrival time), data berikut akan digunakan dalam skenario pengujian kali ini:
**Input Data Beban Komputasi:**

| Proses | Arrival Time | Burst Time | 
| ------ | ------------ | ---------- |
| p1 | 0.0 | 7 |
| p2 | 2.0 | 4 |
| p3 | 4.0 | 1 |
| p4 | 5.0 | 4 |

## 5. Hasil dan Pembahasan 
![Hasil eksekusi program](https://raw.githubusercontent.com/yosmisyael/SisOp-2025/refs/heads/main/embeds/sjf-non-preemptive-at.jpg)

**Data Hasil Proses Penjadwalan:**

| ProcessNo | AT |BT | CT  | TAT | WT | RT |  
|-----------|----|----|-----|-----|----|----|  
| P1        | 0  | 7  | 7   | 7   | 0  | 0  |  
| P3        | 4  | 1  | 8   | 4   | 3  | 3  |  
| P2        | 2  | 4  | 12  | 10  | 6  | 6  |  
| P4        | 5  | 4  | 16  | 11  | 7  | 7  |  
**Perhitugan Rata-Rata:**
- **Average Waiting Time (WT):** 

    $\frac{(7.0 - 0.0 - 7) + (12.0 - 2.0 - 4) + (8.0 - 4.0 - 1) + (16.0 - 5.0 - 4)}{4} = 4.0$
- **Average Turn Around Time (TAT):**

    $\frac{(7.0 - 0.0) + (12.0 - 2.0) + (8.0 - 4.0) + (16.0 - 5.0)}{4} = 8.0$

## 6. Kesimpulan
Laporan ini telah berhasil menganalisis implementasi algoritma penjadwalan Shortest Job First (SJF) non-preemptive dengan mempertimbangkan arrival time proses. Berdasarkan hasil pengujian menggunakan beban komputasi yang ditentukan, algoritma SJF menunjukkan kemampuannya dalam mengoptimalkan waktu tunggu rata-rata proses. Meskipun demikian, algoritma ini memiliki kompleksitas implementasi yang lebih tinggi dibandingkan dengan algoritma sederhana seperti First-Come, First-Served (FCFS), terutama dalam menentukan proses terpendek yang siap dieksekusi pada setiap titik waktu. Hasil perhitungan menunjukkan Average Waiting Time (WT) sebesar 4.0 dan Average Turnaround Time (TAT) sebesar 8.0, mengindikasikan efisiensi algoritma SJF dalam meminimalkan kedua metrik tersebut untuk skenario yang diberikan. Namun, tantangan dalam memprediksi burst time secara akurat di lingkungan nyata tetap menjadi batasan utama penerapan SJF.