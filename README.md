# F1 Intelligence

A data-driven Formula 1 analytics platform for the 2026 season, combining historical race data, circuit-specific performance analysis, statistical prediction, and an interactive web dashboard.

The project is designed as a lightweight, browser-based analytical system rather than a conventional web application: the presentation layer is implemented in HTML/CSS/JavaScript, while the analytical dataset is maintained in structured CSV files and consumed client-side by the dashboard.

## Overview

F1 Intelligence brings together historical and current-season race information into a single analytical interface for evaluating driver performance, constructor competitiveness, circuit-specific trends, race outcomes, and strategic factors.

The repository currently contains a large static dashboard (`index.html`) backed by structured datasets covering drivers, constructors, circuits, qualifying, race results, lap times, pit stops, and championship standings. The repository's current data footprint is approximately 27,000+ records across the underlying datasets.

The system is intended to answer questions such as:

- Which drivers and constructors are currently performing best?
- How does a driver's historical performance vary by circuit?
- What historical indicators are associated with qualifying and race outcomes?
- How do pit-stop behavior and race-control factors affect race scenarios?
- What does the model predict for an upcoming qualifying session or race?
- How can the underlying data be explored through a single interactive dashboard?

## Architecture

The implementation follows a deliberately simple client-side architecture:

```text
                    ┌───────────────────────────┐
                    │      Structured Data      │
                    │                           │
                    │ drivers / constructors   │
                    │ circuits / results       │
                    │ qualifying / lap times   │
                    │ pit stops / standings    │
                    └─────────────┬─────────────┘
                                  │
                                  ▼
                    ┌───────────────────────────┐
                    │     Browser Runtime       │
                    │                           │
                    │ HTML + CSS + JavaScript   │
                    │ data transformation       │
                    │ analytical calculations   │
                    │ prediction logic          │
                    └─────────────┬─────────────┘
                                  │
                                  ▼
                    ┌───────────────────────────┐
                    │      Dashboard UI         │
                    │                           │
                    │ standings · predictions  │
                    │ circuit analysis          │
                    │ historical performance    │
                    │ race intelligence         │
                    └───────────────────────────┘
```

There is no conventional server-side application, database service, or JavaScript framework in the repository. This keeps deployment straightforward and allows the dashboard to be served as a static site.

## Core Capabilities

### Championship analytics

The dashboard provides driver and constructor standings and exposes the underlying championship data used to calculate and display current-season competitive position.

### Historical performance analysis

Historical datasets are used to evaluate driver and circuit-level performance across multiple seasons. This enables comparisons based on actual race and qualifying records rather than relying exclusively on manually curated summaries.

### Circuit intelligence

Circuit metadata and historical results are combined to provide circuit-specific context. The repository includes dedicated circuit datasets alongside race, qualifying, lap-time, and pit-stop records.

### Qualifying and race prediction

The dashboard includes predictive functionality for qualifying and race outcomes. The README previously described this as an "XGBoost ML model"; the repository itself is a client-side static implementation, so this documentation deliberately describes the prediction layer as an **XGBoost-inspired / model-based prediction system** rather than claiming a server-hosted trained XGBoost pipeline that is not present in the repository.

Prediction outputs should be interpreted as probabilistic analytical estimates, not guaranteed outcomes.

### Race strategy and operational factors

The data layer includes pit-stop history and race-event information that can be used to analyze strategic behavior and race scenarios. These variables provide context around stint execution, pit timing, and race outcomes.

### AI Race Engineer

The dashboard includes an AI Race Engineer interface intended to provide natural-language analysis of the available F1 data and analytical context. The current project documentation identifies Anthropic Claude as the model/API layer used by this component.

API credentials should never be committed to the repository. Client-side API integration must be treated carefully because exposing a private API key in browser JavaScript makes the key recoverable by users.

## Data Model

The repository uses flat CSV files as its primary analytical data layer. The main datasets include:

| Dataset | Purpose |
|---|---|
| `drivers.csv` | Historical driver reference data |
| `constructors.csv` / `constructors_data.csv` | Constructor reference information |
| `circuits.csv` | Circuit reference and historical circuit data |
| `circuit_info.csv` | Additional circuit-specific attributes |
| `race_results.csv` | Race-level driver results, grid position, classification, points and laps |
| `qualifying.csv` / `qualifying_results.csv` | Qualifying session and result data |
| `lap_times.csv` | Lap-level timing records |
| `pit_stops.csv` | Pit-stop timing and historical pit-stop records |
| `driver_standings.csv` | Driver championship standings |
| `constructor_standings.csv` | Constructor championship standings |
| `drivers_data.csv` | Dashboard-oriented driver metadata |
| `constructors_data.csv` | Dashboard-oriented constructor metadata |

The exact schema and coverage of individual files may evolve as the project is updated. The CSV layer should therefore be treated as the canonical data interface for the current static dashboard implementation.

## Analytical Pipeline

The conceptual processing pipeline is:

