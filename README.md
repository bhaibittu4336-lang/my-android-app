# All Games - Android Studio Project (APK & AAB)

This directory contains the **complete, production-ready native Android Studio project** for the **All Games** multi-game app.

It is pre-configured to build:
1. **Debug APK (`app-debug.apk`)**: For instant side-loading, USB installation, and testing on any Android phone/tablet.
2. **Release Android App Bundle (`app-release.aab`)**: The standard distribution format required by the **Google Play Console** for publishing.

---

## 📱 Project Specifications

| Property | Value | Notes |
| :--- | :--- | :--- |
| **App Name** | All Games | Configured in `res/values/strings.xml` |
| **Package / Application ID** | `com.allgames.app` | Configured in `app/build.gradle` |
| **Target SDK** | **API 35 (Android 15)** | Meets Google Play 2025/2026 requirements |
| **Minimum SDK** | **API 24 (Android 7.0)** | Compatible with >96% of active Android devices |
| **Version Code** | `1` | Increment for subsequent Play Store updates |
| **Version Name** | `1.0.0` | User-facing version string |
| **Architecture** | Modern Offline WebView (`WebViewAssetLoader`) | Instant load, 100% offline, zero CORS issues |
| **Hardware Acceleration** | Enabled (`LAYER_TYPE_HARDWARE`) | Smooth 60fps canvas & Web Audio playback |

---

## 🚀 Building the Project

### Method 1: Using Command Line (Terminal)

Ensure you have **Java JDK 17+** and Android SDK installed on your machine.

#### Step 1: Sync Latest Web Assets (if changes were made)
In the root directory of the project, run:
```bash
npm run android:sync
```
*(This builds the Vite app and copies all HTML, CSS, and JS bundle assets into `android/app/src/main/assets/www/`)*

#### Step 2: Build APK for Testing
```bash
cd android
./gradlew assembleDebug
```
*(On Windows Command Prompt or PowerShell, run: `gradlew.bat assembleDebug`)*

- **Generated APK location**:
  `android/app/build/outputs/apk/debug/app-debug.apk`
- **Install on your Android device**:
  ```bash
  adb install -r app/build/outputs/apk/debug/app-debug.apk
  ```
  *(Or copy `app-debug.apk` directly to your phone via USB cable, Google Drive, or messaging apps)*

---

#### Step 3: Build AAB for Google Play Store Publishing
```bash
cd android
./gradlew bundleRelease
```
*(On Windows: `gradlew.bat bundleRelease`)*

- **Generated AAB location**:
  `android/app/build/outputs/bundle/release/app-release.aab`

---

## 🔑 Signing for Google Play Store

When uploading to the Google Play Console, your app must be signed.

### Step 1: Generate a Keystore (Run once)
```bash
keytool -genkey -v -keystore my-release-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias allgames
```
Keep `my-release-key.jks` and your passwords safe.

### Step 2: Configure Signing in `app/build.gradle`
In `android/app/build.gradle`, you can configure your release signing credentials:
```groovy
android {
    signingConfigs {
        release {
            storeFile file("my-release-key.jks")
            storePassword "YOUR_STORE_PASSWORD"
            keyAlias "allgames"
            keyPassword "YOUR_KEY_PASSWORD"
        }
    }
    buildTypes {
        release {
            signingConfig signingConfigs.release
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
}
```
Or you can use **Google Play App Signing** where Google signs the production APKs automatically!

---

## 🛠️ Method 2: Opening in Android Studio

1. Open **Android Studio**.
2. Select **File > Open...** and choose the `android/` directory.
3. Allow Gradle to sync dependencies (it will automatically download AndroidX, Material, and WebKit).
4. **To Test**: Select your connected phone or emulator and click the green **Run (▶)** button.
5. **To Generate Signed Bundle / APK**:
   - Go to **Build > Generate Signed Bundle / APK...**
   - Choose **Android App Bundle (.aab)** for Google Play or **APK** for direct testing.
   - Follow the wizard to choose your key and export!

---

## 🎮 Included Games in this Package
1. **Chess**: Play vs smart AI (Easy/Medium/Hard) or 2-player local pass-and-play.
2. **Ludo**: 2-4 Players with dice rolling, yard exits on 6, token capture, and safe star tiles.
3. **Snake & Ladder**: 1-100 boustrophedon board with animated dice, ladders, and snakes.
4. **Fruit Cut**: Fruit Ninja arcade swipe slice game with combo multipliers and bombs.
5. **Puzzle**: 3x3 and 4x4 sliding tile puzzle with solvable scramble engine.
6. **Match the Card**: 3D flip card memory matching with streaks and multiple themes.
7. **Find the Hidden Object**: Interactive detective search-and-find scene with inventory dock.
8. **Target Hit**: Precision knife throw reflex game with rotating target wheel and apples.
