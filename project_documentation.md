# 🌾 Dokumentasi Proyek - Petani Maju

Aplikasi Mobile Pintar untuk Kemajuan Pertanian Indonesia

---

## 📋 Daftar Isi

- [Latar Belakang](#latar-belakang)
- [Tujuan](#tujuan)
- [Nilai Inovasi dan Dampak](#nilai-inovasi-dan-dampak)
- [Deskripsi Fungsional](#deskripsi-fungsional)

---

## Latar Belakang

### Kondisi Pertanian Indonesia

Indonesia merupakan negara agraris dengan **lebih dari 33 juta petani** yang menjadi tulang punggung ketahanan pangan nasional. Namun, sektor pertanian menghadapi berbagai tantangan:

| Tantangan | Dampak |
|-----------|--------|
| **Keterbatasan Informasi Cuaca** | Petani sulit memprediksi waktu tanam optimal, risiko gagal panen tinggi |
| **Serangan Hama & Penyakit** | Kerugian ekonomi besar akibat keterlambatan deteksi dan penanganan |
| **Manajemen Jadwal Manual** | Lupa waktu pemupukan, penyiraman, dan perawatan tanaman |
| **Kurangnya Edukasi Modern** | Teknik budidaya tradisional yang kurang efisien |
| **Kesenjangan Digital** | Informasi pertanian tidak sampai ke petani di daerah terpencil |

### Permasalahan Utama

```
┌─────────────────────────────────────────────────────────────┐
│                    PERMASALAHAN PETANI                      │
├─────────────────────────────────────────────────────────────┤
│  📊 Data cuaca tidak akurat untuk skala lokal               │
│  ⏰ Tidak ada sistem pengingat aktivitas pertanian          │
│  🐛 Informasi hama tersebar dan sulit diakses               │
│  📚 Konten edukasi tidak tersedia secara offline            │
│  🔔 Tidak ada peringatan dini cuaca ekstrem                 │
└─────────────────────────────────────────────────────────────┘
```

### Kebutuhan Solusi

Diperlukan sebuah **aplikasi mobile terintegrasi** yang dapat:
- Memberikan informasi cuaca real-time berbasis lokasi
- Mengelola jadwal aktivitas pertanian dengan notifikasi otomatis
- Menyediakan database hama & penyakit yang komprehensif
- Menawarkan konten edukasi yang dapat diakses offline
- Mengirimkan peringatan dini cuaca untuk mitigasi risiko

---

## Tujuan

### Tujuan Umum

Mengembangkan aplikasi mobile **"Petani Maju"** sebagai solusi digital terintegrasi untuk membantu petani Indonesia meningkatkan produktivitas dan mengurangi risiko gagal panen melalui pemanfaatan teknologi informasi.

### Tujuan Khusus

| No | Tujuan | Indikator Keberhasilan |
|----|--------|------------------------|
| 1 | **Menyediakan informasi cuaca akurat** | Data cuaca real-time dari OpenWeatherMap dengan prakiraan 5 hari |
| 2 | **Membangun sistem peringatan dini** | Notifikasi otomatis untuk hujan deras, angin kencang, dan badai petir |
| 3 | **Digitalisasi manajemen jadwal tanam** | Kalender interaktif dengan pengingat H-1, H-1 jam, dan tepat waktu |
| 4 | **Menyediakan database hama & penyakit** | Informasi lengkap dengan gejala, pengendalian, dan pencegahan |
| 5 | **Mendistribusikan konten edukasi** | Tips pertanian terkurasi untuk Padi, Jagung, dan Nutrisi Tanaman |
| 6 | **Mendukung penggunaan offline** | Caching data lokal untuk akses tanpa internet |
| 7 | **Personalisasi pengalaman pengguna** | Profil lokal dan pengaturan notifikasi yang dapat dikustomisasi |

---

## Nilai Inovasi dan Dampak

### 🚀 Nilai Inovasi

#### 1. Arsitektur Offline-First
```
┌─────────────────────────────────────────┐
│         OFFLINE-FIRST APPROACH          │
├─────────────────────────────────────────┤
│  1. Load data dari cache lokal (instant)│
│  2. Tampilkan ke pengguna               │
│  3. Fetch data baru di background       │
│  4. Update cache jika berhasil          │
│  5. Tetap fungsional tanpa internet     │
└─────────────────────────────────────────┘
```
**Inovasi:** Petani di daerah dengan koneksi internet terbatas tetap dapat mengakses informasi penting.

#### 2. Sistem Notifikasi Pintar
- **Morning Briefing:** Ringkasan cuaca harian di pagi hari
- **Weather Alerts:** Peringatan otomatis cuaca ekstrem
- **Calendar Reminders:** Pengingat jadwal multi-level (H-1, H-1jam, H)
- **Quiet Mode:** Mode tenang otomatis di malam hari (22:00-05:00)
- **Background Processing:** Notifikasi tetap berjalan meski app tertutup

#### 3. Analisis Risiko Hama Berbasis Cuaca
Sistem yang menganalisis kondisi cuaca (suhu & kelembaban) untuk memberikan peringatan potensi serangan hama.

#### 4. Lokalisasi Bahasa Indonesia
Aplikasi sepenuhnya mendukung Bahasa Indonesia dengan opsi English, termasuk terjemahan deskripsi cuaca.

### 📈 Dampak Pemanfaatan

#### Dampak Ekonomi

| Aspek | Sebelum | Sesudah | Potensi Peningkatan |
|-------|---------|---------|---------------------|
| Gagal panen akibat cuaca | Tinggi | Rendah | ↓ 30-50% kerugian |
| Efisiensi pemupukan | Manual, sering lupa | Terjadwal otomatis | ↑ 20-30% efisiensi |
| Penanganan hama | Reaktif (setelah serangan) | Preventif (analisis risiko) | ↓ 40% kerugian hama |

#### Dampak Sosial

- **Peningkatan Literasi Digital:** Petani terbiasa menggunakan teknologi
- **Akses Informasi Merata:** Petani di daerah terpencil mendapat informasi sama
- **Kemandirian Petani:** Mengurangi ketergantungan pada penyuluh pertanian
- **Komunitas Terkoneksi:** Potensi berbagi tips dan pengalaman

#### Dampak Lingkungan

- **Penggunaan Pestisida Efisien:** Pengendalian hama tepat sasaran
- **Konservasi Air:** Smart watering berdasarkan cuaca
- **Pertanian Berkelanjutan:** Edukasi teknik budidaya ramah lingkungan

---

## Deskripsi Fungsional

### Arsitektur Sistem

```
┌──────────────────────────────────────────────────────────────┐
│                    PETANI MAJU APP                           │
├──────────────────────────────────────────────────────────────┤
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐           │
│  │   Flutter   │  │    BLoC     │  │    Hive     │           │
│  │     UI      │←→│   Pattern   │←→│   Cache     │           │
│  └─────────────┘  └─────────────┘  └─────────────┘           │
│         ↑                ↑                ↑                  │
│         │                │                │                  │
│  ┌──────┴────────────────┴────────────────┴──────┐           │
│  │              Repository Layer                 │           │
│  └───────────────────────────────────────────────┘           │
│         ↑                ↑                ↑                  │
│  ┌──────┴──────┐  ┌──────┴──────┐  ┌──────┴──────┐           │
│  │ OpenWeather │  │  Supabase   │  │ Background  │           │
│  │     API     │  │  Database   │  │  Service    │           │
│  └─────────────┘  └─────────────┘  └─────────────┘           │
└──────────────────────────────────────────────────────────────┘
```

---

### Detail Fitur

#### 1. 🌤️ Sistem Cuaca Cerdas

**Deskripsi:** Menampilkan informasi cuaca real-time dan prakiraan untuk membantu petani merencanakan aktivitas.

| Komponen | Fungsi |
|----------|--------|
| **Main Weather Card** | Suhu, kondisi cuaca, lokasi, icon dinamis |
| **Hourly Forecast** | Prakiraan per jam untuk 24 jam ke depan |
| **Daily Forecast** | Prakiraan 5 hari dengan suhu min/max |
| **Weather Detail** | Kelembaban, tekanan udara, kecepatan angin, visibilitas |
| **Pest Risk Analysis** | Analisis risiko hama berdasarkan suhu & kelembaban |

**Sumber Data:** OpenWeatherMap API

**Teknologi:**
- Koordinat GPS untuk lokasi akurat
- Caching 30 menit untuk mengurangi API calls
- Reverse geocoding untuk nama lokasi detail

---

#### 2. 🔔 Sistem Notifikasi Pintar

**Deskripsi:** Sistem notifikasi background yang memberikan informasi dan pengingat tepat waktu.

| Jenis Notifikasi | Waktu | Konten |
|------------------|-------|--------|
| **Morning Briefing** | 05:00 (konfigurabel) | Ringkasan cuaca hari ini |
| **Weather Alert** | Real-time | Peringatan hujan deras, angin kencang, badai |
| **Calendar Reminder** | H-1, H-1 jam, Tepat waktu | Pengingat jadwal aktivitas |

**Fitur Lanjutan:**
- ✅ **Quiet Mode:** Notifikasi diblokir di jam tertentu (22:00-05:00)
- ✅ **Konfigurasi Granular:** Toggle individual untuk setiap jenis notifikasi
- ✅ **Background Processing:** Menggunakan Workmanager & AlarmManager
- ✅ **Offline Support:** Notifikasi tetap berjalan tanpa internet

---

#### 3. 📅 Kalender Tanam Digital

**Deskripsi:** Manajemen jadwal aktivitas pertanian dengan kalender interaktif.

| Fungsi | Detail |
|--------|--------|
| **Lihat Kalender** | Kalender bulanan dengan marker event |
| **Tambah Jadwal** | Input nama kegiatan, tanggal/waktu, kategori, catatan |
| **Edit Jadwal** | Modifikasi jadwal yang sudah ada |
| **Hapus Jadwal** | Konfirmasi sebelum menghapus |
| **Tandai Selesai** | Status completed untuk tracking |
| **Rekomendasi Bulanan** | Saran aktivitas berdasarkan bulan berjalan |

**Integrasi Notifikasi:**
- Otomatis setup pengingat saat membuat jadwal
- Update pengingat saat jadwal diedit
- Cancel pengingat saat jadwal dihapus

---

#### 4. 📚 Tips Pertanian

**Deskripsi:** Database artikel edukasi terkurasi untuk petani.

| Fitur | Fungsi |
|-------|--------|
| **Kategori** | Filter: Semua, Padi, Jagung, Nutrisi |
| **Search** | Pencarian berdasarkan keyword |
| **Detail Artikel** | Gambar header, konten lengkap |
| **Offline Cache** | Artikel tersimpan untuk akses offline |

**Sumber Data:** Supabase Database

---

#### 5. 🐛 Informasi Hama & Penyakit

**Deskripsi:** Database komprehensif tentang hama dan penyakit tanaman.

| Konten | Detail |
|--------|--------|
| **Tab Hama** | Daftar hama umum tanaman |
| **Tab Penyakit** | Daftar penyakit tanaman |
| **Search** | Pencarian berdasarkan nama |
| **Detail** | Deskripsi, gejala, pengendalian, pencegahan, gambar |

**Sumber Data:** Supabase Database

---

#### 6. ⚙️ Pengaturan Aplikasi

**Deskripsi:** Personalisasi dan konfigurasi aplikasi.

| Menu | Fungsi |
|------|--------|
| **Profil** | Edit nama dan foto profil (local storage) |
| **Notifikasi** | Toggle granular untuk semua jenis notifikasi |
| **Bahasa** | Pilihan Indonesia / English |
| **Bantuan** | FAQ, Email Support, Rate App |
| **Tentang** | Versi aplikasi, Tim Developer, Lisensi |

---

### Teknologi yang Digunakan

| Layer | Teknologi |
|-------|-----------|
| **Framework** | Flutter 3.x (Dart) |
| **State Management** | Flutter BLoC |
| **Backend Database** | Supabase (PostgreSQL) |
| **Weather API** | OpenWeatherMap |
| **Geocoding** | OpenStreetMap Nominatim |
| **Local Storage** | Hive (NoSQL) |
| **Background Service** | Workmanager & Android Alarm Manager |
| **Notifications** | Flutter Local Notifications |
| **Localization** | Easy Localization |

---

## Daftar Komponen/Library dan Lisensi

### Dependencies Utama

| Library | Versi | Fungsi | Lisensi |
|---------|-------|--------|---------|
| **flutter_bloc** | ^8.1.6 | State management dengan BLoC pattern | MIT |
| **equatable** | ^2.0.7 | Value equality untuk Dart objects | MIT |
| **supabase_flutter** | ^2.0.0 | Backend database & authentication | MIT |
| **hive** | ^2.2.3 | NoSQL local database | Apache 2.0 |
| **hive_flutter** | ^1.1.0 | Hive adapter untuk Flutter | Apache 2.0 |
| **http** | ^1.6.0 | HTTP client untuk API calls | BSD-3-Clause |
| **geolocator** | ^14.0.2 | Akses lokasi GPS device | MIT |
| **connectivity_plus** | ^6.1.0 | Monitoring koneksi internet | BSD-3-Clause |
| **flutter_local_notifications** | ^17.0.0 | Local push notifications | BSD-3-Clause |
| **workmanager** | ^0.9.0+3 | Background task scheduling | MIT |
| **timezone** | ^0.9.2 | Timezone handling | BSD-2-Clause |
| **flutter_dotenv** | ^5.1.0 | Environment variable management | MIT |
| **intl** | ^0.20.2 | Internationalization & formatting | BSD-3-Clause |
| **easy_localization** | ^3.0.8 | Multi-language support | MIT |
| **table_calendar** | ^3.1.2 | Interactive calendar widget | Apache 2.0 |
| **cached_network_image** | ^3.4.1 | Image caching dari network | MIT |
| **flutter_cache_manager** | ^3.4.1 | Generic cache manager | MIT |
| **image_picker** | ^1.2.1 | Memilih gambar dari gallery/camera | Apache 2.0 |
| **url_launcher** | ^6.3.2 | Membuka URL di browser/app | BSD-3-Clause |
| **permission_handler** | ^12.0.1 | Request runtime permissions | MIT |
| **flutter_secure_storage** | ^10.0.0 | Encrypted secure storage | BSD-3-Clause |
| **shimmer** | ^3.0.0 | Shimmer loading effect | MIT |

### Dev Dependencies

| Library | Versi | Fungsi | Lisensi |
|---------|-------|--------|---------|
| **flutter_test** | SDK | Unit & widget testing | BSD-3-Clause |
| **flutter_launcher_icons** | ^0.13.1 | Generate app icons | MIT |
| **flutter_lints** | ^5.0.0 | Code linting rules | BSD-3-Clause |

### External API Services

| Service | Fungsi | Lisensi/Terms |
|---------|--------|---------------|
| **OpenWeatherMap** | Data cuaca real-time & forecast | [Free Tier - Attribution Required](https://openweathermap.org/terms) |
| **Supabase** | Backend database PostgreSQL | [Apache 2.0](https://supabase.com/docs/guides/self-hosting) |
| **OpenStreetMap Nominatim** | Reverse geocoding | [ODbL](https://www.openstreetmap.org/copyright) |

### Ringkasan Lisensi

```
┌─────────────────────────────────────────────────────────────┐
│                    RINGKASAN LISENSI                        │
├─────────────────────────────────────────────────────────────┤
│  MIT License         : 13 libraries (bebas komersial)       │
│  BSD-3-Clause        : 8 libraries (bebas komersial)        │
│  Apache 2.0          : 4 libraries (bebas komersial)        │
│  BSD-2-Clause        : 1 library (bebas komersial)          │
├─────────────────────────────────────────────────────────────┤
│  ✅ Semua library menggunakan lisensi open-source yang      │
│     memperbolehkan penggunaan komersial tanpa biaya.        │
└─────────────────────────────────────────────────────────────┘
```

---

*Dokumentasi ini dibuat untuk presentasi Proyek Petani Maju*
