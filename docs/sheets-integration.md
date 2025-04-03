# Integracja z Google Sheets

## Konfiguracja
1. Włącz Google Sheets API w Google Cloud Console
2. Utwórz credentials (OAuth 2.0 lub Service Account)
3. Pobierz plik konfiguracyjny (credentials.json)

## Instalacja
```bash
npm install googleapis
```

## Przykłady Implementacji

### Podstawowa konfiguracja
```javascript
const { google } = require('googleapis');
const sheets = google.sheets('v4');

async function getAuthClient() {
  const auth = new google.auth.GoogleAuth({
    keyFile: 'path/to/credentials.json',
    scopes: ['https://www.googleapis.com/auth/spreadsheets.readonly'],
  });
  return auth;
}
```

### Pobieranie danych z arkusza
```javascript
async function getSheetData(spreadsheetId, range) {
  const auth = await getAuthClient();
  
  try {
    const response = await sheets.spreadsheets.values.get({
      auth,
      spreadsheetId,
      range,
    });

    return response.data.values;
  } catch (error) {
    console.error('Error fetching sheet data:', error);
    throw error;
  }
}
```

### Zaawansowane operacje
```javascript
class GoogleSheetsService {
  constructor(auth) {
    this.sheets = google.sheets({ version: 'v4', auth });
  }

  // Pobierz wiele zakresów
  async getBatchData(spreadsheetId, ranges) {
    const response = await this.sheets.spreadsheets.values.batchGet({
      spreadsheetId,
      ranges,
    });
    return response.data.valueRanges;
  }

  // Aktualizuj dane
  async updateData(spreadsheetId, range, values) {
    const response = await this.sheets.spreadsheets.values.update({
      spreadsheetId,
      range,
      valueInputOption: 'USER_ENTERED',
      resource: { values },
    });
    return response.data;
  }

  // Dodaj nowe wiersze
  async appendData(spreadsheetId, range, values) {
    const response = await this.sheets.spreadsheets.values.append({
      spreadsheetId,
      range,
      valueInputOption: 'USER_ENTERED',
      resource: { values },
    });
    return response.data;
  }
}
```

## Cachowanie Wyników
```javascript
const Redis = require('redis');
const client = Redis.createClient();

async function getCachedSheetData(spreadsheetId, range) {
  const cacheKey = `sheets:${spreadsheetId}:${range}`;
  
  // Sprawdź cache
  const cachedData = await client.get(cacheKey);
  if (cachedData) {
    return JSON.parse(cachedData);
  }

  // Pobierz nowe dane
  const data = await getSheetData(spreadsheetId, range);
  
  // Zapisz w cache na 5 minut
  await client.set(cacheKey, JSON.stringify(data), 'EX', 300);
  
  return data;
}
```

## Transformacja Danych
```javascript
class SheetDataTransformer {
  // Konwertuj dane arkusza na obiekty
  static toObjects(data, headers) {
    const [headerRow, ...rows] = headers ? data : data;
    const columnHeaders = headers || headerRow;
    
    return rows.map(row => {
      return row.reduce((obj, value, index) => {
        obj[columnHeaders[index]] = value;
        return obj;
      }, {});
    });
  }

  // Konwertuj obiekty na format arkusza
  static toSheetFormat(objects, headers) {
    const rows = objects.map(obj => 
      headers.map(header => obj[header] || '')
    );
    return [headers, ...rows];
  }
}
```

## Obsługa Błędów
```javascript
class SheetsError extends Error {
  constructor(message, code, details) {
    super(message);
    this.name = 'SheetsError';
    this.code = code;
    this.details = details;
  }
}

async function safeSheetsCall(fn) {
  try {
    return await fn();
  } catch (error) {
    if (error.code === 403) {
      throw new SheetsError('Brak dostępu do arkusza', 403, error);
    }
    if (error.code === 429) {
      throw new SheetsError('Przekroczono limit zapytań', 429, error);
    }
    throw new SheetsError('Błąd Google Sheets', 500, error);
  }
}
```

## Dobre Praktyki
1. Używaj batch operacji dla wielu zakresów
2. Implementuj cachowanie dla często używanych danych
3. Ogranicz zakres dostępu do arkuszy (readonly gdy możliwe)
4. Używaj named ranges zamiast adresów komórek
5. Implementuj walidację danych przed zapisem
6. Monitoruj limity API

## Limity i Ograniczenia
- 100 zapytań na minutę na użytkownika
- 500 zapytań na 100 sekund na projekt
- Maksymalnie 10MB na zapytanie
- Maksymalnie 5 milionów komórek na arkusz 