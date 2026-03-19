<div align="center">

# 🛡️ JhopanStoreVPN (Android)

**Klien VPN Android Next-Gen Berbasis VLESS + Xray**

<p align="center">
  <img src="https://img.shields.io/badge/Android-24%2B-3DDC84?logo=android&logoColor=white" alt="Android API 24+"/>
  <img src="https://img.shields.io/badge/Kotlin-1.9.0-7F52FF?logo=kotlin&logoColor=white" alt="Kotlin"/>
  <img src="https://img.shields.io/badge/Jetpack_Compose-UI-4285F4?logo=android&logoColor=white" alt="Jetpack Compose"/>
  <img src="https://img.shields.io/github/v/release/jhopan/JhopanStoreVPN_Xray_APK?color=blue&label=Latest%20Release" alt="Release"/>
</p>

Dibangun dengan antarmuka **Jetpack Compose** murni, arsitektur *binary dependency* revolusioner, dan mengutamakan stabilitas koneksi tingkat tinggi di berbagai kondisi jaringan.

---
</div>

## 📑 Daftar Isi
- [🚀 Fitur Utama](#-fitur-utama)
- [📦 Arsitektur Binary & Performa](#-arsitektur-binary--performa)
- [🛠️ Cara Build Sendiri (Local)](#️-cara-build-sendiri-local)
- [⚙️ Pengaturan URL Lanjutan](#️-pengaturan-url-lanjutan)
- [🔄 CI/CD & Pipeline Release](#-cicd--pipeline-release)

---

## 🚀 Fitur Utama

Aplikasi ini tidak sekadar VPN biasa; ini dirancang dengan berbagai fitur tingkat lanjut untuk memastikan **stabilitas, keamanan, dan kenyamanan maksimum** saat berselancar di internet:

### ⚡ Performa & Koneksi
- **Core Xray Terintegrasi (VLESS)**: Ditenagai langsung oleh arsitektur Xray (via libXray.aar) untuk menyediakan koneksi protokol VLESS yang sangat responsif, ngebut, dan tangguh terhadap *Deep Packet Inspection* (DPI).
- **Routing Cerdas Tanpa Root (tun2socks)**: Menggunakan engine libtun2socks.so secara efisien untuk menangkap dan merutekan seluruh lalu lintas aplikasi di sistem Android secara transparan. Tidak perlu akses *root*!

### 🔄 Sistem Pemulihan Otonom
Mencegah pemutusan jaringan yang mengganggu akibat transisi sinyal (misalnya dari Wi-Fi ke Seluler):
- **Auto-Reconnect Pintar**: Mekanisme koneksi ulang otomatis yang 100% bisa dikustomisasi.
- **Max Retry Control**: Tentukan sendiri batasan maksimal sistem mencoba menghubungkan ulang.
- **Delay Asinkron**: Sesuaikan jeda waktu (*Reconnect Delay*) antar percobaan ulang untuk menjaga baterai dan terhindar dari pemblokiran server akibat *spamming* koneksi saat *blank spot*.

### 📡 Monitoring & Stabilitas
- **Real-Time Ping Monitor**: Pantau selalu kualitas sambungan (latensi) secara *live* di layar. Ritme pengecekan (*Ping Interval*) dapat disetel sesuai keinginan.
- **Stabilitas Background Prioritas Tertinggi**:
  - **Foreground Service Elite**: Membungkus mesin VPN dalam layanan dengan *Notifikasi Status* persisten yang elegan. Sangat sulit "di-kill" oleh *task killer* bawaan Android.
  - **Wake Lock (Opsional)**: Senjata rahasia agar koneksi server dan CPU Android Anda tidak terhenti (*stand-by*) saat layar dikunci, menjamin koneksi selalu aktif di latar belakang.

### 🎨 Desain Modern
- **UI/UX Futuristik**: Antarmuka dibangun _from scratch_ menggunakan **Jetpack Compose**, menyuguhkan animasi yang mulus, respon sentuh seketika, dan kompatibilitas tata letak yang sempurna di semua ukuran layar.

---

## 📦 Arsitektur Binary & Performa

Kami memahami bahwa ukuran repositori yang membengkak adalah mimpi buruk bagi developer. Oleh karena itu, JhopanStoreVPN menggunakan **Arsitektur Binary Ramping**:

1. **Anti-Bloatware Repo**: File .aar dan .so berukuran raksasa tidak diunggah ke dalam repositori utama.
2. **On-Demand Download**:
   - libXray.aar diunduh otomatis secara dinamis dari GitHub Release tag \inary-assets\.
   - libtun2socks.so diambil dari *upstream release* resmi setiap kali proses *build* awal berjalan.
3. **Build Varian Diferensiasi**:
   - **phone**: Cepat dan ringan, dioptimalkan khusus untuk arsitektur modern ARM (v7a & v8a).
   - **ull**: Versi universal yang mencakup x86_64 untuk emulasi dan kompatibilitas perangkat yang lebih luas.

Output *build* lokal akan otomatis dipilah sesuai foldernya:
\\\	ext
app/build/outputs/apk/{varian}/release/app-{varian}-{arsitektur}-release.apk
\\\

---

## 🛠️ Cara Build Sendiri (Local)

Ingin nge-build dan memodifikasi proyek ini sendiri? Sangat mudah! Semuanya sudah diotomatisasi lewat skrip build Gradle.

**💻 Persyaratan Awal:**
- Koneksi internet stabil (sangat penting untuk mengunduh binary saat *build* pertama kali).
- JDK 17 (direkomendasikan).
- Android Studio / Command Line Tools bawaan Android terbaru.

**Langkah-langkah Build:**

1. Clone repositori ini dan masuk ke direktori ndroid/.
2. Buka terminal lalu eksekusi *Gradle Wrapper* berikut:

\\\ash
# Build khusus untuk perangkat HP nyata (rekomendasi)
./gradlew assemblePhoneRelease

# Build varian lengkap untuk Universal/Emulator
./gradlew assembleFullRelease
\\\

> **✨ Keajaiban Terjadi di Balik Layar:** Task preBuild akan secara **otomatis** memicu proses unduhan library jaringan lewat modul downloadLibXrayAar & downloadTun2socks. Anda sama sekali tidak perlu mencari dan meletakkan file secara manual!

---

## ⚙️ Pengaturan URL Lanjutan

Punya server aset *private* sendiri? Anda bisa mengganti secara leluasa dari mana unduhan file *binary* berasal dengan pengaturan *Environment Variables* atau melewati parameter bawaan Gradle:

**Via Terminal (Environment):**
\\\ash
set LIBXRAY_AAR_URL=https://github.com/<owner>/<repo>/.../libXray.aar
set TUN2SOCKS_ARM64_URL=https://github.com/<owner>/<repo>/.../libtun2socks-arm64-v8a.so
set TUN2SOCKS_ARMV7_URL=https://github.com/<owner>/<repo>/.../libtun2socks-armeabi-v7a.so

./gradlew assemblePhoneRelease
\\\

**Via Baris Perintah Gradle Arguments:**
\\\ash
./gradlew assemblePhoneRelease \
  -PlibXrayAarUrl=https://... \
  -Ptun2socks.arm64-v8a.url=https://... \
  -Ptun2socks.armeabi-v7a.url=https://...
\\\

---

## 🔄 CI/CD & Pipeline Release

Proyek ini telah dibekali dengan alur kerja *GitHub Actions* (.github/workflows/build.yaml) untuk integrasi otomatis tanpa campur tangan teknis tinggi.

**🛠 Alur Kerja CI (Continuous Integration):**
1. Setiap proses *push* (*commit*) akan menjalankan linting, validasi, serta proses *build multi-ABI*.
2. Secara otomatis menghasilkan *Artifact* instalasi dengan label elegan: JhopanStoreVPN-*.apk.

**📦 Otomasi Rilis Publik:**
1. Anda cukup menekan tombol "*Draft a new release*" dan mencetak Tag berinisial huruf  (sebagai contoh: 1.0.0).
2. Bot _GitHub Workflow_ akan mengambil alih kemudian merilis file APK siap pakai di tab Releases sekaligus mengunggah kembali JhopanStoreVPN-libXray.aar.

**♻️ Teknik Mutakhir: Pembaruan Satu Kali Upload**
Jika sewaktu-waktu ada rilis versi dari modul *core* (seperti update mesin protokol Xray / tun2socks), Anda **hanya perlu menimpa (replace)** versi rilis yang tersemat pada *tag* inary-assets. 
Kemudian, seluruh skema *Build* di lokal Anda dan tahapan pipeline (CI/CD) akan **otomatis menarik berkas dengan versi terbaru tadi**! Ini memangkas habis keharusan repot-repot *commit* file raksasa ke riwayat git log proyek utama!

---
<div align="center">
  Dibuat dengan ❤️ untuk memastikan kebebasan dan ketangguhan berselancar internet.
</div>
