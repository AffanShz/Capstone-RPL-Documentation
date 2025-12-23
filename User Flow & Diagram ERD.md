# Petani Maju - Dokumentasi User Flow & ERD

Dokumentasi lengkap mengenai alur pengguna (User Flow) dan Entity Relationship Diagram (ERD) untuk aplikasi Petani Maju.

---

## 📱 Gambaran Umum Aplikasi

**Petani Maju** adalah aplikasi mobile berbasis Flutter yang dirancang untuk membantu petani Indonesia dengan fitur:
- Informasi cuaca real-time berdasarkan lokasi
- Kalender perencanaan jadwal tanam
- Tips pertanian dari berbagai kategori
- Informasi hama & penyakit tanaman
- Mode offline dengan caching lokal

---

## 🔄 User Flow Diagram

```mermaid
flowchart TD
    A[App Launch] --> B{Cek Koneksi Internet}
    B -->|Online| C[Inisialisasi Supabase]
    B -->|Offline/Timeout| D[Mode Offline - Load Cache]
    C --> E[MainScreen dengan Navigation Bar]
    D --> E
    
    E --> F[Beranda / HomeScreen]
    E --> G[Kalender / CalendarScreen]
    E --> H[Tips / TipsScreen]
    E --> I[Pengaturan / SettingsScreen]
    
    subgraph HomeFlow["Beranda Flow"]
        F --> F1[Tampilkan Cuaca Saat Ini]
        F1 --> F2{Lokasi Tersedia?}
        F2 -->|Ya| F3[Ambil Koordinat GPS]
        F2 -->|Tidak| F4[Gunakan Lokasi Default/Cache]
        F3 --> F5[Fetch Weather API]
        F4 --> F5
        F5 --> F6[Tampilkan Data Cuaca]
        F6 --> F7{Prediksi Hujan 24 Jam?}
        F7 -->|Ya| F8[Tampilkan Alert & Notifikasi]
        F7 -->|Tidak| F9[Lanjut Normal]
        
        F --> F10[Lihat Prediksi Cuaca]
        F --> F11[Lihat Tips Ringkas]
        F --> F12[Quick Access Menu]
        
        F12 --> P1[Info Hama & Penyakit]
        F10 --> WD[Weather Detail Screen]
    end
    
    subgraph CalendarFlow["Kalender Flow"]
        G --> G1[Tampilkan Kalender Bulanan]
        G1 --> G2[Pilih Tanggal]
        G2 --> G3[Lihat Jadwal Hari Itu]
        G3 --> G4{Aksi}
        G4 -->|Tambah| G5[Dialog Tambah Jadwal]
        G4 -->|Edit| G6[Dialog Edit Jadwal]
        G4 -->|Hapus| G7[Konfirmasi Hapus]
        G5 --> G8[Simpan ke Supabase]
        G6 --> G8
        G7 --> G9[Hapus dari Supabase]
        G8 --> G10[Jadwalkan Notifikasi]
        G8 --> G1
        G9 --> G1
    end
    
    subgraph TipsFlow["Tips Flow"]
        H --> H1[Tampilkan Semua Tips]
        H1 --> H2[Filter by Kategori]
        H2 --> H3[Cari Tips]
        H3 --> H4[Pilih Tips]
        H4 --> TD[Tips Detail Screen]
        TD --> TD1[Lihat Gambar]
        TD --> TD2[Baca Konten Lengkap]
    end
    
    subgraph PestFlow["Hama & Penyakit Flow"]
        P1 --> P2[Tampilkan List Hama]
        P2 --> P3[Filter by Kategori]
        P3 --> P4[Cari Hama]
        P4 --> P5[Pilih Hama]
        P5 --> PD[Pest Detail Screen]
        PD --> PD1[Lihat Deskripsi]
        PD --> PD2[Lihat Ciri-ciri]
        PD --> PD3[Lihat Dampak]
        PD --> PD4[Cara Mengatasi - BottomSheet]
    end
    
    subgraph SettingsFlow["Pengaturan Flow"]
        I --> I1[Lihat Profil User]
        I --> I2[Pengaturan Akun]
        I --> I3[Pengaturan Preferensi]
        I3 --> I4[Toggle Mode Offline]
        I4 --> I5[Notifikasi]
        I --> I6[Tentang Aplikasi]
        I --> I7[Logout]
    end
```

