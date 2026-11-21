# Changelog — WargaKITA (RT 17 / RW 03 Kebonagung)

Seluruh riwayat penambahan fitur, pembaruan antarmuka, dan perbaikan *bug* pada aplikasi **WargaKITA** diklasifikasikan secara terperinci pada dokumen ini.

---

## [v1.14.3] - 2026-11-21 (Latest)

### ✨ Ditambahkan (New Features)
- **QRIS Layar Penuh Ala Perbankan & Kecerahan Layar Otomatis (`MainActivity.kt`):**
  - Menambahkan modal layar penuh `showQrisFullscreenModal()` untuk pembayaran **QRIS DANA (`085607686867` a.n. `FIBRIONO`)** dengan kartu putih kontras tinggi, tampilan nominal otomatis, tombol salin nomor DANA, serta opsi **Ganti QRIS Asli** untuk menyimpan tangkapan layar QRIS resmi.
  - Menambahkan `AndroidBridge.setMaxScreenBrightness(enable: Boolean)` di `MainActivity.kt` agar kecerahan layar HP Android otomatis naik ke 100% saat QRIS dibuka dan kembali normal saat ditutup.
- **Manajemen User Login & 10 Centang Hak Akses Granular (`showManageUsersModal`):**
  - Menambahkan fitur **Kelola User Login** di **Panel Khusus Developer** untuk melihat seluruh daftar akun (`developer_list_users()`), membuat akun login baru (`developer_create_user()`), mengedit nama/email/role/sandi (`developer_update_user()`), dan menghapus akun (`developer_delete_user()`).
  - Menambahkan **10 centang hak akses granular** (`custom_permissions`) yang selaras dengan kolom `ROLE_META` (*Lihat Warga & KK, Tambah Warga, Edit Warga, Hapus Warga, Catat Bayar Iuran, Catat Kas RT, Approval Kas, Edit Saldo Awal, Kelola Agenda RT, Lihat NIK Penuh*) dan otomatis digabung pada `getPerms()`.
- **Status Kependudukan `Meninggal Dunia`:**
  - Menambahkan opsi **`Meninggal Dunia`** pada modal **Tinjau / Edit Data Warga**, filter dan kartu **Data Warga**, **Detail KK** (header & lencana anggota keluarga), serta daftar **Data KK & Iuran** (`MENINGGAL DUNIA`, otomatis masuk kategori *Tanpa Tagihan* agar tidak ditagih iuran bulanan).
- **Pop-up Pilihan & Konfirmasi *Frosted-Blur* Flat UI Universal:**
  - Menambahkan `openBlurSelectPicker()` yang menggantikan seluruh dialog `<select>` bawaan Android dengan pop-up *frosted-blur* (`backdrop-filter: blur(12px)`) selaras tema Terang/Gelap.
  - Menambahkan `showConfirmModal()` untuk konfirmasi tindakan penting database (*Hapus Akun User Login*, *Hapus Agenda Resmi RT*, *Setujui/ACC & Tolak/Reject Transaksi Kas*).
- **Template Ucapan Ulang Tahun Sesuai Usia & Ekspansi Kategori Pemasukan:**
  - Menambahkan dropdown **6 template ucapan ulang tahun** (*Resmi & Hangat*, *Hormat & Doa Sesepuh ≥55 Th*, *Santai Usia Muda*, *Lucu Malangan*, *Lucu Awet Muda*, *Islami*) yang otomatis menyesuaikan usia warga terpilih.
  - Memperluas pilihan kategori pemasukan di **Catat Transaksi Kas RT** dari 4 menjadi **10 kategori** yang umum digunakan di lingkungan RT.
- **Dukungan Lampiran Semua Jenis File di NawakAI:**
  - Membuka tombol `+` NawakAI untuk semua pengguna dan mendukung pembacaan **PNG, JPG, WEBP, PDF (hingga 10 halaman + OCR), Excel/CSV/ODS (seluruh sheet), Word (`.docx` via Mammoth.js & `.doc`), serta TXT/MD/JSON/XML/LOG**, lengkap dengan penyajian data, rangkuman detail, dan deteksi otomatis tombol import database.
