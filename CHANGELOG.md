# Changelog — WargaKITA (RT 17 / RW 03 Kebonagung)

Semua perubahan penting pada aplikasi **WargaKITA** dicatat secara mendetail dalam file ini.

---

## [v1.14.1] - 2026-11-20 (Latest)

### Ditambahkan
- **6 Ekspresi Lucu & Dinamis Maskot NawakAI (`getNawakSvg`):**
  - Maskot Wajik Biru kini otomatis berganti ekspresi sesuai momen: **Welcome Ceria** (`welcome`), **Lagi Mikir** (`thinking`), **Selesai & Bangga** (`proud`), **Kaget Pending Approval** (`shocked` + lencana `!`), **Pesta Ultah & Agenda** (`party` + topi pesta & konfeti), dan **Siaga Update** (`update` + panah `↑`).
- **Efek Suara Unik per Notifikasi (*Web Audio Synthesizer*):**
  - Nada sintesis ringan tanpa file MP3 eksternal yang berbeda di setiap kejadian: *welcome* (*"Ting-tong-bloop!"*), tips pendataan (*"Pop-pik!"*), berpikir (*"Hmm-tick"*), jawaban selesai (*"Ding-spark!"*), pending approval (*"Triing-koing!"*), ulang tahun/agenda (*"Ta-da!"*), dan wajib update (*"Beep-boop-up!"*).
- **Suara Bicara Bahasa Jawa Otomatis (`TextToSpeech` Android + Web):**
  - Menambahkan `AndroidBridge.speakJavanese` & `stopSpeak` di `MainActivity.kt` (plus fallback `speechSynthesis` untuk web) agar setiap *welcome message* dan *popup* tips NawakAI langsung dibacakan dengan suara berbahasa Jawa-Malangan.
  - Dilengkapi tombol sakelar **Speaker On/Mute** (`🔊/🔇`) di header jendela NawakAI.
- **Pembacaan Dokumen Lokal Tanpa Kuota API (`Tesseract.js` + `PDF.js`) & *Fuzzy Compare* KK:**
  - Ketika kuota API Gemini gratis sedang habis, NawakAI otomatis beralih membaca lampiran **PNG, JPG, dan PDF** secara lokal di perangkat (memakai gambar resolusi penuh + *preprocessing* kontras tinggi 2× serta pemindaian khusus area atas KK).
  - Menambahkan `compareRowsWithWargaKITA` dengan *fuzzy matching* (NIK, tanggal lahir, nama anggota, dan Kepala Keluarga) untuk membandingkan hasil scan terhadap database WargaKITA—data yang sudah ada otomatis dilewati, sedangkan data baru siap diinput dengan persetujuan pengguna.
- **Memori & Riwayat Percakapan Per Akun (`wk_nawak_history_*` & `wk_nawak_memory_*`):**
  - Riwayat chat dan catatan penting tersimpan otomatis per pengguna dan dikirim kembali ke NawakAI (`12` pesan terakhir + memori penting) agar NawakAI langsung memahami konteks tanpa mengulang pertanyaan.
- **Mode Wajib Update + Notifikasi HP Android Langsung:**
  - Saat versi baru tersedia di GitHub Releases, aplikasi langsung mengirimkan notifikasi ke HP Android (`sendPhoneNotif`) dan menampilkan modal **Wajib Update Aplikasi** (*non-dismissible*, tanpa tombol *Nanti Saja*) lengkap dengan rincian *changelog* terbaru.
- **Dukungan Lampiran Multimodal & Tempel Data Mentah di Chat NawakAI:**
  - Mendukung tempel data mentah (`|`, `;`, tab, atau CSV) langsung di chat untuk diimpor ke database (khusus **Developer Admin** & **Pak RT**), serta pembaruan `nawak-ai-edge-function.ts` untuk ekstraksi Scan KK/PDF multimodal.

### Diperbarui
- **Akses Database Penuh Tanpa Batas 15 KK (`buildNawakContext`):**
  - Menghapus pembatasan `.slice(0, 15)` sehingga NawakAI membaca seluruh KK aktif (Hijau, Kuning, Merah), seluruh daftar warga, seluruh transaksi pending, agenda, dan ulang tahun, disertai sinkronisasi langsung dari Supabase sebelum menjawab.