---

## 📊 Entity Relationship Diagram (ERD)

```mermaid
erDiagram
    USER ||--o{ JADWAL_TANAM : creates
    USER ||--o{ CACHE_DATA : stores
    
    TIPS ||--o{ CATEGORY : belongs_to
    HAMA ||--o{ KATEGORI : belongs_to
    
    WEATHER_API ||--|| CACHE_DATA : cached_in
    LOCATION ||--|| CACHE_DATA : cached_in

    USER {
        string nama "Pak Budi Santoso"
        string email "budisantoso@email.com"
        string lokasi "Subang, Jawa Barat"
    }
    
    JADWAL_TANAM {
        int id PK
        timestamp created_at
        string nama_tanaman
        date tanggal_tanam
        string catatan
    }
    
    TIPS {
        int id PK
        timestamp created_at
        string title
        string category
        string image_url
        string content
    }
    
    HAMA {
        int id PK
        timestamp created_at
        string nama
        string kategori
        string gambar_url
        string deskripsi
        string ciri_ciri
        string dampak
        string cara_mengatasi
    }
    
    CACHE_DATA {
        string key PK
        json current_weather
        json forecast_list
        json tips_list
        json pests_list
        string detailed_location
        double latitude
        double longitude
        timestamp cache_time
        boolean offline_mode
    }
    
    WEATHER_API {
        json main "temp, humidity, feels_like"
        json weather "main, description, icon"
        json wind "speed, deg"
        string name "city name"
        json coord "lat, lon"
    }
    
    LOCATION {
        double latitude
        double longitude
        string full_address
        string district
        string city
    }
```

---

## 🗂️ Struktur Data & Relasi

### 1. Tabel Supabase

#### `jadwal_tanam` (Planting Schedule)
| Kolom | Tipe | Deskripsi |
|-------|------|-----------|
| `id` | int | Primary Key, auto-increment |
| `created_at` | timestamp | Waktu pembuatan record |
| `nama_tanaman` | string | Nama tanaman (e.g., "Padi", "Jagung") |
| `tanggal_tanam` | date | Tanggal jadwal tanam |
| `catatan` | string | Catatan tambahan (opsional) |

#### `tips` (Farming Tips)
| Kolom | Tipe | Deskripsi |
|-------|------|-----------|
| `id` | int | Primary Key |
| `created_at` | timestamp | Waktu pembuatan |
| `title` | string | Judul tips |
| `category` | string | Kategori (Padi, Jagung, Nutrisi, dll) |
| `image_url` | string | URL gambar ilustrasi |
| `content` | text | Konten lengkap tips |

#### `hama` (Pests & Diseases)
| Kolom | Tipe | Deskripsi |
|-------|------|-----------|
| `id` | int | Primary Key |
| `created_at` | timestamp | Waktu pembuatan |
| `nama` | string | Nama hama/penyakit |
| `kategori` | string | Kategori (Hama Padi, Hama Jagung, Hama Umum) |
| `gambar_url` | string | URL gambar hama |
| `deskripsi` | text | Deskripsi umum |
| `ciri_ciri` | text | Ciri-ciri hama/penyakit |
| `dampak` | text | Dampak (dipisahkan dengan `\n`) |
| `cara_mengatasi` | text | Solusi penanganan |

---

### 2. External API

#### OpenWeatherMap API
**Endpoints yang digunakan:**
- `GET /weather` - Current weather data
- `GET /forecast` - 5-day/3-hour forecast

