# Flickr Video Downloader 📥

> Alat web simpel buat nyimpen video Flickr yang publik ke perangkat kamu. Nggak ribet, nggak ngumpulin data, langsung bisa dipakai.

## 👋 Kenapa alat ini ada?

Jujur aja: kadang kita lagi browsing Flickr terus nemu video yang bener-bener berguna — tutorial fotografi, klip behind-the-scenes, atau malah konten yang dulu kita upload sendiri dan sekarang pengin dibackup. Tapi download-nya? Nggak selalu gampang.

Makanya aku bikin alat ini, awalnya buat pakai sendiri, terus kepikiran: "Kenapa nggak dibagiin aja?". Nggak ada fitur berlebihan, nggak ada tracking pengguna, nggak perlu bikin akun. Cukup tempel link Flickr yang publik, klik "Analisis", kalau videonya bisa diakses, opsi download langsung muncul. Selesai.

Semua pemrosesan jalan di server: aku nggak catat apa yang kamu download, nggak simpan riwayat, nggak kumpulin data pribadi. Privasi kamu tetap jadi urusan kamu.

## ✨ Apa sih yang bisa dilakukan?

- **Dukungan link Flickr umum**: Bisa proses album publik, halaman video pengguna, link share langsung — selama nggak privat atau dikunci password
- **Tampilin pilihan kualitas**: Kalau Flickr nyediain beberapa resolusi (Original, High, Standard), kamu bisa pilih mau download yang mana
- **Nggak perlu login**: Cuma proses konten yang aksesnya publik; nggak pernah minta username atau password Flickr kamu
- **Tampilan rapi & responsif**: Enak dilihat di HP, tablet, atau laptop tanpa perlu framework frontend yang berat
- **Proteksi anti-abuse dasar**: Batasin request per IP otomatis biar server nggak kebebani dan layanan tetap stabil buat semua orang
- **Proses nggak nge-block**: Meski lagi analisis video panjang, tab browser kamu nggak bakal freeze, pengalaman pakai tetap lancar

## 🛠 Apa yang dipakai di belakang layar?

| Lapisan | Teknologi |
|---------|-----------|
| Backend | Python 3.11, Django 4.2 LTS |
| Parsing | httpx, lxml, regex buat ekstraksi metadata |
| Frontend | HTML5 semantik, CSS3 ringan, JavaScript vanilla |
| Deployment | Gunicorn + Nginx, siap Docker |
| Utilitas | python-dotenv, django-ratelimit, whitenoise |

Nol library AI. Nol panggilan API eksternal yang "kirim laporan ke rumah". Cuma request HTTP standar dan parsing HTML yang ditulis dengan hati-hati — kode yang bener-bener bisa kamu baca, paham, dan modif tanpa pusing.

## 🚀 Jalanin di mesin sendiri

### Yang perlu disiapkan
- Python 3.10 atau lebih baru
- pip + venv (atau virtualenv)
- Paham dasar struktur proyek Django

### Setup environment development

```bash
# Clone repository
git clone https://github.com/username-kamu/flickr-downloader-in.git
cd flickr-downloader-in

# Buat dan aktifkan virtual environment
python -m venv .venv
source .venv/bin/activate  # Linux/macOS
# atau .venv\Scripts\activate  # Windows

# Install dependencies
pip install -r requirements.txt

# Konfigurasi environment variables
cp .env.example .env
# Edit .env sesuai kebutuhan (SECRET_KEY, DEBUG, dll)

# Jalankan migration dan start server development
python manage.py migrate
python manage.py runserver
```

Lalu buka `http://127.0.0.1:8000` di browser.

### Catatan untuk production

- Set `DEBUG=False` dan konfigurasi `ALLOWED_HOSTS` dengan domain yang benar
- Jalankan di belakang Nginx pakai Gunicorn (atau uWSGI kalau lebih nyaman)
- Aktifkan HTTPS di level proxy
- Kumpulkan file statis: `python manage.py collectstatic`
- Kalau traffic mulai tinggi, pertimbangkan tambah Redis untuk caching

Contoh command Gunicorn:
```bash
gunicorn config.wsgi:application \
  --bind 127.0.0.1:8000 \
  --workers 2 \
  --timeout 90
```

Kalau nambah jumlah worker, pantau penggunaan memory — parsing video bisa agak berat dikit.

## 📋 Cara pakai

1. Cari halaman Flickr publik yang ada videonya (album, profil pengguna, atau link share)
2. Copy URL-nya dan paste di kolom input alat ini
3. Klik "Analisis" — backend bakal ekstraksi stream video yang tersedia
4. Kalau berhasil, tombol download dengan label resolusi bakal muncul
5. Pilih opsi yang kamu mau; file bakal mulai didownload lewat browser

