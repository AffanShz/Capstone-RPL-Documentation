# 📑 Laporan Lengkap Pengembangan Petani Maju

Dokumen ini mencakup dokumentasi teknis, visualisasi alur kerja, struktur data, riwayat pengerjaan, dan keputusan teknis penting selama pengembangan aplikasi **Petani Maju**.

---

## 1. User Workflow Diagram (Alur Pengguna)

Diagram berikut menggambarkan alur interaksi utama pengguna di dalam aplikasi.

```mermaid
graph TD
    Start([Buka Aplikasi]) --> CheckConn{Cek Internet?}
    
    CheckConn -- Offline --> LoadCache[Load Hive Cache]
    CheckConn -- Online --> FetchAPI[Fetch API Data]
    
    LoadCache --> HomeScreen
    FetchAPI --> HomeScreen
    
    subgraph "Fitur Utama"
        HomeScreen -->|Klik Cuaca| WeatherDetail[Detail Cuaca]
        HomeScreen -->|Tab Tips| TipsList[Daftar Tips]
        HomeScreen -->|Tab Kalender| CalendarView[Kalender Tanam]
        HomeScreen -->|Tab Settings| SettingsView[Pengaturan]
    end
    
    subgraph "Fitur Calendar"
        CalendarView -->|Tambah Jadwal| AddForm[Form Input Jadwal]
        AddForm -->|Simpan| SaveHive[Simpan ke Local DB]
        SaveHive -->|Trigger| ScheduleNotif[Jadwalkan Notifikasi]
        ScheduleNotif -->|Success| BackCal[Kembali ke Kalender]
    end
    
    subgraph "Fitur Tips"
        TipsList -->|Pilih Kategori| FilterTips[Filter Content]
        FilterTips -->|Klik Item| ReadTip[Baca Detail Tips]
    end

    subgraph "Background System"
        Workmanager((Background Worker)) -->|Per 1-4 Jam| CheckWeather[Cek Cuaca Ekstrem]
        CheckWeather -- Hujan/Badai --> PushNotif[Kirim Notifikasi Peringatan]
    end
```

---

## 2. Entity Relationship Diagram (ERD) & Data Schema

Struktur data aplikasi yang menggabungkan Local Database (Hive) dan Remote Database (Supabase).

```mermaid
erDiagram
    %% Local Hive Storage
    CALENDAR_SCHEDULE {
        String id PK
        String title
        DateTime date
        String note
        String category
        bool isCompleted
    }

    NOTIFICATION_SETTINGS {
        bool heavyRainAlert
        bool pestWarning
        bool morningBriefing
        bool quietMode
        TimeOfDay quietStart
        TimeOfDay quietEnd
    }

    WEATHER_CACHE {
        Map currentData
        List forecastData
        DateTime lastUpdated
    }

    %% Remote Supabase
    TIPS_ARTICLE {
        UUID id PK
        String title
        String content
        String category
        String image_url
        Timestamp created_at
    }

    %% Relationships
    CALENDAR_SCHEDULE ||--|| NOTIFICATION_SCHEDULER : triggers
    TIPS_ARTICLE ||--o{ CACHE_SERVICE : cached_locally
```

---

## 3. Log Pengerjaan Harian (Berdasarkan Git Commit)

Berikut adalah rekapitulasi pekerjaan berdasarkan riwayat commit di repository:

## 3. Log Pengerjaan Harian (Berdasarkan Git Commit)

Berikut adalah rekapitulasi pekerjaan berdasarkan riwayat commit di repository:

| Tanggal | Commit Message Utama | Detail Aktivitas |
|---------|---------------------|------------------|
| **31 Des** | `update docs` | Memperbarui dokumentasi lengkap (README, CHANGELOG, API, CONTRIBUTING) pasca refactor. |
| **31 Des** | `Refactor: Implement BLoC logic for Calendar, Notifications, and Home navigation` | Finalisasi migrasi BLoC untuk fitur Kalender dan Notifikasi, serta perbaikan navigasi Home. |
| **29 Des** | `refactor: Separate weather widgets, fix calendar notifications, improve daily forecast` | UI Cleanup: Memisahkan widget cuaca, memperbaiki bug notifikasi kalender, dan meningkatkan tampilan forecast harian. |
| **28 Des** | `optimize background service and notification service` | Optimasi performa service latar belakang agar notifikasi cuaca lebih reliable. |
| **25 Des** | `add background services logic for alert and notification weather` | Implementasi logika inti untuk deteksi peringatan cuaca di background. |
| **25 Des** | `Refactor: Implement Clean Architecture & BLoC for Home, Calendar, Tips, Pests, and Weather features` | **Major Milestone**: Migrasi data layer dan state management ke Clean Architecture + BLoC. |
| **23 Des** | `add jam, dinamis notification` | Penambahan fitur waktu dinamis pada notifikasi. |
| **22 Des** | `add feature update data to calendar` | Fitur edit/update jadwal tanam di kalender dan integrasi Supabase. |
| **21 Des** | `fix(offline): add timeout and offline mode support` | Implementasi fitur offline support dan peningkatan stabilitas. |
| **20 Des** | `perf: Add cached_network_image for faster image loading` | Optimasi loading gambar dan perbaikan info hama. |
| **17 Des** | `add notifikasi alert cuaca` | Implementasi awal notifikasi cuaca dan rilis versi v0.1.0. |
| **15 Des** | `fetch tips | implementation hive to apps` | Integrasi Supabase untuk Tips dan Hive untuk local caching. |
| **05 Des** | `fix request location` | Perbaikan logika permintaan izin lokasi. |
| **30 Nov** | `API Weather 0.1` | Integrasi awal OpenWeatherMap API dan UI Homescreen. |
| **25 Nov** | `Initial commit` | Inisialisasi project dan pembuatan UI Login/Register dasar. |

