# S540d.github.io

GitHub Pages Root-Repository für Android App Links Verifizierung und das
zentrale Impressum aller Projekte.

## Zweck

Hostet `.well-known/assetlinks.json` für alle Android Apps unter `s540d.github.io` (App-Links öffnen statt des Browsers direkt die App) und die zentrale `impressum.html`, auf die der „Über"-Abschnitt jedes Web-Projekts verlinkt (siehe [about-section.md](https://github.com/S540d/project-templates/blob/main/dev-standards/about-section.md)).

## Live URLs

- **Impressum:** https://s540d.github.io/impressum.html
- **Android App Links:** https://s540d.github.io/.well-known/assetlinks.json

## Verifizierte Apps

- **1x1 Trainer** (`com.sven4321.trainer1x1`) — [Play Store](https://play.google.com/store/apps/details?id=com.sven4321.trainer1x1)
- **Energy Price Germany** (`com.sven4321.energypricegermany`) — [Play Store](https://play.google.com/store/apps/details?id=com.sven4321.energypricegermany)

Details (SHA-256 Fingerprints, App-URLs) siehe `.well-known/assetlinks.json`.

## Struktur

```
impressum.html               # Zentrales Impressum, verlinkt von jedem Projekt
.well-known/assetlinks.json  # Android App Links Verifizierung
.nojekyll                    # GitHub Pages: dotfiles nicht ignorieren
```

## Pflege

`impressum.html` (Pflichtangaben nach §5 TMG, reines HTML) oder `.well-known/assetlinks.json` bearbeiten, committen, pushen — GitHub Pages deployed automatisch.
Fingerprint aktualisieren oder neue App hinzufügen: Format `AB:CD:EF:...` mit Doppelpunkten.

Siehe auch: [Digital Asset Links Doku](https://developer.android.com/training/app-links/verify-android-applinks)
