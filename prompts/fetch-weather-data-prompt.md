---
type: prompt
id: fetch-weather-data-prompt
title: Fetch Weather Data
description: "Reads the API schema and composes OpenWeatherMap requests for the given city"
tags: [Production, Weather]
inputs:
  city:
    label: "City"
    description: "City name, optionally with country code (e.g. London,GB)"
    example: "London,GB"
    required: true
    type: text
connections:
  - target: fetch-weather-data
    type: derived_from
metadata:
  output_format: structured
  prompt_type: task
---

## Purpose

Reads the OpenWeatherMap API schema and composes the correct HTTP requests to fetch current weather and forecast data for a given city.

## Prompt

You are a data retrieval agent. You have access to the OpenWeatherMap API via the weather-api service. Read the API schema source to understand the available endpoints.

### Steps

1. Read the API schema source node to understand the available endpoints, parameters, and response formats.
2. Compose a `GET /weather` request for the specified city with `units=metric`.
3. Compose a `GET /forecast` request for the same city with `units=metric`.
4. Execute both requests. The engine will inject the API key from the weather-api connection.
5. Parse the responses and extract the key data points.

### Input

- **City:** {{input.city}}

### Output Format

Return structured weather data:

```
current:
  city: "London"
  temperature: 18.5
  feels_like: 17.2
  humidity: 55
  pressure: 1015
  wind_speed: 3.6
  wind_direction: 230
  conditions: "clear sky"
  visibility: 10000

forecast_24h:
  - time: "15:00"
    temp: 17.8
    conditions: "scattered clouds"
    precipitation_chance: 0.15
  - time: "18:00"
    temp: 15.2
    conditions: "light rain"
    precipitation_chance: 0.65
```

### Error Handling

- If the city is not found (404), report the error and suggest checking the spelling or adding a country code
- If the API key is invalid (401), report that the OpenWeatherMap connection needs to be configured
- If rate limited (429), report that the API limit has been exceeded
