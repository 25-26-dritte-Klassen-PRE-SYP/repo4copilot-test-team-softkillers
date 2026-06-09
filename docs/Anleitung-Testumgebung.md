# CI/CD Testumgebung - Schritt für Schritt Anleitung

## Übersicht

Diese Anleitung führt dich durch die Einrichtung einer vollständigen Testumgebung mit CI/CD-Pipeline und automatischen Prüfungen für das Projekt. Die Anleitung ist nicht redundant strukturiert und ermöglicht es allen Teammitgliedern, die Umgebung selbständig einzurichten und zu nutzen.

---

## 1. Vorbereitung & Grundlagen

### 1.1 Repository-Einstellungen überprüfen

- Navigiere zu **Settings** → **General**
- Stelle sicher, dass das Repository nach Projektanforderungen konfiguriert ist (öffentlich/privat)
- Notiere die Repository-URL: `https://github.com/25-26-dritte-Klassen-PRE-SYP/repo4copilot-test-team-softkillers`

### 1.2 Erforderliche Zugriffsrechte überprüfen

- Admin oder Maintainer-Rechte im Repository erforderlich für CI/CD-Setup
- Alle Team-Mitglieder benötigen mindestens **Write-Zugriff**
- Prüfe unter **Settings** → **Collaborators** die Zugriffsrechte

### 1.3 Voraussetzungen für lokale Entwicklung

```bash
# Erforderliche Tools installieren
- Git (mindestens Version 2.25)
- Node.js / npm (falls Projekt Node-basiert ist)
- Docker & Docker Compose (für lokale Container-Tests)
- Dein bevorzugter Code-Editor (VS Code, WebStorm, etc.)
```

---

## 2. GitHub Actions Workflow Setup

### 2.1 Workflow-Verzeichnis erstellen

GitHub Actions sucht automatisch nach Workflows im Verzeichnis `.github/workflows/`.

```bash
mkdir -p .github/workflows
```

### 2.2 Automatische Prüfungen definieren

Erstelle folgende Workflow-Dateien im `.github/workflows/`-Verzeichnis:

#### A. **Linting & Code Quality** (`lint.yml`)

Automatische Überprüfung von Code-Stil und Formatierung bei jedem Push und Pull Request.

```yaml
name: Lint & Code Quality

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main, develop ]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run ESLint
        run: npm run lint
      
      - name: Check code formatting
        run: npm run format:check
```

#### B. **Build & Compilation** (`build.yml`)

Prüfe, dass das Projekt erfolgreich kompiliert/gebaut wird.

```yaml
name: Build

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main, develop ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Build project
        run: npm run build
      
      - name: Upload build artifacts
        uses: actions/upload-artifact@v3
        with:
          name: build-output
          path: dist/
```

#### C. **Automatisierte Tests** (`test.yml`)

Führe Unit-Tests und Integrationstests aus.

```yaml
name: Test

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main, develop ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run unit tests
        run: npm run test:unit
      
      - name: Run integration tests
        run: npm run test:integration
      
      - name: Generate coverage report
        run: npm run test:coverage
      
      - name: Upload coverage to Codecov
        uses: codecov/codecov-action@v3
        with:
          files: ./coverage/coverage-final.json
```

#### D. **Security Scanning** (`security.yml`)

Automatische Überprüfung auf Sicherheitslücken in Abhängigkeiten.

```yaml
name: Security Scan

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main, develop ]
  schedule:
    - cron: '0 0 * * 0'  # Wöchentlich Sonntags

jobs:
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Run npm audit
        run: npm audit --audit-level=moderate
      
      - name: Run Snyk security scan
        uses: snyk/actions/node@master
        env:
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}
        with:
          args: --severity-threshold=high
```

### 2.3 Workflows aktivieren

- Gehe zu **Actions** im Repository
- Alle Workflows sollten automatisch erkannt und aktiviert sein
- Teste die Workflows durch einen Push auf `main` oder `develop`

---

## 3. Lokale Testumgebung einrichten

### 3.1 Repository klonen

