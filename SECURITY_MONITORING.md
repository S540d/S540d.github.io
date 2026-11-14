# Security Monitoring für GitHub Pages

## Übersicht

Dieses Repository enthält einen zentralen Security-Monitoring-Workflow, der alle S540d GitHub Pages Projekte auf sensible Dateien überwacht.

## Was wird überwacht?

Der Workflow scannt alle konfigurierten gh-pages Branches auf:

### Sensible Dateien:
- `node_modules/` - NPM Dependencies (können tausende Dateien enthalten)
- `credentials.json` - Google Service Account Credentials
- `.env*` - Umgebungsvariablen (alle Varianten)
- `*.jks` - Android Keystore Dateien
- `.vscode/` - IDE Konfiguration
- `.husky/` - Git Hooks
- `coverage/` - Test Coverage Reports
- `.expo/` - Expo Build Artifacts
- `.templates/` - Template Dateien
- `.venv/` - Python Virtual Environments
- `Keystore/` - Android Signing Keys
- Weitere projekt-spezifische Dateien

## Wie funktioniert es?

### Zeitplan
- **Automatisch:** Jeden Montag um 9:00 Uhr UTC
- **Manuell:** Über GitHub Actions Tab → "Security Monitor" → "Run workflow"

### Ablauf
1. Checkout des gh-pages Branch für jedes Projekt
2. Scan nach sensiblen Dateien
3. Bei Fund:
   - ❌ Workflow schlägt fehl (rote Markierung)
   - 🎫 Automatisches Issue wird erstellt
   - 📧 GitHub sendet Email-Benachrichtigung

4. Bei Erfolg:
   - ✅ Workflow erfolgreich (grüne Markierung)
   - 📊 Summary zeigt "All repositories are clean!"

## Überwachte Repositories

Aktuell werden folgende Projekte überwacht:
- ✅ 1x1_Trainer
- ✅ Energy_Price_Germany
- ✅ Eisenhauer
- ✅ Pflanzkalender
- ✅ DrawFromMemory

### Bewusst nicht überwacht

- **safe-my-plants** — deployt per GitHub-Actions-Artefakt (`actions/upload-pages-artifact` mit `path: dist`, `build_type: workflow`), es gibt keinen `gh-pages`-Branch. Der Monitor checkt fest `ref: gh-pages` aus (mit `continue-on-error`), ein Eintrag würde also still nichts prüfen. Veröffentlicht wird ausschließlich der `expo export`-Output aus `dist/`; Quellcode-Dateien wie `node_modules/`, `.env*` oder Keystores können dort nicht hineingeraten. Das Quell-Repo selbst wird durch dessen eigenen `security-scan.yml` (Secret-Scan) abgedeckt.

Gilt sinngemäß für jedes weitere Projekt mit Actions-Deployment: nicht in `matrix.repo` aufnehmen, sondern hier eintragen.

### Neues Projekt hinzufügen

Editiere `.github/workflows/security-monitor.yml`:

```yaml
strategy:
  matrix:
    repo:
      - 1x1_Trainer
      - Energy_Price_Germany
      - Eisenhauer
      - Pflanzkalender
      - DrawFromMemory
      - NeuesProjekt  # <-- Hier hinzufügen (nur mit gh-pages-Branch)
```

## Was tun bei einer Warnung?

Wenn ein Issue erstellt wird:

### 1. Lokales Cleanup ausführen

```bash
cd /path/to/PROJEKT
git checkout gh-pages
git pull origin gh-pages

# Option A: Cleanup-Skript verwenden
./scripts/cleanup-gh-pages.sh

# Option B: Manuell löschen
rm -rf node_modules/ .vscode/ .husky/ coverage/
git add -A
git commit -m "🧹 Security: Remove sensitive files from gh-pages"
git push origin gh-pages
```

### 2. Deployment-Workflow prüfen

Stelle sicher, dass der Deployment-Workflow einen Cleanup-Step enthält:

```yaml
- name: Cleanup sensitive files from gh-pages
  run: |
    echo "🧹 Cleaning up sensitive files..."
    git checkout gh-pages
    # ... cleanup logic ...
```

