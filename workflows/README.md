# Workflows Directory

Ten katalog zawiera wszystkie pliki workflow n8n w formacie JSON.

## Struktura

### 📁 production/
Aktywne workflow działające w produkcji.

**Charakterystyka:**
- Przetestowane i stabilne
- Aktywowane i działające
- Krytyczne dla codziennych operacji

### 📁 development/
Workflow w fazie rozwoju i testowania.

**Charakterystyka:**
- Eksperymentalne
- W trakcie testowania
- Nieaktywne lub działające tylko w środowisku dev

### 📁 templates/
Szablony workflow do wielokrotnego użycia.

**Charakterystyka:**
- Wzorce do kopiowania
- Najlepsze praktyki
- Gotowe do dostosowania

## Konwencje nazewnictwa

Używaj opisowych nazw w formacie kebab-case:

```
[trigger]-[action]-[purpose].json
```

### Przykłady:

**Schedulowane zadania:**
- `schedule-daily-sales-report.json`
- `schedule-weekly-backup.json`
- `schedule-hourly-data-sync.json`

**Webhooki:**
- `webhook-customer-signup-notification.json`
- `webhook-payment-processing.json`
- `webhook-form-submission.json`

**Manualne:**
- `manual-database-cleanup.json`
- `manual-data-export.json`
- `manual-test-email-template.json`

**Email triggers:**
- `email-trigger-support-ticket.json`
- `email-trigger-invoice-processing.json`

## Eksportowanie workflow

```bash
# Pojedynczy workflow
n8n export:workflow --id=<workflow-id> --output=workflows/production/workflow-name.json

# Wszystkie workflow
n8n export:workflow --all --output=workflows/
```

## Importowanie workflow

```bash
# Pojedynczy workflow
n8n import:workflow --input=workflows/production/workflow-name.json

# Wszystkie z katalogu
n8n import:workflow --input=workflows/production/
```

## Dobre praktyki

1. **Zawsze testuj** workflow przed przeniesieniem do production/
2. **Dokumentuj zmiany** w commit message
3. **Eksportuj regularnie** po każdej znaczącej zmianie
4. **Używaj wersjonowania** - Git śledzi wszystkie zmiany
5. **Przechowuj sekrety bezpiecznie** - nigdy nie commituj credentials

## Przenoszenie workflow

```bash
# Z development do production
git mv workflows/development/my-workflow.json workflows/production/
git commit -m "Promote my-workflow to production"

# Z production do templates (jako szablon)
cp workflows/production/useful-pattern.json workflows/templates/template-useful-pattern.json
# Usuń specyficzne dane i credentials przed commitem
```
