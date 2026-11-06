# Website Ucapan Ulang Tahun Interaktif

DEMO LIVE : https://poizocodee.github.io/website-ulang-tahun-edit-data-di-admin/

Website ucapan ulang tahun yang dibuat dalam **satu file HTML** (tanpa framework, tanpa server, tanpa build). Cukup buka file-nya di browser, dan penerima akan melewati serangkaian halaman interaktif: mulai dari tirai/amplop yang dibuka, kartu ucapan, galeri foto, tiup lilin, sampai kejutan di akhir.

Dibuat sebagai hadiah personal untuk ulang tahun ke-20. Semua teks, foto, dan tema bisa diganti lewat panel admin yang ada di dalam file itu sendiri, jadi proyek ini bisa dipakai ulang untuk orang lain.

## Fitur

**Alur halaman**

1. **Pembuka**: tirai panggung yang terbuka (default) atau amplop yang dibuka dengan ketukan.
2. **Sampul**: foto bulat, nama, dan ucapan singkat.
3. **Ucapan**: 14 kartu pesan yang berganti satu per satu dengan indikator titik.
4. **Galeri**: dua baris foto yang bisa digeser, yaitu "Kita Berdua" dan "Solo".
5. **Kue ulang tahun**: ketuk setiap lilin untuk memadamkannya.
6. **Kejutan**: gambar X-ray cincin custom dan nota pembeliannya (bisa dibuka/ditutup).
7. **Penutup**: pesan akhir, kembang api, konfeti, dan bunga bermekaran, lalu tombol untuk mengulang dari awal.

**Efek & suasana**

- Musik *Happy Birthday* yang **disintesis langsung di browser** lewat Web Audio API (bukan file audio, jadi tidak ada masalah hak cipta dan ukuran file tetap kecil). Ada tombol suara di pojok layar untuk mute/unmute.
- Kembang api, konfeti, balon, dan hati/bunga melayang di latar.
- Efek percikan kecil setiap kali layar disentuh.
- Mode ringan otomatis di perangkat berspesifikasi rendah (animasi latar dikurangi).
- Menghormati pengaturan `prefers-reduced-motion`.
- Layout mobile-first, ukuran kartu menyesuaikan layar.

**Panel admin (tanpa backend)**

Bisa diganti langsung dari browser:

- Nama lengkap dan nama panggilan
- **Semua teks**: 14 kartu ucapan (tambah, hapus, ubah urutan), kalimat di sampul, teks kejutan cincin, dan seluruh pesan penutup
- Foto sampul
- Foto galeri (tambah/hapus, mendukung upload banyak sekaligus)
- Gambar X-ray dan nota
- Gaya pembuka (tirai atau amplop)
- Jenis bunga (mawar, sakura, matahari, tulip, teratai, aster, kembang sepatu, buket, atau tanpa bunga)
- 8 tema warna: Mawar, Sakura, Lavender, Laut, Senja, Hutan, Emas, Malam
- Password admin

## Cara Pakai

1. Download file `ucapan-ulang-tahun-imelda.html`.
2. Buka di browser (Chrome, Safari, Firefox, Edge). Tidak perlu internet, kecuali untuk memuat font Google Fonts. Tanpa internet tampilan tetap jalan dengan font cadangan.
3. Ketuk/klik layar untuk memulai.

> Musik baru bisa berbunyi setelah ada interaksi pertama dari pengguna (aturan autoplay browser). Itu sebabnya musik dimulai saat tirai/amplop dibuka.

### Hosting di GitHub Pages (opsional)

1. Ganti nama file menjadi `index.html`.
2. Buka **Settings → Pages**, pilih branch `main` dan folder `/ (root)`.
3. Website akan tersedia di `https://<username>.github.io/<nama-repo>/`.

## Cara Mengubah Isi

### Lewat panel admin

Panel admin **disembunyikan** dan tidak ada tombolnya di layar. Ada dua cara membukanya:

**Cara 1: ketuk bar progres 5 kali**

1. Buka website-nya.
2. Cari bar progres, yaitu deretan garis kecil di **bagian atas layar, tepat di tengah**.
3. Ketuk/klik bar itu **5 kali berturut-turut dengan cepat** (jeda antar ketukan maksimal 1,5 detik). Kalau terlalu lambat, hitungannya mengulang dari nol.
4. Kotak login admin akan muncul.

**Cara 2: lewat URL**

