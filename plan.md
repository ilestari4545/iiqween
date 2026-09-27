# 📋 Rencana Pengembangan Sistem Undangan Digital WedApps

## 🎯 Overview Proyek

**Nama Proyek:** WedApps - Platform Undangan Digital Premium

**Stack Teknologi:**
- Frontend: HTML5, Tailwind CSS, Vanilla JavaScript
- Backend: Google Apps Script (Node.js-like)
- Database: Google Sheets
- Hosting: Google Blogger (Gratis)

**Tujuan:** Membangun sistem undangan digital pernikahan yang elegan, interaktif, dan mudah dikelola tanpa biaya server/database berbayar.

---

## 📊 Arsitektur Sistem

```
┌─────────────────────────────────────────────────────────────┐
│                     FRONTEND (Blogger)                       │
│  ┌──────────────┐   ┌──────────────┐   ┌──────────────┐      │
│  │  Salespage   │   │  Guest Page  │   │   KirimKit   │      │
│  │  & Katalog   │   │  (Undangan)  │   │  (WA Tool)   │      │
│  └──────┬───────┘   └──────┬───────┘   └──────┬───────┘      │
│         │                  │                  │              │
└─────────┼──────────────────┼──────────────────┼──────────────┘
          │                  │                  │
          └──────────────────┼──────────────────┘
                              │
                     ┌────────▼────────┐
                     │  Google Apps    │
                     │     Script      │
                     │   (API Layer)   │
                     └────────┬────────┘
                              │
                     ┌────────▼────────┐
                     │  Google Sheets  │
                     │    (Database)   │
                     │  - SETTINGS     │
                     │  - DATA_UNDANGAN│
                     │  - DATA_RSVP    │
                     └─────────────────┘
```

---

## 🚀 Tahap 1: Backend & Database (Google Apps Script)

### 📌 Tujuan
Membangun API backend dan database menggunakan Google Sheets + Apps Script sebagai pengganti server konvensional.

### 🎯 Deliverables

1. **Google Sheets Database** dengan 3 sheet utama:
   - `SETTINGS`: Konfigurasi aplikasi (nama, harga, kontak admin, dll)
   - `DATA_UNDANGAN`: Data undangan yang dibuat klien
   - `DATA_RSVP`: Konfirmasi kehadiran & ucapan tamu

2. **Google Apps Script (`Code.gs`)** dengan fungsi:
   - `doGet(e)`: Handler untuk GET requests (ambil data)
   - `doPost(e)`: Handler untuk POST requests (simpan data)
   - `getSettings()`: Ambil konfigurasi aplikasi
   - `getDemos()`: Ambil daftar tema undangan
   - `createInvitation(e)`: Simpan data undangan baru
   - `submitRSVP(e)`: Simpan konfirmasi tamu

### 💻 Prompt untuk AI

```text
Bertindaklah sebagai Senior Google Apps Script Developer. Saya sedang membangun sistem Undangan Digital Pernikahan (seperti WedApps) yang menggunakan Google Sheets sebagai database dan Google Apps Script sebagai API Backend. Frontend-nya akan di-hosting di Google Blogger.

Tugas Anda:
1. Buatkan struktur kolom (header) untuk Google Sheets yang dibutuhkan (Sheet utama: 'DATA_UNDANGAN', 'DATA_RSVP', dan 'SETTINGS').
2. Tuliskan kode lengkap `Code.gs` untuk Google Apps Script.

Kode `Code.gs` HARUS memenuhi syarat berikut:
- Menggunakan fungsi `doGet(e)` dan `doPost(e)` sebagai router API.
- WAJIB menyertakan header CORS (`Access-Control-Allow-Origin: *`) agar bisa dipanggil dari domain Blogger (fetch API).
- Buat fungsi `getSettings()`: Mengambil data dari sheet SETTINGS (seperti nomor WA admin, harga, nama aplikasi) dan return sebagai JSON.
- Buat fungsi `getDemos()`: Mengambil daftar tema undangan dari sheet DATA_UNDANGAN (kolom: slug, nama_mempelai, kategori, cover_img) dan return sebagai JSON array.
- Buat fungsi `createInvitation(e)`: Menerima data dari form admin (mempelai, tanggal, tema, dll), men-generate slug otomatis jika kosong, dan menyimpannya ke sheet DATA_UNDANGAN. Return JSON status success dan slug yang dibuat.
- Buat fungsi `submitRSVP(e)`: Menerima data dari form tamu (nama, kehadiran, ucapan) dan menyimpannya ke sheet DATA_RSVP. Return JSON status success.

Format Output:
- Berikan penjelasan singkat struktur sheet.
- Berikan kode `Code.gs` yang bersih, aman, dan siap di-deploy. Gunakan `ContentService.createTextOutput()` dan `JSON.stringify()`.
```

