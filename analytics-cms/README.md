# Analytics CMS

## O Projekcie
System CMS do wizualizacji i zarządzania danymi analitycznymi z Google Analytics 4 i Google Sheets. Projekt umożliwia tworzenie własnych dashboardów, raportów i automatyczne odświeżanie danych.

## Funkcjonalności
- Integracja z Google Analytics 4
- Integracja z Google Sheets
- Interaktywne dashboardy
- System cachowania danych
- Eksport raportów do PDF
- System powiadomień
- Zarządzanie użytkownikami
- Personalizacja widoków

## Technologie
### Frontend
- React.js
- Chart.js/D3.js do wizualizacji
- Material UI/Tailwind CSS

### Backend
- Node.js
- Express.js
- MongoDB/PostgreSQL
- Redis (cache)

### APIs
- Google Analytics Data API
- Google Sheets API

## Dokumentacja
- [Architektura Systemu](docs/architecture.md)
- [Konfiguracja API](docs/api-setup.md)
- [Integracja z GA4](docs/ga4-integration.md)
- [Integracja z Google Sheets](docs/sheets-integration.md)
- [System Cachowania](docs/caching.md)
- [Bezpieczeństwo](docs/security.md)
- [Deployment](docs/deployment.md)

## Instalacja
```bash
# Klonowanie repozytorium
git clone https://github.com/barszczxl2/promoceny.git

# Przejście do katalogu projektu
cd promoceny/analytics-cms

# Instalacja zależności
npm install

# Konfiguracja zmiennych środowiskowych
cp .env.example .env

# Uruchomienie aplikacji
npm run dev
```

## Konfiguracja
1. Utwórz projekt w Google Cloud Console
2. Włącz Google Analytics Data API i Google Sheets API
3. Utwórz credentials (OAuth 2.0)
4. Skonfiguruj zmienne środowiskowe w pliku .env

## Licencja
MIT License 