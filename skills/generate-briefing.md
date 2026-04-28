---
type: skill
id: generate-briefing
title: Generate Briefing
description: "Produces a concise, human-readable weather briefing from the analysis"
tags: [Production, Weather]
connections:
  - target: llm-service
    type: runs_on
---

## Capability

Takes the weather analysis and generates a polished, concise briefing suitable for display or sharing. Adapts tone and detail level based on conditions — brief for unremarkable weather, detailed when there are notable conditions or changes.

## When to Use

- As the final step in the weather pipeline
- When you need a publication-ready weather summary

## What It Does

1. **Lead with what matters** — opens with the most relevant condition: if it's going to rain, lead with that; if it's a beautiful day, say so
2. **Include key numbers** — temperature, wind speed, and precipitation probability in context (not just raw data)
3. **Add the outlook** — what the next 24 hours look like relative to now
4. **Keep it concise** — aim for 3-5 sentences for normal conditions, more only if there are genuine alerts or notable changes

## Inputs

Analysis from {{steps.Analyse Conditions.output}}.

## Outputs

Formatted weather briefing in markdown: 3-5 sentences covering current conditions, comfort assessment, and outlook.
