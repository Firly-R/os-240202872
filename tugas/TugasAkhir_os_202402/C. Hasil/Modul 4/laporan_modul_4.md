# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi
**Semester**: Genap / Tahun Ajaran 2024–2025
**Nama**: `Muhammad Firly Ramadhan`
**NIM**: `240202872`
**Modul yang Dikerjakan**:
`(Modul 4 – Subsistem Kernel Alternatif (chmod() dan /dev/random)`

---

## 📌 Deskripsi Singkat Tugas

* **Modul 4 – Subsistem Kernel Alternatif (chmod() dan /dev/random**:

1. **Implementasi syscall `chmod(path, mode)`** yang memungkinkan pengaturan mode akses file menjadi *read-only* atau *read-write*.
2. **Penambahan driver pseudo-device `/dev/random`** yang menghasilkan byte acak saat dibaca oleh user program.

Keduanya bertujuan untuk memperluas fungsionalitas subsistem file dan device pada kernel xv6.

---

## 🛠️ Rincian Implementasi

Berikut adalah **rincian implementasi Modul 4** dalam bentuk nomor dan kalimat yang lebih ringkas:

---
A. Syscall `chmod(path, mode)`
1. Tambahkan field `short mode` ke `struct inode` di `fs.h`.
2. Tambahkan nomor syscall `SYS_chmod` di `syscall.h`.
3. Deklarasikan `chmod()` di `user.h` dan `usys.S`.
4. Daftarkan `sys_chmod` di `syscall.c`.
5. Implementasikan `sys_chmod()` di `sysfile.c` untuk mengatur `ip->mode`.
6. Tambahkan pengecekan `ip->mode == 1` di `filewrite()` pada `file.c` untuk blokir tulis.
7. Buat program uji `chmodtest.c` yang memverifikasi penulisan diblokir jika file read-only.
---
B. Device `/dev/random`
1. Buat file `random.c` dengan fungsi `randomread()` untuk hasilkan byte acak.
2. Registrasikan `randomread` di `devsw[]` index `3` pada `file.c`.
3. Tambahkan `mknod("/dev/random", 1, 3);` di `init.c`.
4. Buat program `randomtest.c` untuk membaca dan mencetak byte dari `/dev/random`.
---

## ✅ Uji Fungsionalitas

### A. `chmod(path, mode)`

1. Jalankan `chmodtest` di xv6.
2. Buat dan tulis file `myfile.txt`.
3. Ubah file jadi read-only dengan `chmod`.
4. Coba tulis ulang ke file.
5. Jika gagal tulis, tampilkan: **"Write blocked as expected"**.

---

### B. `/dev/random`

1. Jalankan `randomtest` di xv6.
2. Baca 8 byte dari `/dev/random`.
3. Cetak byte sebagai angka.
4. Output harus berupa 8 angka acak.
---

## 📷 Hasil Uji

### 📍 Contoh Output `chmodtest`:

```
Write blocked as expected
```

### 📍 Contoh Output `randomtest`:

```
159 114 41 116 67 198 109 232
```

Jika ada screenshot:

```
![hasil cowtest](./screenshots/cowtest_output.png)
```

---

## ⚠️ Kendala yang Dihadapi

### A. `chmod(path, mode)`

1. Field `mode` pada `inode` bisa hilang saat inode di-reload karena tidak disimpan permanen.
2. Lupa menambahkan validasi `mode` (harus 0 atau 1) di `sys_chmod()`.
3. Penulisan tetap bisa terjadi jika pengecekan `mode` di `filewrite()` terlewat.
4. Syscall tidak dikenali jika entri di `syscall.c`, `syscall.h`, `usys.S`, atau `user.h` tidak lengkap.
5. File uji `chmodtest.c` tidak berhasil dibangun jika tidak ditambahkan ke `Makefile`.

---

### B. `/dev/random`

1. Fungsi `randomread()` menghasilkan nilai tetap jika `seed` tidak berubah antar proses.
2. `/dev/random` tidak bisa diakses jika `mknod()` tidak dipanggil dengan benar di `init.c`.
3. Gagal baca jika `devsw[3]` belum didaftarkan di `file.c`.
4. File `random.c` tidak dikompilasi jika lupa dimasukkan ke daftar build di `Makefile`.
5. Output `randomtest` kosong jika terjadi kesalahan pada `open()` atau `read()`.

---

## 📚 Referensi

* Buku xv6 MIT: [https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
* Repositori xv6-public: [https://github.com/mit-pdos/xv6-public](https://github.com/mit-pdos/xv6-public)
* Stack Overflow, GitHub Issues, diskusi praktikum

---

