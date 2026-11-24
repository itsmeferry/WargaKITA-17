## [1.16.0] — 2026-11-24

### Paket B: Optimasi Kesehatan Aplikasi + Feedback Suara & Aksi Cepat NawakAI
- **Optimasi Kesehatan Aplikasi:**
  - *In-Flight Lock* (`__wkLoadInFlight`) pada `loadAllData()` untuk mencegah permintaan ganda ke Supabase saat refresh cepat.
  - Banner deteksi *Offline* & *Auto-Reconnect* otomatis menyinkronkan data begitu koneksi kembali aktif.
  - Kartu **Kesehatan Sistem WargaKITA** di tab **Lainnya** (khusus Developer / Pak RT / Wakil RT) menampilkan status koneksi, jumlah KK/Warga/Agenda termuat, status Voice TTS, serta tombol **Bersihkan Cache Lokal & Segarkan**.
- **Feedback Suara (TTS) & Aksi Cepat NawakAI:**
  - Tombol toggle **Speaker (🔊 / 🔇)** di header popup NawakAI untuk membacakan jawaban otomatis dalam bahasa Indonesia (`AndroidBridge.speakJavanese` / Web Speech API), berhenti instan saat tombol Henti (`□`) atau tutup ditekan.
  - **Aksi Cepat (*Quick Action Navigation*)**: Perintah suara/teks (*"Buka Cash RT"*, *"Buka Data KK"*, *"Buka Kalender"*) langsung menampilkan tombol navigasi 1 ketuk ke halaman terkait.
- **NLU 5 Lapis Anti-Typo & Slang Jawa-Malangan (`NawakAI`):**
  - Menambahkan kamus normalisasi 150+ singkatan WA, slang Indonesia, dan dialek Jawa-Malangan (`brp`, `jnd`, `rondo`, `drg`, `byr`, `rnda`, `bsk`, `sopo`, `bkn`, dll).
  - Algoritma **Damerau-Levenshtein (≤ 2)** otomatis mengoreksi typo kata perintah & nama warga RT 17 (*"kasiyati"* → *KASIJATI*) beserta catatan pemahaman otomatis.
  - Pemahaman perintah singkat 1–2 kata (*"saldo?"*, *"kas saiki"*, *"ronda besok"*) dan **Disambiguasi Nama Ganda** (tombol pilih 1 ketuk bila ada beberapa warga bernama mirip seperti *"Agus"*).
- **Perbaikan Bug & Optimasi Performa Menyeluruh:**
  - **Lazy-Load Sejati jsPDF & XLSX:** Menghapus muatan paksa `jspdf` dan `xlsx` (~650 KB) dari `<head>` sehingga waktu buka awal aplikasi jauh lebih cepat; modul hanya dimuat saat tombol Export ditekan.
  - **Migrasi Cache Media ke IndexedDB (`WargaKITA_MediaDB`):** Memindahkan penyimpanan lokal bukti transaksi (`wk_receipts_v1`) dan avatar (`wk_avatars_v1`) dari `localStorage` ke `IndexedDB` agar terbebas dari error kuota 5 MB (`QuotaExceededError`).
  - **Perbaikan Kebocoran Event Listener (`__wkNawakDragCleanup`):** Membersihkan listener `mousemove`/`mouseup` pada `window` setiap `render()` agar animasi dan guliran tetap mulus dalam sesi panjang.
  - **Debounced Search Cash RT (Anti Loncat Scroll):** Pencarian transaksi di tab Cash RT kini mempertahankan posisi guliran dan fokus ketik tanpa berkedip.
- **Build Android (`v1.16.0`):** Menaikkan `versionCode = 26`, `versionName = "1.16.0"` di `android/app/build.gradle.kts`, `APP_VERSION` di HTML, dan `.github/workflows/build-apk.yml`.

## [1.15.3] — 2026-11-24

### Tema Perangkat Menyeluruh (Full Material You Dynamic Palette)
- **Palet Warna Perangkat Menyeluruh (`MainActivity.kt` & `index.html`):** Mengambil token warna dinamis Material You Android 12+ secara lengkap (`bgApp`, `bgCard`, `bgSurface`, `border`, `accent`, `accentText`, `accentSoft`, `accentSoftText`, `muted`) dan menerapkannya ke seluruh elemen aplikasi (latar belakang, kartu, `.flat-card-sm`, navigasi bawah, kolom input, *filter pills*, badge, NawakAI popup, tombol FAB, hingga seluruh ikon SVG) dengan spesifisitas CSS penuh (`html.wk-dyn` & `html.wk-dark.wk-dyn`).

