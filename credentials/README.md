# Credentials Directory

⚠️ **UWAGA: NIGDY NIE COMMITUJ PRAWDZIWYCH CREDENTIALI DO GITA!**

## Cel tego katalogu

Ten katalog służy do przechowywania **szablonów** konfiguracji credentials, nie prawdziwych danych uwierzytelniających.

## Bezpieczeństwo

### ✅ MOŻNA commitować:
- Pliki `*-template.json` z placeholderami
- Dokumentację dotyczącą wymaganych credentials
- `.gitkeep` (pusty plik strukturalny)

### ❌ NIGDY NIE COMMITUJ:
- Prawdziwych API keys
- Haseł
- Tokenów dostępu
- Database connection strings z prawdziwymi danymi
- Jakichkolwiek sekretów

## Jak używać szablonów

### 1. Tworzenie szablonu credentials

```json
{
  "name": "api-service-template",
  "type": "httpHeaderAuth",
  "data": {
    "name": "Authorization",
    "value": "Bearer YOUR_API_KEY_HERE"
  }
}
```

Zapisz jako: `api-service-template.json`

### 2. Używanie szablonu

1. Skopiuj szablon
2. Zastąp placeholdery prawdziwymi wartościami
3. Importuj do n8n przez UI w sekcji **Credentials**
4. **NIE ZAPISUJ** pliku z prawdziwymi danymi w tym katalogu

## Zarządzanie credentials w n8n

### Konfiguracja w UI
1. Otwórz n8n: `http://localhost:5678`
2. Przejdź do **Settings** → **Credentials**
3. Kliknij **Add Credential**
4. Wypełnij dane uwierzytelniające
5. **Zapisz tylko w n8n**, nie eksportuj do pliku

### Zmienne środowiskowe (zalecane dla produkcji)

Użyj zmiennych środowiskowych dla credentials:

```bash
# .env (dodany do .gitignore)
API_KEY=twoj_prawdziwy_klucz
DB_PASSWORD=twoje_prawdziwe_haslo
WEBHOOK_SECRET=twoj_sekret
```

W n8n użyj:
```javascript
{{ $env.API_KEY }}
{{ $env.DB_PASSWORD }}
```

## Przykładowe szablony

### HTTP Authentication Template
```json
{
  "name": "http-auth-template",
  "type": "httpHeaderAuth",
  "data": {
    "name": "Authorization",
    "value": "Bearer <YOUR_TOKEN_HERE>"
  }
}
```

### OAuth2 Template
```json
{
  "name": "oauth2-template",
  "type": "oAuth2Api",
  "data": {
    "clientId": "<YOUR_CLIENT_ID>",
    "clientSecret": "<YOUR_CLIENT_SECRET>",
    "authUrl": "https://api.example.com/oauth/authorize",
    "accessTokenUrl": "https://api.example.com/oauth/token"
  }
}
```

### Database Template
```json
{
  "name": "postgres-template",
  "type": "postgres",
  "data": {
    "host": "localhost",
    "port": 5432,
    "database": "<DATABASE_NAME>",
    "user": "<USERNAME>",
    "password": "<PASSWORD>"
  }
}
```

## Najlepsze praktyki

1. **Używaj n8n Credentials UI** - to najbezpieczniejszy sposób
2. **Zmienne środowiskowe** - dla deployment i produkcji
3. **Secrets management** - rozważ użycie vault (HashiCorp Vault, AWS Secrets Manager)
4. **Dokumentuj wymagania** - jakie credentials są potrzebne dla workflow
5. **Rotacja kluczy** - regularnie zmieniaj API keys i tokeny
6. **Minimalne uprawnienia** - używaj credentials z najmniejszymi wymaganymi uprawnieniami

## Dokumentacja wymaganych credentials

Utrzymuj listę wymaganych credentials dla każdego workflow:

```markdown
### workflow: schedule-daily-sales-report.json
Wymagane credentials:
- Google Sheets API (OAuth2)
- SendGrid API Key
- PostgreSQL Database

### workflow: webhook-customer-signup.json
Wymagane credentials:
- Slack Webhook URL
- CRM API Token
```

## Co robić w razie wycieku credentials?

1. **Natychmiast** rotuj/zmień skompromitowane credentials
2. Sprawdź git history: `git log -p credentials/`
3. Jeśli secret został commitnięty:
   - Zmień natychmiast credential
   - Rozważ rewrite git history (skomplikowane)
   - Powiadom zespół
4. Włącz monitoring dla nieautoryzowanego dostępu

## Przydatne komendy

```bash
# Sprawdź czy przypadkiem nie commitowałeś secretów
git log --all --full-history --source -- credentials/*.json

# Sprawdź current status
git status credentials/

# Pokaż co jest ignorowane
git check-ignore -v credentials/*
```
