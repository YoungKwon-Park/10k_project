# Multi-Source Financial Data Dashboard

A layered financial data workflow that combines SEC filing data and market-linked data, converts them into normalized annual snapshots, derives reusable KPI layers, generates structured diagnostic insights, and preserves the results in a queryable SQLite database.

## What This Project Does

This project is not only a dashboard.  
It is an end-to-end workflow for turning fragmented financial records into structured analytical outputs.

The workflow:
1. retrieves filing-based financial data from SEC sources,
2. normalizes those records into year-level financial snapshots,
3. derives comparable KPI and price-linked metrics,
4. reorganizes those results into structured insights and controlled narrative inputs,
5. and stores the outputs in exportable and queryable formats.

The goal is not to generate investment recommendations.  
The goal is to build a structured review workflow that makes raw financial records easier to inspect, compare, interpret, and reuse.

## Workflow Structure

### 1. Source Integration
SEC filing data is converted into normalized annual snapshots.  
Each snapshot preserves not only financial values, but also source traceability and coverage metadata so downstream layers do not depend directly on raw filing responses.

### 2. Metric Engineering
Normalized snapshots are transformed into reusable KPI layers, including:
- long-term growth metrics such as Revenue CAGR, Net Income CAGR, FCF CAGR, EPS CAGR, and BVPS CAGR
- margin trends
- cash-conversion and balance-sheet indicators
- year-over-year change metrics

This layer also preserves calculation metadata and validity flags so missing or structurally weak values are not treated as ordinary outputs.

### 3. Interpretation Layer
Derived metrics are reorganized into:
- structured diagnostic insights
- threshold-based summaries
- controlled narrative input blocks

This layer separates raw calculation from interpretation so outputs remain easier to review, sort, export, and reuse.

### 4. Delivery Layer
Final outputs are preserved in multiple forms:
- normalized snapshot tables
- KPI summary tables
- structured insight tables
- SQLite records for later querying and inspection

## Key Features

- SEC filing snapshot normalization
- market-linked price integration
- long-term and recent-change KPI computation
- price-linked context metrics
- structured insight generation with evidence and risk notes
- controlled narrative input construction
- exportable tabular outputs
- SQLite-based persistence and query support

## Repository Structure

```text
app.py
core/
├── multi_10k_fetcher.py   # SEC retrieval and annual snapshot construction
├── cagr_analysis.py       # KPI engineering and year-over-year logic
├── valuation.py           # price-linked metrics and threshold summaries
├── insight_engine.py      # structured diagnostic insight generation
├── ai_analysis.py         # controlled narrative prompt construction
├── marketstack.py         # latest market price retrieval
└── sql_store.py           # SQLite schema, persistence, and query helpers
```

## Main Outputs

The workflow produces several reusable outputs:

- **Normalized annual snapshots**  
  Year-level financial records with source and coverage metadata.

- **KPI summary tables**  
  Growth, profitability, cash-conversion, balance-sheet, and recent-change metrics.

- **Structured insights**  
  Priority-sorted diagnostic objects with status, summary, evidence, and risk notes.

- **Narrative input blocks**  
  Controlled prompt-ready inputs built from the current metric state.

- **SQLite tables**  
  Persisted outputs for later querying and inspection:
  - `analysis_runs`
  - `snapshots`
  - `kpi_metrics`
  - `insights`

## Run

```bash
streamlit run app.py
```

Then provide:
- a ticker
- a Marketstack API key
- the number of recent 10-K snapshots to retrieve
