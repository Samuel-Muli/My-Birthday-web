# Birthday App → Android APK
## Two options — pick the one that suits you

---

## OPTION A · Progressive Web App (easiest, no APK needed)

Host the files anywhere and add a home-screen shortcut on Android.
Android Chrome will install it as a full-screen app icon.

**Steps**
1. Upload all files to any free host (Netlify, GitHub Pages, etc.)
2. Open the URL in Chrome on your Android phone
3. Tap the three-dot menu → **Add to Home screen**
4. It opens full-screen, no browser chrome, exactly like an app

**That's it.** No Android Studio, no build tools.

---

## OPTION B · Native APK with Capacitor (proper installable APK)

### Prerequisites
- Node.js 18+  (https://nodejs.org)
- Android Studio (https://developer.android.com/studio)
- Java 17+ (bundled with Android Studio)

---

### 1 · File structure

Arrange your files like this before starting:

```
birthday-app/
├── index.html                        ← the web app
├── aa.jpg                            ← slideshow images
├── bb.jpg
├── cc.jpg
├── Rayvanny_Happy Birthday.mp4       ← video
└── Rayvanny_Happy Birthday.mp3       ← music
```

---

### 2 · Create the Capacitor project

```bash
# Create project folder
mkdir birthday-android && cd birthday-android

# Init Node project
npm init -y

# Install Capacitor
npm install @capacitor/core @capacitor/cli @capacitor/android

# Init Capacitor (answer the prompts)
npx cap init "Birthday Surprise" "com.samuel.birthday" --web-dir www
```

---

### 3 · Copy your web files into www/

```bash
mkdir www

# Copy everything into www/
cp /path/to/index.html                            www/
cp /path/to/aa.jpg                                www/
cp /path/to/bb.jpg                                www/
cp /path/to/cc.jpg                                www/
cp /path/to/"Rayvanny_Happy Birthday.mp4"         www/
cp /path/to/"Rayvanny_Happy Birthday.mp3"         www/
```

---

### 4 · Add Android platform

```bash
npx cap add android
npx cap sync
```

---

### 5 · Configure the app (optional but recommended)

Edit `capacitor.config.json`:

```json
{
  "appId": "com.samuel.birthday",
  "appName": "Birthday Surprise",
  "webDir": "www",
  "server": {
    "androidScheme": "https"
  },
  "android": {
    "allowMixedContent": true,
    "backgroundColor": "#000000"
  }
}
```

Edit `android/app/src/main/res/values/strings.xml`:
```xml
<string name="app_name">Birthday Surprise</string>
```

---

### 6 · Open in Android Studio and build

```bash
npx cap open android
```

Android Studio opens. Then:

1. **Build → Build Bundle(s) / APK(s) → Build APK(s)**
2. APK will be at:
   `android/app/build/outputs/apk/debug/app-debug.apk`

---

### 7 · Install on phone

**Option 1 — USB**
```bash
# Enable USB Debugging on your phone first
adb install android/app/build/outputs/apk/debug/app-debug.apk
```

**Option 2 — File transfer**
Copy the APK to your phone and open it.
(You may need to allow "Install from unknown sources" in Settings → Security)

---

## OPTION C · Apache Cordova (alternative to Capacitor)

```bash
npm install -g cordova

cordova create birthday-app com.samuel.birthday "Birthday Surprise"
cd birthday-app

# Copy web files into www/
cp /path/to/index.html www/
cp /path/to/*.jpg      www/
cp /path/to/*.mp4      www/
cp /path/to/*.mp3      www/

cordova platform add android
cordova build android

# APK at:
# platforms/android/app/build/outputs/apk/debug/app-debug.apk
```

---

## Tips

**Large media files**
If your video/music files are large, consider hosting them online and
referencing them by URL in index.html instead of bundling them.

**Screen orientation**
To lock to portrait, add to `android/app/src/main/AndroidManifest.xml`:
```xml
android:screenOrientation="portrait"
```

**Splash screen**
```bash
npm install @capacitor/splash-screen
npx cap sync
```

**App icon**
Replace `android/app/src/main/res/mipmap-*/ic_launcher.png`
with your own icons (use Android Studio's Image Asset tool).

**Hide status bar (full immersive)**
In `MainActivity.java`, add:
```java
getWindow().setFlags(
  WindowManager.LayoutParams.FLAG_FULLSCREEN,
  WindowManager.LayoutParams.FLAG_FULLSCREEN
);
```
