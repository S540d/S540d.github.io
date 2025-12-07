# S540d.github.io

GitHub Pages Root-Repository für Android App Links Verifizierung.

## Zweck

Dieses Repository hostet die `.well-known/assetlinks.json` Datei für alle Android Apps unter der Domain `s540d.github.io`.

Android App Links ermöglichen es, dass beim Klicken auf einen Website-Link automatisch die App geöffnet wird (falls installiert), statt des Browsers.

## Live URL

**https://s540d.github.io/.well-known/assetlinks.json**

## Verifizierte Apps

### 1. 1x1 Trainer ✓
- **Package:** `com.sven4321.trainer1x1`
- **App URL:** https://s540d.github.io/1x1_Trainer
- **Play Store:** https://play.google.com/store/apps/details?id=com.sven4321.trainer1x1
- **SHA-256:** `C9:B7:5C:A8:F4:23:48:5D:D6:E3:87:EB:9A:13:5B:4F:B8:24:A4:AE:E5:56:9C:58:56:E6:E6:AE:73:C4:BB:78`
- **Status:** Verifiziert

### 2. Energy Price Germany ✓
- **Package:** `com.sven4321.energypricegermany`
- **App URL:** https://s540d.github.io/Energy_Price_Germany
- **Play Store:** https://play.google.com/store/apps/details?id=com.sven4321.energypricegermany
- **SHA-256:** `CE:E0:C0:38:E3:E7:74:17:2E:33:7A:D3:36:3E:F2:16:E3:1B:C1:0E:94:B2:C5:96:E9:A7:BD:1C:CB:64:DD:EF`
- **Status:** Verifiziert

## Struktur

```
.well-known/
  assetlinks.json  # Android App Links Verifizierung
.nojekyll          # GitHub Pages: dotfiles nicht ignorieren
README.md          # Diese Datei
```

## SHA-256 Fingerprint aktualisieren

Falls der SHA-256 Fingerprint für eine App aktualisiert werden muss:

1. Hole den neuen Fingerprint aus der **Play Console → App-Integrität → App signing key certificate**
2. Bearbeite `.well-known/assetlinks.json`
3. Aktualisiere den entsprechenden `sha256_cert_fingerprints` Eintrag
4. **Wichtig:** Format MIT Doppelpunkten verwenden (z.B. `AB:CD:EF:12:...`)
5. Commit und Push (GitHub Pages deployed automatisch innerhalb von ~1 Minute)

## Neue App hinzufügen

So fügst du eine neue App zur Verifizierung hinzu:

1. Bearbeite `.well-known/assetlinks.json`
2. Füge einen neuen Eintrag im JSON-Array hinzu:
```json
{
  "relation": [
    "delegate_permission/common.handle_all_urls",
    "delegate_permission/common.get_login_creds"
  ],
  "target": {
    "namespace": "android_app",
    "package_name": "com.yourcompany.yourapp",
    "sha256_cert_fingerprints": [
      "SHA256_FROM_PLAY_CONSOLE"
    ]
  }
}
```
3. Commit und Push

## Verifizierung testen

```bash
# assetlinks.json abrufen
curl https://s540d.github.io/.well-known/assetlinks.json

# Deep Link für 1x1 Trainer testen
adb shell am start -W -a android.intent.action.VIEW \
  -d "https://s540d.github.io/1x1_Trainer" \
  com.sven4321.trainer1x1

# Deep Link für Energy Price Germany testen
adb shell am start -W -a android.intent.action.VIEW \
  -d "https://s540d.github.io/Energy_Price_Germany" \
  com.sven4321.energypricegermany
```

## Weitere Infos

- [Android App Links Guide](https://developer.android.com/training/app-links)
- [Digital Asset Links](https://developer.android.com/training/app-links/verify-android-applinks)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)