> Catatan: Cuma video yang aksesnya publik yang bisa diproses. Album privat, konten khusus teman, video dikunci password, atau konten dengan pembatasan regional bakal return error. Ini disengaja — alat ini menghormati pengaturan privasi Flickr.

## ⚠️ Tolong baca bagian ini baik-baik

Alat ini dibuat **khusus untuk penggunaan pribadi dan non-komersial**. Contoh penggunaan yang sesuai:
- Backup video yang kamu sendiri upload ke Flickr
- Simpan konten edukasi atau referensi yang dibagikan publik buat belajar offline
- Riset atau kebutuhan aksesibilitas dalam batas fair use

**Kamu yang bertanggung jawab untuk**:
- Patuhi [Syarat & Ketentuan Flickr](https://www.flickr.com/help/terms)
- Hormati hak cipta dan lisensi Creative Commons dari pembuat konten
- Ikuti hukum yang berlaku di wilayah kamu terkait penyalinan konten digital

Aku nggak monitor download dan nggak tanggung jawab kalau ada penyalahgunaan. Mohon jangan pakai alat ini untuk:
- Scraping massal atau pengumpulan konten otomatis
- Distribusi ulang materi berhak cipta tanpa izin eksplisit
- Menghindari pengaturan privasi atau kontrol akses
- Layanan komersial atau re-hosting tanpa persetujuan sebelumnya

Kalau kamu ragu apakah use case kamu oke, kemungkinan besar nggak oke. Kalau bingung, tanya dulu ke pembuat kontennya.

## 🤝 Mau ikut berkontribusi?

Nemu bug? Mikir parsing-nya bisa lebih robust? Punya ide buat improve UI? Kontribusi sangat diterima — nggak ada gatekeeping.

### Cara kontribusi
1. Fork repo dan buat branch fitur (`git checkout -b fix/tampilan-mobile`)
2. Lakukan perubahan dalam commit kecil yang logis dengan pesan yang jelas
3. Test di lokal — pastikan fitur yang sudah ada masih jalan normal
4. Buka Pull Request dengan deskripsi singkat tentang apa yang berubah dan alasannya

### Panduan gaya kode
- Backend: Ikuti PEP 8, pakai type hints kalau bikin kode lebih jelas
- Frontend: Jaga JavaScript tetap minimal; utamakan progressive enhancement daripada framework berat
- Commit: Pakai prefix konvensional (`feat:`, `fix:`, `docs:`, `chore:`, dll)

### Melaporkan masalah
Kalau mau lapor bug, mohon sertakan:
- URL Flickr yang bermasalah (kalau boleh dibagikan)
- Nama + versi browser, sistem operasi, tipe device
- Langkah-langkah untuk mereproduksi masalah
- Perilaku yang diharapkan vs. yang sebenarnya terjadi

Screenshot atau log console juga membantu, terutama untuk masalah frontend.

## 🔧 Opsi konfigurasi

| Variabel | Fungsi | Contoh |
|----------|--------|---------|
| `DEBUG` | Aktifkan/nonaktifkan mode debug Django | `False` |
| `SECRET_KEY` | Kunci keamanan Django | `string-acak-aman-punya-kamu` |
| `MAX_VIDEO_SIZE_MB` | Tolak file yang lebih besar dari X MB | `500` |
| `RATE_LIMIT_PER_MIN` | Maksimum request per IP per menit | `10` |
| `ALLOWED_HOSTS` | Domain yang diizinkan (dipisah koma) | `.domainkamu.co.id` |

Semua setting dimuat lewat `python-dotenv`; nggak ada data sensitif yang hardcode di source code. Di production, rotasi `SECRET_KEY` kamu secara berkala.

## 📄 Lisensi

Lisensi MIT — lihat file [LICENSE](./LICENSE) untuk teks lengkap.  
Kamu bebas menggunakan, memodifikasi, dan mendistribusikan software ini, selama tetap menyertakan notice copyright asli.

## 📬 Kontak & Dukungan

- Laporan bug & saran fitur: Pakai tab Issues di GitHub
- Pertanyaan umum: support@twittervideodownloaderx.com
- Kerentanan keamanan: Mohon kirim via email langsung sebelum diumumkan ke publik

Aku coba respond issue dalam beberapa hari. Kalau sudah lebih lama dan belum dapat balasan, jangan ragu buat nanya lagi — kadang ada yang kelewat.

---

*Proyek ini tidak berafiliasi, tidak didukung, dan tidak terhubung dengan Flickr / SmugMug, Inc. Semua merek dagang, logo, dan hak konten adalah milik masing-masing pemiliknya.*

*Terakhir diperbarui: Mei  | Versi 1.2.0*

*Demo langsung: https://twittervideodownloaderx.com/flickr_downloader_in*

*Dibuat oleh manusia, untuk manusia. Nggak ada kecerdasan buatan yang terlibat dalam penulisan README ini maupun kode proyeknya.*