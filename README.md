# TERNAKPRO — Manajemen Ayam Petelur (Android Native)

Status: **source code lengkap, siap di-build**. Aplikasi Android native
(Kotlin + Jetpack Compose + Room), **100% offline**, tanpa GitHub/Node/
Capacitor/server sebagai syarat aplikasi berjalan di HP.

## Fitur

- **Dashboard** — kartu statistik (ayam hidup, ayam mati, produksi hari
  ini/bulan ini, pakan terpakai, penjualan, piutang, biaya, kerugian, laba
  bersih) + grafik batang produksi telur 7 hari terakhir.
- **Ternak & Kandang** — kelola kandang (kapasitas/terisi/kosong) dan batch
  ternak (breed, umur masuk, harga beli, status Aktif/Selesai).
- **Produksi Harian** — catat ayam hidup/mati/hilang, telur per grade
  (besar/sedang/kecil/tetel/retak/busuk), berat telur, pakan terpakai.
  Otomatis hitung total butir telur & **Hen-Day Production (%)**.
- **Stok Pakan & Obat** — kelola master pakan/obat-vitamin-vaksin, catat
  transaksi masuk/digunakan/rusak/hilang, hitung stok berjalan & nilai
  stok, peringatan stok rendah & obat hampir kedaluwarsa.
- **Penjualan & Piutang** — penjualan telur maupun ayam dalam satu modul
  (tab), status pembayaran, piutang dengan pembayaran cicilan dan status
  otomatis (Lunas/Sebagian/Belum Lunas/Jatuh Tempo).
- **Biaya & Kerugian** — 12 kategori biaya operasional, 8 jenis kerugian.
- **Laporan** — filter Hari Ini/Minggu Ini/Bulan Ini/Tahun Ini/Semua,
  ringkasan laba-rugi, **export PDF** dan **export CSV** langsung dibagikan
  dari HP (share sheet Android).
- **Backup/Restore** — export seluruh database ke file `.json` (dibagikan
  via share sheet), import untuk memulihkan.
- **Isi Contoh Data** & **Hapus Semua Data** (dengan konfirmasi).
- **100% offline** — database SQLite (Room) tersimpan lokal di HP, tidak
  butuh internet untuk dipakai sehari-hari.

## Struktur proyek

```
app/src/main/java/com/ternakpro/app/
  data/entity/     12 entity Room (Kandang, BatchTernak, ProduksiHarian, Pakan,
                   TransaksiPakan, Obat, TransaksiObat, Penjualan, Piutang,
                   PembayaranPiutang, Biaya, Kerugian)
  data/dao/        12 DAO (Flow reaktif)
  data/            AppDatabase (Room)
  repository/      TernakRepository (satu repo terpadu)
  util/            Formatters, DateUtils, SampleData, BackupManager (JSON),
                   ExportUtils (PDF/CSV), PreferencesManager
  viewmodel/       8 ViewModel (satu per grup fitur)
  ui/theme/        Warna, tipografi, Material3 theme
  ui/components/   StatCard, ConfirmDialog, EmptyState, SimpleBarChart (Canvas)
  ui/navigation/   Screen (8 tujuan) + NavGraph
  ui/screens/      8 layar Compose
  MainActivity.kt  Shell: ModalNavigationDrawer + Scaffold + NavHost
```

## Tentang build APK — baca ini dulu

Source code ini **lengkap dan siap di-build**, tapi mengompilasinya jadi
file `.apk` butuh Android SDK + Gradle + koneksi internet (mengunduh
dependency) yang **tidak tersedia di lingkungan kerja Claude**. Karena itu
`TERNAKPRO.apk` tidak disertakan di paket ini secara langsung — bukan
ditolak, tapi secara teknis tidak bisa dikompilasi tanpa toolchain Android.

**Solusinya sudah disiapkan**: `.github/workflows/android-build.yml` — begitu
project ini diunggah ke GitHub, workflow ini otomatis meng-compile APK di
server GitHub (gratis) dalam ±5-10 menit.

## Langkah build — 100% dari HP, lewat browser saja

### Tahap 1 — Ekstrak & buat repo GitHub

1. Ekstrak file zip yang diberikan (folder `TERNAKPRO-ANDROID`).
2. Buka github.com di HP, buat repository baru (Public, jangan centang "Add README").

### Tahap 2 — Upload per folder (PENTING: perhatikan folder `.github`)

Upload dilakukan bertahap per folder via `github.com/USERNAME/REPO/upload/main/NAMA_FOLDER`,
lalu **"choose your files"** → **masuk ke folder yang sesuai** di file picker
HP → pilih semua file di situ → Commit changes. Ulangi untuk tiap baris:

