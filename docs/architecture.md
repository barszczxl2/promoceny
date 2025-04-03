# Architektura Systemu

## Przegląd
System składa się z trzech głównych warstw:
1. Frontend (Warstwa prezentacji)
2. Backend (Warstwa logiki biznesowej)
3. Baza danych i Cache

## Struktura Projektu
```
/src
├── /api                    # Backend API
│   ├── /controllers        # Kontrolery API
│   ├── /services          # Serwisy biznesowe
│   ├── /models            # Modele danych
│   └── /middleware        # Middleware
│
├── /client                # Frontend aplikacji
│   ├── /components        # Komponenty React
│   ├── /hooks            # Custom hooks
│   ├── /pages            # Strony aplikacji
│   └── /utils            # Narzędzia pomocnicze
│
├── /config                # Konfiguracja
├── /scripts              # Skrypty pomocnicze
└── /tests                # Testy
```

## Komponenty Systemu

### Frontend
- **Dashboard**
  - Widoki raportów
  - Interaktywne wykresy
  - Filtry i kontrolki
  - System nawigacji

- **Panel Administracyjny**
  - Zarządzanie użytkownikami
  - Konfiguracja raportów
  - Ustawienia systemu

- **System Powiadomień**
  - Alerty
  - Powiadomienia o aktualizacjach
  - Status systemu

### Backend
- **API Gateway**
  - Routing
  - Autoryzacja
  - Rate limiting
  - Walidacja

- **Serwisy**
  - GoogleAnalyticsService
  - GoogleSheetsService
  - ReportingService
  - NotificationService

- **System Cache**
  - Redis
  - Strategie cachowania
  - Invalidacja cache

### Baza Danych
- **Collections/Tables**
  - Users
  - Reports
  - Dashboards
  - Settings
  - Notifications

## Przepływ Danych
1. Użytkownik żąda danych
2. Frontend wysyła zapytanie do API
3. API sprawdza cache
4. Jeśli brak w cache:
   - Pobiera dane z GA4/Sheets
   - Przetwarza dane
   - Zapisuje w cache
5. Zwraca dane do frontendu

## Bezpieczeństwo
- OAuth 2.0
- JWT
- HTTPS
- Rate Limiting
- Input Validation
- XSS Protection
- CORS

## Skalowanie
- Horizontal scaling
- Load balancing
- Cache distribution
- Database sharding

## Monitoring
- Error tracking
- Performance monitoring
- User activity logging
- System health checks 