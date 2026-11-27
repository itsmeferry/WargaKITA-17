## [1.17.0] — 2026-11-27 (`versionCode = 30`)

### Desain Android 17 (Material 3 Expressive), Dark Mode & Navigasi Responsif
- **Redesign Konsisten 5 Tab (*Dashboard, Data Warga, Data KK & Iuran, Cash RT, Lainnya*):** Mengusung bahasa visual **Material 3 Expressive** dengan sudut membulat modern (`rounded-2xl`/`rounded-3xl`), kartu tonal (`.m3-tonal-surface`, `.m3-hero-tonal`), tombol pill responsif di layar mobile (`<= 390px`), **Bottom Navigation Bar** mobile, dan **Desktop Navigation Rail** (`renderDesktopNavRail`) otomatis di layar lebar.
- **Dark Mode Dinamis & 4 Palet Material You (`MATERIAL_PALETTES`):** Mode tampilan kini mendukung **Terang**, **Gelap**, dan **Otomatis Sistem** dipadukan dengan 4 pilihan palet warna dinamis (*Samudra RT 17*, *Zamrud Asri*, *Violet Senja*, dan *Koral Hangat*) yang langsung menyelaraskan variabel CSS, komponen antarmuka, dan warna grafik.
- **Pembaruan Grafik Dashboard & Donat Cash RT:** Grafik batang pemasukan 6 bulan terakhir di **Dashboard** dan grafik donat distribusi kas masuk di **Cash RT** kini selaras otomatis dengan token warna (`chartColors`, `barMuted`, `barActive`) dari palet Material You yang sedang aktif.
- **Perapihan Tab Data Warga, Data KK & Iuran, serta Lainnya:**
  - **Data Warga:** Tombol sortir ringkas (*Urut KK & Tgl Lahir* vs *Urut Abjad A–Z*) dan filter status/agama yang bersih (*Permanen, Kontrak, Kost, Tidak Aktif di RT 17, Pindah Domisili, Meninggal Dunia*).
  - **Data KK & Iuran:** Pembaruan header ringkas, bar progres kepatuhan, dan 4 kartu filter cepat (*Wajib Penuh, Janda Rukem, Kost Bebas, Menunggak*).
  - **Lainnya & Laporan:** Tata letak pemilih mode Terang/Gelap/Otomatis, kartu pilih palet Material You 2 kolom, bahasa `ID / EN / JV`, dan kartu versi aplikasi yang bersih.

### Aturan Iuran Tahunan, Cicilan Lintas Bulan & Tunggakan Manual Beda Tahun (`010_annual_dues_and_manual_arrears.sql`)
- **Tagihan Tahunan Otomatis (`1×/Tahun per Kategori`):** Kewajiban 4 kategori iuran (*17 Agustus, Keuangan Social, Pembangunan + Fogging, Rukem + Juru Kunci*) dikonsolidasikan menjadi **1 tagihan resmi per tahun per kategori (`period_month = 1`)** via `ensure_period_dues()` & `getAnnualDuesForHouseholdYear()`.
- **Cicilan Lintas Bulan & Rollover Lintas Tahun (`2026 → 2027`):** Warga dapat mencicil tagihan tahunan di bulan mana pun (misal bayar sebagian di November lalu melunasi sisanya di Desember, sisa tagihan dan nominal terbayar otomatis tersambung). Perpindahan bulan `‹` / `›` dari Desember ke Januari otomatis menaikkan/menurunkan `periodYear`, status kartu menampilkan `LUNAS <Tahun>` agar tidak rancu dengan piutang tahun lalu, dan pelunasan otomatis memotong tahun terlama lebih dulu (*FIFO*).
- **Input Tunggakan Manual Beda Tahun (`showManualArrearsModal` & `upsert_manual_household_due`):** Petugas berwenang (Developer, Pak RT, Wakil RT, Bendahara RT) dapat mencatat, mengedit, atau menyesuaikan nominal tunggakan historis tahun-tahun sebelumnya (`>= 2020`) per kategori iuran langsung dari Detail KK.
- **Rincian Piutang Terakumulasi & Pengingat WA:** Total tunggakan per KK kini menjabarkan rincian per tahun dan per kategori secara jelas (termasuk selisih kurang bayar cicilan) beserta tombol pengingat WhatsApp langsung.

### Hierarki Urutan Anggota KK Baku & Keamanan View Database (`009_complaints_realtime_and_kk_ordering.sql`)
- **Urutan Baku Anggota KK (`sortKKMembers`):** Seluruh tampilan anggota keluarga di Detail KK, Data Warga, NawakAI, dan seluruh format Export (**PDF, Excel, Word `.docx`, PNG**) diseragamkan mengikuti hierarki: `Kepala Keluarga → Istri/Suami → Anak → Menantu → Cucu → Orang Tua Laki-laki/Perempuan → Mertua → Famili Lain → Lainnya`, diurutkan dari tanggal lahir tertua ke termuda (dilengkapi fallback urutan NIK).
- **Perapihan Database & Keamanan View (`security_invoker = true`):** Normalisasi variasi penulisan hubungan keluarga di `public.residents`, penambahan `public.v_residents_kk_ordered` dengan `security_invoker = true` agar mematuhi RLS (bebas temuan *Security Definer View* di Supabase Security Advisor), serta aktivasi Supabase Realtime untuk tabel inti.

