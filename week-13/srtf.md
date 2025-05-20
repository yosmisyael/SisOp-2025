# Laporan Sistem Operasi: Algoritma Penjadwalan Shorthest Remaining Time First (SRTF - Preemptive)

**Nama Anggota Kelompok:**
1. Misyael Yosevian Wiarda (3124500039)
2. Fahroldhi Sukirno (3124500046)
3. Erick Haidar Rahmat (3124500047)

---

## 1. Pendahuluan
Penjadwalan CPU merupakan salah satu komponen penting dalam sistem operasi modern yang bertanggung jawab mengatur alokasi sumber daya CPU kepada berbagai proses yang bersaing. Efisiensi penjadwalan secara signifikan memengaruhi kinerja keseluruhan sistem, termasuk waktu respons, throughput, dan utilisasi CPU.

Shortest Remaining Time First (SRTF), yang juga dikenal sebagai SJF Preemptive, adalah varian dari algoritma SJF yang memungkinkan preemption. Ini berarti jika ada proses baru yang tiba dengan burst time yang lebih pendek dari sisa waktu eksekusi proses yang sedang berjalan, CPU akan menghentikan proses yang sedang berjalan dan beralih ke proses yang baru tiba tersebut. Pendekatan ini secara teoritis mampu memberikan waktu tunggu rata-rata minimum yang lebih baik dibandingkan SJF non-preemptive, karena lebih responsif terhadap kedatangan proses-proses pendek. Laporan ini akan menganalisis metode penjadwalan CPU SRTF, khususnya dengan mempertimbangkan waktu kedatangan (arrival time) proses.
 
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
#define MAX 9999
struct proc
{
    int no,at,bt,rt,ct,tat,wt;
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
    p.rt=p.bt;
    return p;
}
int main()
{
    struct proc p[10],temp;
    float avgtat=0,avgwt=0;
    int n,s,remain=0,time;
    printf("<--SRTF Scheduling Algorithm (Preemptive)-->\n");
    printf("Enter Number of Processes: ");
    scanf("%d",&n);
    for(int i=0;i<n;i++)
        p[i]=read(i+1);
    for(int i=0;i<n-1;i++)
        for(int j=0;j<n-i-1;j++)    
            if(p[j].at>p[j+1].at)
            {
            temp=p[j];
            p[j]=p[j+1];
            p[j+1]=temp;
            }
    printf("\nProcess\t\tAT\tBT\tCT\tTAT\tWT\n");
    p[9].rt=MAX;
    for(time=0;remain!=n;time++)
    {
        s=9;
        for(int i=0;i<n;i++)
            if(p[i].at<=time&&p[i].rt<p[s].rt&&p[i].rt>0)
                s=i;
        p[s].rt--;
        if(p[s].rt==0)
        {
            remain++;
            p[s].ct=time+1;
            p[s].tat=p[s].ct-p[s].at;
            avgtat+=p[s].tat;
            p[s].wt=p[s].tat-p[s].bt;
            avgwt+=p[s].wt;
            printf("P%d\t\t%d\t%d\t%d\t%d\t%d\n",p[s].no,p[s].at,p[s].bt,p[s].ct,p[s].tat,p[s].wt);
        }
    }
    avgtat/=n,avgwt/=n;
    printf("\nAverage TurnAroundTime=%f\nAverage WaitingTime=%f",avgtat,avgwt);
}

