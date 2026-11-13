# adm2-guru-nerin
adm2 guru nerin 13112026

# EduClay — Aplikasi Administrasi Guru

Aplikasi web statis (satu file `index.html`) dengan Firebase Authentication (Admin) dan Firestore (data & login Guru). Siap di-deploy ke **GitHub + Vercel** tanpa proses build.

## Isi repositori

| File | Fungsi |
|---|---|
| `index.html` | Seluruh aplikasi (HTML, CSS, JS, PWA) |
| `vercel.json` | Konfigurasi hosting statis Vercel |
| `firestore.rules` | Security Rules Firestore (salin ke Firebase Console) |
| `.gitignore` | Mengabaikan file lokal |

## 1. Siapkan Firebase

1. Buka [console.firebase.google.com](https://console.firebase.google.com) dan pilih project Anda.
2. **Build → Firestore Database** → buat database.
3. **Build → Authentication → Sign-in method** → aktifkan **Email/Password**, lalu tambahkan akun Admin di tab *Users*.
4. **Firestore → Rules** → tempel isi `firestore.rules` → **Publish**.
5. **Project settings → Your apps (Web)** → salin `firebaseConfig` dan ganti di `index.html` (bagian `<script type="module">`). Pastikan `const IS_PREVIEW = false;`.

## 2. Upload ke GitHub

```bash
git init
git add .
git commit -m "EduClay: deploy pertama"
git branch -M main
git remote add origin https://github.com/USERNAME/NAMA-REPO.git
git push -u origin main
```

Tanpa terminal: buka repositori baru di GitHub → **Add file → Upload files** → seret semua file → **Commit changes**.

## 3. Deploy ke Vercel

1. Masuk ke [vercel.com](https://vercel.com) dengan akun GitHub.
2. **Add New… → Project** → pilih repositori.
3. *Framework Preset*: **Other**. Kosongkan Build Command dan Output Directory (root `./`).
4. Klik **Deploy**.

## 4. Wajib: daftarkan domain Vercel di Firebase

Tanpa langkah ini login Admin akan gagal (`auth/unauthorized-domain`).

**Firebase Console → Authentication → Settings → Authorized domains → Add domain** → masukkan domain Vercel Anda (mis. `nama-app.vercel.app`) dan domain kustom bila ada.

## Fitur revisi terbaru (Admin)

- **Logo sekolah**: *Pengaturan → Upload Logo* (PNG/JPG/WEBP/SVG). Gambar otomatis diperkecil maks 256 px dan disimpan di Firestore (tidak butuh Firebase Storage). Bisa juga memakai URL, dan tombol *Hapus Logo*.
- **Impor Data Kelas**: *Data Kelas → Impor Excel*. Kolom: `Nama Kelas`, `Tahun Ajaran`, `Username Wali Kelas`, `Keterangan`.
- **Impor Mapel & Pengampu**: *Mata Pelajaran → Impor Excel*. Kolom: `Kode Mapel`, `Nama Mapel`, `Nama Kelas`, `Username Guru`.
- **Impor Akun Guru**: *Akun Guru → Impor Excel*. Kolom: `Nama Lengkap`, `NIP`, `Mapel Utama`, `Username`, `Password`.

Urutan impor yang disarankan: **Akun Guru → Kelas → Mapel & Pengampu → Siswa** (karena Kelas membutuhkan wali dari Akun Guru, dan Mapel membutuhkan Kelas serta Guru). Setiap menu menyediakan tombol **Unduh Format .XLSX**, dan baris duplikat atau tidak valid otomatis dilewati dengan pratinjau sebelum diproses.

## Catatan keamanan

- Rules saat ini mengizinkan baca/tulis terbuka untuk `classes`, `students`, `attendances`, `grades`, `journals` agar login Guru (tanpa Firebase Auth) berfungsi. Password guru tersimpan sebagai teks biasa di koleksi `users` yang dapat dibaca publik. Untuk sekolah dengan data sensitif, pertimbangkan migrasi login Guru ke Firebase Auth.
- Kunci `apiKey` Firebase pada aplikasi web memang publik; keamanan ditentukan oleh Security Rules dan Authorized domains.
