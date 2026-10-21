# Tutorial: Portofolio di Vercel + Firebase

Hasil akhir: situs portofolio publik di `https://nama-anda.vercel.app`. Anda login dengan Google, klik **Edit situs**, isi link Instagram dan Drive, lalu **Simpan & terbitkan**. Perubahan langsung tampil di situs tanpa deploy ulang.

Cara kerjanya:
- **Vercel** menyajikan file `index.html` (situs statis, tanpa build).
- **Firebase Firestore** menyimpan isi situs (profil, karya, folder Drive, alat) di dokumen `site/main`.
- **Firebase Authentication (Google)** menentukan siapa yang boleh mengedit. Aturan keamanan (`firestore.rules`) hanya mengizinkan email Anda.
- Semua berjalan di paket gratis (Firebase Spark dan Vercel Hobby).

File di folder ini:
- `index.html`: seluruh situs dan dashboard admin
- `firestore.rules`: aturan keamanan database

---

## Langkah 1. Buat proyek Firebase

1. Buka https://console.firebase.google.com lalu klik **Create a project** (atau **Add project**).
2. Beri nama, misalnya `portofolio-saya`. Google Analytics boleh dimatikan.
3. Setelah proyek jadi, di menu kiri pilih **Build > Firestore Database > Create database**.
   - Lokasi: `asia-southeast2 (Jakarta)`
   - Mode: **Production mode**
4. Di menu kiri pilih **Build > Authentication > Get started > Sign-in method > Google**, aktifkan (**Enable**), pilih email dukungan, lalu **Save**.

## Langkah 2. Ambil konfigurasi web

1. Klik ikon roda gigi di samping "Project Overview", lalu **Project settings**.
2. Gulir ke **Your apps**, klik ikon web `</>`, beri nama aplikasi, lalu **Register app** (centang Firebase Hosting tidak perlu).
3. Salin isi `firebaseConfig` yang tampil.
4. Buka `index.html`, cari bagian ini di dekat awal script, lalu ganti isinya:

```js
const firebaseConfig={apiKey:"ISI",authDomain:"ISI.firebaseapp.com",projectId:"ISI",appId:"ISI"};
const ADMIN_EMAIL="emailanda@gmail.com";
```

Gunakan nilai dari Firebase untuk `apiKey`, `authDomain`, `projectId`, dan `appId`. Ganti `ADMIN_EMAIL` dengan email Google yang akan Anda pakai untuk login.

> API key Firebase memang bersifat publik dan aman ada di kode. Keamanan data dijaga oleh aturan di Langkah 3.

## Langkah 3. Pasang aturan keamanan

1. Di Firebase, buka **Firestore Database > Rules**.
2. Hapus isinya, tempel isi file `firestore.rules`.
3. Ganti `emailanda@gmail.com` dengan email Google Anda (harus sama persis dengan `ADMIN_EMAIL`).
4. Klik **Publish**.

Dengan aturan ini semua orang bisa membaca situs, tapi hanya akun Google Anda yang bisa mengubahnya.

## Langkah 4. Coba di komputer sendiri

File `index.html` memakai ES module, jadi tidak bisa dibuka dengan klik dua kali. Jalankan lewat server lokal:

```bash
cd folder-ini
npx serve
```

