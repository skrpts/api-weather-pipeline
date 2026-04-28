---
type: source
id: openweather-api-schema
title: OpenWeatherMap API Schema
description: "Endpoint reference for the OpenWeatherMap Current Weather and Forecast APIs"
tags: [Production, Reference]
connections: []
metadata:
  source_type: api-schema
  api_version: "2.5"
  format: rest-endpoints
---

## API Reference

Base URL: https://api.openweathermap.org/data/2.5

Authentication: all endpoints require `appid` query parameter with a valid API key.

### GET /weather — Current Weather

Returns current weather conditions for a location.

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `q` | string | Yes (one of) | City name, optionally with country code: `London,GB` |
| `lat` | number | Yes (one of) | Latitude |
| `lon` | number | Yes (one of) | Longitude |
| `units` | string | No | `metric` (Celsius), `imperial` (Fahrenheit), `standard` (Kelvin, default) |
| `appid` | string | Yes | API key |

**Response (200):**

```json
{
  "coord": { "lon": -0.1257, "lat": 51.5085 },
  "weather": [
    { "id": 800, "main": "Clear", "description": "clear sky", "icon": "01d" }
  ],
  "main": {
    "temp": 18.5,
    "feels_like": 17.2,
    "temp_min": 16.0,
    "temp_max": 20.1,
    "pressure": 1015,
    "humidity": 55
  },
  "wind": { "speed": 3.6, "deg": 230, "gust": 5.1 },
  "clouds": { "all": 0 },
  "visibility": 10000,
  "dt": 1680000000,
  "sys": { "sunrise": 1679980000, "sunset": 1680025000 },
  "name": "London"
}
```

### GET /forecast — 5-Day / 3-Hour Forecast

Returns forecast data in 3-hour intervals for the next 5 days.

**Parameters:** Same as `/weather`.

**Response (200):**

```json
{
  "cnt": 40,
  "list": [
    {
      "dt": 1680003600,
      "main": {
        "temp": 17.8,
        "feels_like": 16.5,
        "humidity": 60,
        "pressure": 1014
      },
      "weather": [
        { "main": "Clouds", "description": "scattered clouds" }
      ],
      "wind": { "speed": 4.2, "deg": 210 },
      "pop": 0.15,
      "dt_txt": "2025-03-28 15:00:00"
    }
  ]
}
```

Key fields:
- `pop` — probability of precipitation (0 to 1)
- `rain.3h` / `snow.3h` — volume in mm for the 3-hour period (when present)

### Error Responses

| Status | Meaning |
|--------|---------|
| 401 | Invalid or missing API key |
| 404 | City not found |
| 429 | Rate limit exceeded |

```json
{ "cod": 401, "message": "Invalid API key" }
```