### ✅ Checklist Implementasi

- [ ] Buat Google Sheets baru
- [ ] Buat 3 sheet: `SETTINGS`, `DATA_UNDANGAN`, `DATA_RSVP`
- [ ] Isi header kolom sesuai struktur
- [ ] Buka Extensions > Apps Script
- [ ] Paste kode `Code.gs` dari AI
- [ ] Deploy sebagai Web App (Execute as: Me, Access: Anyone)
- [ ] Copy URL Web App untuk digunakan di frontend
- [ ] Test endpoint dengan browser/Postman

---

## 🎨 Tahap 2: Halaman Undangan Tamu (Guest Page)

### 📌 Tujuan
Membuat halaman undangan yang dilihat tamu dengan desain elegan, mobile-first, dan interaktif.

### 🎯 Deliverables

1. **Single Page Application** dengan section:
   - Cover (dengan tombol "Buka Undangan")
   - Musik Backsound (auto-play)
   - Info Mempelai (foto, nama, orang tua)
   - Acara & Countdown Timer
   - Google Maps & Calendar
   - Love Story & Galeri Foto
   - RSVP & Ucapan (form + feed)
   - Amplop Digital (rekening + QRIS)

2. **Fitur Teknis:**
   - Dynamic data fetching dari API
   - Personalisasi nama tamu via URL parameter
   - Animasi scroll (Intersection Observer)
   - Lightbox untuk galeri foto
   - Bottom navigation bar
   - Dark mode support

### 💻 Prompt untuk AI

```text
Bertindaklah sebagai Senior Frontend Developer & UI/UX Expert. Saya sedang membangun halaman undangan digital pernikahan (Guest Page) yang akan dilihat oleh tamu undangan. Halaman ini akan di-hosting di Google Blogger (sebagai Static Page) atau bisa juga sebagai standalone HTML.

Tugas Anda:
Buatkan kode lengkap HTML, Tailwind CSS, dan Vanilla JavaScript untuk "Halaman Undangan Tamu" yang elegan, modern, interaktif, dan mobile-first.

Spesifikasi Teknis & Logika:
1. Dynamic Data Fetching:
   - Halaman harus membaca parameter URL `?slug=nama-slug` (contoh: `?slug=fahri-aisyah`).
   - Gunakan `fetch()` untuk mengambil data undangan dari API Google Apps Script (gunakan endpoint: `API_URL + "?action=getInvitation&slug=xxx"`).
   - Jika data tidak ditemukan, tampilkan pesan error yang elegan.
2. Personalisasi Tamu:
   - Baca parameter URL `?to=Nama%20Tamu`.
   - Tampilkan nama tamu di halaman Cover (Contoh: "Kepada Yth. Budi Santoso"). Jika tidak ada, tampilkan "Kepada Yth. Tamu Undangan".
3. Struktur Section (Single Page Scroll):
   - Cover: Background image, sapaan nama tamu, nama mempelai, tombol "Buka Undangan" (saat diklik, cover hilang/fade out, scroll ke section utama, dan putar musik).
   - Musik: Audio player tersembunyi, auto-play saat tombol "Buka Undangan" diklik (looping). Sertakan tombol mute/unmute.
   - Mempelai: Foto, nama lengkap, nama orang tua, dan link IG.
   - Acara: Tanggal, waktu, lokasi. Sertakan fitur Countdown Timer (Hari, Jam, Menit, Detik). Tombol "Lihat Peta" (Google Maps) dan "Simpan ke Kalender" (Google Calendar).
   - Love Story & Galeri: Linimasa cerita dan grid foto galeri (klik foto untuk membuka Lightbox/Zoom).
   - RSVP & Ucapan: Form (Nama, Status Kehadiran, Ucapan). Data dikirim via POST ke API Apps Script (`action=submitRSVP`). Tampilkan daftar ucapan terbaru secara dinamis.
   - Amplop Digital: Nomor rekening (dengan tombol copy to clipboard) dan gambar QRIS.
4. UI/UX & Styling:
   - Gunakan Tailwind CSS (via CDN).
   - Tema: Glassmorphism, elegan, warna pastel/emas (gunakan variabel CSS agar warna utama bisa disesuaikan dari data API `theme_color`).
   - Animasi: Gunakan animasi fade-in/slide-up saat section muncul di viewport (gunakan Intersection Observer).
   - Navigasi: Bottom navigation bar (fixed di bawah) untuk pindah section cepat.
   - Fully responsive, fokus pada tampilan Mobile (karena 95% tamu buka via HP).
   - Gunakan Font Awesome untuk ikon dan Google Fonts (Playfair Display & Poppins) untuk tipografi.

Format Output:
- Berikan satu file HTML lengkap (berisi `<style>`, `<body>`, dan `<script>`).
- Pastikan kode bersih, terstruktur, dan siap pakai.
- Gunakan variabel `const API_URL = "YOUR_APPS_SCRIPT_URL";` di bagian atas script agar mudah diganti.
```

