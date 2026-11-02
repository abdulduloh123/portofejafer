# Folder bukti

Tempat screenshot bukti untuk studi kasus, karya, dan sertifikat. File di sini ikut ter-deploy ke Vercel dan **tidak** membebani database Firestore.

## Cara menambah screenshot

1. Samarkan dulu data sensitif: nama dan nomor HP pelanggan, nomor CS, omzet, password, URL admin, dan token.
2. Perkecil ukurannya. Lebar 1600 px dalam format `.webp` biasanya cukup (100–300 KB).
3. Beri nama yang jelas, tanpa spasi: `sales-dashboard-admin.webp`, `meta-ads-audit.webp`.
4. Simpan di folder ini, lalu push ke GitHub (Vercel otomatis deploy ulang).
5. Di situs, buka **Edit situs → Bagian halaman → Studi kasus → pilih proyek**, lalu isi kolom **Screenshot bukti**, satu per baris:

```
Dashboard admin | bukti/sales-dashboard-admin.webp
Alur input closing | bukti/input-closing.webp
```

6. Klik **Simpan & terbitkan**.

Format yang didukung: `.webp`, `.png`, `.jpg`, `.gif`, `.avif`. Link file Google Drive juga bisa dipakai (akses "Anyone with the link").

Jalur yang sama (`bukti/nama-file.webp`) juga bisa dipakai di kolom link karya, thumbnail, dan bukti sertifikat.
