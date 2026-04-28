---
type: skill
id: analyse-conditions
title: Analyse Conditions
description: "Interprets raw weather data into actionable insights — comfort level, outdoor suitability, and notable conditions"
tags: [Production, Weather]
connections:
  - target: llm-service
    type: runs_on
---

## Capability

Takes the structured weather data from the fetch step and produces a human-readable analysis: comfort assessment, outdoor activity suitability, notable weather alerts, and trend interpretation.

## When to Use

- After fetching weather data, before generating the final briefing
- When you need weather interpreted in practical terms rather than raw numbers

## What It Does

1. **Assess comfort** — combines temperature, humidity, and wind to determine feels-like conditions and comfort level (comfortable, warm, cold, humid, windy)
2. **Evaluate outdoor suitability** — rates conditions for outdoor activities (ideal, acceptable, poor) based on precipitation probability, wind speed, and temperature
3. **Flag notable conditions** — identifies anything worth highlighting: extreme temperatures, high wind, rain expected, rapid temperature changes
4. **Interpret trends** — compares current conditions to the next 24 hours: warming, cooling, stable, deteriorating, improving

## Inputs

Weather data from {{steps.Fetch Weather Data.output}}.

## Outputs

Structured analysis: comfort rating, outdoor suitability, notable conditions list, and trend summary.
