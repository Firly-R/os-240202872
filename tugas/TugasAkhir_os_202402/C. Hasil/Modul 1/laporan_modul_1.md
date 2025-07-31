# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi
**Semester**: Genap / Tahun Ajaran 2024–2025
**Nama**: `Muhammad Firly Ramadhan`
**NIM**: `240202872`
**Modul yang Dikerjakan**:
`(Modul 1 – System Call dan Instrumentasi Kernel)`

---

## 📌 Deskripsi Singkat Tugas

* **Modul 1 – System Call dan Instrumentasi Kernel**:
  Menambahkan dua system call baru, yaitu `getpinfo()` untuk melihat proses yang aktif dan `getReadCount()` untuk menghitung jumlah pemanggilan `read()` sejak boot.
---

## 🛠️ Rincian Implementasi

* Menambahkan dua system call baru di file `sysproc.c` dan `syscall.c`
* Mengedit `user.h`, `usys.S`, dan `syscall.h` untuk mendaftarkan syscall
* Menambahkan struktur `struct pinfo` di `proc.h`
* Menambahkan counter `readcount` di kernel
* Membuat dua program uji: `ptest.c` dan `rtest.c`
---

## ✅ Uji Fungsionalitas

* `ptest`: untuk menguji `getpinfo()`
* `rtest`: untuk menguji `getReadCount()`

---

## 📷 Hasil Uji

### 📍 Contoh Output `ptest`:

```
PID    MEM    NAME
1      12288  init
2      16384  sh
3      12288  ptest
```

### 📍 Contoh Output `rtest`:

```
Read Count Sebelum:  12
hello
Read Count Sebelum:  13
```

Jika ada screenshot:

```![Screenshot Output](/C.Hasil/Modul 1/screenshot1.png)

```

---

## ⚠️ Kendala yang Dihadapi

1. Perbedaan struktur struct pinfo antara kernel (proc.h) dan user (ptest.c) dapat menyebabkan data salah atau rusak.

2. ptable_lock tidak tersedia di xv6-public, sehingga perlu menggunakan ptable.lock atau mendefinisikan spinlock sendiri.

3. Fungsi argptr() bisa gagal jika pointer dari user space tidak valid atau struct pinfo terlalu besar untuk dialokasikan.

4. Operasi readcount++ di sys_read() tidak aman jika dipanggil oleh banyak proses secara bersamaan (race condition).

5. Lupa mendaftarkan syscall baru di salah satu dari syscall.h, syscall.c, usys.S, atau user.h dapat menyebabkan error link atau syscall tidak aktif.

6. Program uji (ptest dan rtest) tidak muncul di xv6 jika tidak ditambahkan ke daftar UPROGS di Makefile.

7. Output ptest kosong karena proses tidak terdeteksi aktif atau pointer hasil penyalinan salah.

8. Nilai readcount tidak berubah karena pemanggilan read() gagal atau counter tidak diletakkan di tempat yang benar.

9. Header file yang tidak lengkap, salah include, atau duplikat dapat menyebabkan error kompilasi.

10. Penggunaan safestrcpy bisa menyebabkan crash jika panjang nama proses melebihi batas buffer (16 karakter).

---

## 📚 Referensi

* Buku xv6 MIT: [https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
* Repositori xv6-public: [https://github.com/mit-pdos/xv6-public](https://github.com/mit-pdos/xv6-public)
* Stack Overflow, GitHub Issues, diskusi praktikum

---

