<div align="center">

# 🏎️ F1 Intelligence

### A data-driven F1 dashboard for finding the pattern behind the result.

Historical performance, circuit intelligence, race predictions, and an interactive 2026 dashboard — all in one place.

[![Live Dashboard](https://img.shields.io/badge/live-dashboard-E10600?logo=formula1&logoColor=white)](https://Kavy1206.github.io/f1-intelligence/)
[![JavaScript](https://img.shields.io/badge/JavaScript-vanilla-F7DF1E?logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![Data](https://img.shields.io/badge/data-CSV-lightgrey)](#data)
[![GitHub Pages](https://img.shields.io/badge/hosting-GitHub%20Pages-222222?logo=github)](https://pages.github.com/)

### ▶︎ [Open the live dashboard](https://Kavy1206.github.io/f1-intelligence/)

</div>

---

## What is this?

F1 Intelligence started with a fairly simple question:

> **Can historical F1 data tell us something useful about what is likely to happen next?**

The answer is not *"feed some CSVs into a model and magically know who wins."* F1 is annoyingly good at ruining simple predictions. Car performance changes, circuits favour different characteristics, strategy matters, and one safety car can turn an apparently sensible forecast into modern art.

So this project combines several layers of information instead of treating one statistic as the truth:

- **Current championship data** for where drivers and teams actually stand.
- **Historical race and qualifying data** to identify longer-term patterns.
- **Circuit-specific performance** to understand how the same driver or team behaves at different tracks.
- **Lap-time and pit-stop data** for race and strategy context.
- **Prediction logic** for qualifying and race outcomes.
- **An AI Race Engineer** for exploring the data in natural language.

The result is a browser-based dashboard that is much closer to an analytical workspace than a traditional static F1 website.

<div align="center">

| 🏁 Race data | 📍 Circuit data | 🔮 Predictions | 🤖 AI Race Engineer |
|---|---|---|---|
| Results, standings & laps | Track-level context | Quali & race estimates | Natural-language analysis |

</div>

---

## The idea behind the dashboard

A race result by itself does not explain much. A driver finishing P4 could have had a terrible qualifying session, gained positions through strategy, benefited from a safety car, or simply driven a genuinely fast car.

F1 Intelligence tries to keep those pieces of context together.

```text
Historical data ──────┐
                       │
Circuit characteristics ─┤
                       │
Qualifying / race data ─┤──→ analysis ──→ prediction ──→ dashboard
                       │
Lap & pit-stop data ───┤
                       │
Championship state ────┘
```

The important bit is that the prediction is **an output of the analysis, not a replacement for it**.

---

## What you can actually do with it

### 📊 Championship view

Track the driver and constructor championships and see how the 2026 season is developing.

The underlying data is stored separately from the presentation layer, so standings are not just manually typed into the page. The driver reference dataset, for example, contains driver IDs, abbreviations, names and team associations, while race results carry grid position, classification, points and laps. citeturn19file0turn23file0

### 🧠 Driver & circuit analysis

Look at a driver's historical performance and compare how that performance changes from circuit to circuit. Circuit data includes track names, locations, countries and coordinates, while separate circuit information stores event and session-level context. citeturn26file0turn20file0

### 🔮 Qualifying & race predictions

The dashboard includes model-based predictions for qualifying and race outcomes.

These are deliberately treated as **probabilistic estimates**, not promises. There is a large difference between *"the model rates this driver highly"* and *"this driver will definitely finish P2"*, although the internet has spent decades pretending otherwise.

### 🛞 Race & strategy context

Lap-time and pit-stop datasets provide additional context when looking at how a race unfolded rather than just looking at the finishing order.

### 🤖 AI Race Engineer

The dashboard also includes an AI Race Engineer interface intended to let you ask questions about the analysis in natural language rather than manually digging through tables.

---

## The data

This project is built around structured F1 datasets rather than a traditional application database.

Some of the main files are:

| File | What it contains |
|---|---|
| `drivers.csv` | Historical driver reference data |
| `drivers_data.csv` | Driver metadata used by the dashboard |
| `driver_standings.csv` | Championship standings |
| `circuits.csv` | Circuit reference data and locations |
| `circuit_info.csv` | Event and session information |
| `qualifying.csv` | Historical qualifying data |
| `qualifying_results.csv` | Session-level qualifying results |
| `race_results.csv` | Race position, grid, status, points and laps |
| `lap_times.csv` | Lap-level timing data |
| `pit_stops.csv` | Pit-stop records |
| `constructor_results.csv` | Constructor race results |
| `constructor_standings.csv` | Constructor championship standings |

The repository currently contains a sizeable historical dataset, with the dashboard and its data living together in the same repository. The current `index.html` is a large static application, while the CSV files act as the data layer. fileciteturn18file0turn23file0

---

## How it works

At its core, the project keeps the architecture intentionally simple.

```text
                ┌──────────────────────┐
                │    F1 CSV datasets   │
                │                      │
                │ results · laps       │
                │ quali · pit stops    │
                │ circuits · standings │
                └──────────┬───────────┘
                           │
                           ▼
                ┌──────────────────────┐
                │   Browser / JS layer │
                │                      │
                │ parsing · filtering  │
                │ calculations         │
                │ prediction logic     │
                └──────────┬───────────┘
                           │
                           ▼
                ┌──────────────────────┐
                │   F1 Intelligence    │
                │      dashboard       │
                └──────────────────────┘
```

There is intentionally no heavy front-end framework or application server. The current implementation is a static HTML/CSS/JavaScript application hosted through GitHub Pages.

That makes it easy to deploy, easy to inspect, and extremely easy to accidentally turn into a 6 MB HTML file, which is apparently what happened here.

---

## Why use historical data?

Historical data is useful for finding recurring relationships:

- drivers who consistently perform well at particular circuits;
- qualifying performance versus eventual race position;
- circuit characteristics associated with different performance profiles;
- historical pit-stop and lap-time behaviour;
- how frequently certain race outcomes occur.

But historical performance is **context, not destiny**.

A 2024 result is not automatically a good predictor of a 2026 race simply because the circuit has not moved. Cars change, regulations change, tyres change, drivers change, and teams occasionally discover something useful while everyone else is busy staring at the timing screen.

The dashboard therefore works best when historical evidence is combined with current-season information.

---

## Prediction philosophy

The prediction layer is designed around the idea of combining measurable signals rather than producing arbitrary numbers.

Conceptually:

```text
Driver history
      +
Circuit suitability
      +
Qualifying / race performance
      +
Current championship form
      +
Race-specific factors
      ↓
Prediction score
      ↓
Relative driver / team outlook
```

The output should answer **"who is favoured and why?"**, not pretend that uncertainty has stopped existing.

---

## Tech stack

| Part | Technology |
|---|---|
| Dashboard | HTML / CSS / Vanilla JavaScript |
| Data | CSV datasets |
| Prediction layer | Client-side model-based logic |
| AI assistant | Anthropic Claude API |
| Deployment | GitHub Pages |

The repository currently has no Node.js build pipeline or front-end framework dependency. The main dashboard is served as a static page, with `index.html` acting as the application entry point. fileciteturn12file0turn18file0

---

## Run it locally

There is no build process to wrestle with.

```bash
git clone https://github.com/Kavy1206/f1-intelligence.git
cd f1-intelligence
python3 -m http.server 8000
```

Then open:

```text
http://localhost:8000/
```

Or just use the hosted version:

### [→ Open F1 Intelligence](https://Kavy1206.github.io/f1-intelligence/)

---

## Project structure

The repository is intentionally data-heavy:

```text
f1-intelligence/
│
├── index.html                  # Dashboard / application
├── README.md
│
├── drivers.csv                 # Historical driver data
├── drivers_data.csv            # Dashboard driver metadata
├── driver_standings.csv
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
├── constructor_results.csv
├── constructor_standings.csv
├── constructors_data.csv
│
└── ...                         # Additional reference datasets
```

The main dashboard is deliberately self-contained, while the CSV files provide the historical and race-level data it works with. The repository currently includes dedicated datasets for drivers, circuits, qualifying, race results, lap times, pit stops and championship standings. fileciteturn18file0

---

## Data accuracy & limitations

There are a few things worth keeping in mind before taking any prediction too seriously.

**Historical bias.** A strong historical record can reflect a previous car advantage rather than an intrinsic driver advantage.

**Small samples.** Circuit-specific records can become noisy when only a handful of races are available.

**Race randomness.** Safety cars, weather, reliability, penalties and strategy can dominate a race that looked predictable beforehand.

**Model uncertainty.** A prediction is a ranking of possible outcomes, not a guarantee.

The project is therefore most useful as a **decision-support and analysis tool**, not as a machine that has somehow defeated Formula 1.

---

## What's next

The current foundation can be pushed much further. The obvious next steps are better live-data integration, more explicit feature engineering, proper model evaluation and calibration, richer tyre/strategy modelling, and eventually separating the monolithic dashboard into a cleaner application structure.

The goal is not to make the README longer. Humanity has suffered enough from README files that require their own table of contents.

The goal is to make the analysis better.

---

## Disclaimer

F1 Intelligence is an independent project and is not affiliated with Formula 1, the FIA, or any Formula 1 team.

Predictions and analytical outputs are informational and inherently uncertain. They are not official F1 data, engineering advice, or guaranteed race outcomes.

<div align="center">

**Built for people who would rather ask _why_ a driver finished there than just look at the finishing order.**

[🏎️ Live Dashboard](https://Kavy1206.github.io/f1-intelligence/) · [GitHub Repository](https://github.com/Kavy1206/f1-intelligence)

</div>
