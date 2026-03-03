# 🧠 DataMind AI — Agentic Data Intelligence Framework

> **Turn raw data into executive insights through natural language conversation — no SQL, no pipelines, no engineering overhead.**

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![Streamlit](https://img.shields.io/badge/Streamlit-1.30+-FF4B4B.svg)](https://streamlit.io)
[![Anthropic Claude](https://img.shields.io/badge/Claude-Sonnet--4-blueviolet.svg)](https://www.anthropic.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

---

## 🎯 The Problem

**Enterprises spend more money on engineers cleaning data than actually using it for insights.**

| Current Reality | The Cost |
|----------------|----------|
| Executives need insights ASAP, but are blocked by technical barriers | Decisions delayed by days/weeks |
| Engineers are bogged down writing DBT tests, SQL, and pipeline code | High-value talent on low-value work |
| Traditional tools (DBT, Great Expectations) rely on hardcoded, non-semantic rules | Can't catch logical issues like `ship_date < order_date` |
| Every new dataset requires a new pipeline | Non-trivial tech stack management |
| Tools can't understand business context | Miss domain-specific data quality issues |

**Sources:** Gartner "Predicts 2024: Data and Analytics Strategies" | IBM Institute for Business Value "The AI Data Quality Challenge" (2023)

---

## 💡 Our Solution

**DataMind AI** is an LLM-powered agentic framework that lets non-technical users upload data and ask questions in plain English. The system automatically:

1. **Validates data quality** using semantic understanding (not just hardcoded rules)
2. **Explores patterns** through automated EDA
3. **Explains everything** in plain English with interactive visualizations
4. **Catches what rules can't** — temporal inconsistencies, statistical outliers, cross-column logic violations, domain-specific impossibilities

### How It's Different

| Approach | What It Does | Limitation |
|----------|-------------|------------|
| **DBT** | Rule-based SQL tests | Hardcoded, needs engineers, not semantic |
| **Great Expectations** | Data validation framework | Complex setup, rule-based only |
| **Monte Carlo** | Data observability/monitoring | Monitors but doesn't analyze or explain |
| **Manual Process** | Engineers write custom code | Slow, expensive, doesn't scale |
| **DataMind AI** ✅ | LLM-powered semantic validation + conversational EDA | Understands context, adapts to new data, explains in English |

We sit at the **intersection of syntactic and semantic data quality evaluation** — we fix what other tools can't even see is broken.

---

## ✨ Key Features

### 🔍 Semantic Data Validation
- **Temporal logic checks** — catches `ship_date` before `order_date`, future dates, suspicious gaps
- **Statistical outlier detection** — Z-score, IQR, and pattern-based anomaly detection
- **Cross-column validation** — totals that don't match sums, min > max, contradictory categories
- **LLM-powered semantic analysis** — catches domain-specific issues, unit mismatches, encoding problems
- **Data quality scoring** — 0-100 score with prioritized findings and fix suggestions

### 📊 Automated Exploratory Data Analysis
- Descriptive statistics with distribution tests (Shapiro-Wilk normality)
- Correlation analysis (Pearson, Spearman, Kendall) with significance detection
- Time series trend detection with seasonality decomposition
- Change point detection for identifying regime shifts
- Feature importance ranking (mutual information + random forest)

### 📈 Interactive Visualizations
- Histograms, scatter plots, line charts, bar charts
- Box plots, correlation heatmaps, pie charts
- All charts are interactive (Plotly) — zoom, hover, export

### 🤖 Conversational Agent (ReAct Pattern)
- Single-agent architecture using Claude's native tool_use API
- 11 specialized tools the AI can call autonomously
- Smart context management — handles datasets of any size through intelligent sampling
- Sliding window conversation history with token budgeting

### 💬 Plain English Interface
- Ask questions like *"What are the trends in revenue?"* or *"Are there any data quality issues?"*
- Results explained in executive-friendly language
- Suggests follow-up questions and next steps

---

## 🏗️ Architecture

```
User uploads CSV/Excel/JSON/Parquet
          ↓
┌─────────────────────────────┐
│     Streamlit Web UI        │ ← Conversational chat interface
├─────────────────────────────┤
│   Agent Orchestrator        │ ← ReAct loop with Claude API
│   (11 tools available)      │
├──────┬──────┬──────┬────────┤
│Ingest│Valid.│ EDA  │Explain │ ← Modular pipeline layers
│      │      │      │        │
│Loader│Stats │Stats │Plain   │
│Schema│Temp. │Corr. │English │
│Sample│Cross │Trend │Results │
│      │Seman.│Viz   │        │
└──────┴──────┴──────┴────────┘
```

**Agent Tools:** `describe_dataset` · `get_data_sample` · `validate_data` · `compute_statistics` · `compute_correlations` · `detect_trends` · `create_visualization` · `detect_outliers` · `analyze_missing_data` · `compute_feature_importance` · `run_custom_analysis`

---

## 🚀 Quick Start

### Prerequisites
- Python 3.10+
- An [Anthropic API key](https://console.anthropic.com/) (Claude Sonnet)

### Installation

```bash
# Clone the repository
git clone https://github.com/wasimakrammind/agentic_ai.git
cd agentic_ai

# Install dependencies
pip install -r requirements.txt

# Set up your API key
cp .env.example .env
# Edit .env and add your Anthropic API key: ANTHROPIC_API_KEY=sk-ant-...

# Run the app
streamlit run ui/app.py
```

### First Use

1. Open `http://localhost:8501` in your browser
2. Enter your Anthropic API key in the sidebar (or set it in `.env`)
3. Upload a CSV, Excel, JSON, or Parquet file
4. Start asking questions!

### Example Queries

```
"Validate my data and report any quality issues"
"What are the trends in revenue over time?"
"Show me the correlation between price and quantity"
"Are there any outliers in the sales column?"
"What drives customer satisfaction score?"
"Give me a comprehensive EDA of this dataset"
```

---

## 📁 Project Structure

```
datamind-ai/
├── config/
│   ├── settings.py              # App configuration (Pydantic)
│   └── prompts/                 # LLM system prompts
├── src/
│   ├── agent/                   # 🤖 LLM orchestration
│   │   ├── orchestrator.py      # ReAct agent loop
│   │   ├── tool_registry.py     # Tool dispatch system
│   │   ├── tool_definitions.py  # 11 Claude API tool schemas
│   │   ├── context_manager.py   # Token budgeting & context window
│   │   └── conversation.py      # Session state management
│   ├── ingestion/               # 📥 Data loading
│   │   ├── loader.py            # Multi-format loader (CSV/Excel/JSON/Parquet)
│   │   ├── schema_detector.py   # Semantic type inference
│   │   └── sampling.py          # Smart sampling for LLM context
│   ├── validation/              # ✅ Data quality
│   │   ├── validator.py         # Validation orchestrator
│   │   └── rules/
│   │       ├── statistical.py   # Outlier detection
│   │       ├── temporal.py      # Date logic validation
│   │       ├── schema_rules.py  # Type consistency & missing data
│   │       ├── cross_column.py  # Cross-column logic
│   │       └── semantic.py      # LLM-powered semantic checks
│   ├── eda/                     # 📊 Exploratory analysis
│   │   ├── analyzer.py          # Auto-EDA orchestrator
│   │   ├── statistics.py        # Descriptive stats & distributions
│   │   ├── correlations.py      # Correlation analysis
│   │   ├── trends.py            # Trend & seasonality detection
│   │   ├── feature_importance.py# Feature ranking
│   │   └── visualizer.py        # Plotly chart generation
│   ├── explanation/             # 💬 Plain-English output
│   │   └── explainer.py         # Result explanation templates
│   ├── models/                  # 📦 Data models
│   │   ├── dataset.py           # Dataset & metadata
│   │   ├── validation_result.py # Findings & reports
│   │   └── analysis_result.py   # EDA results
│   └── utils/                   # 🔧 Utilities
│       ├── serialization.py     # DataFrame ↔ LLM formatting
│       └── token_counter.py     # Token estimation
├── ui/                          # 🖥️ Streamlit interface
│   ├── app.py                   # Main entry point
│   └── components/
│       ├── sidebar.py           # Upload, settings, quick actions
│       ├── chat.py              # Conversational interface
│       └── charts.py            # Chart rendering
├── tests/                       # 🧪 Test suite (25 tests)
│   ├── fixtures/                # Test data files
│   └── unit/                    # Unit tests
├── requirements.txt
├── .env.example
└── README.md
```

---

## 🧪 Running Tests

```bash
# Run all tests
pytest tests/ -v

# Run specific test module
pytest tests/unit/test_validator.py -v

# Run with coverage
pytest tests/ --cov=src --cov-report=term-missing
```

**Current status: 25/25 tests passing** ✅

---

## ⚙️ Configuration

All settings are managed via environment variables or `.env` file:

| Variable | Default | Description |
|----------|---------|-------------|
| `ANTHROPIC_API_KEY` | (required) | Your Anthropic API key |
| `ANTHROPIC_MODEL` | `claude-sonnet-4-20250514` | Claude model to use |
| `ANTHROPIC_MAX_TOKENS` | `4096` | Max response tokens |
| `MAX_UPLOAD_SIZE_MB` | `200` | Maximum file upload size |
| `OUTLIER_Z_THRESHOLD` | `3.0` | Z-score threshold for outlier detection |
| `MISSING_DATA_WARNING_PCT` | `5.0` | Missing data warning threshold (%) |
| `MISSING_DATA_CRITICAL_PCT` | `30.0` | Missing data critical threshold (%) |

---

## 🗺️ Roadmap

### Phase 1 (Current) — Data Validation + EDA
- [x] Multi-format data ingestion (CSV, Excel, JSON, Parquet)
- [x] LLM-powered semantic data validation
- [x] Automated EDA with 11 analysis tools
- [x] Interactive Plotly visualizations
- [x] Conversational Streamlit interface
- [x] Smart context management for large datasets

### Phase 2 (Planned) — AutoML
- [ ] AutoGluon integration for automated model training
- [ ] Model explainability with SHAP values
- [ ] Forecasting capabilities (Prophet, ARIMA)
- [ ] "Forecast next 3 months of sales" workflow

### Phase 3 (Future) — Enterprise
- [ ] Cloud platform integration (AWS, Azure, GCP)
- [ ] Database connectors (PostgreSQL, BigQuery, Snowflake)
- [ ] Multi-user sessions with auth
- [ ] Scheduled data quality monitoring
- [ ] API endpoint for programmatic access

---

## 🎓 The Value Proposition

### For Cloud Providers (Microsoft, Google, Databricks, Snowflake)
- **New revenue stream** — offer as a premium data quality service
- **Platform stickiness** — "companies stay because we clean their data better than they can"
- **Market differentiation** — semantic validation that competitors don't have

### For Enterprises
- **Save engineering time** — no more manual pipeline creation for every new dataset
- **Faster insights** — executives get answers in minutes, not days
- **Better data quality** — catches issues that rule-based tools miss
- **Lower barrier** — no SQL knowledge required

---

## 🤝 Contributing

Contributions are welcome! Please open an issue first to discuss what you'd like to change.

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- [Anthropic Claude](https://www.anthropic.com) — powering the AI agent
- [Streamlit](https://streamlit.io) — for the beautiful web interface
- [Plotly](https://plotly.com) — interactive visualizations
- [scikit-learn](https://scikit-learn.org) — ML utilities

---

<p align="center">
  <b>DataMind AI</b> — Because your data shouldn't need an army of engineers to be useful.
</p>
