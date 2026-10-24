# Perpustakaan Ceria — Situs Publik

Situs perpustakaan untuk pengunjung: cari buku, pinjam, ngobrol dengan pustakawan digital, dan lihat info keanggotaan. Dibuat dengan HTML, CSS, dan JavaScript murni (vanilla), tanpa framework atau proses build.

Situs admin ada di proyek terpisah: `perpustakaan-ceria-admin`. Keduanya tersambung lewat data bersama (lihat bagian Cara kerja data).

## Struktur folder

```
perpustakaan-ceria-publik/
├── index.html      Halaman utama (semua halaman ada di sini)
├── css/style.css   Gaya dan animasi
├── js/app.js       Logika katalog, peminjaman, chat, dan panel demo AI
└── README.md
```

## Cara menjalankan

1. Ekstrak folder, lalu buka `index.html` di browser (klik dua kali).
2. Atau jalankan server lokal dari dalam folder, misalnya `python3 -m http.server 8000`, lalu buka `http://localhost:8000`.

Huruf (Fredoka, Nunito, Space Mono) dimuat dari Google Fonts, jadi butuh internet. Tanpa internet, tampilan memakai huruf bawaan browser. Fitur Pindai ISBN di panel AI juga butuh internet karena memanggil Google Books.

## Halaman

| Halaman | Isi |
|---|---|
| Beranda | Pencarian cepat, statistik, buku populer, pengumuman |
| Katalog | Cari berdasarkan judul atau penulis, filter per tema |
| Pinjam Buku | Daftar pinjaman, perpanjang, kembalikan, riwayat aktivitas |
| Chat Pustakawan | Perintah seperti `pinjam Sapiens`, `cari buku sejarah`, `kembalikan Cosmos`, `jam buka` |
| Info & Keanggotaan | Kartu anggota, jam operasional, lokasi, FAQ |
| Panel AI | Simulasi klasifikasi tema, pindai ISBN, tata letak rak, dashboard rak, skema data |

## Animasi

Kartu dan halaman muncul bergantian, banner beranda mengalir warnanya, bola dekoratif melayang, angka statistik menghitung naik, dan tombol ajakan chat berdenyut. Semua animasi mati otomatis bagi pengguna yang mengaktifkan pengaturan "kurangi gerakan". Gayanya ada di bagian paling bawah `css/style.css`.

## Menjalankan bersama situs admin

Kedua situs tersambung lewat `localStorage`, dan penyimpanan itu hanya dibagi antar halaman yang berasal dari satu alamat (domain dan port yang sama). Karena itu **jangan membuka file dengan klik dua kali**. Jalankan dari server lokal:

1. Taruh folder `perpustakaan-ceria-publik` dan `perpustakaan-ceria-admin` berdampingan dalam satu folder induk.
2. Dari folder induk itu, jalankan `python3 -m http.server 8000`.
3. Buka `http://localhost:8000/perpustakaan-ceria-publik/` dan `http://localhost:8000/perpustakaan-ceria-admin/` di browser yang sama.

## Cara kerja data

- Data buku dan pengumuman disimpan di `localStorage` dengan kunci `pc-shared`. Kunci yang sama dipakai situs admin.
- Buku yang ditambah, diubah, atau dihapus di admin, serta pengumuman yang diterbitkan di admin, muncul di situs ini. Peminjaman, pengembalian, dan perpanjangan di sini muncul di admin.
- Kalau kedua situs terbuka di tab berbeda, perubahan langsung terlihat tanpa memuat ulang.
- Datanya hanya ada di satu browser dan satu perangkat. Ini sambungan untuk demo, bukan database sungguhan.
- Data awal (`CATALOG` dan `ANN` di `js/app.js`) dipakai hanya bila penyimpanan masih kosong.
- Riwayat aktivitas di halaman Pinjam Buku tetap di memori dan kembali kosong setelah dimuat ulang.

## Langkah pengembangan berikutnya

1. Pasang database (misalnya Supabase atau Firebase, atau server sendiri) untuk buku, pinjaman, dan pengumuman.
2. Ganti data di dalam kode dengan pemanggilan `fetch` ke API.
3. Ganti penyimpanan bersama di browser dengan database yang dipakai kedua situs.

## Menyesuaikan

- Warna dan huruf: variabel di bagian `:root` pada `css/style.css`.
- Tema rak dan warnanya: `THEME_COLORS` di `js/app.js`. Kapasitas rak: `SHELF_CAP`.
- Jam buka, alamat, dan FAQ: bagian Info di `index.html` dan `FAQS` di `js/app.js`.
