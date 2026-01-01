# Przewodnik po workflowach n8n - Krok po kroku

## Spis treści
1. [Czym jest n8n?](#czym-jest-n8n)
2. [Instalacja i uruchomienie](#instalacja-i-uruchomienie)
3. [Interfejs użytkownika](#interfejs-użytkownika)
4. [Tworzenie pierwszego workflow](#tworzenie-pierwszego-workflow)
5. [Rodzaje node'ów](#rodzaje-nodeów)
6. [Praca z danymi](#praca-z-danymi)
7. [Debugowanie i testowanie](#debugowanie-i-testowanie)
8. [Dobre praktyki](#dobre-praktyki)
9. [Przykładowe workflow](#przykładowe-workflow)

---

## Czym jest n8n?

**n8n** (pronounced "n-eight-n") to platforma do automatyzacji workflow, która łączy możliwości AI z automatyzacją procesów biznesowych. Jest to narzędzie open-source, które pozwala na:

- Łączenie dowolnych aplikacji z API
- Manipulowanie danymi z minimalną ilością kodu
- Tworzenie złożonych automatyzacji
- Integrację z AI i modelami językowymi

---

## Instalacja i uruchomienie

### Opcja 1: Docker (zalecana)

```bash
# Uruchomienie n8n w Dockerze
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

### Opcja 2: npm

```bash
# Instalacja globalna
npm install n8n -g

# Uruchomienie
n8n start
```

### Opcja 3: n8n Cloud

Możesz również skorzystać z hostowanej wersji na [n8n.io](https://n8n.io/).

Po uruchomieniu n8n będzie dostępne pod adresem: `http://localhost:5678`

---

## Interfejs użytkownika

### Główne elementy interfejsu:

```
┌─────────────────────────────────────────────────────────────┐
│  Menu główne  │           Canvas (płótno)                   │
│               │                                             │
│  - Workflows  │    ┌─────┐      ┌─────┐      ┌─────┐       │
│  - Credentials│    │Node │ ───► │Node │ ───► │Node │       │
│  - Variables  │    │  1  │      │  2  │      │  3  │       │
│  - Settings   │    └─────┘      └─────┘      └─────┘       │
│               │                                             │
│               │                           [+ Add Node]      │
└─────────────────────────────────────────────────────────────┘
```

### Nawigacja:
- **Canvas** - główne płótno gdzie budujesz workflow
- **Node Menu** - panel z dostępnymi node'ami (Tab lub +)
- **Execution Panel** - podgląd wyników wykonania

---

## Tworzenie pierwszego workflow

### Krok 1: Utwórz nowy workflow

1. Kliknij **"Create New Workflow"** lub użyj skrótu `Ctrl+N`
2. Nadaj nazwę workflow

### Krok 2: Dodaj Trigger Node

Każdy workflow zaczyna się od **trigger node** - węzła wyzwalającego:

1. Kliknij **"Add first step"** lub naciśnij `Tab`
2. Wyszukaj odpowiedni trigger, np.:
   - **Schedule Trigger** - uruchomienie według harmonogramu
   - **Webhook** - uruchomienie przez HTTP request
   - **Manual Trigger** - ręczne uruchomienie do testów

### Krok 3: Dodaj kolejne node'y

1. Kliknij **+** obok istniejącego node'a
2. Wybierz node z menu
3. Skonfiguruj parametry

### Krok 4: Połącz node'y

- Node'y łączą się automatycznie
- Możesz przeciągać połączenia między node'ami
- Jeden node może mieć wiele wyjść (np. IF node)

### Krok 5: Aktywuj workflow

1. Kliknij przycisk **"Active"** w prawym górnym rogu
2. Workflow zacznie działać według zdefiniowanych triggerów

---

## Rodzaje node'ów

### 1. Trigger Nodes (Wyzwalacze)

| Node | Opis |
|------|------|
| **Schedule Trigger** | Uruchamia workflow według harmonogramu (cron) |
| **Webhook** | Uruchamia workflow przez HTTP request |
| **Email Trigger** | Uruchamia przy nowym emailu |
| **Manual Trigger** | Do ręcznego testowania |

### 2. Action Nodes (Akcje)

| Node | Opis |
|------|------|
| **HTTP Request** | Wykonuje zapytania HTTP/API |
| **Set** | Ustawia/modyfikuje dane |
| **Code** | Wykonuje własny kod JavaScript/Python |
| **Send Email** | Wysyła email |

### 3. Logic Nodes (Logika)

| Node | Opis |
|------|------|
| **IF** | Warunkowe rozgałęzienie workflow |
| **Switch** | Wielokrotne rozgałęzienie |
| **Merge** | Łączy dane z wielu ścieżek |
| **Loop Over Items** | Iteracja po elementach |

### 4. Integration Nodes (Integracje)

n8n oferuje **400+ gotowych integracji**, m.in.:
- Google (Sheets, Drive, Gmail)
- Slack, Discord
- GitHub, GitLab
- Databases (MySQL, PostgreSQL, MongoDB)
- CRM (Salesforce, HubSpot)
- AI (OpenAI, Anthropic)

---

## Praca z danymi

### Struktura danych w n8n

Dane przepływają między node'ami jako tablica obiektów JSON:

```json
[
  {
    "json": {
      "name": "Jan Kowalski",
      "email": "jan@example.com",
      "age": 30
    }
  },
  {
    "json": {
      "name": "Anna Nowak",
      "email": "anna@example.com",
      "age": 25
    }
  }
]
```

### Wyrażenia (Expressions)

Używaj wyrażeń do dynamicznego dostępu do danych:

```javascript
// Dostęp do pola z poprzedniego node'a
{{ $json.name }}

// Dostęp do konkretnego elementu
{{ $node["HTTP Request"].json.data[0].id }}

// Wyrażenia JavaScript
{{ $json.price * 1.23 }}

// Funkcje pomocnicze
{{ $now.format('YYYY-MM-DD') }}
```

### Node "Set" - modyfikacja danych

```javascript
// Dodawanie nowych pól
{
  "fullName": "{{ $json.firstName }} {{ $json.lastName }}",
  "createdAt": "{{ $now }}",
  "status": "active"
}
```

### Node "Code" - własny kod

```javascript
// JavaScript w node Code
for (const item of $input.all()) {
  item.json.processed = true;
  item.json.timestamp = new Date().toISOString();
}

return $input.all();
```

---

## Debugowanie i testowanie

### 1. Wykonanie testowe

1. Kliknij **"Test workflow"** lub użyj `Ctrl+Enter`
2. Workflow wykona się od początku do końca
3. Zobaczysz dane wyjściowe każdego node'a

### 2. Wykonanie pojedynczego node'a

1. Wybierz node
2. Kliknij **"Test step"**
3. Node wykona się z danymi z poprzedniego kroku

### 3. Podgląd danych

- Kliknij na node aby zobaczyć jego output
- Użyj zakładki **"Input"** i **"Output"**
- Przełączaj między widokiem **Table** i **JSON**

### 4. Logi wykonania

- Przejdź do **Executions** w menu
- Zobacz historię wszystkich wykonań
- Analizuj błędy i czas wykonania

### 5. Obsługa błędów

Dodaj **Error Trigger** node aby:
- Przechwytywać błędy workflow
- Wysyłać powiadomienia o błędach
- Implementować logikę retry

---

## Dobre praktyki

### 1. Nazewnictwo
- Używaj opisowych nazw dla workflow i node'ów
- Dodawaj notatki wyjaśniające logikę

### 2. Modularność
- Dziel duże workflow na mniejsze
- Używaj **Sub-workflow** dla powtarzalnych operacji

### 3. Obsługa błędów
- Zawsze dodawaj obsługę błędów
- Używaj **Error Trigger** i powiadomień

### 4. Bezpieczeństwo
- Przechowuj sekrety w **Credentials**, nie w kodzie
- Używaj zmiennych środowiskowych

### 5. Wersjonowanie
- Eksportuj workflow jako JSON
- Przechowuj w Git

```bash
# Eksport workflow
n8n export:workflow --id=<workflow-id> --output=workflow.json

# Import workflow
n8n import:workflow --input=workflow.json
```

### 6. Optymalizacja
- Unikaj niepotrzebnych node'ów
- Używaj filtrowania danych jak najwcześniej
- Monitoruj czas wykonania

---

## Przykładowe workflow

### Przykład 1: Codzienny raport email

```
[Schedule Trigger] → [HTTP Request] → [Set] → [Send Email]
     (cron)           (pobierz dane)   (formatuj)  (wyślij)
```

**Konfiguracja:**

1. **Schedule Trigger**: `0 8 * * *` (codziennie o 8:00)
2. **HTTP Request**: GET do API z danymi
3. **Set**: Formatowanie danych do emaila
4. **Send Email**: Wysyłka raportu

### Przykład 2: Webhook z walidacją

```
[Webhook] → [IF] → [Success Path] → [Response]
              ↓
         [Error Path] → [Response (error)]
```

### Przykład 3: Synchronizacja danych

```
[Schedule] → [Database Query] → [Loop] → [HTTP Request] → [Update Database]
```

### Przykład 4: AI Chatbot

```
[Chat Trigger] → [AI Agent] → [Tool Nodes] → [Response]
                     ↑
              [Memory Node]
```

---

## Przydatne skróty klawiszowe

| Skrót | Akcja |
|-------|-------|
| `Tab` | Otwórz menu node'ów |
| `Ctrl+Enter` | Uruchom workflow |
| `Ctrl+S` | Zapisz workflow |
| `Ctrl+C/V` | Kopiuj/Wklej node |
| `Delete` | Usuń wybrany node |
| `Ctrl+Z` | Cofnij |
| `Space` | Przesuń canvas |

---

## Zasoby dodatkowe

- [Oficjalna dokumentacja n8n](https://docs.n8n.io/)
- [Biblioteka gotowych workflow](https://n8n.io/workflows/)
- [Blog n8n z tutorialami](https://blog.n8n.io/tag/tutorial/)
- [Społeczność n8n](https://community.n8n.io/)

---

## Podsumowanie

Praca z n8n workflow składa się z następujących kroków:

1. **Zaplanuj** - określ cel automatyzacji
2. **Wybierz trigger** - zdecyduj co uruchamia workflow
3. **Dodaj node'y** - zbuduj logikę krok po kroku
4. **Połącz dane** - użyj wyrażeń do przekazywania danych
5. **Testuj** - sprawdź każdy krok przed aktywacją
6. **Aktywuj** - włącz workflow
7. **Monitoruj** - obserwuj wykonania i błędy

n8n daje Ci elastyczność kodu z prostotą no-code. Zacznij od prostych automatyzacji i stopniowo rozwijaj swoje workflow!
