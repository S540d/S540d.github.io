# S540d.github.io

GitHub Pages Root-Repository für Android App Links Verifizierung.

## Zweck

Hostet `.well-known/assetlinks.json` für alle Android Apps unter `s540d.github.io`, damit App-Links statt des Browsers direkt die App öffnen.

## Live URL

**https://s540d.github.io/.well-known/assetlinks.json**

## Verifizierte Apps

- **1x1 Trainer** (`com.sven4321.trainer1x1`) — [Play Store](https://play.google.com/store/apps/details?id=com.sven4321.trainer1x1)
- **Energy Price Germany** (`com.sven4321.energypricegermany`) — [Play Store](https://play.google.com/store/apps/details?id=com.sven4321.energypricegermany)

Details (SHA-256 Fingerprints, App-URLs) siehe `.well-known/assetlinks.json`.

## Struktur

```
.well-known/assetlinks.json  # Android App Links Verifizierung
.nojekyll                    # GitHub Pages: dotfiles nicht ignorieren
```

## Pflege

Fingerprint aktualisieren oder neue App hinzufügen: `.well-known/assetlinks.json` bearbeiten, committen, pushen (GitHub Pages deployed automatisch). Format der Fingerprints: `AB:CD:EF:...` mit Doppelpunkten.

Siehe auch: [Digital Asset Links Doku](https://developer.android.com/training/app-links/verify-android-applinks)