- **Berbagi Laporan Multi-Halaman PNG (`saveAndShareMultipleBase64`):**
  - Menambahkan `AndroidBridge.saveAndShareMultipleBase64()` menggunakan `Intent.ACTION_SEND_MULTIPLE` di `MainActivity.kt` agar seluruh halaman PNG laporan keuangan dapat dibagikan sekaligus dalam 1× ketuk.

### 🔄 Diperbarui (Improvements & UI/UX)
- **Tampilan Chat NawakAI Bergaya Gemini:**
  - Mengubah tata letak chat NawakAI dengan palet adaptif Tema Terang & OLED Gelap (`#000000`, *bubble* user `#28292c`, kapsul input `#1e1f20`), ikon resmi **Wajik NawakAI + `NawakAI RT 17`** di tengah header yang menyesuaikan tema, loader wajah (mata + senyum putih saja), garis vertikal `1px` hingga ikon centang **`Selesai`**, *minimize/expand* pesan panjang (`▾/▴`), kotak ketik `<textarea>` multi-baris (`Takok opo wae nang NawakAI...`), dan pratinjau lampiran di dalam bagian atas kapsul input.
- **Redesign Panel Khusus Developer (Flat UI) & Dropdown Bahasa Atas (`ID / EN / JV`):**
  - Memperbarui **Panel Khusus Developer** dengan header biru solid Flat UI berikon gembok + lencana `SUPERUSER` dan teks tombol **`Ubah Password User`**.
  - Memindahkan pemilih bahasa menjadi dropdown ringkas di sudut kanan atas **Lainnya & Laporan** serta melengkapi terjemahan `ID`, `EN`, dan `JV`.
- **Grafik Donat Seluruh Kategori Pemasukan & Rekap Kas Fokus Bulan Aktif:**
  - Memperbarui `getGlobalSummary()` (`allIncomeByCat`), `renderCashRT()`, dan `cashDonutChart` agar seluruh kategori pemasukan tambahan (seperti *Sumbangan / Donasi Warga*) tampil dengan warna dan persentase masing-masing.
  - Menyorot **Bulan Aktif** pada kartu **Rekap Kas Tahunan** di bagian teratas dengan tombol lipat **`Lihat Semua 12 Bulan ▾`**.
- **Sinkronisasi Sapaan NawakAI WIB Malang & Aturan Jeda Tips Tab:**
  - Mengunci jam sapaan awal ke zona waktu **WIB (`Asia/Jakarta`)** agar teks *bubble* dan 3 audio ElevenLabs (`welcome_pagi`, `welcome_siang`, `welcome_malam`) cocok persis, sementara pengingat Agenda RT & Ulang Tahun dijalankan berkala (1×/hari).
  - Mengatur suara tips tab **Data Warga** menjadi **1×24 jam**, membisukan tab **Data KK** dan **Cash RT** (popup teks saja), dengan jeda global 2 jam & per-tab 6 jam.
- **Versi Build Android (`v1.14.3`):**
  - Menaikkan `versionCode = 20` (`versionName = "1.14.3"`) di `android/app/build.gradle.kts`, `APP_VERSION` di HTML, dan `.github/workflows/build-apk.yml`.

### 🐛 Diperbaiki (Bug Fixes)
- **Tombol Hapus User Tidak Merespons & Error `column "created_by" does not exist`:**
  - Mengganti `window.confirm()` (yang diblokir diam-diam oleh WebView Android) dengan `showConfirmModal()`, serta memperbarui `developer_delete_user()` menggunakan introspeksi dinamis `pg_constraint` untuk membersihkan seluruh *foreign key* secara aman sebelum menghapus dari `profiles` dan `auth.users`.
- **Daftar Akun Kosong `(0)` di Menu Kelola User:**
  - Menambahkan RPC `developer_list_users()` (`SECURITY DEFINER`) yang melakukan *join* antara `public.profiles` dan `auth.users` agar seluruh akun selalu terbaca tanpa terhalang RLS maupun error kolom `email` yang tidak ada di `profiles`.