---

## 4. Key Technical Decisions (Keputusan Teknis)

Berikut adalah alasan di balik pemilihan teknologi dan arsitektur kunci:

### A. State Management: **Flutter BLoC**
- **Mengapa?** Dibandingkan `setState` atau `Provider` biasa, BLoC memberikan pemisahan yang sangat tegas antara UI dan Logic.
- **Benefit:** Code lebih mudah di-test, state lebih terprediksi (finite state machine), dan sangat cocok untuk aplikasi skala menengah-besar dengan banyak event asinkron (API calls).

### B. Local Database: **Hive**
- **Mengapa?** NoSQL database yang sangat cepat (pure Dart), ringan, dan mudah digunakan dibanding SQLite/Sqflite.
- **Benefit:** Performa read/write sangat tinggi untuk caching data cuaca dan jadwal yang sering diakses. Mendukung custom objects (`TypeAdapter`).

### C. Background Service: **Workmanager**
- **Mengapa?** Membutuhkan cara untuk menjalankan kode dart (cek cuaca) bahkan saat aplikasi *terminated* (ditutup paksa).
- **Benefit:** Bisa scheduling periodic task (misal per 1 jam) yang reliable di Android.

### D. Architecture: **Feature-First**
- **Mengapa?** Struktur `lib/features/nama_fitur` lebih scalable dibanding `lib/screens`, `lib/controllers`.
- **Benefit:** Saat tim bertambah, developer bisa bekerja di fitur spesifik tanpa konflik dengan fitur lain. Kode terkait (UI, Logic, Widget) terkumpul di satu tempat.

---

## 5. Hambatan Utama & Solusi

Selama pengembangan, kami menghadapi beberapa tantangan teknis signifikan:

### Masalah 1: Calendar Stuck State
- **Gejala:** Setelah menambah jadwal, UI kalender tidak bisa diklik dan loading spinner tidak hilang.
- **Penyebab:** State `CalendarLoading` tidak pernah dikembalikan ke `CalendarLoaded` setelah operasi sukses, atau listener tidak merespon state success dengan benar.
- **Solusi:** Membuat state khusus `CalendarScheduleAdded`. Di BLoC, setelah emit success, kita secara manual emit `CalendarLoaded` kembali agar UI me-render ulang data terbaru.

### Masalah 2: Notification Ghosting (Sync Issue)
- **Gejala:** Jadwal dihapus di aplikasi, tapi alarm notifikasi tetap bunyi.
- **Penyebab:** ID notifikasi di `flutter_local_notifications` tidak sinkron dengan ID database jadwal.
- **Solusi:** Mekanisme hashing ID. Menggunakan `schedule.id.hashCode` sebagai ID notifikasi yang konsisten. Saat edit/delete, sistem menghitung hash yang sama untuk membatalkan alarm lama sebelum membuat yang baru.

### Masalah 3: Context Usage in Async Gaps
- **Gejala:** Warning linter `Do not use BuildContexts across async gaps`. Berpotensi crash jika user pindah layar saat loading.
- **Penyebab:** Mengakses `context` (untuk navigasi/snackbar) setelah `await` function.
- **Solusi:** Selalu cek `if (!context.mounted) return;` setelah proses async sebelum menggunakan context, atau pindahkan logika navigasi ke `BlocListener` yang aman.

### Masalah 4: Background Limitation
- **Gejala:** Notifikasi cuaca tidak muncul jika HP dalam mode *Doze* (tidur).
- **Solusi:** Menggunakan `Workmanager` dengan policy `ExistingPeriodicWorkPolicy.update`, dan memastikan notifikasi memiliki priority `High` agar bisa membangunkan device sejenak.

---
*Dokumen ini disusun sebagai referensi lengkap untuk arsitektur dan perjalanan pengembangan aplikasi Petani Maju.*
