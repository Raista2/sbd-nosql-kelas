# Sistem Simulasi Gacha

## K2 Group 16 :
- Salim (2306204604)
- Muhammad Raihan Mustofa (2306161946)
- Rowen Rodotua Harahap (2306260604)

Aplikasi web *full-stack* yang mengimplementasikan sistem permainan gacha bertema Fate dengan fitur autentikasi pengguna, pull karakter, dan penyimpanan karakter.

## 🌟 Fitur-Fitur

- Autentikasi pengguna (registrasi dan masuk)
- Sistem pemanggilan gacha dengan tingkat kelangkaan berbeda (1★ hingga 5★)
- Penyimpanan karakter
- Beragam pool gacha dengan tingkat drop rate yang dapat dikonfigurasi
- Sistem mata uang dalam permainan (Saint Quartz)
- Animasi saat pemanggilan
- Desain responsif untuk semua perangkat

## 🏗 Arsitektur

Aplikasi ini mengikuti arsitektur *microservices* dengan dua komponen utama:

### Frontend (React + Vite)

- **Teknologi yang digunakan**:
  - React 19.0.0
  - Vite 6.3.1
  - Tailwind CSS untuk styling
  - Axios untuk komunikasi API
  - React Router DOM untuk navigasi

- **Komponen Utama**:
  - `App.jsx`: Komponen utama aplikasi
  - `NavBar.jsx`: Komponen navigasi
  - `AuthContext.jsx`: Penyedia konteks autentikasi
  - **Halaman-halaman**:
    - `Login.jsx`: Masuk pengguna
    - `Register.jsx`: Registrasi pengguna
    - `Gacha.jsx`: Antarmuka pemanggilan gacha utama
    - `Card.jsx`: Tampilan kartu karakter

### Backend (Node.js + Express + MongoDB)

- **Teknologi yang digunakan**:
  - Express 5.1.0
  - Mongoose 8.14.1
  - MongoDB Atlas untuk basis data
  - CORS untuk berbagi sumber daya lintas asal
  - dotenv untuk variabel lingkungan

- **Komponen Utama**:
  - **Model-model**:
    - `Character`: Mendefinisikan atribut servant/karakter
    - `User`: Data pengguna dan inventaris
    - `GachaPool`: Konfigurasi pool pemanggilan
  
  - **Pengendali**:
    - Autentikasi pengguna
    - Mekanik gacha
    - Manajemen inventaris
    - Logika pemanggilan karakter

## 💾 Model Data

### Skema Karakter
```javascript
{
    name: String,
    class: Enum['Saber', 'Archer', 'Lancer', 'Rider', 'Caster', 'Assassin', 
                'Berserker', 'Ruler', 'Avenger', 'Alter Ego', 'Foreigner'],
    rarity: Number(1-5),
    imageUrl: String,
    description: String
}
```

### Skema Pengguna
```javascript
{
    username: String,
    email: String,
    password: String,
    servants: [{
        character: ObjectId,
        obtainedAt: Date,
        level: Number
    }],
    currency: Number,
    lastLogin: Date
}
```

### Skema Pool Gacha
```javascript
{
    name: String,
    description: String,
    imageUrl: String,
    characters: [{
        character: ObjectId,
        dropRate: Number
    }],
    cost: Number,
    isActive: Boolean,
    startDate: Date,
    endDate: Date
}
```

## 🚀 Petunjuk Pemasangan

### Prasyarat
- Node.js 16+
- Docker dan Docker Compose
- Akun MongoDB Atlas

### Variabel Lingkungan

#### Backend (.env)
```
MONGODB_URI=string_koneksi_mongodb_anda
PORT=3000
FRONTEND_URL=http://localhost:80
```

#### Frontend (.env)
```
VITE_API_BASE_URL=http://localhost:3000/api
```

### Pemasangan untuk Pengembangan

1. Klon repositori
```bash
git clone <url-repositori>
cd sbd-nosql-kelas
```

2. Jalankan aplikasi menggunakan Docker Compose
```bash
docker-compose up --build
```

Aplikasi akan tersedia di:
- Frontend: http://localhost:80
- Backend API: http://localhost:3000

## 🎮 End Point API

### Autentikasi
- `POST /api/users/register` - Registrasi pengguna baru
- `POST /api/users/login` - Masuk pengguna
- `DELETE /api/users/:userId` - Hapus akun pengguna

### Sistem Gacha
- `GET /api/pools` - Dapatkan semua pool gacha aktif
- `GET /api/pools/:poolId` - Dapatkan detail pool tertentu
- `POST /api/gacha/pull` - Lakukan pemanggilan gacha
- `GET /api/users/:userId/servants` - Dapatkan daftar servant pengguna

## 🎯 Mekanik Permainan

### Sistem Pemanggilan
- Menggunakan sistem probabilitas terbobot untuk drop karakter
- Tingkat kelangkaan mempengaruhi tingkat drop:
  - 5★: Tingkat drop terendah
  - 4★: Tingkat drop menengah
  - 1-3★: Tingkat drop lebih tinggi
- Setiap pemanggilan membutuhkan sejumlah Saint Quartz (mata uang dalam permainan)
- Pengguna baru mulai dengan 100 Saint Quartz

### Koleksi Karakter
- Karakter otomatis ditambahkan ke inventaris pengguna saat dipanggil
- Setiap karakter memiliki:
  - Kelas dan kelangkaan unik
  - Sistem level
  - Cap waktu perolehan

## 🎨 Fitur Antarmuka Pengguna

- Desain responsif yang berfungsi di perangkat mobile dan desktop
- Animasi saat pemanggilan
- Sistem warna berdasarkan kelangkaan:
  - 5★: Emas
  - 4★: Ungu
  - 1-3★: Biru
- Pemilihan pool gacha interaktif
- Pembaruan mata uang secara real-time
- Tampilan grid inventaris servant

## 🛠 Catatan Pengembangan

- Proyek menggunakan Docker untuk kontainerisasi
- MongoDB Atlas untuk hosting basis data cloud
- Tailwind CSS untuk desain responsif
- React untuk manajemen state frontend
- Express untuk implementasi API RESTful

## 🔒 Fitur Keamanan

- Rute API terproteksi
- Konfigurasi CORS
- Manajemen variabel lingkungan
- Validasi dan sanitasi input

## Screenshot Page
- Home Page
![HomePage](https://hackmd.io/_uploads/Hyx0FXmmxe.png)
- Register Page
![RegisterPage](https://hackmd.io/_uploads/SkeCFXXmxx.png)
- Login Page
![Login Page](https://hackmd.io/_uploads/BJlCYQmmel.png)
- Gacha Page
![Select Banner](https://hackmd.io/_uploads/B1gCFmQQle.png)
![Pull button dan penyimpanan kode](https://hackmd.io/_uploads/HJlAYmQ7ex.png)

## ⚠️ Pertimbangan Produksi

1. Implementasi hash kata sandi yang tepat menggunakan bcrypt
2. Tambahkan pembatasan rate untuk titik akhir API
3. Implementasi JWT untuk autentikasi
4. Siapkan sertifikat SSL/TLS yang tepat
5. Konfigurasi indeks MongoDB yang tepat
6. Implementasi pencatatan error yang tepat
7. Siapkan pemantauan dan analitik
8. Implementasi strategi backup untuk basis data