### Notifikasi Lingkungan Real-Time WebSocket & Kontrol Siaran Info Warga (`server.ts`)
- **Server Real-Time WebSocket (`/ws` & `/api/notifications/*`):** Menambahkan server Node.js/Express + `ws` di `server.ts` yang menyiarkan pengumuman **Info Ronda, Kerja Bakti, Agenda Warga, Pembayaran Iuran, dan Pengumuman RT** secara langsung ke semua perangkat yang terhubung disertai indikator jumlah perangkat aktif (`Live WS • N aktif`).
- **Pemisahan Notifikasi Info Warga vs Approval Kas:** Pusat Notifikasi memisahkan tab **Info Warga** (hanya info lingkungan yang relevan, tidak lagi tercampur spam *pending approval*) dengan tab **Approval Kas**.
- **Pusat Siaran Info Terbaru (`showBroadcastCenterModal`) & Kartu Ulang Tahun Khusus:** Tombol **Siarkan Info Baru** beserta 4 kategori *template otomatis* yang dapat diedit sebelum dikirim, serta hak edit **Kartu Ucapan Ulang Tahun**, kini dibatasi khusus untuk **Developer, Pak RT, dan Wakil RT**.

### Laporan Kas Bulanan & Per Petugas PDF Detail (`buildMonthlyCashOfficerReport`)
- **Otomatis Sesuai Akun Bendahara Login:** Saat membuka **Laporan Kas Bulanan & Per Petugas**, cakupan petugas otomatis terpilih dan diarahkan sesuai akun yang sedang login (termasuk 4 Bendahara: *Novan Hermawan, Ahmad Abin, A. Teguh Purwanto, Jodik Purwanto*, serta Pak RT, Wakil RT, Sekretaris, dan Developer), serta menghapus teks label `"1x klik"` yang tidak perlu.
- **Perbandingan Month-over-Month (MoM) & Rincian Warga Terbayar:** Menampilkan perbandingan pemasukan, pengeluaran, dan surplus bersih **Bulan Ini vs Bulan Sebelumnya**, lengkap dengan rekap per kategori dan daftar warga yang telah membayar per petugas dalam modal dan ekspor **PDF siap cetak**.

### Panel Developer: 14 Hak Akses Granular & Sinkronisasi RLS
- **Ekspansi Izin Granular (`PERM_DEFS` & `010`):** Menambah 4 izin baru di menu **Kelola User Login** sehingga menjadi **14 centang hak akses granular** (`canViewWarga`, `canAddWarga`, `canEditWarga`, `canDeleteWarga`, `canPay`, `canCash`, `canApprove`, `canEditOpening`, `canEditCalendar`, `canBroadcastInfo`, `canEditBirthdayCard`, `canManageManualArrears`, `canExportReports`, `canUnmaskNik`), tersinkronisasi penuh dengan fungsi `public.user_has_permission()` dan RLS PostgreSQL.

### Pembaruan Sistem Update Sub-APK (`Version Code`), Popup Dashboard & Perbaikan UI (`versionCode = 30`)
- **Identifikasi & Rilis Sub-APK Berbasis `Version Code` (`v1.17.0 (Version Code: 30)`):**
  - Menampilkan badge lengkap **`v1.17.0 (Version Code: 30)`** di tab **Lainnya** dan dialog pembaruan aplikasi via `AndroidBridge.getVersionCode()` & `APP_VERSION_CODE`.
  - GitHub Release `v1.17.0` kini mendukung multi aset sub-APK bernomor (`WargaKITA-RT17-v1.17.0-VersionCode-30.apk`, `VersionCode-31.apk`, dst.). `checkAppUpdate()` & `UpdateCheckReceiver.kt` memindai seluruh aset `.apk` dan otomatis memilih **Version Code tertinggi**, serta mengarahkan **semua versi lama** (`v1.15.x`, `v1.16.x`, maupun `v1.17.0` build awal) ke rilis terbaru.
- **Popup Update Otomatis di Dashboard & Notifikasi Latar Belakang:**
  - Saat petugas membuka aplikasi dan belum memakai build terbaru, popup **Pembaruan Aplikasi Tersedia** langsung tampil otomatis di **Dashboard** beserta daftar lengkap **"Apa yang Baru"**, progress bar unduhan native in-app, dan tombol **Unduh & Pasang Sekarang**.
  - `UpdateCheckReceiver.kt` berjalan periodik tiap **30 menit** (termasuk pasca reboot & `MY_PACKAGE_REPLACED`) sehingga pengguna yang belum membuka aplikasi tetap menerima notifikasi Android saat ada pembaruan.
