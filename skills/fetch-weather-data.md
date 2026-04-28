---
type: skill
id: fetch-weather-data
title: Fetch Weather Data
description: "Reads the API schema, composes HTTP requests to OpenWeatherMap, and returns structured weather data"
tags: [Production, Weather]
connections:
  - target: weather-api
    type: runs_on
  - target: llm-service
    type: runs_on
  - target: openweather-api-schema
    type: references
---

## Capability

Reads the OpenWeatherMap API schema source node and composes the correct HTTP requests to fetch current weather and forecast data for a given city. The engine executes the requests with the API credential injected from the weather-api connection.

## When to Use

- As the first step in any weather-related pipeline
- When you need current conditions and short-term forecast for a location

## What It Does

1. **Read the API schema** — examines the openweather-api-schema source to understand available endpoints, parameters, and response shapes
2. **Compose the current weather request** — builds `GET /weather?q={city}&units=metric` with the correct parameters
3. **Compose the forecast request** — builds `GET /forecast?q={city}&units=metric` for the 5-day outlook
4. **Parse responses** — extracts temperature, humidity, wind, conditions, and forecast trends into a structured format

## Inputs

| Name | Required | Description | Example |
|------|----------|-------------|---------|
| `{{input.city}}` | Yes | City name, optionally with country code | `London,GB` |

## Outputs

Structured weather data: current conditions (temperature, humidity, wind, pressure, description) and forecast summary (next 24 hours, trend direction).
