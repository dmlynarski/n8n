# n8n Workflows Repository

Repozytorium zawierające workflow n8n oraz kompleksowy przewodnik po pracy z automatyzacją n8n.

## Struktura projektu

```
n8n/
├── CLAUDE.md                          # Instrukcje dla Claude Code
├── README.md                          # Ten plik
├── n8n-workflow-guide.md             # Kompleksowy przewodnik n8n (PL)
├── .gitignore                        # Reguły Git ignore
└── workflows/                        # Wszystkie pliki workflow JSON
    ├── Feb_17_Voice_Agent.json
    ├── Feb_17th_Google_Maps.json
    ├── Fireflies Meeting Summary.json
    ├── Main_AI_Agent.json
    ├── Notion Things Processing.json
    ├── Personal_Contact_Finder.json
    └── README.md                     # Dokumentacja workflows
```

## Szybki start

### 1. Zainstaluj n8n

**Docker (zalecane):**
```bash
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

**npm:**
```bash
npm install n8n -g
n8n start
```

Dostęp: `http://localhost:5678`

### 2. Importuj workflow

```bash
# Import pojedynczego workflow
n8n import:workflow --input=workflows/My-Workflow.json

# Import wszystkich workflow
n8n import:workflow --input=workflows/
```

### 3. Skonfiguruj credentials

1. Otwórz n8n UI
2. Przejdź do **Settings** → **Credentials**
3. Dodaj wymagane credentials dla workflow
4. ⚠️ **NIGDY** nie commituj prawdziwych sekretów do Git!

### 4. Aktywuj workflow

1. Otwórz workflow w n8n
2. Kliknij przycisk **"Active"**
3. Workflow zacznie działać według konfiguracji

## Praca z workflows

### Tworzenie nowego workflow

1. Utwórz workflow w n8n UI
2. Przetestuj dokładnie
3. Eksportuj do katalogu workflows:

```bash
n8n export:workflow --id=<id> --output=workflows/My-Workflow.json
```

### Konwencje nazewnictwa

Używaj opisowych nazw, które jasno określają cel workflow:

Przykłady:
- `Fireflies Meeting Summary.json`
- `Notion Things Processing.json`
- `Daily Sales Report.json`
- `Customer Signup Notification.json`

### Aktualizacja workflow

```bash
# Po zmianach w n8n UI
n8n export:workflow --id=<id> --output=workflows/My-Workflow.json

# Commituj zmiany
git add workflows/My-Workflow.json
git commit -m "Update My Workflow: opis zmian"
git push
```

## Dokumentacja

### [n8n-workflow-guide.md](./n8n-workflow-guide.md)
Pełny przewodnik po n8n zawierający:
- Podstawy n8n
- Rodzaje node'ów
- Praca z danymi i expressions
- Debugowanie i testowanie
- Dobre praktyki
- Przykładowe workflow
- Skróty klawiszowe

### [workflows/README.md](./workflows/README.md)
Dokumentacja organizacji workflow w tym projekcie.

### [CLAUDE.md](./CLAUDE.md)
Szczegółowe instrukcje dla Claude Code przy pracy z tym projektem.

## Bezpieczeństwo

### ⚠️ NIGDY NIE COMMITUJ:
- API keys
- Passwords
- Tokens
- Database credentials
- Webhook secrets

### ✅ ZAWSZE:
- Używaj n8n Credentials UI
- Przechowuj sekrety w zmiennych środowiskowych
- Przeglądaj pliki przed commitem
- Używaj `.gitignore`

## Przydatne komendy

```bash
# Listowanie workflow
n8n list:workflow

# Eksport wszystkich workflow
n8n export:workflow --all --output=workflows/

# Import workflow
n8n import:workflow --input=workflows/production/

# Wykonanie workflow
n8n execute --id=<workflow-id>

# Start n8n
n8n start
```

## Zasoby

- [Oficjalna dokumentacja n8n](https://docs.n8n.io/)
- [Biblioteka workflow](https://n8n.io/workflows/)
- [Blog n8n](https://blog.n8n.io/tag/tutorial/)
- [Społeczność n8n](https://community.n8n.io/)

## Wsparcie

W razie pytań lub problemów:
1. Sprawdź [n8n-workflow-guide.md](./n8n-workflow-guide.md)
2. Przejrzyj [dokumentację n8n](https://docs.n8n.io/)
3. Szukaj na [forum społeczności](https://community.n8n.io/)

## Licencja

Workflow w tym repozytorium są własnością prywatną. Przewodnik i dokumentacja są udostępnione w celach edukacyjnych.
