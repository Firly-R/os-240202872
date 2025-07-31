# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi
**Semester**: Genap / Tahun Ajaran 2024–2025
**Nama**: `Muhammad Firly Ramadhan`
**NIM**: `240202872`
**Modul yang Dikerjakan**:
`(Modul 3 – Manajemen Memori Tingkat Lanjut (xv6-public x86))`

---

## 📌 Deskripsi Singkat Tugas

* **Modul 3 – Manajemen Memori Tingkat Lanjut (xv6-public x86)**:
1. Copy-on-Write (CoW) Fork, yang membuat fork() lebih efisien dengan menunda penyalinan halaman memori sampai terjadi penulisan (write).

2. Shared Memory (shmget & shmrelease), yang memungkinkan proses saling berbagi halaman memori menggunakan sistem refcount dan key, mirip seperti System V shared memory.
---

## 🛠️ Rincian Implementasi

### Copy-on-Write Fork

1. Tambahkan `ref_count[]`, `incref()`, dan `decref()` di `vm.c`.
2. Tambahkan flag `PTE_COW` di `mmu.h`.
3. Ganti `copyuvm()` menjadi `cowuvm()` untuk sharing halaman read-only dan tambah refcount.
4. Ubah `fork()` di `proc.c` agar memakai `cowuvm()`.
5. Tangani page fault `T_PGFLT` di `trap.c` untuk mendeteksi `PTE_COW`, lalu salin halaman saat write.

---

### Shared Memory

1. Tambahkan tabel `shmtab[]` di `vm.c` untuk menyimpan key, frame, dan refcount.
2. Buat `sys_shmget()` di `sysproc.c` untuk alokasi atau akses shared memory berdasarkan key.
3. Buat `sys_shmrelease()` untuk mengurangi refcount dan membebaskan jika nol.

---

### Uji Fungsionalitas

1. Buat `cowtest.c` untuk uji `fork()` dan copy-on-write.
2. Buat `shmtest.c` untuk uji shared memory antar proses.


## 📷 Hasil Uji

### 📍 Contoh Output `cowtest`:

```
Child sees: Y
Parent sees: X
```

### 📍 Contoh Output `shmtest`:

```
Child reads: A
Parent reads: B
```
---

## ⚠️ Kendala yang Dihadapi

1. Refcount tidak akurat jika `incref()`/`decref()` tidak dipanggil konsisten.
2. Page fault tidak tertangani jika `PTE_COW` tidak terdeteksi di `trap.c`.
3. Memory leak bisa terjadi jika halaman tidak dibebaskan saat refcount nol.
4. `fork()` gagal jika `cowuvm()` tidak memetakan halaman dengan benar.
5. Data rusak jika `memmove()` gagal menyalin isi halaman saat fault.
6. Shared memory gagal dimapping jika `shmtab` penuh atau `key` tidak ditemukan.
7. Alamat mapping `USERTOP - (i+1)*PGSIZE` bisa bentrok dengan heap atau stack.
8. Frame tidak dibebaskan jika `shmrelease()` tidak menurunkan refcount dengan benar.
9. Race condition bisa muncul saat dua proses mengakses `shmtab` bersamaan.
10. Proses hasil `fork()` bisa kehilangan akses shared memory jika `shmtab` tidak disinkronkan.

---

## 📚 Referensi

* Buku xv6 MIT: [https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
* Repositori xv6-public: [https://github.com/mit-pdos/xv6-public](https://github.com/mit-pdos/xv6-public)
* Stack Overflow, GitHub Issues, diskusi praktikum

---

