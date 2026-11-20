# Changelog — WargaKITA (RT 17 / RW 03 Kebonagung)

Semua perubahan penting pada aplikasi **WargaKITA** dicatat secara mendetail dalam file ini.

---

## [v1.14.2] - 2026-11-20 (Latest)

### Ditambahkan
- **Tampilan Chat NawakAI 100% Bergaya Gemini:**
  - Palet warna adaptif Tema Terang & Gelap (OLED `#000000`, *bubble* user `#28292c`, kapsul input `#1e1f20`), teks balasan tanpa kotak pembatas, dan kontras langkah proses selesai (redup) vs langkah aktif (terang).
  - Header modal menampilkan ikon resmi **Wajik NawakAI + teks `NawakAI RT 17`** tepat di tengah header dengan warna yang otomatis menyesuaikan tema Terang maupun Gelap.
  - Loader animasi wajah NawakAI (hanya mata berkedip + senyum putih) pada bagian `Sedang berpikir`, garis vertikal `1px`, dan ikon centang **`Selesai`** yang sejajar presisi dalam kolom tetap `16px`.
- **Input Multi-Baris, Minimize Bubble & Preview Lampiran ala Gemini:**
  - Kotak ketik `<textarea>` transparan yang tumbuh otomatis ke atas dengan *placeholder* berbahasa Jawa (**`Takok opo wae nang NawakAI...`**) di mana tombol `Enter` membuat baris baru tanpa langsung mengirim.
  - Pesan user panjang (>4 baris atau >110 karakter) otomatis diringkas dengan tombol buka/tutup (`▾/▴`).
  - Tombol lampiran **`+`** ala Gemini yang menampilkan pratinjau thumbnail gambar atau kartu dokumen lengkap dengan tombol silang **`✕`** langsung di bagian dalam atas kotak input.
- **Dukungan Lampiran Semua Jenis File & Input Database Otomatis:**
  - Menghapus batasan format dan hak akses pada tombol `+` sehingga semua pengguna dapat melampirkan **PNG, JPG, WEBP, PDF (multi-halaman hingga 10 halaman + fallback OCR), Excel/CSV/ODS (membaca seluruh sheet), Word (`.docx` via Mammoth.js & `.doc`), serta file teks/data (`TXT`, `MD`, `JSON`, `XML`, `LOG`)**.
  - NawakAI menyajikan data terstruktur, merangkum, dan menyimpulkan isi file secara detail, serta otomatis memunculkan tombol konfirmasi **`Jalankan Import Sekarang`** (anti-duplikat) jika file mengandung data warga/KK dan akun memiliki izin pengurus (`Developer`, `Pak RT`, `Wakil RT`).

### Diperbarui
- **Sinkronisasi Salam Masuk Sesuai Jam WIB Malang & Pengingat Berkala:**
  - Mengunci perhitungan jam ke zona waktu **WIB (`Asia/Jakarta`)** wilayah Malang dan menyinkronkan teks *bubble* dengan isi tepat dari 3 audio sapaan ElevenLabs (**04.00–10.59 WIB** *Sugeng enjang* / `welcome_pagi`, **11.00–16.59 WIB** *Sugeng siang* / `welcome_siang`, **17.00–03.59 WIB** *Sugeng dalu* / `welcome_malam`).
  - Saat pertama kali masuk aplikasi, NawakAI kini **selalu** mengucapkan salam sesuai waktu, sedangkan pengingat **Agenda Penting RT** & **Ulang Tahun Kepala Keluarga** dipindahkan ke pengecekan berkala di dalam aplikasi (maksimal 1 kali per hari).
- **Suara Tips Tab Data KK & Cash RT:**
  - Mempersingkat jeda ulang pemutaran suara tips kontekstual menjadi 60 detik saat berpindah ke tab **Data Warga**, **Data KK**, dan **Cash RT**.
- **Pembaruan Versi Build Android (`v1.14.2`):**
  - Menaikkan `versionCode = 19` (`versionName = "1.14.2"`) di `android/app/build.gradle.kts`, memperbarui `APP_VERSION` di `index.html` & `assets/wargakita.html`, serta menyesuaikan `.github/workflows/build-apk.yml`, `README.md`, dan `CHANGELOG.md`.

### Diperbaiki
- **Latar Belakang Textarea Input NawakAI di Mode Gelap:**
  - Menimpa aturan global `html.wk-dark textarea` agar latar belakang `#nawakInput` transparan penuh dan menyatu rapi di dalam kapsul `#1e1f20`.
- **Perataan Ikon Centang `Selesai` pada Timeline Proses:**
  - Menyamakan lebar wadah titik langkah dan ikon centang `Selesai` (`w-4 h-4`) agar lurus sejajar secara vertikal.

---

## [v1.14.1] - 2026-11-20

### Ditambahkan
- **7 Audio Suara ElevenLabs MP3 Tertanam (*Offline*):**
  - Menanamkan 7 file MP3 ElevenLabs berbahasa Jawa-Malangan langsung ke `NAWAK_AUDIO` (`welcome_pagi`, `welcome_siang`, `welcome_malam`, `pending`, `party`, `tips`, `update`) lengkap dengan `mediaPlaybackRequiresUserGesture = false` di `MainActivity.kt`.
- **6 Ekspresi Lucu & Dinamis Maskot NawakAI (`getNawakSvg`):**
  - Maskot Wajik Biru otomatis berganti ekspresi sesuai momen: **Welcome Ceria** (`welcome`), **Lagi Mikir** (`thinking`), **Selesai & Bangga** (`proud`), **Kaget Pending Approval** (`shocked` + lencana `!`), **Pesta Ultah & Agenda** (`party` + topi pesta & konfeti), dan **Siaga Update** (`update` + panah `↑`).