- **Tampilan Proses Berpikir NawakAI 100% ala Gemini AI:**
  - Menghapus seluruh border kotak di sekitar proses berpikir (`border: none`), menampilkan garis vertikal tipis `1px` dengan tahapan nyata secara *real-time* (*Sinkronisasi data terbaru...* → *Maca seluruh KK & warga...* → *Ngolah jawaban...*), dan ditutup ikon centang **`Selesai`**.
- **Alur Lampiran File Lebih Rapi (Ikon Klip Saja + Ditahan Sebelum Dikirim):**
  - Menghapus teks `"Import File"` sehingga hanya tampil ikon klip (`📎`), dan menahan file lampiran sebagai kartu pratinjau di atas kotak ketik sampai pengguna selesai mengetik perintah lalu menekan **Kirim**.
- **Pemahaman Maksud Lokal Lebih Cerdas:**
  - Mesin lokal kini mengenali pertanyaan alamat warga (termasuk pencarian alamat di luar Kebonagung / nama jalan), rincian status iuran (Hijau/Kuning/Merah), dan panduan upload Scan KK/PDF.
- **Pembaruan Versi Build Android (`v1.14.1`):**
  - Menaikkan `versionCode = 18` (`versionName = "1.14.1"`) di `android/app/build.gradle.kts` dan memperbarui `.github/workflows/build-apk.yml` ke rilis `v1.14.1`.

### Diperbaiki
- **Responsif Terhadap Keyboard Android di Semua Layar:**
  - Menambahkan `android:windowSoftInputMode="adjustResize"` di `AndroidManifest.xml`, penanganan `WindowInsets.Type.ime()` pada `MainActivity.kt`, tinggi modal `88dvh`, serta `scrollIntoView` otomatis saat input difokuskan agar kolom ketik tidak tertutupi keyboard.
- **Perbaikan Fungsi Notifikasi Update (`sendAndroidNotification`):**
  - Menghubungkan `sendAndroidNotification` ke `sendPhoneNotif` agar notifikasi pembaruan aplikasi benar-benar masuk ke sistem notifikasi Android.
- **Penghapusan Pesan Error Teknis Kuota API:**
  - Menghilangkan tampilan error mentah (`quota exceeded` / *high demand*) dari chat pengguna saat lampiran diproses secara lokal.
- **Kontras Teks Proses NawakAI di Mode Gelap:**
  - Menghapus latar abu-abu terang pada kotak proses sehingga teks selalu jelas di mode Terang maupun Gelap.

---

## [v1.14.0] - 2026-11-20

### Ditambahkan
- **Asisten Pintar NawakAI (Gemini AI + Supabase Edge Function `nawak-ai`):**
  - Integrasi penuh dengan Supabase Edge Function `nawak-ai` menggunakan native `Deno.serve` dan mekanisme *fallback* model berantai (`gemini-3.8-flash` → `gemini-flash-latest` → `gemini-3-flash-preview`) serta `thinkingConfig: { thinkingBudget: 0 }` untuk respons cepat.
- **Ikon Melayang Wajik Biru Interaktif (*Draggable*) & Gelembung Sapaan Mengikuti Posisi:**
  - Maskot **NawakAI** bergaya Flat UI (`#1d4ed8` dan `#60a5fa`) dengan posisi tersimpan otomatis di `localStorage` (`wk_nawak_pos`).
  - Gelembung sapaan (*welcome bubble* kontekstual Pagi/Siang/Sore/Malam & situasi RT 17) dilengkapi ekor *speech bubble* yang otomatis bergerak mengikuti posisi ikon NawakAI secara *real-time*.
- **Import File Data Warga via NawakAI (Khusus Developer & Pak RT):**
  - Tombol lampiran file (`.xlsx`, `.xls`, `.csv`) di dalam jendela chat NawakAI dengan pembatasan hak akses ketat hanya untuk **Developer Admin** dan **Pak RT**.
- **Export Data Warga 4 Format Lengkap via NawakAI (`exportWargaByFormat`):**
  - Ekspor data warga menyeluruh dalam format **PNG**, **Excel (`.xlsx`)**, **Word (`.doc`)**, dan **PDF** menggunakan struktur 7 kolom lengkap yang identik dengan Menu Lainnya.
- **Notifikasi HP Berkala (Pending Approval 1 Jam Sekali + Agenda & Ultah):**
  - Pengingat otomatis ke notifikasi HP Android setiap minimal **1 jam sekali** bagi **Pak RT**, **Wakil RT**, dan **Developer** selama masih ada transaksi **Pending Approval**, serta pengingat **Agenda Resmi RT (H-3 hingga H-0)** dan **Ulang Tahun Kepala Keluarga**.