- **Grafik Donat Cash RT Hilang & Tooltip Menutupi Total Tengah:**
  - Menambahkan deklarasi `const s = getGlobalSummary()` di dalam blok `donutCanvas` pada `initCharts()` sehingga grafik donat tidak lagi gagal dirender, dan menempatkan *tooltip* di tepi luar irisan (`wkOuter`) agar tidak menimpa angka total di tengah.
- **Duplikasi Baris Pemasukan & Ketidaklengkapan Laporan Share PNG/PDF:**
  - Menghapus baris duplikat `s.otherIncome` pada **Laporan Keuangan 1 Bulan** & PDF, serta menyertakan seluruh kategori pemasukan (`allIncomeByCat`) dan **Daftar KK Belum Lunas** secara penuh ke ekspor multi-PNG dan PDF.
- **Kontras Mode Gelap (Latar `Data Kartu Keluarga Baru`, Textarea NawakAI, & Lencana `Meninggal Dunia`):**
  - Memperbaiki aturan CSS `html.wk-dark [class*="bg-blue-50"]`, menimpa `background: transparent !important` pada `#nawakInput`, menyamakan lebar kolom titik dan centang `Selesai` (`w-4 h-4`), serta menambahkan kelas `.wk-badge-deceased` agar lencana *Meninggal Dunia* kontras di mode Terang maupun Gelap.

### 🗄️ Skema Database
- Menambahkan `supabase/005_developer_user_management.sql` (`custom_permissions` pada `public.profiles` + 4 RPC khusus Developer: `developer_list_users`, `developer_create_user`, `developer_update_user`, `developer_delete_user`).

---

## [v1.14.2] - 2026-11-20

### ✨ Ditambahkan & Diperbarui
- **Integrasi Awal Tampilan Chat NawakAI Bergaya Gemini:** Struktur modal chat gelap OLED/terang adaptif, *bubble* user, loader wajah mata+senyum, dan input multi-baris.
- **Build Android (`v1.14.2`):** `versionCode = 19`, `versionName = "1.14.2"`.

---

## [v1.14.1] - 2026-11-20

### ✨ Ditambahkan & Diperbarui
- **7 Audio Suara ElevenLabs MP3 Tertanam (*Offline*)** & sakelar **Speaker On/Mute (`🔊/🔇`)**.
- **6 Ekspresi Dinamis Maskot NawakAI (`getNawakSvg`)** & efek suara *Web Audio Synth*.
- **Grafik Donat *Soft-Track* + Badge `%` Mengambang**, pemilih **3 Bahasa (`id`/`en`/`jv`)**, palet dinamis **Material You** Android, dan **Paginasi 10 Item** pada riwayat transaksi & aktivitas.
- **Pembacaan Dokumen Lokal Tanpa Kuota (`Tesseract.js` + `PDF.js`)**, *fuzzy compare* anti-duplikat KK, memori percakapan per akun, dan mode **Wajib Update** + pengecekan latar belakang 2 jam (`UpdateCheckReceiver.kt`).

### 🐛 Diperbaiki
- **Responsif Keyboard Android:** `adjustResize`, `WindowInsets.Type.ime()`, `--vvh`, `scrollIntoView`, dan penghapusan pesan error teknis kuota API.

---

## [v1.14.0] - 2026-11-20

### ✨ Ditambahkan & Diperbaiki
- **Asisten Pintar NawakAI** via Supabase Edge Function `nawak-ai` + *draggable floating button* + *export* data warga 4 format (**PNG, Excel, Word, PDF**).
- **Notifikasi HP Berkala** (Pending Approval 1 jam sekali, Agenda H-3..H-0, Ulang Tahun), **Kartu Edit Profil**, akses terkunci **4 Bendahara Khusus**, desain **Kartu Ucapan Ulang Tahun 3 Tema**, serta sinkronisasi edit alamat ke `households`.

---

## [v1.13.0 – v1.13.3] - 2026-11-18 s.d. 2026-11-19
- Fondasi **WargaKITA Flat UI + Supabase**, ikon vektor penuh, *full-bleed launcher icon*, kalender ulang tahun warga permanen, tombol telepon & WhatsApp di Detail KK, perbaikan *status bar padding*, dan perbaikan tampilan pop-up Detail Transaksi.