### Perbaikan Export, Agenda Ronda & Workflow Rilis
- **Perbaikan Export Word (`.docx` OOXML Asli):** Mengganti format `.doc` HTML dengan generator `.docx` murni (`buildValidDocxBytes`) agar bebas error *file rusak* di Word/Docs Android.
- **Filter Status Warga & Pemisahan Warga Kontrak:** Menambahkan centang filter status warga (*Permanen, Kontrak, Kost, Janda, Pindah Domisili, Meninggal Dunia*) di menu Export NawakAI, memisahkan **Warga Kontrak** ke bagian paling bawah tabel di semua format export, serta menyelaraskan footer **PDF** dengan tata letak 7 kartu KPI milik **PNG**.
- **Perbaikan Bug Versi Kembali ke `v1.15.1`:** Memperbaiki seleksi ZIP pada `.github/workflows/build-apk.yml` dari `ls -t` (yang terpengaruh waktu checkout git) ke pencocokan versi eksplisit + `sort -V | tail -n1`, dan menaikkan versi ke **`v1.15.3` (`versionCode = 25`)**.

## [1.15.2] — 2026-11-23

### Perbaikan Login, Crash Startup & Peningkatan Export
- **Perbaikan Login (`ev is not defined`):** Memperbaiki kondisi render tombol `+ Kegiatan` pada header Kalender Kegiatan yang sebelumnya ikut memakai variabel loop `ev._isRonda`, sehingga login dan render Dashboard berjalan lancar tanpa error.
- **Perbaikan Crash Startup (`MainActivity.kt`):** Memindahkan pemanggilan `requestInitialPermissions()` ke setelah inisialisasi `webView` serta menambahkan pengaman `evalJsSafe()` (`::webView.isInitialized`) pada seluruh *callback* izin kalender dan `SpeechRecognizer` agar aplikasi tidak pernah mengalami `UninitializedPropertyAccessException` saat baru diinstal ulang.
- **Export Word `.docx` Asli:** Mengganti format `.doc` HTML dengan generator `.docx` OOXML asli (`buildValidDocxBytes`) agar terbuka mulus di Word, Google Docs, dan WPS Office Android.
- **Filter Status Warga & Pemisahan Warga Kontrak:** Menambahkan filter status (*Permanen, Kontrak, Kost, Janda, Pindah Domisili, Meninggal Dunia*) pada menu export NawakAI serta memisahkan **Warga Kontrak** ke bagian paling bawah tabel di semua format (**PNG, Excel, `.docx`, PDF**).
- **Footer PDF Serasi PNG:** Menyamakan footer laporan **PDF** dengan desain **7 kartu KPI berwarna + kotak kesimpulan** milik **PNG**.
- **Kerapian Agenda Resmi RT:** Menambahkan `whitespace-pre-line` pada daftar nama ronda dan menyembunyikan tombol `Edit`/`Hapus` untuk jadwal ronda bawaan Desember 2026.
- **Build Android (`v1.15.2`):** Menaikkan `versionCode = 24`, `versionName = "1.15.2"` di `android/app/build.gradle.kts`, `APP_VERSION` di HTML, dan `.github/workflows/build-apk.yml`.

## [1.15.1] — 2026-11-23

### NawakAI Voice Inline ala Gemini & Tombol Henti Paksa (`□`)
- **Inline `SpeechRecognizer` + Live Waveform 14 Bar (`MainActivity.kt` & `index.html`):** Mengganti popup sistem Google dengan pengenalan suara *in-app* senyap yang mengalirkan level audio (*RMS*) secara langsung ke 14 bar *waveform* ala Gemini di dalam kapsul *chatbox*, lengkap dengan transkrip *real-time* dan eksekusi perintah otomatis.
- **Tombol Henti Paksa (`□`):** Menambahkan tombol kotak `□` dengan `AbortController` dan pembatalan run aktif (`activeNawakRunId`) di seluruh perintah NawakAI agar pengguna dapat menghentikan paksa proses yang sedang berjalan kapan saja.

