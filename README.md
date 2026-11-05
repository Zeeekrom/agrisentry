# AgriSentry

[![English](https://img.shields.io/badge/English-1B5E20?style=flat-square)](README.md) [![中文](https://img.shields.io/badge/%E4%B8%AD%E6%96%87-9e9e9e?style=flat-square)](README.zh-CN.md)

> A configurable farm-monitoring system on the Microsoft Power Platform. It turns (simulated) weather and sensor data into **per-crop risk alerts that a person approves** before any action, with an immutable audit trail and a Power BI dashboard.

![Platform](https://img.shields.io/badge/platform-Microsoft%20Power%20Platform-742774)
![Demo data](https://img.shields.io/badge/demo%20data-synthetic-0f766e)
![Status](https://img.shields.io/badge/status-in%20development-b45309)

**This is a curated, privacy-safe showcase.** The application source, configuration, solution files and data pipelines live in a private repository. This public edition holds the write-up, high-level diagrams and (as the build progresses) synthetic screenshots only — **no source code and no real data**. Source is available for review on request.

This is an independent portfolio prototype. It is **not affiliated with any real farm or organisation**, uses only synthetic data, and performs no real-world actions.

## Overview

![AgriSentry — one-page overview: the problem, how a risk becomes a decision, the Power BI dashboard, and the rules it is built on (synthetic data)](media/poster.jpg)

## Dashboard (synthetic data)

![AgriSentry Power BI dashboard — alerts by severity, by rule, and across a simulated 24-hour window (synthetic data)](screenshots/dashboard.png)

*Built in Power BI over the rule engine's synthetic output: alerts by severity, by rule, and across a simulated 24-hour window. Every figure is synthetic.*

## At a glance

| | |
|---|---|
| What it is | A configurable farm-monitoring workflow: per-crop risk rules, human-approved alerts, a tamper-evident record and a dashboard |
| Who it is for | Small / mid farms and the people who run them — managers, field workers, owners |
| The problem | Farm data is scattered across paper and spreadsheets; high-risk events (e.g. frost) are caught late and leave no record of who decided what |
| Approach | One data model, a configuration-driven rule engine, a *system-suggests / human-approves* loop, and tamper-evident records |
| Configurable | Crops, parameters and risk rules are **configuration, not code** — layered as content / tenant / role |
| Stack | Power Apps · Dataverse · Power Automate · Power BI · Azure Functions (.NET/C#) |
| Status | In development; simulated data; source private |

## The system in blocks

```mermaid
flowchart TB
    A[AgriSentry]
    A --> B[Monitoring]
    B --> B1[Blocks / crops / sensors]
    B --> B2[Readings in · status by block]
    A --> C[Risk & automation]
    C --> C1[Configurable per-crop rules]
    C --> C2[Alert → human approval → action]
    A --> D[Assurance]
    D --> D1[Immutable audit trail]
    D --> D2[Power BI dashboard]
    A --> E[Configuration]
    E --> E1[Crops / parameters / rules]
    E --> E2[Tenant / role options]
```

## Configurable by design

- **Content** — crops, monitored parameters, risk rules and thresholds are data you extend (frost for vineyards, late blight for potatoes, botrytis for berries, …), not hard-coded.
- **Tenant** — each farm enables its own crops/rules, notification channels and approval strictness: a common core with options, no rebuild per customer.
- **Role** — manager / field worker / owner get tailored views and permissions.

## How a risk alert flows

```mermaid
flowchart LR
    S[Simulated weather + sensor feed] --> F[Azure Function: ingest + rule engine]
    F --> DV[(Dataverse)]
    DV --> PA[Power Automate: notify → approve → audit]
    DV --> BI[Power BI dashboard]
    DV --> APP[Power App: blocks / inspections / approvals]
```

Each crop maps to one representative flow (e.g. vineyard → frost as the lead example). The rule engine is configuration-driven, so adding a crop or a rule is a config change, not a code change.

## Design principles

- **Numbers from code, words from the model** — risk decisions come from a deterministic rule engine; any AI only writes narrative, and every figure is traceable.
- **System suggests, a person approves** — no automatic physical action; high-risk steps need a named approver.
- **Tamper-evident audit** — every alert and decision is recorded immutably (who, when, on what evidence).
- **Data residency & least exposure** — data kept in-region; model payloads minimised.
- **Tested rule engine, separated from I/O** — the decision logic is unit-tested against gold-standard cases.

## Technology

Microsoft Power Platform (Power Apps · Dataverse · Power Automate · Power BI) · Azure Functions (.NET/C#) · optional 3D block view via a PCF (React/three.js) control · Git.

## Boundaries / what is not finished

- Simulated data only; not connected to real farms, sensors or weather sources.
- Risk thresholds are illustrative and would need agronomy calibration.
- Single-tenant demo; multi-tenant and higher-assurance profiles are designed-for, not built.
- Aquaculture is out of scope by design.
- A portfolio prototype — not production-ready.

## Status & source

In active development. The source, tests and configuration are kept in a private repository and are **available for review on request** (e.g. a walkthrough in an interview).