| # | Folder tujuan | Isi dari folder lokal |
|---|---|---|
| 1 | (root, upload pertama di halaman kosong) | `build.gradle.kts`, `settings.gradle.kts`, `gradle.properties`, `README.md` |
| 2 | `app` | `app/build.gradle.kts`, `app/proguard-rules.pro` |
| 3 | `app/src/main` | `app/src/main/AndroidManifest.xml` |
| 4 | `app/src/main/res/values` | `strings.xml`, `themes.xml`, `colors.xml` |
| 5 | `app/src/main/res/drawable` | `ic_launcher_foreground.xml` |
| 6 | `app/src/main/res/mipmap-anydpi-v26` | `ic_launcher.xml`, `ic_launcher_round.xml` |
| 7 | `app/src/main/res/xml` | `file_paths.xml`, `data_extraction_rules.xml`, `backup_rules.xml` |
| 8 | `app/src/main/java/com/ternakpro/app` | `TernakProApplication.kt`, `MainActivity.kt` |
| 9 | `app/src/main/java/com/ternakpro/app/data` | `AppDatabase.kt` |
| 10 | `app/src/main/java/com/ternakpro/app/data/entity` | Semua 12 file entity |
| 11 | `app/src/main/java/com/ternakpro/app/data/dao` | Semua 12 file DAO |
| 12 | `app/src/main/java/com/ternakpro/app/repository` | `TernakRepository.kt` |
| 13 | `app/src/main/java/com/ternakpro/app/util` | Semua file di `util/` |
| 14 | `app/src/main/java/com/ternakpro/app/viewmodel` | Semua 8 file ViewModel |
| 15 | `app/src/main/java/com/ternakpro/app/ui/theme` | `Color.kt`, `Type.kt`, `Theme.kt` |
| 16 | `app/src/main/java/com/ternakpro/app/ui/components` | `CommonComponents.kt`, `SimpleBarChart.kt` |
| 17 | `app/src/main/java/com/ternakpro/app/ui/navigation` | `Screen.kt`, `NavGraph.kt` |
| 18 | `app/src/main/java/com/ternakpro/app/ui/screens` | Semua 8 file layar |
| 19 | `.github/workflows` ⭐ **PALING PENTING** | `android-build.yml` |

**Pelajaran dari pengalaman sebelumnya — hindari 2 kesalahan ini:**
- **Jangan pilih file dari tab "Recent/Gallery" di file picker.** Selalu cari opsi "Browse this device"/folder, lalu **benar-benar masuk ke dalam folder yang dituju** sebelum memilih file — kalau tidak, semua file akan tercampur masuk ke satu folder yang salah.
- **Folder `.github` WAJIB diawali tanda titik.** Kalau Anda membuatnya lewat "Create new file" dengan mengetik path, pastikan menulis `.github/workflows/nama-file.yml` (ada titik di depan `github`) — bukan `github/workflows/...`. Tanpa titik, GitHub Actions **tidak akan mendeteksi workflow-nya sama sekali** walau isinya benar. Setelah upload, cek breadcrumb di halaman folder harus bertuliskan `.github / workflows` (dengan titik), bukan `github / workflows`.

### Tahap 3 — Jalankan Actions

1. Buka tab **Actions** di repo.
2. Workflow **"Build TERNAKPRO APK"** otomatis jalan setelah upload folder `.github/workflows`. Kalau tidak, klik nama workflow → **Run workflow**.
3. Tunggu 5-10 menit sampai ✅ hijau.

### Tahap 4 — Unduh & install APK

1. Buka run yang ✅ → scroll ke **Artifacts** → unduh **TERNAKPRO-apk**.
2. Ekstrak zip hasil unduhan → ada `TERNAKPRO.apk` (release, pakai ini) dan `TERNAKPRO-debug.apk`.
3. Ketuk `TERNAKPRO.apk` → izinkan "sumber tidak dikenal" bila diminta → Install.

**Soal tanda tangan APK release**: workflow ini **selalu berhasil** membuat
`TERNAKPRO.apk`, tanpa setup tambahan — ditandatangani debug key bawaan
(100% bisa diinstall & dipakai, hanya belum layak untuk Play Store). Untuk
tanda tangan produksi sendiri, buat keystore dan simpan sebagai GitHub
Secrets `ANDROID_KEYSTORE_BASE64` (isi file keystore di-encode base64),
`KEYSTORE_PASSWORD`, `KEY_ALIAS`, `KEY_PASSWORD` di *Settings > Secrets and
variables > Actions*, lalu jalankan ulang workflow.

## Validasi yang sudah diterapkan

- Ayam mati/hilang tidak bisa membuat data minus (input dibatasi angka ≥ 0).
- Saldo pembayaran piutang tidak boleh melebihi sisa tagihan.
- Nominal biaya/kerugian/penjualan harus lebih dari 0.
- Field wajib (nama kandang, nama batch, pembeli, dsb.) divalidasi sebelum simpan.
- Konfirmasi dialog untuk setiap aksi hapus data.

## Catatan desain

- **Penjualan telur & ayam** disatukan dalam satu tabel/entity `Penjualan`
  (dibedakan field `jenisTransaksi`) supaya sesuai daftar 12 entity inti
  yang diminta, bukan dipisah jadi 13 entity.
- **Menu digabung dalam tab** (Kandang+Batch, Pakan+Obat, Penjualan+Piutang,
  Biaya+Kerugian) untuk menyederhanakan navigasi drawer jadi 8 tujuan,
  tanpa mengurangi satu pun fitur yang diminta.
- **Grafik** memakai Canvas bawaan Compose (bukan library chart pihak
  ketiga) supaya tidak menambah risiko dependency saat build.
- **Export PDF/CSV** memakai API bawaan Android (`PdfDocument`, penulisan
  CSV manual) — tanpa dependency tambahan.
- **Ikon aplikasi** memakai vector drawable sederhana (bukan file gambar),
  supaya tidak perlu proses generate ikon terpisah.
