---
type: service
id: weather-api
title: OpenWeatherMap API
description: "REST API for current weather data, forecasts, and conditions by location"
tags: [Production, Weather]
connections: []
metadata:
  serviceType: api
  provider: openweathermap
  protocol: rest
  auth_type: api_key
  env_var: OPENWEATHER_API_KEY
  base_url: "https://api.openweathermap.org/data/2.5"
---

## Service Description

Provides access to current weather data and forecasts from OpenWeatherMap. This service is used to fetch real-time weather conditions for a given location, which the pipeline then analyses and formats into a briefing.

## Configuration

### Authentication

Requires an OpenWeatherMap API key set as the `OPENWEATHER_API_KEY` environment variable. Sign up and generate a key at `https://openweathermap.org/api`.

The free tier allows 1,000 API calls per day with current weather and 3-hour forecast data. No credit card required.

### Connection Setup

In your skrptiq app, add a new connection:

1. Go to **Settings > Connections**
2. Click **Add Connection**
3. Select **API Credential**
4. Set the provider to `openweathermap`
5. Paste your API key
6. Save — the app binds it to `OPENWEATHER_API_KEY` at runtime

## Endpoints Used

### Current Weather

`GET /weather?q={city}&appid={API_KEY}&units=metric`

Returns current temperature, humidity, wind speed, pressure, conditions, and cloud cover for a city.

### 5-Day Forecast

`GET /forecast?q={city}&appid={API_KEY}&units=metric`

Returns 3-hour interval forecasts for the next 5 days. Used for trend analysis.

## Rate Limiting

Free tier: 1,000 calls/day, 60 calls/minute. The pipeline typically makes 2 calls per run (current + forecast). No rate limiting concerns for normal use.

## Privacy Considerations

Only the city name is sent to the OpenWeatherMap API. No personal data is transmitted.
