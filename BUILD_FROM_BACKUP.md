# 📦 Build JhoVPN dari Backup Repository

Panduan lengkap untuk melakukan build/restore aplikasi JhoVPN dari backup repository.

---

## 🔗 Repository Backup

**URL**: `https://github.com/jhopan/JhopanStoreVPN_XrayCore_Backup.git`

---

## 📋 Persyaratan Sebelum Build

### Hardware & OS
- **Windows 10/11** atau **macOS** atau **Linux**
- **Minimal 8GB RAM** (rekomendasi 16GB untuk smooth build)
- **50GB+ free disk space** (untuk SDK, build cache, binary downloads)
- **Koneksi internet stabil** (sangat penting untuk download binary)

### Software Requirements

#### 1. **Java Development Kit (JDK) 17**
```powershell
# Check JDK version
java -version

# Download dari: https://www.oracle.com/java/technologies/downloads/
# Atau gunakan OpenJDK:
# https://adoptium.net/ (Eclipse Temurin JDK 17)
```

#### 2. **Android SDK & Build Tools**
```powershell
# Option A: Install Android Studio
# Download: https://developer.android.com/studio
# → Includes SDK, Build Tools, Gradle, Emulator

# Option B: Command-line tools only
# Download: https://developer.android.com/studio
# Setup: %LOCALAPPDATA%\Android\Sdk
```

**Minimal SDK components**:
- Android SDK Platform 34 (or latest)
- Android SDK Build-Tools 34.0.0+
- Android Emulator (optional)
- Android Platform Tools

#### 3. **Git**
```powershell
# Check Git installation
git --version

# Download: https://git-scm.com/download/win
```

#### 4. **Gradle** (Optional - bundled in project)
```powershell
# Project include Gradle Wrapper (./gradlew)
# No manual installation required
```

---

## 🚀 Step-by-Step Build Process

### **Step 1: Clone dari Backup Repository**

```powershell
# Navigate ke directory tempat developer projects disimpan
cd C:\Users\ACER\Documents\Project\Aplikasi

# Clone backup repository
git clone https://github.com/jhopan/JhopanStoreVPN_XrayCore_Backup.git JhoVPN_Restore

# Enter project directory
cd JhoVPN_Restore\android
```

**Output yang diharapkan:**
```
Cloning into 'JhoVPN_Restore'...
remote: Enumerating objects: 112, done.
...
Resolving deltas: 100% done.
```

---

### **Step 2: Setup Environment Variables**

Beberapa path perlu dikonfigurasi:

```powershell
# For Windows (PowerShell)
# Set Java Home
$env:JAVA_HOME = "C:\Program Files\Java\jdk-17"  # Adjust path sesuai instalasi

# Set Android SDK Home
$env:ANDROID_SDK_ROOT = "$env:LOCALAPPDATA\Android\Sdk"

# Verify
echo $env:JAVA_HOME
echo $env:ANDROID_SDK_ROOT
```

**Atau edit `local.properties`** (di root `android/` folder):

```properties
# local.properties
sdk.dir=C:\\Users\\ACER\\AppData\\Local\\Android\\Sdk
java.home=C:\\Program Files\\Java\\jdk-17
```

---

### **Step 3: Sync Gradle & Dependencies**

```powershell
# Navigate to android folder
cd C:\Users\ACER\Documents\Project\Aplikasi\JhoVPN_Restore\android

# Sync Gradle (opsional, build akan trigger ini)
.\gradlew clean

# Output:
# Welcome to Gradle 8.x.x
# ...
# BUILD SUCCESSFUL
```

**⏱️ Waktu**: 2-5 menit (first time slower)

---

### **Step 4: Download Binary Assets**

Saat build pertama, project akan otomatis download binary (libXray.aar, tun2socks).

**Tidak perlu download manual**, tapi Anda bisa override URL:

```powershell
# Build dengan custom binary URL (optional)
.\gradlew assemblePhoneRelease `
  -PlibXrayAarUrl="https://github.com/jhopan/JhopanStoreVPN_Xray_APK/releases/download/v1.0.0/libXray.aar"
```

---

### **Step 5: Build APK**

#### **Option A: Build untuk Device/Phone (RECOMMENDED)**

```powershell
# Generate Phone Release APK
.\gradlew assemblePhoneRelease

# Output APK akan berada di:
# app/build/outputs/apk/phone/release/app-phone-release.apk
```

**⏱️ Waktu**: 10-20 menit (first build slower karena download binary)

**Output yang diharapkan:**
```
> Task :app:assemblePhoneRelease
...
BUILD SUCCESSFUL in 15m 45s
```

---

#### **Option B: Build Universal (Phone + x86_64 emulator)**

```powershell
# Generate Full Release APK (untuk compatibility lebih luas)
.\gradlew assembleFullRelease

# Output APK akan berada di:
# app/build/outputs/apk/full/release/app-full-release.apk
```

**⏱️ Waktu**: 15-25 menit (lebih besar file binary)

---

#### **Option C: Build Debug (untuk Development)**

```powershell
# Generate Debug APK (faster, lebih kecil, untuk testing saja)
.\gradlew assemblePhoneDebug

# Output APK akan berada di:
# app/build/outputs/apk/phone/debug/app-phone-debug.apk
```

**⏱️ Waktu**: 5-10 menit

---

### **Step 6: Locate & Verify APK**

```powershell
# List semua output APK
Get-ChildItem C:\Users\ACER\Documents\Project\Aplikasi\JhoVPN_Restore\android\app\build\outputs\apk\phone\release\