```text
Raw / historical F1 records
          │
          ▼
   Structured CSV datasets
          │
          ▼
 Client-side parsing / filtering
          │
          ├───────────────┐
          ▼               ▼
Historical metrics    Current-season data
          │               │
          └───────┬───────┘
                  ▼
        Analytical / prediction layer
                  │
                  ▼
             Dashboard UI
```

The system favors transparent, inspectable data transformations over a large backend stack. This makes the project easy to deploy and inspect, while also placing responsibility on the client-side implementation for data loading, filtering, and analytical computation.

## Technology Stack

| Layer | Technology |
|---|---|
| Presentation | HTML5, CSS3 |
| Application logic | Vanilla JavaScript |
| Data storage | CSV / flat-file datasets |
| Prediction | Client-side model-based / XGBoost-inspired logic |
| AI assistant | Anthropic Claude API |
| Hosting | GitHub Pages |

The project intentionally avoids a front-end framework. There is currently no `package.json`, Node.js dependency tree, or framework-specific build pipeline required to render the dashboard.

## Repository Structure

```text
f1-intelligence/
├── README.md
├── index.html
│
├── drivers.csv
├── drivers_data.csv
├── driver_standings.csv
│
├── constructors.csv
├── constructors_data.csv
├── constructor_results.csv
├── constructor_standings.csv
│
├── circuits.csv
├── circuit_info.csv
│
├── qualifying.csv
├── qualifying_results.csv
├── race_results.csv
├── lap_times.csv
├── pit_stops.csv
│
└── additional historical / reference datasets
```

The unusually large `index.html` is intentional in the current implementation: the dashboard, styling, and client-side application logic are consolidated into the static entry point rather than split into a conventional component/build structure.

## Running Locally

No build step is required.

### 1. Clone the repository

```bash
git clone https://github.com/Kavy1206/f1-intelligence.git
cd f1-intelligence
```

### 2. Serve the directory

A local HTTP server is recommended because browsers may restrict certain resource-loading behavior when HTML is opened directly with `file://`.

Using Python:

```bash
python3 -m http.server 8000
```

Open:

```text
http://localhost:8000/
```

Alternatively, the repository can be deployed directly through GitHub Pages because the application is a static site.

## Live Deployment

The current public deployment is available through GitHub Pages:

**https://Kavy1206.github.io/f1-intelligence/**

The deployment model is intentionally simple:

```text
Git push
   ↓
GitHub repository
   ↓
GitHub Pages
   ↓
Static dashboard
```

There is no application server that must be provisioned for the current architecture.

## Data and Model Considerations

### Historical vs current-season data

Historical data is useful for identifying persistent driver/circuit relationships and long-term performance patterns. It should not automatically be interpreted as a direct measure of current car competitiveness, particularly when technical regulations, tyres, or team structures change substantially between seasons.

### Prediction uncertainty

Race outcomes are highly stochastic. Safety cars, weather, reliability, penalties, tyre behavior, qualifying incidents, and strategic decisions can materially change an expected result. Model outputs should therefore be interpreted as estimates with uncertainty rather than deterministic predictions.

### Data quality

Because the dashboard is backed by flat files, correctness depends on the provenance and consistency of those datasets. When extending the project, preserve stable column names and data types where possible and validate joins between driver, constructor, circuit, and race identifiers.

## Security Considerations

The AI Race Engineer introduces an important deployment constraint: **private API credentials must not be embedded in publicly served client-side JavaScript.** Any production implementation requiring a secret API key should place the credential behind a server-side proxy, serverless function, or equivalent protected execution environment.

Public static assets should be assumed to be fully inspectable by visitors.

## Development Guidelines

When extending the project:

1. Keep raw/reference data separate from derived dashboard data where practical.
2. Preserve consistent identifiers across drivers, constructors, circuits, races, and seasons.
3. Avoid hard-coding analytical conclusions when the same result can be derived from the underlying dataset.
4. Clearly distinguish historical statistics from current-season observations.
5. Validate prediction changes against historical races before treating them as model improvements.
6. Avoid presenting model estimates as guaranteed race outcomes.
7. Do not commit API credentials or other secrets.
8. Keep the static deployment model in mind when introducing new dependencies.

## Roadmap

Potential future improvements include:

- separating the monolithic dashboard into modular JavaScript components;
- introducing a formal data-processing pipeline for CSV ingestion and validation;
- moving computationally expensive model inference off the browser where appropriate;
- adding automated model evaluation and calibration metrics;
- versioning datasets independently from presentation code;
- introducing a serverless backend for secure AI API access;
- expanding live-session data integration;
- adding automated refresh/update workflows for current-season results;
- improving circuit-level strategy and tyre-degradation models.

## Disclaimer

F1 Intelligence is an independent Formula 1 analytics project and is not affiliated with Formula 1, the FIA, or any Formula 1 team.

Predictions and analytical outputs are informational and inherently uncertain. They should not be interpreted as guaranteed race results, official engineering analysis, or professional financial advice.

## Links

- **Repository:** https://github.com/Kavy1206/f1-intelligence
- **Live dashboard:** https://Kavy1206.github.io/f1-intelligence/

---

**Maintainer:** Kavy1206  
**Primary branch:** `main`
