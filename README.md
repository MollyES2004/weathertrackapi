# WeatherTrack API

## Table of Contents
- [Description](#description)
- [Base URL](#base-url)
- [Authorization](#authorization)
- [Endpoints](#endpoints)
  - [Get Current Weather](#get-current-weather)
  - [Get Forecast](#get-forecast)
  - [Get Historical Weather](#get-historical-weather)
- [Response Attributes](#response-attributes)
- [Success and Error Codes](#success-and-error-codes)

## Description
The WeatherTrack API provides developers with accurate current, forecast, and historical weather data for all locations in **North America**. 

---

## Base URL
```
http://apiweathertrack.com/v1
```
---

## Authorization
**All** requests require an API key in the headers:
```http
Authorization: Bearer {{apiKey}}
Accept: application/json
Content-Type: application/json
```

---

## Endpoints
### Get Current Weather
Returns the current weather conditions for a specified location.

**Request**
```http
GET /weather/current?city=Provo&countryCode=US&units=metric
Authorization: Bearer YOUR_API_KEY
Accept: application/json
```

**Parameters**
| Name        | Type   | Required | Description                          | Example     |
|-------------|--------|----------|--------------------------------------|-------------|
| city        | string | No       | Name of the city                     | Provo       |
| countryCode | string | No       | ISO country code                     | US          |
| latitude    | number | No       | Geographic latitude                  | 40.2338     |
| longitude   | number | No       | Geographic longitude                 | -111.6585   |
| units       | string | No       | Unit system (metric or imperial)     | metric      |
| language    | string | No       | Language for weather description     | en          |

Note: Provide either city and countryCode, or latitude and longitude.

### Headers
```http
Authorization: Bearer {{apiKey}}
Accept: application/json
Content-Type: application/json
```

## Response Body

```json
{
  "location": {
    "city": "Provo",
    "country": "US",
    "latitude": 40.2338,
    "longitude": -111.6585
  },
  "current": {
    "temperature": 18,
    "feels_like": 16,
    "humidity": 72,
    "wind_speed": 14,
    "condition": "Cloudy",
    "observation_time": "2026-04-09T18:00:00Z"
  }
}
```


### Get Forecast
Returns forecast weather data for a specified location.

**Request**
```http
GET /weather/forecast?city=Toronto&countryCode=CA&units=metric
Authorization: Bearer YOUR_API_KEY
Accept: application/json
```

**Parameters**
| Name        | Type   | Required | Description                          | Example            |
|-------------|--------|----------|--------------------------------------|--------------------|
| city        | string | No       | Name of the city                     | Medford            |
| countryCode | string | No       | ISO country code                     | US                 |
| latitude    | number | No       | Geographic latitude                  | 43.6532            |
| longitude   | number | No       | Geographic longitude                 | -79.3832           |
| units       | string | No       | Unit system (metric or imperial)     | metric             |
| language    | string | No       | Language for weather description     | en                 |
| timezone    | string | No       | Timezone for the requested data      | America/Los_Angeles|    

Note: Provide either city and countryCode, or latitude and longitude.

### Headers
```http
Authorization: Bearer {{apiKey}}
Accept: application/json
Content-Type: application/json
```

## Response Body

```json
{
  "location": {
    "city": "Medford",
    "country": "US"
  },
  "forecast": [
    {
      "date": "2026-04-10",
      "high_temperature": 57,
      "low_temperature": 36,
      "condition": "Rain"
    },
    {
      "date": "2026-04-11",
      "high_temperature": 59,
      "low_temperature": 35,
      "condition": "Partly Cloudy"
    }
  ]
}
```


### Get Historical Weather
Returns historical weather data for a specified location and date range.

**Request** 
```http
GET /weather/historical?city=New%20York&countryCode=US&startDate=2025-01-01&endDate=2025-01-07&units=metric
Authorization: Bearer YOUR_API_KEY
Accept: application/json
```

**Parameters**
| Name        | Type   | Required | Description                          | Example            |
|-------------|--------|----------|--------------------------------------|--------------------|
| city        | string | No       | Name of the city                     | San Diego          |
| countryCode | string | No       | ISO country code                     | US                 |
| latitude    | number | No       | Geographic latitude                  | 40.7128            |
| longitude   | number | No       | Geographic longitude                 | -74.0060           |
| startDate   | string | Yes      | Start date (YYYY-MM-DD)              | 2025-01-01         |
| endDate     | string | Yes      | End date (YYYY-MM-DD)                | 2025-01-07         |
| units       | string | No       | Unit system (metric or imperial)     | metric             |
| timezone    | string | No       | Timezone for the requested data      | America/Los_Angeles|  

Note: Provide either city and countryCode, or latitude and longitude.

### Headers
```http
Authorization: Bearer {{apiKey}}
Accept: application/json
Content-Type: application/json
```

## Response Body
```json
{
  "location": {
    "city": "San Diego",
    "country": "US"
  },
  "historical": [
    {
      "date": "2025-01-01",
      "temperature": 90,
      "humidity": 89,
      "condition": "Sunny"
    },
    {
      "date": "2025-01-02",
      "temperature": 85,
      "humidity": 98,
      "condition": "Sunny"
    }
  ]
}
```

---

## Response Attributes

### Current Weather Response Attributes
location
- city: string — Name of the city
- country: string — ISO country code
- latitude: number — Geographic latitude
- longitude: number — Geographic longitude

current
- temperature: number — Current air temperature
- feels_like: number — Perceived temperature based on weather conditions
- humidity: number — Relative humidity percentage
- wind_speed: number — Wind speed at the time of observation
- condition: string — Short weather condition summary
- observation_time: string — Observation timestamp in ISO 8601 format

### Forecast Response Attributes
location
- city: string — Name of the city
- country: string — ISO country code
- latitude: number — Geographic latitude
- longitude: number — Geographic longitude

forecast
- date: string — Forecast date in YYYY-MM-DD format
- high_temperature: number — Predicted high_temperature
- low_temperature: number — Predicted low_temperature
- condition: string — Forecasted weather condition
- precipitation_chance: number — Chance of precipitation as a percentage
- wind_speed: number — Expected wind speed

  ### Historical Weather Response Attributes
  location
- city: string — Name of the city
- country: string — ISO country code
- latitude: number — Geographic latitude
- longitude: number — Geographic longitude

historical
- date: string — Historical record date in YYYY-MM-DD format
- temperature: number — Recorded air temperature
- humidity: number — Recorded humidity percentage
- wind_speed: number — Recorded wind speed
- condition: string — Recorded weather condition summary
- observation_time: string — Timestamp for the historical reading

## Success and Error Codes

### Success Codes

200 OK — The request was successful and weather data was returned.

### Error Codes

400 Bad Request — The request was invalid. This can happen if required query parameters such as city, latitude, longitude, startDate, or endDate are missing or malformed.

401 Unauthorized — The request could not be authenticated. The API key is missing, invalid, or expired.

403 Forbidden — The client is authenticated but does not have permission to access the requested resource.

404 Not Found — The requested weather resource or location could not be found.

406 Not Applicable —  No content conforms to the criteria given by the user.

429 Too Many Requests — The rate limit for the API has been exceeded. Try again later.

500 Internal Server Error — The server encountered an unexpected condition and could not complete the request.

502 Bad Gateway — The weather service or an upstream provider returned an invalid response.

503 Service Unavailable — The service is temporarily unavailable due to maintenance or high load. Try again later.