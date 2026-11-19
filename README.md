# WargaKITA — RT 17 / RW 03 Kebonagung

Repositori backup resmi aplikasi administrasi warga dan keuangan **WargaKITA**.

## Struktur Folder
- `index.html` — Aplikasi web siap pakai (dapat langsung diaktifkan di **GitHub Pages**).
- `android/` — Source code Android native (Kotlin + Jetpack Compose).
- `supabase/migrations/` — Skema PostgreSQL, kebijakan RLS 5 peran, dan fungsi transaksi atomik.
- `.github/workflows/build-apk.yml` — Otomatis mem-build file `app-debug.apk` setiap kali ada push ke `main`.