### 3. Issue schließen

Nach erfolgreichem Cleanup:
- Gehe zum Issue
- Kommentiere: "✅ Fixed"
- Schließe das Issue

## Benachrichtigungen

### Email-Benachrichtigungen aktivieren

1. Gehe zu GitHub Settings → Notifications
2. Aktiviere "Actions" unter "Email notifications"
3. Du erhältst dann Emails bei:
   - Fehlgeschlagenen Workflows
   - Neu erstellten Issues

### GitHub Notifications

Issues werden automatisch mit Labels versehen:
- 🔴 `security` - Security-relevante Issues
- 📄 `gh-pages` - Betrifft gh-pages Branch
- 🤖 `automated` - Automatisch erstellt

## Manuelle Ausführung

### Alle Projekte scannen

1. Gehe zu https://github.com/S540d/S540d.github.io/actions
2. Klicke "Security Monitor - Scan all gh-pages branches"
3. Klicke "Run workflow"
4. Wähle Branch: `main`
5. Klicke "Run workflow"

### Einzelnes Projekt prüfen

```bash
# Lokal checken
cd /path/to/PROJEKT
git checkout gh-pages

# Diese Dateien sollten NICHT existieren:
ls -la node_modules/      # Sollte nicht existieren
ls -la .vscode/           # Sollte nicht existieren
ls -la credentials.json   # Sollte nicht existieren
ls -la .env*              # Sollte nicht existieren
```

## Häufig gestellte Fragen

### Warum schlägt der Scan fehl, obwohl ich gerade aufgeräumt habe?

- GitHub Pages kann bis zu 10 Minuten brauchen, um zu aktualisieren
- Warte 10-15 Minuten und führe den Workflow erneut aus

### Kann ich die Scan-Frequenz ändern?

Ja! Editiere `.github/workflows/security-monitor.yml`:

```yaml
schedule:
  # Täglich um 9 Uhr
  - cron: '0 9 * * *'

  # Jeden Freitag um 18 Uhr
  - cron: '0 18 * * 5'

  # Jeden 1. des Monats
  - cron: '0 9 1 * *'
```

Cron Syntax: `Minute Hour Day Month Weekday`

### Was ist mit privaten Repositories?

Der Workflow kann nur öffentliche Repos scannen, da `GITHUB_TOKEN` nur limitierten Zugriff hat.

Für private Repos:
1. Erstelle ein Personal Access Token
2. Füge es als Secret `GH_PAT` hinzu
3. Ändere den Workflow: `token: ${{ secrets.GH_PAT }}`

## Best Practices

### ✅ Do's
- Führe den Scan nach jedem Deployment aus
- Schließe Issues zeitnah
- Halte die Projekt-Liste aktuell
- Prüfe wöchentlich den Workflow-Status

### ❌ Don'ts
- Issues nicht ignorieren
- Workflow nicht deaktivieren
- Keine sensiblen Dateien committen (nutze .gitignore!)
- Cleanup-Steps in Workflows nicht entfernen

## Monitoring Dashboard

### Workflow Status prüfen

https://github.com/S540d/S540d.github.io/actions/workflows/security-monitor.yml

### Offene Security Issues

https://github.com/S540d/S540d.github.io/issues?q=is%3Aissue+is%3Aopen+label%3Asecurity

## Wartung

### Monatlich
- [ ] Workflow-Status prüfen
- [ ] Projekt-Liste aktualisieren
- [ ] Neue sensible Dateitypen hinzufügen (falls nötig)

### Bei jedem neuen Projekt
- [ ] Projekt zur Matrix hinzufügen
- [ ] Cleanup-Skript erstellen
- [ ] Deployment-Workflow mit Cleanup erweitern
- [ ] Einmal manuell scannen

## Kontakt & Support

Bei Fragen oder Problemen:
- Erstelle ein Issue in diesem Repository
- Markiere es mit `question` Label

---

🤖 **Automatisiert. Sicher. Wartungsfrei.**

Letzte Aktualisierung: 2026-01-31