### ✅ Checklist Implementasi

- [ ] Simpan kode sebagai `undangan-tamu.html`
- [ ] Ganti `YOUR_APPS_SCRIPT_URL` dengan URL dari Tahap 1
- [ ] Test dengan parameter: `?slug=fahri-aisyah&to=Budi Santoso`
- [ ] Upload ke Blogger sebagai Static Page
- [ ] Test semua fitur (musik, RSVP, galeri, dll)
- [ ] Pastikan responsive di mobile

---

## 🏪 Tahap 3: Salespage, Katalog & Form Order (Frontend Utama)

### 📌 Tujuan
Membuat halaman utama yang berfungsi sebagai landing page, katalog tema, dan form order multi-step untuk klien.

### 🎯 Deliverables

1. **Landing Page Sections:**
   - Navbar (desktop & mobile)
   - Hero Section dengan CTA
   - Katalog Tema (grid + filter + search)
   - Fitur & Perbandingan
   - Pricing dengan countdown
   - Testimoni
   - Cara Pesan (3 langkah)
   - FAQ
   - Footer

2. **Modal Form Order (5 Step Wizard):**
   - Step 1: Data Mempelai (pria & wanita)
   - Step 2: Acara & Lokasi (akad, resepsi, maps)
   - Step 3: Tema & Musik (warna, paket, audio preview)
   - Step 4: Story & Galeri (love story, foto)
   - Step 5: Kado & Fitur (rekening, QRIS, saklar section)

3. **Fitur Teknis:**
   - Auto-generate slug dari nama mempelai
   - Validasi form per step
   - Audio preview untuk backsound
   - Image preview untuk foto
   - Submit ke API dan redirect ke WhatsApp untuk pembayaran

### 💻 Prompt untuk AI

