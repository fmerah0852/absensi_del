# 📱 Absensi Masuk–Keluar Kampus Berbasis Face Recognition  
### Flutter • Firebase • Face++ (Megvii)

Aplikasi ini merupakan sistem absensi mahasiswa berbasis **pengenalan wajah** menggunakan teknologi **Computer Vision**, **Flutter mobile**, **Firebase**, dan **Face++ API**.  
Fitur utama meliputi registrasi wajah, pemindaian untuk absensi, serta tampilan riwayat kehadiran.

Repository:  
🔗 https://github.com/fmerah0852/absensi_del

---

## 🚀 Fitur Utama

### ✔️ Register Wajah (Register Page)
- Mengambil foto wajah mahasiswa.
- Mengirim gambar ke Face++ untuk analisis.
- Menyimpan face_token + userId (misal NIM) ke Firebase.
- Menambahkan wajah ke FaceSet (database wajah).

### ✔️ Scan Absensi (Scan Page)
- Mengambil gambar secara real-time dari kamera.
- Mendeteksi wajah → mengambil face_token.
- Mencocokkan token dengan FaceSet (Face++ Search API).
- Menyimpan data absensi (masuk/keluar) ke Firestore.

### ✔️ Status Kehadiran (Status Page)
- Melihat riwayat absensi.
- Menampilkan jam masuk, jam keluar, dan status lain.

---

## 🏗️ Arsitektur & Modul Utama

### **1. main.dart**
Entry point aplikasi.

- Inisialisasi Firebase (`Firebase.initializeApp()`).
- Menyediakan navigasi **BottomNavigationBar**:
  - Register Page  
  - Scan Page  
  - Status Page  

---

### **2. firebase_options.dart**
Konfigurasi otomatis hasil `flutterfire configure`.

- Berisi key & ID konfigurasi Firebase.
- Mendukung Android, iOS, dan Web.
- Terhubung dengan project Firebase: **absensi-del**.

---

### **3. FaceDetectionPainter**
Widget custom untuk menggambar overlay deteksi wajah.

**Fungsi visual:**
- Menggambar lingkaran sebagai fokus wajah.
- Menampilkan garis sudut (viewfinder).
- Warna indikator:
  - 🟢 **Hijau** → wajah terdeteksi  
  - 🔴 **Merah** → tidak terdeteksi  

Digunakan pada camera preview.

---

### **4. FaceRecognitionService**
Modul inti AI dan interaksi dengan Face++.

#### Alur Kerja Service:

##### **a. getOrCreateFaceSetToken()**
- Mengecek apakah FaceSet atas nama `AttendanceAppSet` sudah ada.
- Jika tidak → membuat baru.
- Menyimpan token FaceSet ke Firestore (`face_config/config`).

##### **b. detectFace()**
- Mengirim gambar ke endpoint Face++ `/detect`.
- Mengembalikan `face_token`.

##### **c. addFace()**
- Menambahkan face_token baru ke FaceSet.

##### **d. setUserId()**
- Menghubungkan face_token dengan userId (misal NIM).

##### **e. searchFace()**
- Mencocokkan wajah yang baru ditangkap dengan seluruh anggota FaceSet.
- Mengembalikan face_token terbaik + confidence score.

---

### **5. Konversi Gambar Kamera**
Aplikasi menerima data kamera dalam format **YUV420**.

Fungsi `_convertYUV420toImage()` mengubah YUV → RGB → JPEG untuk dikirim ke API Face++.

---

## 🧰 Teknologi yang Digunakan

| Bidang | Teknologi |
|--------|-----------|
| Mobile App | Flutter (Dart) |
| Backend | Firebase Auth, Firestore, Storage |
| Face Recognition | Face++ Megvii API |
| Camera | CameraController (YUV420 Processing) |
| State Management | setState / Provider (opsional) |
| Platform | Android & Web |

---

## 📂 Struktur Direktori (Ringkas)

lib/
│
├── main.dart
├── firebase_options.dart
│
├── pages/
│   ├── register_page.dart
│   ├── scan_page.dart
│   └── status_page.dart
│
├── services/
│   ├── face_recognition_service.dart
│   └── camera_service.dart
│
└── widgets/
    └── face_detection_painter.dart