### Jadwal Ronda Desember 2026 & Sinkronisasi Kalender HP Otomatis
- **Jadwal Ronda 1–31 Desember 2026 (`RONDA_DES_2026`):** Memasukkan rolling 7 Regu + 2 personel Kost per malam lengkap dengan daftar nama warga ke Kalender Dashboard & NawakAI.
- **Notifikasi Harian Jam 19:00 WIB (`RondaReminderReceiver.kt`):** Mengirim pengingat otomatis ke HP Pak RT / Wakil RT / Developer setiap pukul 19:00 WIB selama Desember 2026 dan memasang alarm kalender HP 210 menit sebelum ronda dimulai.
- **Sinkronisasi Otomatis & Auto-Hapus Selesai Event (`04:00 WIB`):** Menghapus tombol manual `Simpan ke HP`, menyinkronkan kalender HP otomatis saat install, update APK, dan refresh data, serta menghapus otomatis jadwal yang sudah selesai (`DTEND <= nowMs`).
- **Build Android (`v1.15.1`):** Menaikkan `versionCode = 23`, `versionName = "1.15.1"` di `android/app/build.gradle.kts`, `APP_VERSION` di HTML, dan `.github/workflows/build-apk.yml`.

## [1.15.0] — 2026-11-23

### NawakAI Agent AI (UI Baru, Voice Command & 24 Skill)
- **Antarmuka Popup Messenger + Chatbox Gemini (Flat UI):**
  - Popup mengambang dengan animasi *spring* GPU 60fps bebas kedip, ikon wajik NawakAI 6 ekspresi dinamis di puncak kartu, layar Beranda grid 2×2 (*Ringkasan Kas*, *Cek Tunggakan*, *Data Janda*, *Export Warga*), dan grid kolom export 2 kolom yang rapi.
- **Voice Command Native Android & Web (`id-ID`):**
  - Menambahkan `RecognizerIntent` (`startVoiceInput()`) serta `onPermissionRequest` di `MainActivity.kt` dan izin `RECORD_AUDIO` di `AndroidManifest.xml`, dilengkapi animasi *equalizer* datar saat mendengarkan perintah suara.
- **24 Skill Agentic di 6 Domain Operasional:**
  - *Multi-turn working memory* (revisi draft otomatis), audit klasifikasi KK & kebersihan data NIK/HP, catat pembayaran via suara/chat, simulasi proyeksi saldo kas (*what-if*), audit transaksi *pending*, generator draft pengumuman WA detail (Laporan Kas lengkap Pemasukan & Pengeluaran, Acara, Tahlil Rutinan, Berita Duka/Lelayu, Kerja Bakti), pembuat draf Surat Pengantar RT 17 otomatis, pengingat WA personal santun, jadwal ronda, tambah agenda kalender, log aduan warga, dan paket laporan rapat RT.

### Perbaikan Klasifikasi Janda, Iuran Tahunan & Tunggakan
- **Kepala Keluarga Efektif (`getEffectiveHeadMember`):** Otomatis memprioritaskan nama **Istri aktif** di Data KK, Detail KK, Cash RT, NawakAI, dan Export Warga apabila Kepala Keluarga sebelumnya berstatus *Meninggal Dunia*.
- **Deduplikasi Tagihan Tahunan:** Memastikan 1 tahun kalender hanya menghitung maksimal 1 tagihan per kategori dan badge **Tunggakan** hanya tampil untuk piutang lintas tahun (`>= 2` tahun berbeda seperti `2025 & 2026`).
- **Build Android (`v1.15.0`):** Menaikkan `versionCode = 22`, `versionName = "1.15.0"` di `android/app/build.gradle.kts`, `APP_VERSION` di HTML, dan `.github/workflows/build-apk.yml`.

## [1.14.4] — 2026-11-22