```text
Bertindaklah sebagai Senior Frontend Developer & UI/UX Expert. Saya sedang membangun halaman utama (Salespage & Admin Dashboard) untuk bisnis Undangan Digital. Halaman ini akan di-hosting di Google Blogger atau sebagai standalone HTML.

Tugas Anda:
Buatkan kode lengkap HTML, Tailwind CSS (via CDN), dan Vanilla JavaScript untuk "Salespage & Form Order" yang mewah, modern, dan sangat konversif (menjual).

Spesifikasi Teknis & Logika:
1. Struktur Halaman (Single Page Scroll):
   - Navbar: Logo, Menu (Beranda, Katalog, Harga, FAQ), Tombol "KirimKit" & "Buat Undangan".
   - Hero Section: Headline menarik, CTA utama, dan mockup smartphone menampilkan preview undangan.
   - Katalog Tema: Grid kartu tema. Data diambil via `fetch()` dari API Apps Script (`?action=getDemos`). Sertakan fitur Search dan Filter Kategori.
   - Fitur & Perbandingan: Section perbandingan Undangan Cetak vs Digital, dan grid fitur unggulan.
   - Pricing: Kartu harga promo dengan countdown timer.
   - FAQ: Accordion pertanyaan umum.
   - Footer: Info kontak, sosmed, dan link navigasi.

2. MODAL FORM ORDER (Multi-Step Wizard) - SANGAT PENTING:
   Buat sebuah Modal (Bottom Sheet di mobile) yang muncul saat tombol "Buat Undangan" diklik. Form ini dibagi menjadi 5 langkah (Step 1 sampai 5) dengan validasi per langkah:
   - Step 1 (Mempelai): Input nama panggilan pria & wanita (otomatis generate slug/ID link), nama lengkap, orang tua, IG, dan URL foto.
   - Step 2 (Acara): Tanggal utama, detail Akad & Resepsi (tanggal, jam, tempat), dan Link Google Maps.
   - Step 3 (Tema & Musik): Pilih warna tema (color swatches), paket, input URL MP3 (dengan tombol "Tes Putar" audio preview), URL Cover Image, Quote & Doa.
   - Step 4 (Story & Galeri): Input 3 linimasa Love Story, dan 4 URL foto galeri.
   - Step 5 (Kado & Fitur): Input Rekening Bank (2 bank), QRIS, alamat kado, dan checkbox saklar untuk mengaktifkan/menonaktifkan section (Love Story, Galeri, Kado, dll).

3. Logika JavaScript:
   - `autoGenerateSlug()`: Menggabungkan nama panggilan pria & wanita menjadi slug (contoh: fahri-aisyah).
   - `validateFormStep()`: Validasi field wajib (required) sebelum user bisa klik "Lanjut" ke step berikutnya.
   - `toggleFormAudioPreview()`: Memutar dan menghentikan audio MP3 dari URL yang diinput untuk preview.
   - `submitForm()`: Saat submit di Step 5, kumpulkan semua data dan kirim via `POST` ke API Apps Script (`action=createInvitation`).
   - `showSuccessModal()`: Jika API return success, tampilkan modal sukses berisi "ID Link Undangan", "Total Tagihan", dan tombol "Bayar & Aktivasi via WA" yang otomatis mengarah ke WhatsApp Admin dengan teks berisi detail pesanan dan link draft.

4. UI/UX & Styling:
   - Gunakan Tailwind CSS.
   - Tema Warna: Elegan, dominan warna Stone/Abu-abu gelap dengan aksen Gold/Emas (seperti kode referensi).
   - Gunakan Font Awesome untuk ikon, dan Google Fonts (Playfair Display & Poppins).
   - Animasi: Fade-in, hover effects, dan transisi yang halus (glassmorphism pada modal dan kartu).
   - Fully responsive (Mobile-first).

Format Output:
- Berikan satu file HTML lengkap (berisi `<style>`, `<body>`, dan `<script>`).
- Gunakan variabel `const API_URL = "YOUR_APPS_SCRIPT_URL";` di bagian atas script.
- Pastikan kode bersih, terstruktur, dan siap pakai.
```

### ✅ Checklist Implementasi

- [ ] Simpan kode sebagai `index.html` atau `salespage.html`
- [ ] Ganti `YOUR_APPS_SCRIPT_URL` dengan URL dari Tahap 1
- [ ] Test form order dari Step 1 sampai 5
- [ ] Pastikan data masuk ke Google Sheets
- [ ] Test fitur audio preview
- [ ] Test auto-generate slug
- [ ] Upload ke Blogger sebagai Homepage
- [ ] Test responsive di mobile & desktop

---

## 📱 Tahap 4: Tool KirimKit (WhatsApp Bulk Link Generator)

### 📌 Tujuan
Membuat tool untuk generate link WhatsApp personal massal dengan nama tamu otomatis.