Buka alamat yang ditampilkan (biasanya http://localhost:3000). `localhost` sudah otomatis diizinkan oleh Firebase Auth. Alternatif lain: ekstensi **Live Server** di VS Code.

Uji alur ini: gulir ke footer, klik **Admin**, login Google, klik **Edit situs**, ubah nama, klik **Simpan & terbitkan**, lalu muat ulang halaman. Kalau namanya bertahan, Firebase sudah terhubung.

## Langkah 5. Deploy ke Vercel

### Cara A: lewat GitHub (disarankan)

1. Buat repository baru di GitHub, unggah `index.html` dan `firestore.rules`.
2. Buka https://vercel.com dan login (bisa dengan akun GitHub).
3. Klik **Add New > Project**, pilih repository tadi, klik **Import**.
4. Framework Preset: **Other**. Kosongkan Build Command dan Output Directory.
5. Klik **Deploy**. Dalam sekitar satu menit Anda mendapat alamat `https://nama-proyek.vercel.app`.

Setiap kali Anda mengubah kode di GitHub, Vercel otomatis deploy ulang.

### Cara B: lewat terminal

```bash
npm i -g vercel
cd folder-ini
vercel          # ikuti pertanyaannya, jawab default
vercel --prod   # rilis ke alamat produksi
```

## Langkah 6. Izinkan domain Vercel di Firebase (penting)

Tanpa langkah ini, tombol login Google gagal dengan error `auth/unauthorized-domain`.

1. Di Firebase buka **Authentication > Settings > Authorized domains**.
2. Klik **Add domain**, masukkan `nama-proyek.vercel.app` (tanpa `https://`).
3. Jika nanti memakai domain sendiri, tambahkan juga domain tersebut.

## Langkah 7. Pakai sehari-hari

1. Buka situs Anda, klik **Admin** di footer, login dengan Google.
2. Klik **Edit situs** (pojok kanan bawah).
3. Tab **Profil**: nama, keahlian, bio, WhatsApp, email, Instagram, foto.
4. Tab **Karya**: tambah judul, kategori, link Reel/Post Instagram.
5. Tab **Drive**: tambah nama folder, kategori, link folder Google Drive.
6. Tab **Alat**: tambah software, kamera, atau gaya visual beserta grupnya.
7. Klik **Simpan & terbitkan**. Situs langsung diperbarui untuk semua pengunjung.

Tips link:
- Instagram: buka Reel, salin link dari bilah alamat (bentuknya `https://www.instagram.com/reel/XXXX/`). Akun Instagram harus publik agar bisa diputar di situs.
- Google Drive: klik kanan folder > **Share** > ubah akses ke **Anyone with the link** > **Copy link**.

## Domain sendiri (opsional)

Di Vercel buka **Project > Settings > Domains**, tambahkan domain, lalu ikuti petunjuk DNS. Jangan lupa menambahkannya juga ke Authorized domains Firebase (Langkah 6).

## Pemecahan masalah

| Gejala | Penyebab dan solusi |
|---|---|
| Halaman menampilkan "Isi firebaseConfig dulu" | `apiKey` masih `ISI`. Ganti dengan konfigurasi dari Langkah 2. |
| Layar kosong di komputer sendiri | File dibuka langsung. Jalankan lewat `npx serve` (Langkah 4). |
| `auth/unauthorized-domain` saat login | Tambahkan domain Vercel ke Authorized domains (Langkah 6). |
| "Akun ini tidak punya izin edit" | Email login berbeda dengan `ADMIN_EMAIL`. Samakan di `index.html` dan di Rules. |
| "Gagal menyimpan: permission-denied" | Rules belum di-publish, atau email di Rules salah ketik. |
| "Gagal memuat data: permission-denied" | Rules belum mengizinkan `read` pada `site/main`. Salin ulang dari `firestore.rules`. |
| Video Instagram tidak mau main | Link masih `GANTI`, link salah, atau akun Instagram privat. Tombol "Buka di IG" tetap jadi cadangan. |
| Foto profil tidak muncul | Unggah ulang lewat tab Profil lalu klik Simpan. Foto otomatis diperkecil dan disimpan di Firestore, jadi tidak perlu Firebase Storage berbayar. |

## Catatan

- Data situs ada di Firestore, jadi mengubah isi tidak butuh deploy ulang. Deploy ulang hanya diperlukan kalau Anda mengubah kode `index.html`.
- Paket gratis Firestore (50 ribu pembacaan per hari) sangat cukup untuk portofolio pribadi.
- Untuk mengganti admin, ubah `ADMIN_EMAIL` di `index.html` dan email di Rules.