**Response Structure:**
```json
{
  "main": {
    "temp": 28.5,
    "feels_like": 32.1,
    "humidity": 80
  },
  "weather": [{
    "main": "Rain",
    "description": "light rain",
    "icon": "10d"
  }],
  "wind": {
    "speed": 3.5
  },
  "name": "Subang"
}
```

#### Nominatim (OpenStreetMap) - Location Service
**Endpoint:** Reverse Geocoding
**Response:** Detailed address from coordinates

---

### 3. Local Cache (Hive)

| Key | Tipe Data | Deskripsi |
|-----|-----------|-----------|
| `current_weather` | Map | Data cuaca saat ini |
| `forecast` | List | Data prediksi cuaca |
| `tips` | List | Data tips pertanian |
| `pests` | List | Data hama & penyakit |
| `detailed_location` | String | Alamat lengkap |
| `latitude` | double | Koordinat latitude |
| `longitude` | double | Koordinat longitude |
| `cache_time` | DateTime | Waktu cache disimpan |
| `offline_mode` | bool | Status mode offline |

---

## 🔗 Alur Data & Integrasi

```mermaid
flowchart LR
    subgraph External["External Services"]
        OWM[OpenWeatherMap API]
        NOM[Nominatim API]
        SUP[Supabase Database]
    end
    
    subgraph App["Petani Maju App"]
        WS[WeatherService]
        LS[LocationService]
        PS[PestService]
        TS[TipsService]
        SS[PlantingScheduleService]
        CS[CacheService]
        NS[NotificationService]
    end
    
    subgraph Screens["UI Screens"]
        HS[HomeScreen]
        CAS[CalendarScreen]
        TIS[TipsScreen]
        PES[PestScreen]
        SES[SettingsScreen]
    end
    
    OWM <--> WS
    NOM <--> LS
    SUP <--> PS
    SUP <--> TS
    SUP <--> SS
    
    WS --> CS
    LS --> CS
    PS --> CS
    TS --> CS
    
    CS --> HS
    CS --> CAS
    CS --> TIS
    CS --> PES
    CS --> SES
    
    WS --> HS
    LS --> HS
    PS --> PES
    TS --> TIS
    SS --> CAS
    NS --> CAS
    NS --> HS
```

---

## 📲 Alur Navigasi Detail

### 1. Startup Flow
```
main.dart
├── Load .env
├── Initialize Hive (CacheService)
├── Initialize Notifications
├── Initialize Supabase (with 10s timeout)
│   ├── Success → Online Mode
│   └── Timeout/Error → Offline Mode
└── Run MainApp → MainScreen (NavBar)
```

### 2. Home Screen Flow
```
HomeScreen
├── Load Cache First (instant display)
├── Check Location Permission
│   ├── Granted → Get Current Position
│   └── Denied → Use Default/Cached Location
├── Fetch Weather Data
│   ├── Current Weather → Display Main Card
│   └── Forecast → Display Hourly List
├── Check Rain Prediction (24h)
│   └── If Rain → Show Alert + Push Notification
├── Display Tips List (from cache/API)
└── Quick Access Menu
    ├── Info Hama → PestScreen
    └── Weather Card → WeatherDetailScreen
```

### 3. Calendar Screen Flow
```
CalendarScreen
├── Load Schedules from Supabase
├── Display Calendar (TableCalendar)
├── Select Day → Show Events for Day
└── Actions:
    ├── Add (FAB) → Schedule Dialog
    │   ├── Input: Nama Tanaman, Catatan
    │   ├── Save to Supabase
    │   └── Schedule Notification
    ├── Edit (Icon) → Edit Dialog
    │   └── Update in Supabase
    └── Delete (Icon) → Confirm Dialog
        └── Delete from Supabase
```

### 4. Tips Screen Flow
```
TipsScreen
├── Load from Cache
├── Fetch from Supabase (if online)
├── Display Grid of Tips
├── Category Filter (Semua, Padi, Jagung, Nutrisi)
├── Search Bar
└── Tap Card → TipsDetailScreen
    ├── Hero Image
    ├── Category Badge
    ├── Title
    └── Full Content
```

