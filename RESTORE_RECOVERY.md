# 🔄 Recovery & Restore Guide - JhoVPN Backup

Panduan untuk restore/recovery aplikasi JhoVPN dari backup repository jika repository utama tidak tersedia.

---

## 📌 Backup Repository Info

| Item | Detail |
|------|--------|
| **Backup URL** | https://github.com/jhopan/JhopanStoreVPN_XrayCore_Backup.git |
| **Original URL** | https://github.com/jhopan/JhopanStoreVPN_Xray_APK.git |
| **Backup Date** | March 28, 2026 |
| **Stored Size** | ~91MB (all branches + tags) |
| **Commit Hash** | ab86a97 (latest restore point) |

---

## 🚨 Skenario Recovery

### **Scenario 1: Original Repository Down (GitHub unavailable)**

```powershell
# Clone dari backup repository saja
git clone https://github.com/jhopan/JhopanStoreVPN_XrayCore_Backup.git JhoVPN_Emergency

cd JhoVPN_Emergency/android

# Proceed dengan build normal
.\gradlew assemblePhoneRelease
```

---

### **Scenario 2: Restore Specific Commit**

Jika Anda perlu restore ke commit spesifik:

```powershell
# Clone backup
git clone https://github.com/jhopan/JhopanStoreVPN_XrayCore_Backup.git

cd JhoVPN_XrayCore_Backup/android

# List semua commits
git log --oneline

# Checkout ke specific commit
git checkout ab86a97  # Latest commit

# Atau gunakan commit sebelumnya
git checkout 515e3b8  # Older commit
```

---

### **Scenario 3: Restore Specific Tag/Version**

```powershell
# Clone backup
git clone https://github.com/jhopan/JhopanStoreVPN_XrayCore_Backup.git

cd JhoVPN_XrayCore_Backup/android

# List available tags
git tag

# Checkout ke specific tag/version
git checkout v1.0.0  # Version 1.0.0 tag

# Build dari versioned state
.\gradlew assemblePhoneRelease
```

---

### **Scenario 4: Restore Specific Branch**

```powershell
# Clone all branches
git clone --mirror https://github.com/jhopan/JhopanStoreVPN_XrayCore_Backup.git

cd JhoVPN_XrayCore_Backup.git

# List branches
git branch -a

# Checkout branch sparingly
git checkout main  # or any other branch
```

---

## 🔍 Verify Backup Integrity

Sebelum memulai recovery, verifikasi backup valid:

```powershell
# Test clone (tanpa full checkout - lebih cepat)
git clone --bare --quiet https://github.com/jhopan/JhopanStoreVPN_XrayCore_Backup.git test.git

# Jika tidak error, backup valid
echo "✅ Backup repository valid"

# Cleanup test
rd -r test.git
```

---

## 📥 Recovery Procedure (Step-by-Step)

### **Full Recovery Process**

```powershell
# Step 1: Clone dari backup
Write-Host "[1/5] Cloning backup repository..."
git clone https://github.com/jhopan/JhopanStoreVPN_XrayCore_Backup.git JhoVPN_Restored

# Step 2: Navigate to android folder
Write-Host "[2/5] Navigating to project..."
cd JhoVPN_Restored/android

# Step 3: Setup environment
Write-Host "[3/5] Setting up environment..."
$env:JAVA_HOME = "C:\Program Files\Java\jdk-17"
$env:ANDROID_SDK_ROOT = "$env:LOCALAPPDATA\Android\Sdk"

# Step 4: Clean & build
Write-Host "[4/5] Building application..."
.\gradlew clean
.\gradlew assemblePhoneRelease

# Step 5: Verify output
Write-Host "[5/5] Verifying build..."
$apkPath = "app\build\outputs\apk\phone\release\app-phone-release.apk"
if (Test-Path $apkPath) {
    Write-Host "✅ Build successful! APK: $apkPath" -ForegroundColor Green
    $fileSize = (Get-Item $apkPath).length / 1MB
    Write-Host "   File size: $([math]::Round($fileSize, 2)) MB"
} else {
    Write-Host "❌ Build failed! APK not found." -ForegroundColor Red
}
```

---

## 🔐 Offline Recovery (No Internet)

Jika Anda memiliki local copy dari backup:

```powershell
# Jika Anda sudah punya folder backup lokal
cd C:\Backup\JhoVPN_XrayCore_Backup\android

# Langsung build tanpa clone
.\gradlew assemblePhoneRelease

# Atau jika Anda punya compressed backup
# 1. Extract backup
Expand-Archive -Path C:\Backup\JhoVPN_XrayCore_Backup.zip -DestinationPath C:\Restore

# 2. Navigate & build
cd C:\Restore\android
.\gradlew assemblePhoneRelease
```

---

## 📊 Backup Contents Inventory

Apa yang tersimpan di backup:

