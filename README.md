# GlicoLog — Progetto Capacitor

App nativa Android/iOS generata da GlicoLog PWA tramite Capacitor 6.

---

## Requisiti

- **Node.js** >= 18
- **Android Studio** (per Android) con SDK 34+
- **Xcode 15+** (per iOS, solo macOS)
- **Java 17** (per Android build)

---

## Setup iniziale (una volta sola)

```bash
# 1. Installa le dipendenze
npm install

# 2. Aggiungi la piattaforma Android
npx cap add android

# 3. Aggiungi la piattaforma iOS (solo macOS)
npx cap add ios

# 4. Copia i file web nella cartella nativa
npx cap sync

# 5. (Opzionale) Copia le personalizzazioni Android
#    Vedi sezione "Personalizzazioni Android" sotto
```

---

## Comandi quotidiani

```bash
# Dopo ogni modifica a www/index.html:
npx cap sync

# Avvia su Android (con dispositivo/emulatore connesso)
npx cap run android

# Apri il progetto in Android Studio
npx cap open android

# Apri il progetto in Xcode (solo macOS)
npx cap open ios

# Testa nel browser
npx serve www
```

---

## Personalizzazioni Android

Dopo `npx cap add android`, copia i file dalla cartella `android-config/`:

```bash
# Colori e tema
cp android-config/app/src/main/res/values/colors.xml   android/app/src/main/res/values/colors.xml
cp android-config/app/src/main/res/values/styles.xml   android/app/src/main/res/values/styles.xml

# Splash screen scuro
cp android-config/app/src/main/res/drawable/splash.xml android/app/src/main/res/drawable/splash.xml

# Icona adattiva (Android 8+)
cp android-config/app/src/main/res/drawable/ic_launcher_foreground.xml android/app/src/main/res/drawable/
cp -r android-config/app/src/main/res/mipmap-anydpi-v26 android/app/src/main/res/

# Network security config (necessario per chiamate API nutrizionali)
cp android-config/app/src/main/res/xml/network_security_config.xml android/app/src/main/res/xml/

# Aggiungi al AndroidManifest.xml dentro <application>:
#   android:networkSecurityConfig="@xml/network_security_config"
```

---

## Icone

Capacitor genera le icone automaticamente da un file sorgente 1024×1024.

Installa il plugin e usa `icon-512.png` (rinominalo in `icon-1024.png` e scalalo):

```bash
npm install @capacitor/assets --save-dev
npx capacitor-assets generate --assetPath ./www/icon-512.png
```

Oppure usa **Android Asset Studio** online: https://romannurik.github.io/AndroidAssetStudio/

---

## Build APK (release)

```bash
# Apri Android Studio
npx cap open android

# In Android Studio:
# Build → Generate Signed Bundle / APK
# Scegli APK → crea un keystore → build
```

---

## Struttura del progetto

```
glicolog-capacitor/
├── www/                    ← App web (index.html + assets)
│   ├── index.html          ← App GlicoLog completa
│   ├── sw.js               ← Service Worker (browser only)
│   ├── manifest.json       ← PWA manifest
│   └── icon-*.png          ← Icone
├── android/                ← Progetto Android (generato da cap add android)
├── ios/                    ← Progetto iOS (generato da cap add ios)
├── android-config/         ← Personalizzazioni Android da copiare
├── capacitor.config.json   ← Configurazione Capacitor
└── package.json
```

---

## Note tecniche

- I dati sono salvati in **localStorage** (WebView nativa, persistente)
- Le chiamate API nutrizionali (USDA, OpenFoodFacts) richiedono internet
- Il Service Worker è disattivato nella shell nativa (non necessario)
- AppID: `it.glicolog.app` — modifica in `capacitor.config.json` se pubblichi su store

---

## Pubblicazione su Google Play

1. Crea un account Google Play Developer (25$ una tantum)
2. Genera APK firmato da Android Studio
3. Carica su Play Console → App interno → Test → Produzione
4. Compila i metadati (descrizione, screenshot, privacy policy)

---

## Pubblicazione su App Store (iOS)

1. Account Apple Developer (99$/anno)
2. Genera certificati e provisioning profile in Xcode
3. Archive → Distribute App → App Store Connect
