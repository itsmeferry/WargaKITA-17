# Changelog — WargaKITA (RT 17 / RW 03 Kebonagung)

Semua perubahan penting pada aplikasi **WargaKITA** dicatat dalam file ini.

---

## [v1.13.1] - 2026-11-19 (Latest)

### Ditambahkan
- **Kalender Ulang Tahun Warga Permanen:** Menampilkan tanggal ulang tahun warga berstatus `Permanen` pada kalender (titik oranye) serta daftar khusus yang terpisah dari **Agenda Resmi RT**.
- **Tombol Telepon & WhatsApp Kepala Keluarga:** Akses langsung 1-ketuk (`tel:` dan `wa.me`) di kartu Kepala Keluarga pada halaman **Detail KK** menggunakan nomor telepon terdaftar.

### Diperbaiki
- **Warna Tab Filter Status Warga:** Tab **Semua**, **Permanen**, **Kontrak**, **Kost**, dan **Pindah Domisili** kini memiliki warna aktif masing-masing yang kontras di mode terang maupun gelap.
- **Ikon Mata Login Tercentang:** Tombol tampilkan password pada layar login kini memakai ikon mata dengan tanda centang (`✓`) saat password ditampilkan.

---

## [v1.13.0] - 2026-11-19

### Ditambahkan
- **Filter & Pencarian Lanjutan Cash RT:** Pencarian teks (transaksi, kategori, petugas, metode), filter status (`Pending`, `Approved`, `Rejected`), filter tipe (`Pemasukan`, `Pengeluaran`), serta filter nama petugas di **Riwayat Transaksi Detail**.
- **Rekap Kas Tahunan (12 Bulan):** Kartu ringkasan akumulasi pemasukan, pengeluaran, dan selisih bersih Januari–Desember per tahun pada tab **Lainnya**.
- **Signing Keystore Permanen (GitHub Actions):** Dukungan `assembleRelease` dengan *signing keystore* tetap via GitHub Secrets agar pembaruan APK ke depan bisa langsung menimpa versi sebelumnya tanpa *uninstall*.

---

## [v1.12.0] - 2026-11-19

### Ditambahkan
- **Supabase Storage untuk Bukti Transaksi (`receipts`):** Foto bukti pembayaran iuran dan nota kas kini otomatis diunggah ke Supabase Storage dengan struktur folder `TAHUN/BULAN/PEMASUKKAN` atau `PENGELUARAN` serta penamaan file terstandar (`Pemasukkan_Tanggal_NamaWarga` / `Lainnya_Nominal_NamaPemasukkan` / `Pengeluaran_Nominal_NamaPengeluaran`), sehingga dapat dilihat lintas perangkat oleh seluruh pengurus.
- **Petugas Pemeriksa & Penginput Tagihan (A–Z, Wajib):** Pilihan wajib petugas (**A. Teguh Purwanto**, **Ahmad Abin**, **Jodik Purwanto**, **Novan Hermawan**) pada pencatatan iuran serta pencatatan kas oleh Bendahara/Sekretaris, lengkap dengan tampilan nama petugas di **Riwayat Transaksi Detail** dan pop-up **Detail Transaksi**.
- **Import & Export Massal Database Warga:** Fitur unduh **Template Excel (`.xlsx`)**, **Export Database Warga Global**, dan **Import Massal Warga (`.xlsx` / `.csv`)** yang otomatis mengelompokkan data berdasarkan Nomor KK di tab **Lainnya**.
- **Sinkronisasi Kalender HP & Pengingat Kegiatan:** Tombol **Simpan ke HP** pada setiap agenda di **Kalender Kegiatan Warga** untuk menyimpan acara beserta pengingat (60 & 15 menit sebelumnya) ke Kalender Android, serta notifikasi HP otomatis untuk kegiatan hari ini/besok.
- **Tema Aplikasi (Terang, Gelap, Ikuti Device):** Pengaturan tema Flat UI kontras tinggi di tab **Lainnya** yang mendeteksi tema gelap/terang perangkat Android secara *real-time*.
- **Cek Update Otomatis via GitHub Releases:** Menampilkan versi terpasang (`v1.12.0`), tombol **Unduh APK Terbaru**, tombol **Cek Update Aplikasi**, serta pop-up & notifikasi otomatis saat versi baru tersedia di GitHub Releases.
- **Pop-up Detail Riwayat Aktivitas Akun:** Setiap baris di **Riwayat Aktivitas Akun Anda** dapat diketuk untuk melihat detail aktivitas, kategori, pelaku, dan waktu lengkap; pencatatan aktivitas kini aktif untuk seluruh role login.

### Diperbaiki
- **Alur Persetujuan (*Pending Approval*) Bendahara & Sekretaris:** Seluruh pencatatan pemasukan maupun pengeluaran oleh role **Bendahara RT** dan **Sekretaris RT** kini selalu berstatus **Pending Approval** hingga disetujui oleh **Pak RT**, **Wakil RT**, atau **Developer Admin**.
- **Pembersihan KK Kosong (`0 anggota`):** KK yang seluruh warganya telah dihapus otomatis dibersihkan dan tidak lagi muncul di filter **Tanpa Tagihan**.
- **Stabilitas Navigasi Bawah & Modal:** Tab menu bawah kini menggunakan *grid* 5 kolom dengan ukuran tetap dan *safe-area padding* Android agar tidak berubah ukuran atau terpotong; wadah modal detail kini tampil solid di mode terang maupun gelap.

---

## [v1.11.0] - 2026-11-19

### Ditambahkan
- **Hubungan Keluarga KK Lengkap:** Memperluas pilihan hubungan keluarga di form **Warga Baru**, **Warga Lama**, dan **Edit Warga** (`Kepala Keluarga`, `Suami`, `Istri`, `Pasangan`, `Anak`, `Orang Tua`, `Mertua`, `Menantu`, `Cucu`, `Famili Lain`, `Lainnya`).

---

## [v1.10.0] - 2026-11-18

### Ditambahkan
- **Pemisahan Input Warga Baru (1 KK) & Warga Lama (KK Ada):** Mendukung input satu keluarga penuh sekaligus banyak anggota dalam satu kali simpan.
- **Pop-up Detail Transaksi & Hapus Warga:** Menambahkan pop-up detail saat baris transaksi diketuk serta hak akses hapus warga untuk **Developer**, **Pak RT**, dan **Wakil RT**.
- **Export Laporan PDF/PNG & Excel Warga:** Dukungan bagikan langsung laporan keuangan bulanan ke PDF/PNG dan unduh daftar warga terkelompok per KK ke `.xlsx`.
- **Izin Kamera Langsung & Notifikasi Android:** Integrasi kamera langsung untuk foto bukti bayar, notifikasi sistem Android, *pull-to-refresh*, serta penanganan tombol *Back* Android.

---

## [v1.0.0] - 2026-11-17

### Ditambahkan
- Rilis awal **WargaKITA** (Flat UI) dengan 5 tab utama (**Dashboard**, **Data Warga**, **Data KK**, **Cash RT**, **Lainnya**), autentikasi 5 role `@wargakita.id` terintegrasi Supabase, **Kalender Kegiatan Warga**, serta fitur **Catat Bayar** (QRIS a.n. Fibriono, Transfer Bank, Tunai).