### 🎯 Deliverables

1. **Halaman Tool dengan Fitur:**
   - Auto-fill URL undangan dari parameter
   - Input template pesan WhatsApp (dengan placeholder `{nama}` dan `{link}`)
   - Textarea untuk paste daftar nama tamu (dipisah newline/koma)
   - Generate link WA untuk setiap tamu
   - Fitur copy per link / copy semua
   - Download hasil sebagai file .TXT

2. **Logika JavaScript:**
   - Parse daftar nama
   - Replace placeholder di template
   - Encode URI untuk pesan WA
   - Generate format `https://wa.me/?text=ENCODED_PESAN`
   - Tampilkan hasil dalam list/kartu

### 💻 Prompt untuk AI

```text
Bertindaklah sebagai Senior Frontend Developer & UI/UX Expert. Saya sedang membangun halaman tool bernama "KirimKit" (WhatsApp Bulk Link Generator) untuk platform Undangan Digital. Halaman ini akan di-hosting di Google Blogger sebagai Static Page (URL: /p/kirim.html).

Tugas Anda:
Buatkan kode lengkap HTML, Tailwind CSS (via CDN), dan Vanilla JavaScript untuk halaman "KirimKit" yang modern, elegan, sangat mudah digunakan, dan mobile-first.

Spesifikasi Teknis & Logika:
1. Auto-Fill URL (PENTING):
   - Halaman harus membaca parameter URL `?url=...` (contoh: `/p/kirim.html?url=https://wedapps.blogspot.com/p/fahri-aisyah.html`).
   - Jika parameter ada, otomatis isi kolom "Link Undangan" dengan nilai tersebut.

2. Form Input Tool:
   - Input "Link Dasar Undangan": (Sudah auto-fill jika ada parameter).
   - Textarea "Template Pesan WhatsApp": Berisi teks undangan dengan placeholder `{nama}` dan `{link}`. (Contoh default: "Kepada Yth. {nama},\n\nTanpa mengurangi rasa hormat, kami mengundang Anda untuk menghadiri acara pernikahan kami.\n\nBuka Undangan: {link}?to={nama}\n\nTerima kasih.")
   - Textarea "Daftar Nama Tamu": Tempat user paste ratusan nama tamu (dipisah oleh Enter/Newline, Koma, atau Titik Koma).

3. Logika JavaScript (Generator):
   - Tombol "Generate Link WhatsApp".
   - Saat diklik, parse daftar nama (pecah berdasarkan newline/koma).
   - Bersihkan nama dari spasi berlebih.
   - Loop setiap nama, ganti `{nama}` dan `{link}` di template pesan.
   - Encode URI komponen pesan tersebut.
   - Buat format link WhatsApp: `https://wa.me/?text=ENCODED_PESAN`.
   - Tampilkan hasil di area output.

4. Area Output & Aksi:
   - Tampilkan daftar link yang sudah di-generate dalam bentuk kartu/list yang bisa di-scroll.
   - Setiap item memiliki: Nama Tamu, Tombol "Copy Link", dan Tombol "Buka di WA" (langsung klik untuk test).
   - Tombol Utama di bagian atas output: "Copy Semua Link ke Clipboard" dan "Download sebagai File .TXT".
   - Tampilkan counter: "Total X Tamu Berhasil Dibuat".

5. UI/UX & Styling (Sangat Penting):
   - Gunakan Tailwind CSS.
   - Tema Warna: Konsisten dengan WedApps (Dominan Stone/Abu-abu gelap, aksen Gold/Emas, Glassmorphism).
   - Font: Poppins & Playfair Display.
   - Ikon: Font Awesome.
   - Layout:
     - Hero section kecil yang menjelaskan kegunaan tool.
     - Section Input (Card Glassmorphism).
     - Section Output (Hidden by default, muncul setelah generate).
   - Fully responsive (Mobile-first, karena banyak user yang akan pakai tool ini langsung dari HP).
   - Berikan animasi transisi yang halus (fade-in, slide-up) dan toast notification saat "Link berhasil disalin".

