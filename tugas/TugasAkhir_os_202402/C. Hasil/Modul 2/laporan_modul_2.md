# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi
**Semester**: Genap / Tahun Ajaran 2024–2025
**Nama**: `Muhammad Firly Ramadhan`
**NIM**: `240202872`
**Modul yang Dikerjakan**:
`(Modul 2 - Penjadwalan CPU Non-Preemptive Berbasis Prioritas)`

---

## 📌 Deskripsi Singkat Tugas

* **Modul 2 - Penjadwalan CPU Non-Preemptive Berbasis Prioritas**:
  Modul ini membahas cara mengubah algoritma penjadwalan pada xv6 dari Round Robin menjadi Non-Preemptive Priority Scheduling. Perubahan utama meliputi penambahan field priority pada struktur proses, implementasi syscall set_priority(int), dan modifikasi fungsi scheduler() agar menjalankan proses dengan prioritas tertinggi terlebih dahulu. Disediakan juga program uji (ptest.c) untuk memverifikasi bahwa proses dengan prioritas lebih tinggi dieksekusi lebih dulu.
---

## 🛠️ Rincian Implementasi

### Modul 2:
1. Tambahkan field `priority` ke `struct proc` di `proc.h`
2. Inisialisasi `priority` di fungsi `allocproc()` pada `proc.c`
3. Tambahkan `#define SYS_set_priority 24` di `syscall.h`
4. Tambahkan deklarasi `int set_priority(int);` di `user.h`
5. Tambahkan `SYSCALL(set_priority)` di `usys.S`
6. Tambahkan `extern int sys_set_priority(void);` di `syscall.c`
7. Tambahkan `[SYS_set_priority] sys_set_priority,` di array `syscalls[]` pada `syscall.c`
8. Implementasikan `sys_set_priority()` di `sysproc.c`
9. Modifikasi fungsi `scheduler()` di `proc.c` untuk memilih proses dengan `priority` terkecil
10. Buat program uji `ptest.c`
11. Tambahkan `_ptest` ke bagian `UPROGS` dalam `Makefile`
12. Jalankan `make clean` dan `make qemu-nox`, lalu uji dengan `$ ptest` di shell xv6
---

## ✅ Uji Fungsionalitas

Tuliskan program uji apa saja yang Anda gunakan, misalnya:

* `ptest`: untuk menguji `getpinfo()`
* `rtest`: untuk menguji `getReadCount()`
* `cowtest`: untuk menguji fork dengan Copy-on-Write
* `shmtest`: untuk menguji `shmget()` dan `shmrelease()`
* `chmodtest`: untuk memastikan file `read-only` tidak bisa ditulis
* `audit`: untuk melihat isi log system call (jika dijalankan oleh PID 1)

---

## 📷 Hasil Uji

ptest modul2: menguji penjadwalan berdasarkan prioritas

Child 2 diberi prioritas tinggi (10)

Child 1 diberi prioritas rendah (90)

Diharapkan output: Child 2 selesai muncul sebelum Child 1 selesai

### 📍 Output `ptest`:

```
Child 2 selesai
Child 1 selesai
Parent selesai
```

Jika ada screenshot:

```
![hasil cowtest](./screenshots/cowtest_output.png)
```

---

## ⚠️ Kendala yang Dihadapi

1. Konflik saat menambahkan field `priority` di `struct proc` jika ada modifikasi lain sebelumnya.
2. Lupa menginisialisasi `priority` di `allocproc()`, menyebabkan nilai sampah dan perilaku tak terduga.
3. Nomor syscall `SYS_set_priority` berbenturan dengan syscall lain jika tidak unik.
4. Kesalahan saat menambahkan entri syscall di `syscall.c` atau `usys.S`, sehingga syscall tidak bisa dipanggil.
5. Validasi argumen `priority` di syscall tidak lengkap (misalnya tidak memeriksa batas 0–100).
6. Salah logika dalam pemilihan proses di `scheduler()` (misalnya `>` bukan `<` untuk prioritas).
7. Scheduler tetap melakukan preemptive switch karena lupa menonaktifkan preemption (misalnya tidak cukup kontrol terhadap `yield`/`timer`).
8. Proses dengan prioritas tinggi tidak mendapat CPU jika tidak dalam kondisi RUNNABLE saat pengecekan.
9. Lupa menambahkan program uji `ptest.c` ke `Makefile`, menyebabkan error saat build.
10. Output `ptest` tidak sesuai karena urutan eksekusi bergantung pada race condition (misalnya proses tidak sempat set priority sebelum dijalankan).


---

## 📚 Referensi

* Buku xv6 MIT: [https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
* Repositori xv6-public: [https://github.com/mit-pdos/xv6-public](https://github.com/mit-pdos/xv6-public)
* Stack Overflow, GitHub Issues, diskusi praktikum

---

