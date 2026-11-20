# Changelog — WargaKITA (RT 17 / RW 03 Kebonagung)

Semua perubahan penting pada aplikasi **WargaKITA** dicatat secara mendetail dalam file ini.

---

## [v1.14.0] - 2026-11-20 (Latest)

### Ditambahkan
- **Asisten Pintar NawakAI (Gemini AI + Supabase Edge Function `nawak-ai`):**
  - Integrasi penuh dengan Supabase Edge Function `nawak-ai` menggunakan native `Deno.serve` dan mekanisme *fallback* model berantai (`gemini-3.8-flash` → `gemini-flash-latest` → `gemini-3-flash-preview`) serta `thinkingConfig: { thinkingBudget: 0 }` untuk respons cepat.
  - Mendukung percakapan cerdas berbasis data *real-time* RT 17 langsung dari aplikasi (kas RT, iuran, tunggakan, data warga, agenda kegiatan, dan ulang tahun).
- **Ikon Melayang Wajik Biru Interaktif (*Draggable*) & Gelembung Sapaan Mengikuti Posisi:**
  - Maskot **NawakAI** bergaya Flat UI (`#1d4ed8` dan `#60a5fa`) dilengkapi animasi mengambang (*bob*), berkedip (*blink*), tersenyum (*smile*), dan kilau (*spark*), dengan posisi tersimpan otomatis di `localStorage` (`wk_nawak_pos`).
  - Gelembung sapaan (*welcome bubble* kontekstual Pagi/Siang/Sore/Malam & situasi RT 17) dilengkapi ekor *speech bubble* dan **otomatis bergerak mengikuti posisi ikon NawakAI secara *real-time*** saat digeser, responsif di sisi kiri/kanan layar.
  - Tips interaktif kontekstual dari NawakAI saat pengguna membuka tab **Data Warga**, **Data KK**, atau **Cash RT**.
- **Import File Data Warga via NawakAI (Khusus Developer & Pak RT):**
  - Tombol lampiran file (`.xlsx`, `.xls`, `.csv`) di dalam jendela chat NawakAI untuk memproses dan mengimpor data warga ke WargaKITA dengan pembatasan hak akses ketat hanya untuk **Developer Admin** dan **Pak RT**.
- **Export Data Warga 4 Format Lengkap via NawakAI (`exportWargaByFormat`):**
  - Mendukung ekspor data warga menyeluruh dalam format **PNG**, **Excel (`.xlsx`)**, **Word (`.doc`)**, dan **PDF** menggunakan struktur 7 kolom lengkap yang identik dengan Menu Lainnya (`No / Anggota`, `Alamat`, `Nomor KK`, `NIK`, `Hubungan Keluarga`, `No. HP`, `Status`).
- **Notifikasi HP Berkala (Pending Approval 1 Jam Sekali + Agenda & Ultah):**
  - Pengingat otomatis ke notifikasi HP Android setiap minimal **1 jam sekali** bagi **Pak RT**, **Wakil RT**, dan **Developer** selama masih ada transaksi **Pending Approval**.
  - Pengingat otomatis di notifikasi HP Android untuk **Agenda Resmi RT (H-3 hingga H-0)** serta **Ulang Tahun Kepala Keluarga** hari ini, lengkap dengan *hook* `onResume` & pengecekan berkala di `MainActivity.kt`.
- **Kartu Edit Profil Pengguna (Flat UI):**
  - Menambahkan tombol dan modal **Edit Profil Saya** pada kartu profil di tab **Lainnya** agar setiap pengguna dapat memperbarui Nama Tampilan, No. HP / WhatsApp, dan Keterangan/Jabatan.
  - Menyimpan perubahan melalui `supabase.auth.updateUser` (`user_metadata`) dan sinkronisasi lokal sehingga tetap berhasil meskipun RLS tabel `profiles` membatasi pembaruan langsung.
- **Akses Login Khusus 4 Bendahara RT (Auto-Default & Terkunci):**
  - Dukungan akun login terpisah untuk masing-masing petugas Bendahara RT dengan pengisian otomatis dan penguncian kategori iuran serta nama petugas pada form **Catat Pembayaran Iuran** dan **Catat Transaksi Kas RT**:
    - **Novan Hermawan** (`novan@wargakita.id`) → **17 Agustus**
    - **Ahmad Abin** (`ahmadabin@wargakita.id`) → **Keuangan Social**
    - **A. Teguh Purwanto** (`teguh@wargakita.id`) → **Pembangunan + Voging**
    - **Jodik Purwanto** (`jodik@wargakita.id`) → **Rukem + Juri Kunci**
  - Menambahkan keempat akun Bendahara baru tersebut ke daftar **Ubah Password User Lain** pada **Panel Khusus Developer**.
- **Desain Baru Kartu Ucapan Ulang Tahun (3 Tema Flat UI):**
  - Modal **Kartu Ucapan Ulang Tahun** dengan 3 pilihan tema Flat UI (**Biru RT 17**, **Navy Emas**, **Hijau Segar**), ilustrasi kue ulang tahun & konfeti vektor, tombol **Bagikan / Unduh Gambar Kartu (PNG)**, dan tombol **Kirim Ucapan ke WhatsApp Warga** (`wa.me`).