Format Output:
- Berikan satu file HTML lengkap (berisi `<style>`, `<body>`, dan `<script>`).
- Pastikan kode bersih, terstruktur, dan siap di-copy paste ke editor HTML Blogger.
```

### ✅ Checklist Implementasi

- [ ] Simpan kode sebagai `kirim.html`
- [ ] Upload ke Blogger sebagai Static Page dengan URL `/p/kirim.html`
- [ ] Test dengan parameter: `?url=https://domain.com/p/fahri-aisyah.html`
- [ ] Test generate link untuk 10+ nama tamu
- [ ] Test fitur copy dan download
- [ ] Pastikan responsive di mobile

---

## 🎯 Checklist Final & Deployment

### 📋 Pre-Launch Checklist

- [ ] Semua 4 tahap sudah diimplementasi
- [ ] API URL sudah diganti di semua file
- [ ] Data dummy sudah diisi di Google Sheets
- [ ] Semua link WhatsApp sudah mengarah ke nomor admin yang benar
- [ ] Logo, nama brand, dan kontak sudah disesuaikan
- [ ] Test end-to-end: Order → Data masuk Sheets → Undangan live → Tamu RSVP

### 🚀 Deployment Steps

1. **Setup Google Sheets & Apps Script** (Tahap 1)
   - Deploy Web App dan dapatkan URL API
2. **Upload Frontend ke Blogger**
   - Homepage: `salespage.html` → Set sebagai homepage
   - Guest Page: `undangan-tamu.html` → Upload sebagai template XML
   - KirimKit: `kirim.html` → Upload sebagai Static Page `/p/kirim.html`
3. **Konfigurasi Blogger**
   - Set custom domain (opsional)
   - Aktifkan HTTPS
   - Set favicon dan meta tags
4. **Testing**
   - Test di berbagai browser (Chrome, Safari, Firefox)
   - Test di berbagai device (Android, iOS, Tablet)
   - Test kecepatan loading (PageSpeed Insights)

### 💡 Tips & Best Practices

#### 🎨 Design
- Gunakan warna brand yang konsisten (Gold/Emas untuk kesan premium)
- Pastikan kontras warna cukup untuk readability
- Gunakan whitespace yang cukup agar tidak padat
- Animasi harus smooth, tidak janky

#### ⚡ Performance
- Compress gambar sebelum upload (gunakan TinyPNG)
- Gunakan lazy loading untuk gambar galeri
- Minify CSS dan JavaScript (opsional)
- Gunakan CDN untuk library eksternal (Tailwind, Font Awesome)

#### 🔒 Security
- Jangan expose API key di frontend
- Validasi input di backend (Apps Script)
- Gunakan HTTPS untuk semua halaman
- Sanitasi data sebelum disimpan ke Sheets

#### 📱 Mobile-First
- 95% user akan buka via HP
- Pastikan tombol cukup besar untuk tap (min 44x44px)
- Gunakan font size minimal 14px untuk body text
- Test di layar kecil (320px width)

---

## 📞 Support & Maintenance

### 🔄 Update Berkala
- Cek Google Sheets secara berkala untuk data RSVP
- Backup data Sheets setiap minggu
- Update template jika ada bug/feature request

### 🐛 Troubleshooting
- **API tidak merespon:** Cek deployment status Apps Script
- **Data tidak masuk Sheets:** Cek CORS header dan format data
- **Halaman tidak load:** Cek console browser untuk error
- **Musik tidak putar:** Pastikan URL MP3 valid dan bisa diakses publik

### 📚 Dokumentasi
- Simpan semua source code di GitHub/GitLab
- Dokumentasi perubahan di file `CHANGELOG.md`
- Buat video tutorial untuk klien (opsional)

---

## 🎉 Selesai!

Selamat! Anda sekarang memiliki sistem undangan digital lengkap yang siap menghasilkan pendapatan.

**Next Steps:**

1. Marketing via sosial media (Instagram, TikTok, Facebook)
2. Buat portfolio dari undangan yang sudah dibuat
3. Tawarkan paket bundling (undangan + foto prewedding)
4. Kembangkan fitur baru berdasarkan feedback klien

**Good luck! 🚀**
