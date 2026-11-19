# WargaKITA — Panduan Setup Supabase Gratis & Build APK Android

## 1. Pilihan Database Permanen & Gratis: Supabase (PostgreSQL)
- **Gratis Permanen (Free Tier)**: 500 MB database PostgreSQL, 50.000 pengguna aktif bulanan (Supabase Auth), Row Level Security (RLS), tanpa kartu kredit.
- **Langkah Setup**:
  1. Buat akun di [https://supabase.com](https://supabase.com) dan buat proyek baru (`wargakita-rt17`).
  2. Buka **SQL Editor** di dashboard Supabase, lalu jalankan file secara berurutan:
     - `supabase/migrations/001_schema.sql`
     - `supabase/migrations/002_rls.sql`
     - `supabase/migrations/003_functions.sql`
     - `supabase/migrations/004_seed.sql`
  3. Di **Authentication -> Users**, buat 5 akun pengurus (`pak_rt`, `wakil_rt`, `sekretaris_rt`, `bendahara_rt`, `developer_admin`), matikan registrasi publik (*Disable Signups*).
  4. Salin **Project URL** dan **anon public key** ke `android/app/build.gradle.kts` (jangan pernah memasukkan `service_role` key ke APK).

## 2. Cara Build APK Siap Pasang
1. Buka folder `android/` menggunakan **Android Studio** (versi Ladybug atau lebih baru).
2. Tunggu Gradle sync selesai (memerlukan koneksi internet untuk mengunduh dependensi Jetpack Compose & Supabase SDK).
3. Pilih menu **Build -> Build Bundle(s) / APK(s) -> Build APK(s)**.
4. File APK siap pasang akan tersedia di `android/app/build/outputs/apk/debug/app-debug.apk`.

## 3. Backup & Restore Mandiri
- Ekspor data rutin dalam format CSV dari aplikasi, atau lakukan `pg_dump` melalui connection string PostgreSQL Supabase untuk backup penuh.