```bash
git clone https://github.com/25-26-dritte-Klassen-PRE-SYP/repo4copilot-test-team-softkillers.git
cd repo4copilot-test-team-softkillers
```

### 3.2 Abhängigkeiten installieren

```bash
npm install
```

### 3.3 Lokale Umgebungsvariablen konfigurieren

Erstelle eine `.env.local`-Datei im Projektroot:

```env
# Lokale Konfiguration
NODE_ENV=development
DEBUG=true

# Services (falls relevant)
DATABASE_URL=postgresql://user:password@localhost:5432/test_db
REDIS_URL=redis://localhost:6379
CLICKHOUSE_URL=http://localhost:8123

# API Keys (für Entwicklung)
API_KEY=your-dev-api-key
```

**Wichtig:** `.env.local` und `.env.*.local` gehören in die `.gitignore` (sensitive Daten schützen!)

### 3.4 Docker-basierte Testumgebung (optional)

Falls Services wie PostgreSQL, Redis, ClickHouse benötigt werden:

```bash
# Docker Compose für lokale Services
docker-compose up -d
```

Beispiel `docker-compose.yml`:

```yaml
version: '3.8'
services:
  postgres:
    image: postgres:15-alpine
    ports:
      - "5432:5432"
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: test_db
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"

  clickhouse:
    image: clickhouse/clickhouse-server:latest
    ports:
      - "8123:8123"
    environment:
      CLICKHOUSE_DEFAULT_ACCESS_MANAGEMENT: 1

volumes:
  postgres_data:
```

---

## 4. Automatische Tests lokal ausführen

### 4.1 Alle Tests starten

```bash
npm run test
```

### 4.2 Tests im Watch-Modus (entwicklungsfreundlich)

```bash
npm run test:watch
```

### 4.3 Code-Abdeckung überprüfen

```bash
npm run test:coverage
```

### 4.4 Linting lokal ausführen

```bash
npm run lint
```

### 4.5 Code formatieren

```bash
npm run format
```

---

## 5. Branch-Strategie & Pull Requests

### 5.1 Feature-Branch erstellen

```bash
git checkout -b feature/meine-feature
# oder
git checkout -b fix/bug-name
```

### 5.2 Commits & Push

```bash
git add .
git commit -m "feat: Beschreibung der Änderung"
git push origin feature/meine-feature
```

### 5.3 Pull Request erstellen

- Gehe zu **Pull Requests** im Repository
- Klicke auf **New Pull Request**
- Wähle dein Feature-Branch und zielst auf `main` oder `develop`
- Beschreibe deine Änderungen
- Stelle sicher, dass alle GitHub Actions erfolgreich durchlaufen

### 5.4 Code Review & Merging

- Mindestens **ein Team-Mitglied** muss den PR reviewen
- Alle **CI/CD-Checks müssen bestanden** sein
- Nach Approval: **Squash & Merge** oder **Create a merge commit** (je nach Projekt)

---

## 6. Deploy zu Testumgebung (Cloud/Server)

### 6.1 Deployment-Konfiguration

Erstelle eine `deploy.yml` in `.github/workflows/`:

```yaml
name: Deploy to Test Environment

on:
  push:
    branches: [ develop ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Deploy to staging server
        env:
          DEPLOY_KEY: ${{ secrets.DEPLOY_KEY }}
          DEPLOY_HOST: ${{ secrets.DEPLOY_HOST }}
          DEPLOY_USER: ${{ secrets.DEPLOY_USER }}
        run: |
          mkdir -p ~/.ssh
          echo "$DEPLOY_KEY" > ~/.ssh/deploy_key
          chmod 600 ~/.ssh/deploy_key
          ssh-keyscan -H $DEPLOY_HOST >> ~/.ssh/known_hosts
          ssh -i ~/.ssh/deploy_key $DEPLOY_USER@$DEPLOY_HOST 'cd /var/www/app && git pull && npm install && npm run build'
```

### 6.2 Secrets in GitHub konfigurieren