- **Efek Suara Unik per Notifikasi (*Web Audio Synthesizer*) & Sakelar Speaker (`🔊/🔇`):**
  - Nada sintesis ringan yang berbeda di setiap kejadian beserta tombol mute/unmute di header NawakAI.
- **Grafik Donat *Soft-Track* + Badge Persentase Mengambang, 3 Bahasa, Material You & Paginasi 10 Item:**
  - Menambahkan grafik donat proporsi kategori dengan *badge* persentase mengambang, pemilih **3 bahasa** (`Indonesia`, `English`, `Basa Jawa`), palet dinamis **Material You** Android (`AndroidBridge.getDevicePaletteJson`), serta paginasi 10 item (`Prev / Next`) pada **Riwayat Transaksi Detail** dan **Riwayat Aktivitas Akun**.
- **Pembacaan Dokumen Lokal Tanpa Kuota API (`Tesseract.js` + `PDF.js`) & *Fuzzy Compare* KK:**
  - Fallback OCR lokal resolusi tinggi dan pencocokan *fuzzy* (`compareRowsWithWargaKITA`) anti-duplikat terhadap database WargaKITA.
- **Memori & Riwayat Percakapan Per Akun (`wk_nawak_history_*` & `wk_nawak_memory_*`) & Akses Mutasi Terkonfirmasi:**
  - Riwayat chat tersimpan per pengguna, dukungan akses mutasi database penuh (pelunasan KK, approval massal, hapus warga) khusus **Developer Admin**, **Pak RT**, dan **Wakil RT** dengan tombol konfirmasi wajib.
- **Mode Wajib Update + Pengecekan Latar Belakang 2 Jam (`UpdateCheckReceiver.kt`) + Notifikasi HP Android:**
  - Pengecekan rilis GitHub otomatis di latar belakang dan modal **Wajib Update** lengkap dengan rincian *changelog* terbaru.

### Diperbarui
- **Akses Database Penuh Tanpa Batas 15 KK (`buildNawakContext`)** dan sinkronisasi *real-time* Supabase sebelum menjawab.
- **Versi Build Android (`v1.14.1`):** `versionCode = 18`, `versionName = "1.14.1"`.

### Diperbaiki
- **Responsif Terhadap Keyboard Android di Semua Layar:** `adjustResize`, `WindowInsets.Type.ime()`, `--vvh`, dan `scrollIntoView`.
- **Perbaikan Fungsi Notifikasi Update (`sendAndroidNotification`)** dan penghapusan pesan error kuota API mentah.

---

## [v1.14.0] - 2026-11-20

### Ditambahkan
- **Asisten Pintar NawakAI (Gemini AI + Supabase Edge Function `nawak-ai`):**
  - Integrasi penuh dengan Supabase Edge Function `nawak-ai` menggunakan native `Deno.serve` dan mekanisme *fallback* model berantai (`gemini-3.8-flash` → `gemini-flash-latest` → `gemini-3-flash-preview`) serta `thinkingConfig: { thinkingBudget: 0 }` untuk respons cepat.
- **Ikon Melayang Wajik Biru Interaktif (*Draggable*) & Gelembung Sapaan Mengikuti Posisi:**
  - Maskot **NawakAI** bergaya Flat UI (`#1d4ed8` dan `#60a5fa`) dengan posisi tersimpan otomatis di `localStorage` (`wk_nawak_pos`).
- **Import File Data Warga via NawakAI & Export Data Warga 4 Format Lengkap (`PNG`, `Excel`, `Word`, `PDF`).**
- **Notifikasi HP Berkala (Pending Approval 1 Jam Sekali + Agenda & Ultah).**
- **Kartu Edit Profil Pengguna (Flat UI), Akses Login Khusus 4 Bendahara RT, & Desain Baru Kartu Ucapan Ulang Tahun (3 Tema Flat UI).**

### Diperbaiki
- **Sinkronisasi Edit Alamat Warga ke Data KK**, perbaikan tombol Edit Profil / Ubah Password / Reset Bulanan, ikon logo Base64, ikon mata coret login, anti *ghost-click* `320ms`, dan *silent local fallback*.

---

## [v1.13.3] - 2026-11-19

### Diperbaiki
- **Ikon Klip Bukti Transaksi**, penghapusan kilatan latar putih mode gelap, dan pembatasan lebar pop-up Detail Transaksi.

---

## [v1.13.2] - 2026-11-19

### Diperbarui
- **Desain Ikon Vektor Flat UI Penuh**, ikon aplikasi presisi & *full-bleed* (`mdpi`–`xxxhdpi`), perbaikan kalender, penghapusan garis scrollbar, dan jarak aman *status bar* HP.

---

## [v1.13.1] - 2026-11-19

### Ditambahkan & Diperbaiki
- **Kalender Ulang Tahun Warga Permanen**, tombol **Telepon & WhatsApp Kepala Keluarga** di Detail KK, warna tab filter status warga, dan ikon mata login tercentang.

---

## [v1.13.0] - 2026-11-18

### Ditambahkan
- **Fondasi Aplikasi Android & Web WargaKITA (Flat UI + Supabase):** Dashboard KPI, Grafik Pemasukan 6 Bulan, Kalender Agenda Resmi RT, Data Warga & KK, Catat Iuran (QRIS / Transfer / Tunai), Cash RT (*Pending Approval* + Bukti Storage), Laporan Keuangan 1 Bulan (PNG/PDF), Rekap Kas Tahunan, Import/Export Excel, dan GitHub Actions APK Release.
