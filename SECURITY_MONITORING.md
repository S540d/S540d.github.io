# Security Monitoring für GitHub Pages

Zentraler Security-Monitoring-Workflow, der alle S540d GitHub Pages Projekte auf sensible Dateien überwacht (`node_modules/`, `credentials.json`, `.env*`, `*.jks`, `.vscode/`, `.husky/`, `coverage/`, `.expo/`, `Keystore/` u. a.).

## Ablauf

- **Zeitplan:** wöchentlich (Montag 9:00 UTC) oder manuell über GitHub Actions Tab
- Bei Fund: Workflow schlägt fehl, Issue wird automatisch erstellt (Labels `security`, `gh-pages`, `automated`)
- Bei Erfolg: grüner Status, Summary "All repositories are clean!"

## Überwachte Repositories

1x1_Trainer, Energy_Price_Germany, Eisenhauer — neue Projekte in `.github/workflows/security-monitor.yml` (matrix.repo) ergänzen.

## Bei einer Warnung

```bash
cd /path/to/PROJEKT
git checkout gh-pages && git pull origin gh-pages
rm -rf node_modules/ .vscode/ .husky/ coverage/
git add -A && git commit -m "🧹 Security: Remove sensitive files from gh-pages" && git push
```

Danach prüfen, dass der Deployment-Workflow einen Cleanup-Step hat, und das Issue schließen.

Workflow-Status: https://github.com/S540d/S540d.github.io/actions/workflows/security-monitor.yml