```
JhoVPN_XrayCore_Backup/
├── 📄 README.md
├── 📄 BUILD_FROM_BACKUP.md (NEW)
├── 📄 RESTORE_RECOVERY.md (THIS FILE)
├── 🔧 settings.gradle.kts
├── 🔧 build.gradle.kts
├── 📦 gradle/
│   └── wrapper/
├── 📱 app/
│   ├── 🔧 build.gradle.kts
│   ├── 📦 libs/
│   │   └── libXray.aar (84.10 MB)
│   ├── 📂 src/
│   │   └── main/
│   │       ├── AndroidManifest.xml
│   │       ├── java/ (Kotlin source)
│   │       ├── cpp/ (C++ bindings)
│   │       ├── jniLibs/ (JNI libraries)
│   │       └── res/ (Resources)
│   └── 🔨 build/ (Generated - not stored)
├── 📚 BuildScript history (git log)
└── 🏷️ Tags: v1.0.0
```

---

## 🚀 Quick Recovery Commands

Salin-pasta untuk quick recovery:

```powershell
# === QUICK RECOVERY ===

# 1. Clone
git clone https://github.com/jhopan/JhopanStoreVPN_XrayCore_Backup.git JhoVPN_Recovery
cd JhoVPN_Recovery/android

# 2. Env setup
$env:JAVA_HOME = "C:\Program Files\Java\jdk-17"
$env:ANDROID_SDK_ROOT = "$env:LOCALAPPDATA\Android\Sdk"

# 3. Build
.\gradlew clean
.\gradlew assemblePhoneRelease

# 4. Verify
if (Test-Path "app\build\outputs\apk\phone\release\app-phone-release.apk") {
    Write-Host "✅ SUCCESS!" -ForegroundColor Green
} else {
    Write-Host "❌ FAILED!" -ForegroundColor Red
}
```

---

## 📋 Pre-Recovery Checklist

Sebelum restore, pastikan:

- [ ] **JDK 17** installed (`java -version`)
- [ ] **Android SDK** installed (di `%LOCALAPPDATA%\Android\Sdk`)
- [ ] **Git** installed (`git --version`)
- [ ] **Internet connection** stable (untuk download binary assets)
- [ ] **50GB+ free disk space**
- [ ] **Time**: 20-30 minutes untuk full recovery

---

## ⚠️ Known Issues & Solutions

### **Issue: "Large file detected - Git LFS recommended"**
```powershell
# This is a warning, NOT an error
# libXray.aar (84.10 MB) exceeds GitHub's 50MB recommendation
# Solution: Backup tetap valid, just a warning

# Jika Anda ingin use Git LFS:
git lfs install
git lfs track "*.aar"
git add .gitattributes app/libs/libXray.aar
git commit -m "chore: migrate to Git LFS"
```

---

### **Issue: "Clone timeout"**
```powershell
# Jika clone lambat/timeout:

# 1. Increase git timeout
git config --global http.postBuffer 524288000
git config --global http.lowSpeedLimit 0
git config --global http.lowSpeedTime 999999

# 2. Retry clone dengan shallow copy (lebih cepat)
git clone --depth=1 https://github.com/jhopan/JhopanStoreVPN_XrayCore_Backup.git

# 3. Jika masih timeout, download ZIP:
# GitHub → "Code" → "Download ZIP"
Expand-Archive -Path JhoVPN_XrayCore_Backup-main.zip -DestinationPath .
```

---

### **Issue: "Build cache corrupted"**
```powershell
# Nuclear option - clean everything
.\gradlew clean --no-build-cache
rm -Recurse app\build
rm -Recurse .gradle

# Retry build
.\gradlew assemblePhoneRelease
```

---

## 📞 Recovery Support Reference

Jika recovery gagal, check:

1. **Error logs**: `app/build/outputs/logs/`
2. **Gradle output**: `.\gradlew assemblePhoneRelease --stacktrace`
3. **GitHub Issues**: https://github.com/jhopan/JhopanStoreVPN_XrayCore_Backup/issues
4. **This documentation**: `BUILD_FROM_BACKUP.md` dan `RESTORE_RECOVERY.md`

---

## 🎯 Recovery Success Criteria

Recovery dianggap **berhasil** jika:

✅ Clone completed tanpa error  
✅ `.\gradlew clean` completed  
✅ `.\gradlew assemblePhoneRelease` completed (BUILD SUCCESSFUL)  
✅ APK generated: `app/build/outputs/apk/phone/release/app-phone-release.apk` (±40MB)  
✅ APK dapat di-install ke device/emulator  
✅ App dapat launch & connect to VPN  

---

## 📝 Backup Schedule Recommendations

Untuk future backups, recommend:

| Frequency | When |
|-----------|------|
| **Daily** | Setiap post-commit (automated via CI/CD) |
| **Weekly** | Every Monday (redundant backup) |
| **Monthly** | Release tagged (v1.x.x) |
| **Yearly** | Archive oldest + stability-tested build |

---

## 🔗 Related Documentation

- [BUILD_FROM_BACKUP.md](BUILD_FROM_BACKUP.md) - Build instructions
- [README.md](README.md) - Project overview
- Backup Repo: https://github.com/jhopan/JhopanStoreVPN_XrayCore_Backup

---

**Last Updated**: March 28, 2026  
**Backup Status**: ✅ Active & Valid  
**Last Backup**: March 28, 2026 11:00 UTC
