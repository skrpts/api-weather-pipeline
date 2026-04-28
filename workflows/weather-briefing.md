---
type: workflow
id: weather-briefing
title: Weather Briefing
description: "Fetches weather data from OpenWeatherMap API, analyses conditions, and generates a concise briefing"
tags: [Production, Weather]
connections:
  - target: fetch-weather-data
    type: uses
  - target: analyse-conditions
    type: uses
  - target: generate-briefing
    type: uses
  - target: weather-api
    type: runs_on
  - target: llm-service
    type: runs_on
metadata:
  estimated_duration: "10-20 seconds"
  avg_tokens: 5000
  trigger: manual
output_step: "generate-briefing"
composite_steps:
  - "fetch-weather-data"
  - "analyse-conditions"
  - "generate-briefing"
execution:
  - skill: "fetch-weather-data"
    step_type: "generation"
    prompt: "fetch-weather-data-prompt"
  - skill: "analyse-conditions"
    step_type: "synthesis"
    prompt: "analyse-conditions-prompt"
  - skill: "generate-briefing"
    step_type: "content"
    prompt: "generate-briefing-prompt"
---

## Overview

This workflow demonstrates the **schema-driven API integration** pattern. It fetches live weather data from the OpenWeatherMap REST API, analyses the conditions, and produces a concise briefing.

The key pattern: the LLM reads an API schema source node to understand the available endpoints, then composes the correct HTTP requests. The engine executes the requests with credentials injected from the weather-api connection. The LLM never sees the raw API key.

## Pipeline Stages

### Stage 1: Fetch Weather Data

**Input:** City name

The LLM reads the openweather-api-schema source to understand the API endpoints and response formats. It composes `GET /weather` and `GET /forecast` requests for the specified city. The engine executes these with the API key from the weather-api connection.

**Output:** Structured weather data (current conditions + 24-hour forecast).

### Stage 2: Analyse Conditions

Takes the raw weather data and produces practical analysis: comfort assessment, outdoor activity suitability, notable conditions, and trend direction over the next 24 hours.

**Output:** Structured analysis with ratings and summaries.

### Stage 3: Generate Briefing

Transforms the analysis into a concise, human-readable weather briefing. Leads with what matters most, includes key numbers in context, and adds a brief outlook.

**Output:** 3-5 sentence weather briefing in markdown.

## Setup

Before running this workflow:

1. **OpenWeatherMap API key** — sign up at `https://openweathermap.org/api` (free tier is sufficient) and generate an API key.
2. **Add connection** — in skrptiq Settings > Connections, add an API Credential for `openweathermap` with your API key. The app binds it to `OPENWEATHER_API_KEY` at runtime.

## Example Input

```
City: London,GB
```

## Example Output

> Clear skies over London at 22C, feeling comfortable with barely a breeze. Perfect conditions for anything outdoors. Tomorrow looks similar — warm and dry through the weekend.

## Pattern Notes

This skrpt demonstrates three key patterns for API integration:

1. **Service node with `serviceType: api`** — declares the API dependency, authentication method, and credential environment variable. See `services/weather-api.md`.
2. **API schema as source node** — the OpenWeatherMap endpoint reference lives in `sources/openweather-api-schema.md`. The LLM reads this to understand how to compose requests.
3. **Credential injection** — the `requires.services` manifest entry and `runs_on` connection tell the engine which credential to inject. The skrpt never handles raw keys.

These patterns work with any REST API. Swap the schema source and service node to target a different API.