### ✨ Ditambahkan (New Features)
- **Checkbox Kolom & Export PNG Multi-Halaman (NawakAI):** Pengguna dapat mencentang kolom tertentu (`No / Anggota`, `Alamat`, `Nomor KK`, `NIK`, `Hubungan`, `No. HP`, `Status`) sebelum export ke PNG, Excel, Word, atau PDF. Format PNG otomatis dipecah menjadi beberapa gambar (maks. 28 baris/halaman) agar tetap tajam dan mudah dibaca.
- **Auto-Simpan ke Storage HP & Share Instan (`MainActivity.kt`):** File hasil export otomatis tersimpan ke folder `Download/WargaKITA` (Excel, Word, PDF) atau `Pictures/WargaKITA` (PNG) lalu langsung membuka dialog Bagikan (Share) perangkat Android.
- **QRIS Khusus per Akun Petugas:** Tampilan QRIS dan penyimpanan gambar QRIS (`wk_custom_qris_img_<user_id>`) dipisahkan per akun login sehingga tiap petugas Bendahara hanya menampilkan QRIS atas namanya sendiri.
- **Auto-Remove Agenda (+2 Menit) & Sinkron Kalender HP:** Agenda manual yang melewati waktu jadwalnya + 2 menit otomatis terhapus dari aplikasi, database `community_events`, dan Kalender HP (`CalendarContract`), serta otomatis menyinkronkan agenda dan ulang tahun warga ke Kalender HP.
- **Validasi Wajib Bukti Pembayaran:** Tombol **Simpan Pembayaran** kini mewajibkan lampiran foto bukti pembayaran warga (Galeri / Foto Langsung) dengan penyorotan kotak merah dan pesan peringatan jika belum dilampirkan.
- **Dukungan Android Lawas (`minSdk = 23`):** Menurunkan batas minimum Android dari API 26 (8.0) ke **API 23 (Android 6.0 Marshmallow)** disertai akselerasi hardware WebView (`LAYER_TYPE_HARDWARE`).
- **Migrasi RLS Wakil RT (`supabase/006_wakil_rt_approval_rls.sql`):** Memberikan izin RLS penuh bagi role `wakil_rt` untuk menyetujui/menolak transaksi kas dan melihat pembayaran serta log audit seperti `pak_rt`.

### 🔄 Diperbarui (Improvements & UI/UX)
- **Animasi Loader Wajah NawakAI:** Menghapus border melingkar dan menghadirkan animasi putaran `360°`, pantulan, mata melirik/berkedip, dan gerak mulut ekspresif saat NawakAI berpikir.
- **NawakAI Lebih Cepat & Cerdas:** Cache sinkronisasi database 60 detik, penangan lokal instan untuk query data RT (kas, tunggakan, pending, warga meninggal, tanpa tagihan, gender, tanpa NIK/HP, dan pencarian nama/NIK), serta timeout API 10 detik.
- **Desain Flat UI Modal Persetujuan & Pending Pill:** Pembaruan kartu `.wk-approval-card` dan lencana `.wk-pending-pill` dengan kontras tinggi di Dark Mode dan Light Mode.
- **Label Pembayaran & Persetujuan:** Memperbarui teks opsi menjadi `QRIS - DANA, Shopee, Gopay`, `Transfer ke Rekening Bank`, dan `Tunai / Cash`, serta menghapus kata *Developer* pada pesan persetujuan menjadi `Menunggu persetujuan Pak RT & Wakil RT`.
- **Frekuensi Reminder Ulang Tahun Warga:** Dibatasi tepat 2x saja (H-1 sebelum hari H dan 1 jam sebelum hari H pukul 07:00 WIB).
- **Tata Letak Export Data Warga:** Kolom `Alamat` diperlebar dengan *word-wrap* 2 baris agar tidak menabrak `Nomor KK`, serta `Nomor KK` hanya ditampilkan pada baris Kepala Keluarga.

### 🐛 Diperbaiki (Bug Fixes)
- **Perbaikan Layar Berkedip Saat Pencarian:** Kolom pencarian di **Data Warga** dan **Data KK** kini hanya memperbarui daftar kartu secara parsial tanpa me-render ulang seluruh halaman.
- **Sinkronisasi Status Pindah Domisili Keluarga:** Status *Pindah Domisili* pada Kepala Keluarga otomatis menurun ke seluruh anggota keluarga non-meninggal baik di laporan/export maupun saat disimpan ke database.
- **Akurasi Pencocokan Intent NawakAI:** Menggunakan pencocokan batas kata (*word-boundary*) agar pertanyaan tidak lagi salah rute akibat kemiripan substring.

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
