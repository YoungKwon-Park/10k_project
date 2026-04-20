# Multi-Source Data Integration and Quality Management Pipeline

An end-to-end data pipeline that collects heterogeneous data from external public APIs, validates field-level integrity, converts records into a normalized internal schema, and delivers reusable KPI layers and structured insights through a review-oriented interface.

---

## Background

Data retrieved from external public sources often carries inconsistent field names, varying coverage, and structurally unstable formats across reporting periods.  
Building analysis directly on top of raw API responses creates the following problems:

- Downstream calculation logic breaks whenever source formatting changes
- Missing fields and non-calculable metrics become indistinguishable from actual poor performance
- The same analytical workflow cannot be reliably reapplied to new targets

This project addresses these problems structurally by separating retrieval, normalization, metric computation, interpretation, and delivery into independent layers.

---

## Pipeline Structure

```
External API Retrieval
        ↓
Internal Schema Normalization  ←  field mapping / integrity validation / coverage tracking
        ↓
KPI Layer Computation          ←  calculation validity metadata included
        ↓
Interpretation Layer           ←  structured insights / threshold summaries / AI input construction
        ↓
Delivery Layer                 ←  dashboard / CSV export / SQLite storage / SQL query interface
```

Each layer operates independently, so changes to the data source or additions to the metric set do not cascade across the entire workflow.

---

## Key Features

### Data Integration and Integrity Management
- Collects multi-year records from a public external API (SEC XBRL)
- Searches multiple candidate tags in priority order to maximize field coverage for each concept
- Preserves field-level source traceability: which tag produced which value is stored as metadata
- Tracks available field count, missing field count, and minimum validity status per snapshot
- Filters out records that fail the minimum validity condition before they enter downstream layers

### Metric Computation and Validity Flagging
- Computes long-term growth rates (CAGR), margin trends, cash conversion ratios, and balance-sheet safety indicators
- Stores `calculable` flag, comparison window, and failure reason as metadata for every metric
- Separates non-calculable cases from actual weak performance so downstream layers can handle each correctly
- Maintains year-over-year (YoY) change rates independently from long-term CAGR to track short-term direction separately

### Interpretation Layer and Visualization Planning
- Converts quantitative metrics into nine structured diagnostic insight objects (key / category / status / summary / evidence / risk_note / priority)
- Generates threshold-based summary messages for quick scanning of metric states
- Produces four Altair-based chart types: absolute trend, per-share trend, margin trend, and price-linked indicators
- Includes a prompt builder that assembles a controlled analytical block as structured input for AI-based narrative generation

### Data Operations and Delivery
- Exports analysis results as three separate CSVs: normalized snapshots, KPI summary, and structured insights
- Persists results in a four-table SQLite schema with cumulative run tracking (analysis_runs / snapshots / kpi_metrics / insights)
- Exposes sample SQL queries that can be executed directly inside the application to inspect stored results
- Supports Korean and English dual-language interface

---

## Data Coverage and Reliability Reporting

This pipeline does not expose only final output values.  
Each analysis run also surfaces the following reliability information so that the scope of interpretation remains explicit:

| Item | Description |
|---|---|
| Field-level coverage ratio | Proportion of collected years in which a given field is present |
| Calculability flag | Whether each KPI was actually computable from the available data |
| Calculation failure reason | e.g. missing_start_or_end, non_positive_year_span |
| Minimum validity status | Whether a given annual record meets the threshold for downstream use |

---

## Repository Structure

```
app.py                          # Streamlit main application
core/
├── multi_10k_fetcher.py        # External API retrieval and internal schema construction
├── cagr_analysis.py            # KPI computation and calculation validity metadata
├── valuation.py                # Price-linked derived metric computation
├── insight_engine.py           # Structured diagnostic insight generation
├── ai_analysis.py              # Analytical prompt construction and AI call
├── marketstack.py              # Latest market price retrieval
└── sql_store.py                # SQLite schema, persistence, and query helpers
notebooks/
├── 0_Project_Overview.ipynb
├── 1_Source_Integration_and_Internal_Schema.ipynb
├── 2_Metric_Engineering_and_Calculation_Validity_Logic.ipynb
├── 3_Interpretation_Layer.ipynb
└── 4_Delivery_Layer_and_Reliability_Perspective.ipynb
```

---

## Deliverables

| Output | Description |
|---|---|
| Normalized annual snapshots | Standardized records with source traceability metadata |
| KPI summary table | Growth / profitability / cash conversion / balance-sheet indicators with validity flags |
| Structured insights | Priority-sorted diagnostic objects (status / summary / evidence / risk note) |
| AI input block | Controlled analytical prompt assembled from the current metric state |
| SQLite tables | Layer-separated analysis history for later querying and inspection |

---

## How to Run

```bash
streamlit run app.py
```

Required inputs:
- Ticker symbol (e.g. AAPL, MSFT)
- Marketstack API key
- Number of years to collect

---

## Tech Stack

| Category | Technology |
|---|---|
| Data retrieval | SEC EDGAR XBRL API, Marketstack API |
| Data processing | Python, Pandas |
| Visualization | Altair, Streamlit |
| Storage | SQLite |
| AI integration | Google Gemini API |