- Gehe zu **Settings** → **Secrets and variables** → **Actions**
- Füge folgende Secrets hinzu:
  - `DEPLOY_KEY`: SSH Private Key
  - `DEPLOY_HOST`: Server-IP oder Domain
  - `DEPLOY_USER`: Benutzername für SSH-Zugriff

---

## 7. Monitoring & Fehlerbehandlung

### 7.1 Workflow-Status überprüfen

- Gehe zu **Actions** im Repository
- Überprüfe den Status der letzten Runs
- Klicke auf fehlgeschlagene Workflows für Details

### 7.2 Häufige Fehler beheben

| Problem | Lösung |
|---------|--------|
| Abhängigkeiten nicht installiert | `npm ci` statt `npm install` in CI verwenden |
| Tests schlagen in CI fehl, lokal nicht | Unterschiede in Node-Version prüfen |
| Deployment schlägt fehl | SSH-Keys und Secrets überprüfen |
| Timeout bei Tests | Test-Timeout erhöhen oder Tests parallelisieren |

### 7.3 Logs analysieren

```bash
# Lokale Test-Logs prüfen
npm run test 2>&1 | tee test.log

# GitHub Actions Logs im Web-UI ansehen
# Actions → Workflow → Run → Job → Logs
```

---

## 8. Wartung & Best Practices

### 8.1 Regelmäßige Wartung

- **Wöchentlich:** Dependencies updaten (`npm outdated`, `npm update`)
- **Monatlich:** Security Audit durchführen (`npm audit`)
- **Monatlich:** Workflows überprüfen und optimieren

### 8.2 Best Practices

✅ **Tue:**
- Schreibe aussagekräftige Commit-Messages
- Führe Tests lokal aus vor Push
- Nutze Branches für jede Änderung
- Dokumentiere Abhängigkeiten und Versionen
- Halten Workflows wartbar und lesbar

❌ **Vermeidung:**
- Pushe niemals direkt auf `main`
- Speichere Secrets nicht im Code
- Ignoriere fehlgeschlagene CI/CD-Tests
- Verwende veraltete Dependencies
- Erstelle Workflows ohne Dokumentation

### 8.3 Performance-Optimierung

```yaml
# Caching für schnellere Builds
- uses: actions/cache@v3
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
    restore-keys: |
      ${{ runner.os }}-node-

# Parallele Job-Ausführung
jobs:
  lint:
    # ...
  test:
    # ...
  build:
    # ...
```

---

## 9. Troubleshooting & Support

### 9.1 Häufig gestellte Fragen

**F: Wie lange dauert ein kompletter CI/CD-Run?**
A: Normalerweise 5-15 Minuten, abhängig von Tests und Projekt-Größe.

**F: Kann ich einen fehlgeschlagenen Workflow erneut ausführen?**
A: Ja, in GitHub Actions unter dem fehlgeschlagenen Run auf "Re-run jobs" klicken.

**F: Wie erstelle ich einen neuen Workflow?**
A: Neue YAML-Datei in `.github/workflows/` erstellen und mit dem entsprechenden Trigger konfigurieren.

### 9.2 Kontakt & Eskalation

- **Technische Fragen:** Team Slack-Kanal `#development`
- **Infra-Probleme:** Infra-Team kontaktieren
- **Security-Issues:** Security-Team benachrichtigen (nicht öffentlich!)

---

## Zusammenfassung der Schritte

1. ✅ Repository-Einstellungen prüfen
2. ✅ Zugriffsrechte überprüfen
3. ✅ `.github/workflows/` erstellen
4. ✅ Workflow-YAML-Dateien hinzufügen (lint, build, test, security)
5. ✅ Lokale Umgebung aufsetzen
6. ✅ Tests lokal ausführen
7. ✅ Branch-Strategie etablieren
8. ✅ First Push & Workflow-Validierung
9. ✅ Deployment-Pipeline aufsetzen (optional)
10. ✅ Team-Dokumentation & Schulung

---

**Version:** 1.0  
**Letzte Aktualisierung:** Juni 2026  
**Verantwortlich:** Development Team