- **Kartu Edit Profil Pengguna (Flat UI):**
  - Tombol dan modal **Edit Profil Saya** pada kartu profil di tab **Lainnya** untuk memperbarui Nama Tampilan, No. HP / WhatsApp, dan Keterangan/Jabatan via `supabase.auth.updateUser` + sinkronisasi lokal.
- **Akses Login Khusus 4 Bendahara RT (Auto-Default & Terkunci):**
  - Pengisian otomatis dan penguncian kategori iuran serta nama petugas untuk **Novan Hermawan** (*17 Agustus*), **Ahmad Abin** (*Keuangan Social*), **A. Teguh Purwanto** (*Pembangunan + Voging*), dan **Jodik Purwanto** (*Rukem + Juri Kunci*), serta penambahan keempatnya ke menu **Ubah Password User Lain**.
- **Desain Baru Kartu Ucapan Ulang Tahun (3 Tema Flat UI):**
  - Pilihan tema **Biru RT 17**, **Navy Emas**, dan **Hijau Segar** dengan ilustrasi kue & konfeti vektor, unduh PNG, dan kirim langsung ke WhatsApp warga.

### Diperbaiki
- **Sinkronisasi Edit Alamat Warga ke Data KK:** Memperbarui `households.address` saat alamat warga diedit.
- **Perbaikan Tombol Edit Profil, Ubah Password User Lain & Reset Data Bulanan:** Memindahkan blok skrip yang sempat tersisip di `<head>` ke dalam `<script>` utama.
- **Ikon Logo Base64, Ikon Mata Coret Login, Anti *Ghost-Click* `320ms`, & *Silent Local Fallback*.**

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
- **Supabase Storage untuk Bukti Transaksi (`receipts`):** Foto bukti pembayaran iuran dan nota kas otomatis diunggah ke Supabase Storage dengan struktur folder terstandar.
- **Petugas Pemeriksa & Penginput Tagihan (A–Z, Wajib):** Pilihan wajib petugas pada pencatatan iuran serta pencatatan kas oleh Bendahara/Sekretaris.
- **Import & Export Massal Database Warga:** Fitur unduh **Template Excel (`.xlsx`)**, **Export Database Warga Global**, dan **Import Massal Warga (`.xlsx` / `.csv`)**.
- **Sinkronisasi Kalender HP & Pengingat Kegiatan:** Tombol **Simpan ke HP** pada setiap agenda di **Kalender Kegiatan Warga**.
- **Tema Aplikasi (Terang, Gelap, Ikuti Device)**, **Cek Update Otomatis via GitHub Releases**, dan **Pop-up Detail Riwayat Aktivitas Akun**.

### Diperbaiki
- **Alur Persetujuan (*Pending Approval*) Bendahara & Sekretaris**, **Pembersihan KK Kosong (`0 anggota`)**, dan **Stabilitas Navigasi Bawah & Modal**.

---

## [v1.11.0] - 2026-11-19

### Ditambahkan
- **Hubungan Keluarga KK Lengkap:** Memperluas pilihan hubungan keluarga di form **Warga Baru**, **Warga Lama**, dan **Edit Warga** (`Kepala Keluarga`, `Suami`, `Istri`, `Pasangan`, `Anak`, `Orang Tua`, `Mertua`, `Menantu`, `Cucu`, `Famili Lain`, `Lainnya`).

---

## [v1.10.0] - 2026-11-18

### Ditambahkan
- **Pemisahan Input Warga Baru (1 KK) & Warga Lama (KK Ada)**, **Pop-up Detail Transaksi & Hapus Warga**, **Export Laporan PDF/PNG & Excel Warga**, serta **Izin Kamera Langsung & Notifikasi Android**.

---

## [v1.0.0] - 2026-11-17

### Ditambahkan
- Rilis awal **WargaKITA** (Flat UI) dengan 5 tab utama (**Dashboard**, **Data Warga**, **Data KK**, **Cash RT**, **Lainnya**), autentikasi role `@wargakita.id` terintegrasi Supabase, **Kalender Kegiatan Warga**, serta fitur **Catat Bayar** (QRIS a.n. Fibriono, Transfer Bank, Tunai).