Tambahkan `#admin` di akhir alamat, lalu tekan Enter. Contoh: `index.html#admin` atau `https://username.github.io/nama-repo/#admin`. Kotak login langsung muncul.

**Login default**

| | |
|---|---|
| Username | `admin` |
| Password | `admin` |

Segera ganti password-nya lewat bagian **Ganti password admin** di dalam panel. Tombol **Tutup** atau tombol `Esc` menutup panel.

Setelah mengubah sesuatu:

- **Simpan perubahan** menyimpan ke `localStorage` browser yang sedang dipakai.
- **Unduh HTML** menghasilkan file HTML baru yang sudah memuat semua perubahan. **Pakai tombol ini kalau file mau dikirim ke orang lain**, karena `localStorage` hanya berlaku di browser kamu sendiri.
- **Reset** mengembalikan semuanya ke isi file aslinya.

### Mengedit teks & ucapan

Di panel admin ada bagian **Teks & ucapan**:

- **Kartu ucapan**: ubah isi tiap kartu, tambah kartu baru, hapus, atau geser urutan dengan tombol ↑ ↓. Satu baris di dalam kartu = satu paragraf.
- **Sampul, kejutan cincin, dan penutup**: tiap kalimatnya punya kolom sendiri.
- Ketik `{NICK}` untuk nama panggilan dan `{FULL}` untuk nama lengkap. Keduanya diganti otomatis.
- Kolom yang dikosongkan akan kembali ke teks bawaan. Tombol **Kembalikan semua teks ke bawaan** mengatur ulang semuanya.

Setelah selesai klik **Simpan perubahan**, lalu **Unduh HTML** kalau file mau dikirim ke orang lain.

### Lewat kode

Teks bawaan ada di konstanta `DEFAULT_TEXTS` (dan `DEFAULT_MESSAGES` untuk kartu ucapan) di dalam `<script>`. Teks hasil edit disimpan di `cfg.texts` dan ikut tersimpan di `siteConfig` saat "Unduh HTML".

## Struktur & Cara Kerja

Semuanya ada di satu file:

| Bagian | Fungsi |
|---|---|
| `<style>` | Seluruh CSS, termasuk 8 tema warna lewat CSS variable dan atribut `data-theme` |
| `<section class="stage">` | Tiap halaman. Hanya satu yang aktif pada satu waktu (`goTo('stage-...')`) |
| `<script id="siteConfig" type="application/json">` | Data yang bisa diedit: nama, teks, foto (base64), gambar cincin, tema, akun admin |
| `<script>` | Logika navigasi, musik (Web Audio), efek, dan panel admin |

Beberapa detail teknis:

- **Foto disimpan sebagai base64** di dalam JSON `siteConfig`, jadi file HTML-nya mandiri (self-contained). Konsekuensinya ukuran file besar (sekitar 1 MB).
- **Fitur "Unduh HTML"** bekerja dengan mengambil salinan HTML asli (`ORIGINAL_HTML`), lalu mengganti isi `siteConfig` dengan konfigurasi terbaru.
- **Konfigurasi aktif** dipilih lewat nomor revisi (`rev`): data di `localStorage` hanya dipakai kalau `rev`-nya lebih baru daripada yang ada di file.
- **Musik** dibuat dengan oscillator bertipe `triangle`, dengan notasi nada ditulis sebagai array `tune`.

## Catatan Penting

- **Keamanan panel admin bersifat kosmetik.** Login-nya berjalan sepenuhnya di sisi browser dan password hanya di-hash sederhana, jadi **bukan** perlindungan sungguhan. Siapa pun yang bisa membuka file bisa membaca atau melewati login lewat DevTools. Gunakan hanya untuk mencegah orang iseng mengubah tampilan, bukan untuk menyimpan sesuatu yang rahasia.
- **Foto pribadi ikut tertanam di file.** Kalau repo ini publik, semua foto, X-ray cincin, dan nota di dalam file ikut bisa dilihat dan diunduh siapa saja. Sebelum di-publish, ganti dengan foto contoh atau jadikan repo **private**.
- Font dimuat dari Google Fonts (Fraunces dan Karla).

## Teknologi

HTML, CSS, dan JavaScript murni (vanilla). Tidak ada dependensi, bundler, ataupun backend.

## Lisensi

Silakan pakai dan modifikasi untuk keperluan pribadi. Sesuaikan bagian ini dengan lisensi yang kamu mau (misalnya MIT) sebelum dipublikasikan.
