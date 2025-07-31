# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi
**Semester**: Genap / Tahun Ajaran 2024–2025
**Nama**: `Muhammad Firly Ramadhan`
**NIM**: `240202872`
**Modul yang Dikerjakan**:
`(Contoh: Modul 5 – Audit dan Keamanan Sistem (xv6-public))`

---

## 📌 Deskripsi Singkat Tugas

* **Modul 5 - Audit dan Keamanan Sistem (xv6-public)**:
  Modul ini menambahkan fitur audit log ke xv6 dengan mencatat setiap system call yang dipanggil oleh proses. Hasil pencatatan hanya dapat diakses oleh proses dengan PID 1 melalui syscall get_audit_log(buf, max), sehingga meningkatkan aspek keamanan dan kontrol sistem.
---

## 🛠️ Rincian Implementasi

### Fitur Audit Log dan Keamanan PID 1

1. Tambahkan definisi `MAX_AUDIT`, struct `audit_entry`, array `audit_log[]`, dan `audit_index` di bagian atas file `syscall.c`.
2. Di dalam fungsi `syscall()` di `syscall.c`, tambahkan pencatatan audit untuk setiap system call yang valid (pid, syscall\_num, tick) jika log belum penuh.
3. Tambahkan nomor syscall `#define SYS_get_audit_log 28` di file `syscall.h`.
4. Tambahkan deklarasi struct `audit_entry` dan fungsi `get_audit_log(void*, int)` di `user.h`.
5. Tambahkan entri syscall `SYSCALL(get_audit_log)` ke file `usys.S`.
6. Di `syscall.c`, daftarkan syscall baru dengan `extern int sys_get_audit_log(void);` dan `syscalls[SYS_get_audit_log] = sys_get_audit_log`.
7. Implementasikan fungsi `sys_get_audit_log()` di `sysproc.c`, yang hanya mengizinkan proses dengan `proc->pid == 1` untuk membaca isi `audit_log[]`.
8. Salin isi `audit_log[]` ke buffer user menggunakan `memmove()` jika valid, dan kembalikan jumlah entri yang disalin.
9. Buat program uji `audit.c` yang memanggil `get_audit_log()` dan mencetak isi log, hanya bisa dijalankan oleh PID 1.
10. Tambahkan `_audit` ke bagian `UPROGS` di `Makefile` agar program uji `audit` dibangun.
11. Untuk pengujian sebagai PID 1, edit `init.c` dan ubah `exec("sh", argv)` menjadi `exec("audit", argv)`.
---

## ✅ Uji Fungsionalitas

1. Jalankan program `audit` dari shell xv6 secara normal.
2. Program akan menampilkan pesan "Access denied or error." karena bukan dijalankan oleh PID 1.
3. Edit file `init.c` dan ubah pemanggilan `exec("sh", argv);` menjadi `exec("audit", argv);`.
4. Lakukan build ulang dengan perintah `make clean` lalu `make qemu-nox`.
5. Setelah xv6 booting, program `audit` akan dijalankan secara otomatis sebagai PID 1.
6. Program akan mencetak daftar log syscall yang berisi PID, nomor syscall, dan nilai tick.
7. Validasi berhasil jika log muncul dengan format yang benar dan hanya bisa diakses oleh proses PID 1.

---

## 📷 Hasil Uji

### 📍 Contoh Output `audit`:

```
=== Audit Log ===
[0] PID=3 SYSCALL=2093 TICK=12
[1] PID=3 SYSCALL=2065 TICK=7
[2] PID=3 SYSCALL=2146 TICK=22
```

Jika ada screenshot:

```
![hasil cowtest](./screenshots/cowtest_output.png)
```

---

## ⚠️ Kendala yang Dihadapi

1. Struktur `audit_log` tidak dikenali di file lain jika tidak menggunakan `extern` dengan benar.
2. Pencatatan syscall bisa gagal jika pengecekan batas `audit_index` tidak akurat.
3. Isi log tidak akurat jika `ticks` belum diupdate saat syscall dipanggil.
4. Syscall `get_audit_log()` gagal diakses jika entri tidak lengkap di `syscall.h`, `usys.S`, dan `syscall.c`.
5. Program `audit` tidak bisa membaca log jika dijalankan bukan sebagai PID 1.
6. Output kosong jika buffer `buf` tidak cukup besar atau `argptr()` gagal memvalidasi pointer.
7. Gagal saat build jika `audit.c` tidak dimasukkan ke daftar `UPROGS` di `Makefile`.
8. Log tidak terbaca jika `memmove()` salah menghitung jumlah byte yang disalin.
9. Jika `audit_index` tidak dijaga secara aman dalam konteks multiproses, bisa terjadi race condition.
10. Pengujian sulit dilakukan jika tidak mengubah `init.c` untuk menjalankan `audit` sebagai proses pertama.


## 📚 Referensi

* Buku xv6 MIT: [https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
* Repositori xv6-public: [https://github.com/mit-pdos/xv6-public](https://github.com/mit-pdos/xv6-public)
* Stack Overflow, GitHub Issues, diskusi praktikum

---