- **Perbaikan 3 Bug Antarmuka Penting:**
  - **Anti Auto-Scroll ke Atas Saat Gulir ke Bawah:** Mempertahankan `scrollTop` `#scrollArea` dan fokus ketik saat `render()` dipicu sinkronisasi latar belakang/Realtime, menunda *silent render* saat jari menyentuh layar, menambahkan `overscroll-behavior-y: contain`, serta membatasi auto-`scrollIntoView`.
  - **Tombol Back Menutup Dropdown Terbuka Lebih Dulu:** Memperbarui `window.wkHandleBack()` agar tombol Back Android menutup `#wkBlurSelectOverlay`, `#wkConfirmOverlay`, `#qrisFullscreenOverlay`, popover lampiran NawakAI, dan modal aktif terlebih dahulu sebelum kembali ke menu/tab sebelumnya.
  - **Warna Ikon Reload / Pull-to-Refresh Menyesuaikan Tema:** Menyelaraskan `.ptr-bar` dengan `var(--color-accent)` & `var(--md-on-primary)` di `applyMaterialPaletteCSS` untuk seluruh 4 palet Material You maupun Dark Mode.
- **Build Android (`v1.17.0` / `versionCode = 30`):** Mempertahankan `versionName = "1.17.0"`, menaikkan `versionCode = 30` di `android/app/build.gradle.kts`, dan memperbarui `.github/workflows/build-apk.yml` untuk menghasilkan aset `WargaKITA-RT17-v1.17.0-VersionCode-30.apk`.

## [1.16.1] — 2026-11-24 (`versionCode = 28`)

### Splash Screen Pembuka Final — Fondasi Kebersamaan (Isometric Lift & Grow Bloom)
- **Konsep & Tema Fresh Light (`#F8FBFF`):** Dibangun langsung di atas checkpoint stabil `v1.16.0` (`versionCode = 26`) tanpa mengubah atau mengurangi fitur yang sudah ada.
- **Ikon Utama Hidup (4 Lapisan WebP Transparan Offline-Ready):** Tangan penyangga (`hand`) terangkat dari sudut 3D isometrik sebagai fondasi kokoh, diikuti 3 sosok warga (`center`, `left`, `right`) tumbuh berurutan ke atas tanpa border dan tanpa latar kotak.
- **6 Chip Fitur Utama Heksagonal 3D (Tanpa Ronda):** *Kas RT* (`#0A58ED`), *Iuran* (`#10B981`), *NawakAI* (`#6366F1`), *Laporan* (`#F59E0B`), *Info RT* (`#14B8A6`), dan *Data Warga* (`#0EA5E9`), mekar serentak dari tengah ikon bersama 2 gelombang *pulse* flat (`#E2F1FD` & `#D8F5EA`).
- **Wordmark, Loader & Pembersihan Memori:** Menampilkan `WargaKITA 17`, subjudul `RT 17 / RW 03 · Kebonagung`, serta progress bar `#0A58ED` dengan label *"Guyup rukun melayani warga..."*. Tampil tepat **3,4 detik (`3400ms`)**, diikuti *fade-out* halus (`0.48s`), pengembalian `overflow` dokumen, dan penghapusan elemen `#wk-splash-screen` dari DOM (`window.wkHideSplash()`).
- **Isolasi Penuh & Aksesibilitas:** 100% pure CSS 3D + vanilla JS dengan prefix `.wk-sp-*`, pengaman spesifisitas terhadap tema Material You (`html.wk-dyn`) & mode gelap (`html.wk-dark`), serta dukungan `prefers-reduced-motion`.

## [1.16.0] — 2026-11-24

### NawakAI Agent AI Penuh (P1, P2 & P3 Seutuhnya)
- **P1 — Otak Konteks & Memori Multi-Turn:** Resolusi kata ganti otomatis (*"alamatnya"*, *"no hpnya"*, *"tagihannya"*), pengiriman 12 riwayat pesan + `agentContext` ke Gemini Edge Function, serta *slot-filling* revisi draft otomatis tanpa perlu mengulang perintah.
- **P2 — Eksekusi Interaktif & Batch Action:** *Tool chaining* multi-aksi (*cek tunggakan + langsung buat draft WA*), catat pemasukan/pengeluaran kas & pembayaran iuran via suara/chat dengan *slot-filling* nominal/metode, *Batch Approval* semua transaksi pending 1 ketuk (`approve_all_pending`), serta *Smart Filtered Export* otomatis per status warga (*"Export warga janda ke PDF"*).
- **P3 — Proaktif, Audit & OCR:** **Briefing Harian Cerdas RT 17** (*saldo, pending approval, jadwal ronda, ulang tahun, kebersihan NIK*), Audit Lengkap Kesehatan Data & Keuangan, serta OCR foto nota/struk belanja untuk langsung dicatat ke Kas RT.

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