```
### 3.2 Penjelasan Kode
Struktur Data dan Masukan:
- struct proc: Struktur ini digunakan untuk menyimpan detail setiap proses, yaitu:
    - no: Nomor identifikasi proses.
    - at: Arrival Time (waktu kedatangan proses).
    - bt: Burst Time (total waktu eksekusi yang dibutuhkan proses).
    - rt: Remaining Time (sisa waktu eksekusi proses). Ini adalah elemen kunci untuk SRTF karena akan terus diperbarui.
    - ct: Completion Time (waktu proses selesai dieksekusi).
    - tat: Turnaround Time (waktu perputaran).
    - wt: Waiting Time (waktu tunggu).
- read(): Fungsi ini mengambil input dari pengguna untuk arrival time dan burst time setiap proses, sekaligus menginisialisasi rt (remaining time) dengan nilai bt.

- Proses Penyortiran Awal:
    - Bagian ini mengurutkan proses-proses yang dimasukkan berdasarkan arrival time menggunakan algoritma bubble sort. Meskipun tidak mutlak diperlukan untuk logika utama SRTF (karena pemilihan proses dilakukan secara dinamis), pengurutan ini membantu dalam memproses proses-proses yang tiba lebih awal terlebih dahulu, terutama saat time = 0.

- Logika Penjadwalan SRTF Utama:
    - p[9].rt = MAX: Ini adalah teknik untuk menginisialisasi nilai remaining time yang sangat besar pada indeks dummy. Tujuannya adalah untuk memastikan bahwa pada iterasi pertama pencarian proses terpendek, proses valid mana pun (dengan rt yang lebih kecil) akan selalu terpilih.
    Loop for(time=0; remain!=n; time++): Ini adalah inti dari simulasi SRTF. Loop ini berjalan secara time-slice by time-slice (satu unit waktu per iterasi) hingga semua n proses selesai dieksekusi (remain == n).
    s=9: Pada setiap unit waktu, variabel s diinisialisasi ke indeks dummy (9). s akan menyimpan indeks proses yang saat ini memiliki remaining time terpendek.
    
    - Pencarian Proses Terpendek (for(int i=0;i<n;i++) ...):
        - Kode ini akan mencari di antara semua proses (i=0 hingga n-1).
        Kondisi p[i].at <= time: Memastikan hanya proses yang sudah tiba pada atau sebelum time saat ini yang dipertimbangkan.
        - Kondisi p[i].rt < p[s].rt: Membandingkan remaining time proses saat ini (p[i].rt) dengan proses yang sedang dianggap terpendek (p[s].rt). Jika p[i].rt lebih kecil, maka p[i] menjadi kandidat baru.
        - Kondisi p[i].rt > 0: Memastikan hanya proses yang belum selesai dieksekusi yang dipertimbangkan.
        - Jika semua kondisi terpenuhi, s akan diperbarui ke indeks i, menandakan p[i] adalah proses terpendek yang siap dieksekusi.
        - p[s].rt--: Setelah proses terpendek ditemukan, remaining time dari proses tersebut dikurangi satu unit, mensimulasikan eksekusi selama satu unit waktu.
    - Pengecekan Penyelesaian Proses (if(p[s].rt==0)):
        - Jika remaining time dari proses yang sedang dieksekusi menjadi 0, berarti proses tersebut telah selesai.
        - remain++: Jumlah proses yang selesai ditambahkan.
        - p[s].ct = time + 1: Completion Time dicatat sebagai waktu saat ini ditambah satu (karena time adalah awal dari interval waktu).
        - Turnaround Time dan Waiting Time dihitung untuk proses yang baru selesai ini, dan ditambahkan ke total akumulasi untuk perhitungan rata-rata.
        - Detail proses yang selesai dicetak.

- Perhitungan Waktu dan Output Rata-Rata:
    - Setelah loop simulasi selesai (semua proses selesai), total avgtat dan avgwt dibagi dengan n (jumlah proses) untuk mendapatkan nilai rata-ratanya.
    Hasil rata-rata kemudian dicetak.

## 4. Analisis Skenario Data Pengujian
Untuk menganalisis kinerja algoritma penjadwalan SRTF atau Shortest Job First Preemptive (SJF) dengan mempertimbangkan waktu kedatangan (arrival time), data berikut akan digunakan dalam skenario pengujian kali ini:
**Input Data Beban Komputasi:**

| Proses | Arrival Time | Burst Time | 
| ------ | ------------ | ---------- |
| p1 | 0.0 | 7 |
| p2 | 2.0 | 4 |
| p3 | 4.0 | 1 |
| p4 | 5.0 | 4 |

## 5. Hasil dan Pembahasan 
![Hasil eksekusi program](https://raw.githubusercontent.com/yosmisyael/SisOp-2025/refs/heads/main/embeds/srtf.jpg)

**Data Hasil Proses Penjadwalan:**

| ProcessNo | AT |BT | CT  | TAT | WT |
|-----------|----|----|-----|-----|----|
| P3        | 4  | 1  | 5   | 1   | 1  |
| P2        | 2  | 4  | 7   | 5   | 5  |
| P4        | 5  | 4  | 11  | 6   | 6  |
| P1        | 0  | 7  | 16  | 16  | 16 | 

**Perhitugan Rata-Rata:**
- **Average Waiting Time (WT):** 

    $$\frac{(0) + (1) + (2) + (9)}{4} = \frac{12}{4} = 3.0$$
- **Average Turn Around Time (TAT):**

   $$\frac{(1) + (5) + (6) + (16)}{4} = \frac{28}{4} = 7.0$$

**Gantt Chart:**  

![image](https://github.com/user-attachments/assets/2160e660-7309-4f89-82fe-913ec20911a1)


## 6. Kesimpulan
Laporan ini telah menganalisis implementasi algoritma penjadwalan Shortest Remaining Time First (SRTF), atau SJF Preemptive, dengan mempertimbangkan arrival time proses. Berbeda dengan SJF non-preemptive, SRTF memungkinkan preemption, yang secara teoritis dapat memberikan waktu tunggu rata-rata yang lebih optimal dengan merespons kedatangan proses-proses pendek secara dinamis. Berdasarkan skenario pengujian yang sama, SRTF berhasil mencapai Average Waiting Time (WT) sebesar 3.0 dan Average Turnaround Time (TAT) sebesar 7.0. Hasil ini menunjukkan bahwa SRTF umumnya menghasilkan kinerja yang lebih baik dalam hal waktu tunggu dan perputaran dibandingkan SJF non-preemptive untuk beban kerja yang sama, karena kemampuannya untuk segera menjadwalkan proses dengan burst time terpendek yang baru tiba. Namun, kompleksitas implementasinya lebih tinggi karena overhead yang terkait dengan preemption dan pemantauan sisa waktu eksekusi secara terus-menerus.
