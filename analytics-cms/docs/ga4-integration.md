# Integracja z Google Analytics 4

## Konfiguracja
1. Włącz Google Analytics Data API w Google Cloud Console
2. Utwórz credentials (OAuth 2.0 lub Service Account)
3. Pobierz plik konfiguracyjny (credentials.json)

## Instalacja
```bash
npm install @google-analytics/data
```

## Przykłady Implementacji

### Podstawowa konfiguracja
```javascript
const { BetaAnalyticsDataClient } = require('@google-analytics/data');

const analyticsDataClient = new BetaAnalyticsDataClient({
  keyFilename: 'path/to/credentials.json',
});
```

### Pobieranie podstawowych metryk
```javascript
async function getBasicMetrics(propertyId, dateRange) {
  try {
    const [response] = await analyticsDataClient.runReport({
      property: `properties/${propertyId}`,
      dateRanges: [dateRange],
      metrics: [
        { name: 'activeUsers' },
        { name: 'sessions' },
        { name: 'conversions' }
      ],
      dimensions: [
        { name: 'date' }
      ],
    });

    return response;
  } catch (error) {
    console.error('Error fetching GA4 data:', error);
    throw error;
  }
}
```

### Zaawansowane zapytania
```javascript
async function getDetailedReport(propertyId) {
  const [response] = await analyticsDataClient.runReport({
    property: `properties/${propertyId}`,
    dateRanges: [
      {
        startDate: '30daysAgo',
        endDate: 'today',
      },
    ],
    dimensions: [
      { name: 'date' },
      { name: 'country' },
      { name: 'deviceCategory' }
    ],
    metrics: [
      { name: 'activeUsers' },
      { name: 'newUsers' },
      { name: 'sessions' },
      { name: 'averageSessionDuration' },
      { name: 'screenPageViews' }
    ],
    orderBys: [
      {
        dimension: { orderType: 'NUMERIC', dimensionName: 'date' },
        desc: true
      }
    ],
    limit: 100
  });

  return response;
}
```

### Obsługa danych w czasie rzeczywistym
```javascript
async function getRealTimeData(propertyId) {
  const [response] = await analyticsDataClient.runRealtimeReport({
    property: `properties/${propertyId}`,
    dimensions: [
      { name: 'country' },
      { name: 'city' }
    ],
    metrics: [
      { name: 'activeUsers' }
    ]
  });

  return response;
}
```

## Cachowanie Wyników
```javascript
const Redis = require('redis');
const client = Redis.createClient();

async function getCachedAnalyticsData(propertyId, dateRange) {
  const cacheKey = `ga4:${propertyId}:${dateRange.startDate}:${dateRange.endDate}`;
  
  // Sprawdź cache
  const cachedData = await client.get(cacheKey);
  if (cachedData) {
    return JSON.parse(cachedData);
  }

  // Pobierz nowe dane
  const data = await getBasicMetrics(propertyId, dateRange);
  
  // Zapisz w cache na 1 godzinę
  await client.set(cacheKey, JSON.stringify(data), 'EX', 3600);
  
  return data;
}
```

## Obsługa Błędów
```javascript
class GA4Error extends Error {
  constructor(message, code, details) {
    super(message);
    this.name = 'GA4Error';
    this.code = code;
    this.details = details;
  }
}

async function safeGA4Call(fn) {
  try {
    return await fn();
  } catch (error) {
    if (error.code === 'PERMISSION_DENIED') {
      throw new GA4Error('Brak dostępu do GA4', 403, error);
    }
    if (error.code === 'RESOURCE_EXHAUSTED') {
      throw new GA4Error('Przekroczono limit zapytań', 429, error);
    }
    throw new GA4Error('Błąd GA4', 500, error);
  }
}
```

## Dobre Praktyki
1. Zawsze używaj cachowania dla często używanych zapytań
2. Implementuj retry logic dla zawodnych zapytań
3. Monitoruj limity API
4. Optymalizuj zapytania przez łączenie metryk
5. Używaj batch requests dla wielu zapytań
6. Implementuj obsługę błędów

## Limity i Ograniczenia
- Maksymalnie 120 zapytań na minutę
- Maksymalnie 10 wymiarów i 10 metryk na zapytanie
- Maksymalnie 100,000 wierszy na zapytanie
- Dane dostępne z ostatnich 14 miesięcy 