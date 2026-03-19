# JhopanStoreVPN (Android) 🔒

Android VPN client berbasis VLESS + Xray dengan UI Jetpack Compose, dirancang untuk build ringan, rilis konsisten, dan maintenance yang mudah.

## Tentang Proyek

JhopanStoreVPN adalah aplikasi Android yang menjalankan koneksi VPN berbasis Xray di dalam foreground service.

- Protokol utama: VLESS (core dari `libXray.aar`).
- Minimum Android: API 24+.
- Fokus: stabilitas koneksi, kontrol perilaku jaringan, dan pipeline release yang rapi.

## Fitur Utama 🚀

Aplikasi ini dirancang dengan berbagai fitur tingkat lanjut untuk memastikan stabilitas, keamanan, dan kenyamanan pengguna saat berselancar di internet:

- **⚡ Core Xray Terintegrasi (VLESS)**: Berjalan di atas teknologi jaringan Xray (via `libXray.aar`) untuk menyediakan koneksi protokol VLESS yang sangat sangat responsif dan tangguh terhadap pemblokiran lalu lintas internet.
- **🛣️ Routing Tanpa Root (tun2socks)**: Menggunakan `libtun2socks.so` secara efisien untuk menangkap seluruh lalu lintas aplikasi di sistem Android dan merutekannya ke server proxy secara transparan, tanpa perlu akses *root*.
- **🎨 Antarmuka Modern & Responsif**: Dibangun menggunakan *Jetpack Compose* sepenuhnya, menghadirkan UI yang simpel, interaktif, dan mulus pada berbagai ukuran layar perangkat.
- **🔄 Auto-Reconnect yang Cerdas**: Mencegah pemutusan jaringan yang mengganggu akibat perpindahan sinyal (misalnya dari Wi-Fi ke Seluler) dengan mekanisme koneksi ulang otomatis yang fleksibel:
  - **Max Retry**: Memberikan kontrol bagi pengguna untuk menentukan batasan maksimal percobaan ulang koneksi.
  - **Reconnect Delay**: Menawarkan pengaturan jeda waktu asinkron (dalam detik) sebelum sistem mencoba melakukan rekoneksi ulang, menjaga performa device dan menghindari *spam* tak perlu ke server saat sinyal benar-benar hilang.
- **📡 Real-Time Ping Monitor**: Mengukur kualitas sambungan jaringan (latensi) secara berkala dan menampilkannya langsung di layar pengguna. Ritme pengecekan ini bisa disesuaikan melalui pengaturan **Ping Interval**.
- **🔋 Stabilitas Background Tertinggi**: 
  - **Foreground Service**: Membungkus engine VPN dalam layanan sistem prioritas tinggi lengkap dengan **Notifikasi Status** persisten agar VPN terus menyala dan susah dimatikan (kill) oleh sistem pembersih RAM Android.
  - **Wake Lock (Opsional)**: Fitur tambahan yang sangat berguna agar sinkronisasi koneksi server & CPU Android tetap aktif ("terjaga") walaupun layar HP dimatikan alias stand-by.

## Build Variant dan Output

Project menghasilkan dua jalur build release:

- `phone`: varian utama untuk device Android ARM.
- `full`: varian tambahan untuk target arsitektur yang lebih luas.

Output APK lokal ada di:

- `app/build/outputs/apk/phone/release/app-phone-arm64-v8a-release.apk`
- `app/build/outputs/apk/phone/release/app-phone-armeabi-v7a-release.apk`
- `app/build/outputs/apk/phone/release/app-phone-universal-release.apk`
- `app/build/outputs/apk/full/release/app-full-x86_64-release.apk`
- `app/build/outputs/apk/full/release/app-full-universal-release.apk`

## Arsitektur Binary Dependency 📦

Repository source sengaja dibuat tetap ramping (tidak menyimpan binary besar secara permanen).

- `libXray.aar` diambil dari GitHub Release tag `binary-assets`.
- `libtun2socks.so` default diambil dari upstream tun2socks release.
- URL binary bisa dioverride untuk kebutuhan private release/fork.

Dengan skema ini, source repo tetap ringan tetapi build lokal dan CI tetap berjalan normal.

## CI/CD dan Release

Workflow utama: `.github/workflows/build.yaml`

Alur rilis:

- Build APK multi-ABI untuk release.
- Upload artifacts run dengan format nama `JhopanStoreVPN-*`.
- Saat push tag `v*`, GitHub Release dipublish dengan asset utama:
  - APK bernama `JhopanStoreVPN-...`
  - `JhopanStoreVPN-libXray.aar`

## Update Binary (Sekali Upload) ♻️

Jika ingin update `libXray` atau `tun2socks`:

1. Upload binary baru ke release tag `binary-assets` (replace asset lama).
2. Jalankan build ulang lokal atau CI.
3. Build otomatis menarik binary versi terbaru tanpa commit file besar ke source repo.

## Override URL Binary (Opsional)

Jika ingin pakai URL custom, set environment variable:

```bash
set LIBXRAY_AAR_URL=https://github.com/<owner>/<repo>/releases/download/binary-assets/libXray.aar
set TUN2SOCKS_ARM64_URL=https://github.com/<owner>/<repo>/releases/download/binary-assets/libtun2socks-arm64-v8a.so
set TUN2SOCKS_ARMV7_URL=https://github.com/<owner>/<repo>/releases/download/binary-assets/libtun2socks-armeabi-v7a.so
```

Alternatif via Gradle property:

```bash
./gradlew assemblePhoneRelease -PlibXrayAarUrl=https://...
./gradlew assemblePhoneRelease -Ptun2socks.arm64-v8a.url=https://...
./gradlew assemblePhoneRelease -Ptun2socks.armeabi-v7a.url=https://...
```

## Cara Build Sendiri 🛠️

Jalankan dari folder `android/`:

```bash
./gradlew assemblePhoneRelease
./gradlew assembleFullRelease
```

Catatan:

- `preBuild` otomatis menjalankan `downloadLibXrayAar` dan `downloadTun2socks`.
- Pastikan koneksi internet aktif saat build pertama agar dependency binary dapat diunduh.