### 5. Pest Screen Flow
```
PestScreen
├── Load from Cache
├── Fetch from Supabase (if online)
├── Display List of Pests
├── Category Filter (Semua, Hama Padi, Hama Jagung, Hama Umum)
├── Search Bar (debounced)
└── Tap Card → PestDetailScreen
    ├── Image
    ├── Category Badge
    ├── Description
    ├── Characteristics
    ├── Impact (bullet points)
    └── Solution Button → BottomSheet
```

### 6. Settings Screen Flow
```
SettingsScreen
├── User Profile Section
├── Account Settings
│   ├── My Profile
│   └── Location
├── Preferences
│   ├── Notifications
│   ├── Language
│   └── Offline Mode Toggle
├── About
│   ├── Help & Support
│   ├── Terms & Conditions
│   └── About App (v1.0.0)
└── Logout Button
```

---

## 🔔 Notification Flow

```mermaid
sequenceDiagram
    participant User
    participant App
    participant NotificationService
    participant System
    
    App->>NotificationService: Request Permissions
    NotificationService->>System: requestPermissions()
    System-->>User: Permission Dialog
    User-->>System: Grant/Deny
    
    Note over App: Weather Check
    App->>App: Check Rain in 24h
    alt Rain Predicted
        App->>NotificationService: showNotification()
        NotificationService->>System: Display Notification
        System-->>User: "Peringatan Hujan!"
    end
    
    Note over App: Calendar Schedule
    User->>App: Add Planting Schedule
    App->>NotificationService: scheduleNotification()
    NotificationService->>System: Schedule for Date
    System-->>User: "Jadwal Tanam: [Tanaman]"
```

---

## 📁 Struktur Folder Proyek

```
lib/
├── main.dart                    # Entry point
├── core/
│   ├── constants/
│   │   └── colors.dart          # App color palette
│   ├── services/
│   │   ├── cache_service.dart   # Hive caching
│   │   └── notification_service.dart  # Local notifications
│   └── theme/
├── data/
│   └── datasources/
│       ├── weather_service.dart      # OpenWeatherMap API
│       ├── location_service.dart     # Nominatim API
│       ├── tips_services.dart        # Supabase tips
│       ├── pest_services.dart        # Supabase hama
│       └── planting_schedule_service.dart  # Supabase jadwal
├── features/
│   ├── home/
│   │   ├── screens/home_screen.dart
│   │   └── widgets/
│   │       ├── forecast_list.dart
│   │       ├── quick_access.dart
│   │       ├── tips_list.dart
│   │       └── weather_alert.dart
│   ├── calendar/
│   │   └── screens/calendar_screen.dart
│   ├── tips/
│   │   └── screens/
│   │       ├── tips_screen.dart
│   │       └── tips_detail_screen.dart
│   ├── pests/
│   │   └── screens/
│   │       ├── pest_screen.dart
│   │       └── pest_detail_screen.dart
│   ├── weather/
│   │   └── screens/weather_detail_screen.dart
│   └── settings/
│       └── screens/settings_screen.dart
├── utils/
│   └── weather_utils.dart        # Weather translation
└── widgets/
    ├── navbaar.dart              # Bottom Navigation
    ├── custom_app_bar.dart
    ├── main_weather_card.dart
    └── section_header.dart
```

---

## ✅ Ringkasan

Aplikasi **Petani Maju** menggunakan arsitektur yang terstruktur dengan:

1. **4 Tab Utama**: Beranda, Kalender, Tips, Pengaturan
2. **3 Tabel Supabase**: `jadwal_tanam`, `tips`, `hama`
3. **2 External API**: OpenWeatherMap, Nominatim
4. **1 Local Cache**: Hive untuk mode offline
5. **Notifikasi**: Weather alerts & schedule reminders

Relasi data bersifat **loosely coupled** dimana setiap entitas independen dan data di-cache secara lokal untuk mendukung penggunaan offline.
