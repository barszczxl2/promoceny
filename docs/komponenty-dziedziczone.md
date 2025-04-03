# Komponenty Dziedziczone

## Obszar Nawigacji
Element dziedziczony na każdej podstronie serwisu.

### Składowe:
- **Logotyp**
  - Kliknięcie przenosi użytkownika na stronę główną serwisu
- **Wyszukiwarka**
  - Możliwość wprowadzenia frazy
  - Przenosi na stronę wyników wyszukiwania (`Promoceny.pl/szukaj/[fraza]`)
- **Menu główne**
  - Produkty (`/produkty/`)
  - Gazetki promocyjne (`/gazetki-promocyjne/`)
  - Sieci handlowe (`/sieci-handlowe/`)
  - Sklepy (`/sklepy/`)
  - Poradnik zakupowy (`/poradnik-zakupowy/`)

## Stałe Komponenty

### Sekcja: Promowane Gazetki
Narzędzie przeznaczone do promowania gazetek sprzedażowych:
- Widget z gazetkami promocyjnymi w formie slidera
- Zawartość oparta na okładkach/pierwszych stronach gazetek
- Prezentacja tylko aktywnych gazetek
- Gazetki pobierane z bazy CDD
- Zarządzanie przez CDD (`https://wydawcy.adretail.pl/publishers#internal`)
- Kliknięcie w gazetkę przenosi do jej przeglądarki

### Sekcja: Popularne Sieci Handlowe
- Prezentacja logotypów wybranych sieci
- Maksymalnie 18 logotypów
- Zarządzanie przez CDD (`https://sklepy.adretail.pl/contractors_priorities`)

### Sekcja: Miasta
- Lista 20 miast
- Możliwości generowania listy:
  - Na podstawie przekazanej listy (zarządzane przez IT)
  - Automatycznie na podstawie liczby odwiedzin
  - Aktualizacja co 2-4 tygodnie

## Stopka

### Elementy Stopki:
- **Gazetki promocyjne**
  - Link do strony głównej gazetek
  - Lista dostępnych kategorii
- **Mapa Strony**
  - Strona główna
  - Produkty
  - Gazetki promocyjne
  - Wykaz sklepów
  - Poradnik zakupowy
- **Informacje Prawne**
  - Kontakt (formularz)
  - Reklama
  - Polityka prywatności
  - Regulamin
  - Klauzula informacyjna
  - Copyright

### Elementy Techniczne
- Wstawki reklamowe
- Analityka (GA4, IWA3, Gemius)
- Plansza RODO 