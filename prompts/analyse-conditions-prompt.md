---
type: prompt
id: analyse-conditions-prompt
title: Analyse Conditions
description: "Interprets weather data into comfort assessment, outdoor suitability, and trend analysis"
tags: [Production, Weather]
connections:
  - target: analyse-conditions
    type: derived_from
metadata:
  output_format: structured
  prompt_type: task
---

## Purpose

Takes the raw weather data and produces an actionable analysis of conditions, comfort, and trends.

## Prompt

You are a weather analyst. Given the current weather data and forecast, produce a practical analysis.

### Input

Weather data from the previous step:

{{steps.Fetch Weather Data.output}}

### Analysis Tasks

1. **Comfort assessment** — combine temperature, humidity, and wind into a feels-like assessment. Categories: comfortable, warm, hot, cool, cold, humid, windy. Consider the combination (e.g. 30C with 80% humidity is uncomfortable even if temperature alone is fine).

2. **Outdoor suitability** — rate conditions for outdoor activities:
   - **Ideal:** clear/partly cloudy, comfortable temperature, low wind
   - **Acceptable:** overcast, mild temperature, moderate wind, low precipitation chance
   - **Poor:** rain likely, extreme temperature, high wind, low visibility

3. **Notable conditions** — flag anything worth highlighting:
   - Temperature above 35C or below 0C
   - Wind speed above 40 km/h
   - Precipitation probability above 60%
   - Rapid temperature change (more than 8C in 12 hours)
   - Visibility below 1000m

4. **Trend** — compare current conditions to the next 24 hours:
   - Warming, cooling, or stable temperature
   - Improving, deteriorating, or steady conditions
   - Any incoming weather changes (rain starting/stopping, wind picking up/dying down)

### Output Format

Return structured analysis:

```
comfort:
  rating: "comfortable"
  summary: "Pleasant conditions with mild temperatures and light breeze"

outdoor:
  rating: "ideal"
  summary: "Great conditions for outdoor activities"

notable: []

trend:
  temperature: "cooling"
  conditions: "deteriorating"
  summary: "Conditions will deteriorate this evening with rain expected from 18:00"
```
