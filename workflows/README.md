# Workflows Directory

Ten katalog zawiera wszystkie pliki workflow n8n w formacie JSON.

## Struktura

Wszystkie workflow są przechowywane bezpośrednio w tym folderze - prosta, płaska struktura bez podkatalogów.

```
workflows/
├── AI Superagent - Main.json
├── Contact Finder.json
├── Fireflies Meeting Summary.json
├── Google Maps.json
├── Notion Things Processing.json
├── Voice Agent.json
└── README.md
```

## Konwencje nazewnictwa

Używaj opisowych nazw, które jasno określają cel workflow.

### Przykłady dobrych nazw:

- `Fireflies Meeting Summary.json` - ✅ jasne, opisowe
- `Notion Things Processing.json` - ✅ jasne, opisowe
- `Daily Sales Report.json` - ✅ prosty i zrozumiały
- `Customer Signup Notification.json` - ✅ opisuje cel

### Unikaj:

- `workflow1.json` - ❌ nieopisowe
- `test.json` - ❌ tymczasowe nazwy
- `new.json` - ❌ niespecyficzne

## Eksportowanie workflow

```bash
# Pojedynczy workflow
n8n export:workflow --id=<workflow-id> --output=workflows/My-Workflow.json

# Wszystkie workflow
n8n export:workflow --all --output=workflows/
```

## Importowanie workflow

```bash
# Pojedynczy workflow
n8n import:workflow --input=workflows/My-Workflow.json

# Wszystkie workflow z katalogu
n8n import:workflow --input=workflows/
```

## Dobre praktyki

1. **Używaj opisowych nazw** - nazwa pliku powinna jasno określać cel workflow
2. **Eksportuj regularnie** po każdej znaczącej zmianie
3. **Commituj z opisem** - wyjaśnij co zostało zmienione
4. **Testuj przed commitowaniem** - upewnij się, że workflow działa
5. **Credentials w n8n UI** - nigdy nie commituj sekretów do Git