# Expected output:
# app-phone-release.apk (±40MB)
```

---

### **Step 7: Install ke Device/Emulator**

#### **Via ADB (Android Debug Bridge)**

```powershell
# Pastikan device terhubung via USB atau emulator running
adb devices

# Install APK
adb install -r app\build\outputs\apk\phone\release\app-phone-release.apk

# Output:
# Success
```

#### **Manual Install**
- Copy APK ke device via file manager
- Tap APK → Install

---

## 🐛 Troubleshooting

### **Error: "ANDROID_SDK not found"**
```powershell
# Pastikan Android SDK sudah installed
# Set environment variable:
$env:ANDROID_SDK_ROOT = "C:\Users\ACER\AppData\Local\Android\Sdk"

# Verify:
ls $env:ANDROID_SDK_ROOT
```

### **Error: "Java not found"**
```powershell
# Install JDK 17
# Verify:
java -version

# Set JAVA_HOME:
$env:JAVA_HOME = "C:\Program Files\Java\jdk-17"
```

### **Error: "Gradle sync failed"**
```powershell
# Clear Gradle cache
.\gradlew clean

# Retry sync
.\gradlew build
```

### **Error: "Failed to download libXray.aar"**
```powershell
# Check internet connection
# Jika perlu, set custom URL (lihat Step 4)
# Atau download manual dari: https://github.com/jhopan/JhopanStoreVPN_Xray_APK/releases
```

### **Error: "Native build failed"**
```powershell
# NDK mungkin tidak tercopy dengan benar
# Solution:
.\gradlew clean
.\gradlew assemblePhoneRelease --stacktrace

# Jika masih gagal, configure NDK manually di local.properties:
# ndk.dir=C:\\Users\\ACER\\AppData\\Local\\Android\\Sdk\\ndk\\25.1.8937393
```

### **Build sangat lambat?**
```powershell
# Enable Gradle parallel build
.\gradlew assemblePhoneRelease --parallel --max-workers=4

# Atau edit gradle.properties:
# org.gradle.parallel=true
# org.gradle.workers.max=4
```

---

## 🔄 Build Commands Reference

| Command | Deskripsi | Waktu |
|---------|-----------|-------|
| `.\gradlew clean` | Clean build cache | 30s |
| `.\gradlew assemblePhoneDebug` | Build debug APK (phone) | 5-10m |
| `.\gradlew assemblePhoneRelease` | Build release APK (phone) | 10-20m |
| `.\gradlew assembleFullRelease` | Build release APK (universal) | 15-25m |
| `.\gradlew build` | Full build dengan tests | 20-30m |
| `.\gradlew bundlePhoneRelease` | Build Android App Bundle | 15-20m |

---

## 📁 Output Files

Setelah build selesai, output akan tersedia di:

```
JhoVPN_Restore/android/app/build/outputs/
├── apk/
│   ├── phone/
│   │   ├── debug/
│   │   │   └── app-phone-debug.apk (~35MB)
│   │   └── release/
│   │       └── app-phone-release.apk (~40MB)
│   └── full/
│       └── release/
│           └── app-full-release.apk (~50MB)
├── bundle/
│   └── phoneRelease/
│       └── app-phone-release.aab (~30MB)
└── logs/
    └── manifest-merger-report.txt
```

---

## 🚀 Quick Build Cheatsheet

```powershell
# 1. Clone & navigate
git clone https://github.com/jhopan/JhopanStoreVPN_XrayCore_Backup.git
cd JhoVPN_Restore/android

# 2. Setup env (if needed)
$env:JAVA_HOME = "C:\Program Files\Java\jdk-17"
$env:ANDROID_SDK_ROOT = "$env:LOCALAPPDATA\Android\Sdk"

# 3. Build
.\gradlew clean
.\gradlew assemblePhoneRelease

# 4. Install
adb install -r app\build\outputs\apk\phone\release\app-phone-release.apk
```

---

## ⚡ Performance Tips

1. **Use SSD** untuk faster I/O
2. **Enable Gradle Parallel Build** (`gradle.properties`)
3. **Increase Gradle Heap** (`gradle.properties`):
   ```properties
   org.gradle.jvmargs=-Xmx4096m
   ```
4. **Cache Dependencies** (sekali download, reuse afterward)
5. **Use Release builds** untuk production (Debug lebih cepat untuk testing)

---

## 📝 Dokumentasi Lengkap

Untuk detail lebih lengkap, lihat:
- `README.md` - Fitur utama & overview
- `app/build.gradle.kts` - Build configuration
- `.github/workflows/build.yaml` - CI/CD pipeline

---

## 🆘 Support & Issues

Jika mengalami masalah:

1. **Check GitHub Issues**: https://github.com/jhopan/JhopanStoreVPN_XrayCore_Backup/issues
2. **Check Logs**: `build/outputs/logs/`
3. **Enable Stacktrace**: `.\gradlew assemblePhoneRelease --stacktrace`
4. **Clean rebuild**: `.\gradlew clean && .\gradlew assemblePhoneRelease`

---

**Last Updated**: March 28, 2026  
**Backup Repository**: https://github.com/jhopan/JhopanStoreVPN_XrayCore_Backup.git