### Diperbarui
- **Indikator Loading NawakAI ala Gemini:** Mengganti teks tunggu statis menjadi kartu berpikir animasi dengan ikon *spark* berputar dan *timeline* langkah (*"Maca data warga, KK & kas RT 17..."*, *"Ngecek iuran, agenda & ulang tahun..."*, *"Nyusun jawaban Jawa-Malangan..."*).
- **Gaya Bahasa Jawa-Malangan & Gelembung Chat Flat UI Minimalis:** Respons NawakAI diutamakan memakai bahasa Jawa-Malangan yang lucu, unik, dan ringkas dalam gelembung chat proporsional (`w-fit max-w-[84%]`).
- **Perluasan Skill Mesin Lokal NawakAI:** Cek data warga, status iuran & tunggakan per KK, pencarian petugas penagih, rincian pengeluaran kas, rekap kas tahunan 12 bulan, draf pengumuman WhatsApp, panduan aplikasi, dan pemicu Kartu Ucapan Ulang Tahun.
- **Ikon & Logo WargaKITA Tersemat Base64:** Menyematkan ikon resmi `WargaKITA` langsung sebagai Data URI Base64 agar tampil konsisten di layar Login, Dashboard, dan seluruh aplikasi tanpa bergantung pada path file eksternal.
- **Ikon Mata Coret pada Login:** Mengganti ikon centang hijau pada tombol tampilkan password menjadi ikon **mata dicoret (*eye-slash*)** standar.
- **Pembaruan Versi Build Android & GitHub Actions (`v1.14.0`):** Menaikkan `versionCode = 17` (`versionName = "1.14.0"`) di `build.gradle.kts` dan memperbarui `.github/workflows/build-apk.yml` ke tag `v1.14.0`.

### Diperbaiki
- **Sinkronisasi Edit Alamat Warga ke Data KK:** Memperbaiki fungsi simpan **Edit Data Warga** agar pembaruan alamat turut memperbarui kolom `address` pada tabel `households` serta menampilkan alamat terbaru secara konsisten di **Data Warga**, **Data KK**, dan **Detail KK**.
- **Perbaikan Tombol Edit Profil, Ubah Password User Lain & Reset Data Bulanan:** Memindahkan blok skrip yang sempat tersisip di dalam tag `<script src="...">` pada `<head>` ke dalam `<script>` utama sehingga seluruh tombol di tab **Lainnya** dan **Panel Khusus Developer** berfungsi normal.
- **Perbaikan Simpan Edit Profil (`permission denied for table profiles`):** Menambahkan penyimpanan via `db.auth.updateUser` + `localStorage` agar edit profil tidak gagal karena pembatasan RLS tabel `profiles`.
- **Pencegahan *Ghost-Click* Saat Membuka NawakAI:** Menambahkan `touchend preventDefault` dan jeda pengaman `320ms` agar NawakAI selalu membuka **Beranda NawakAI** terlebih dahulu tanpa memicu pertanyaan otomatis.
- **Silent Local Fallback Tanpa Pesan Error Teknis:** Menghapus tampilan pesan error mentah (kuota API, *503 high demand*, *compute resources*) dari layar pengguna dan otomatis beralih ke mesin lokal Jawa-Malangan.

---

## [v1.13.3] - 2026-11-19

### Diperbaiki
- **Ikon Klip Bukti Transaksi:** Memperbaiki teks literal `${IC.paperclip(...)}` pada daftar **Riwayat Transaksi Detail** agar ikon vektor tampil rapi.
- **Warna Tap/Hover Mode Gelap:** Menghilangkan kilatan latar putih (`hover:bg-slate-50`) saat mengetuk kartu transaksi atau daftar di mode gelap agar teks tetap kontras dan nyaman dibaca.
- **Pop-up Detail Transaksi Rapi:** Menyembunyikan URL mentah Supabase Storage dari baris keterangan dan menambahkan pembatasan lebar (`overflow-x-hidden` & `break-words`) agar pop-up tidak melebar keluar layar.

---

## [v1.13.2] - 2026-11-19

### Diperbarui
- **Desain Ikon Vektor Flat UI Penuh:** Mengganti seluruh emoji pada antarmuka aplikasi dengan ikon vektor Flat UI garis tipis yang seragam dan menyelaraskan palet warna per bagian (Dashboard, Data Warga, Data KK, Cash RT, dan Lainnya) untuk mode terang maupun gelap.
- **Ikon Aplikasi Presisi & Full-Bleed:** Memusatkan ikon resmi **WargaKITA** secara presisi pada latar Flat UI `#eff6ff` untuk seluruh resolusi launcher Android (`mdpi`–`xxxhdpi`) maupun banner di dalam aplikasi.

### Diperbaiki
- **Kalender & Hilangkan Garis Scrollbar:** Memperbaiki tampilan ikon pada **Agenda Resmi RT** serta menyembunyikan garis *scrollbar* di sisi kanan seluruh halaman agar tampilan lebih bersih.
- **Jarak Aman Status Bar HP:** Menambahkan *padding* otomatis `WindowInsets` / `status_bar_height` di `MainActivity.kt` agar header aplikasi tidak menumpuk dengan jam dan ikon notifikasi Android.

---

## [v1.13.1] - 2026-11-19

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
