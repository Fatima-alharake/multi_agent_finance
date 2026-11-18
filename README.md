# Financial Intelligence Platform

Automated financial report processor that ingests annual/quarterly PDF reports, extracts financial figures with verified citations, computes critical solvency and profitability ratios, stores results in a SQL database, and provides both a web interface and an MCP server.

## Features

- **Automated PDF Parsing**: Extracts balance sheet, income statement, and cash flow figures using table detection and layout heuristics, with optional LLM augmentation via LangGraph.
- **KPI Engine**: Computes YoY Revenue Growth, Gross/Operating/Net/EBITDA margins, Current & Quick ratios, Debt-to-Equity, Net Debt/EBITDA leverage, Free Cash Flow, and composite Health Scores (AAA to D).
- **Relational Storage**: SQLite with SQLAlchemy async ORM (`aiosqlite`), tracking companies, filings, document chunks, financial items, ratios, alerts, and query audit logs.
- **Strictly Grounded Q&A**: Question answering pipeline anchored in document chunks with verified page citations.
- **Anomaly Alerts**: Configurable financial risk thresholds (net margin collapse, liquidity deficit, excessive leverage, cash burn) with resolution tracking.
- **LangSmith Tracing & Observability**: Real-time tracking of latency, token consumption, estimated LLM costs, and mathematical coherence checks (balance sheet equality, margin consistency).
- **MCP Server (Model Context Protocol)**: Exposes tools (`get_financial_kpis`, `query_financial_document`, `list_financial_alerts`, `list_companies`) for integration with Claude Desktop, Antigravity, or other MCP clients.

## Tech Stack

- Python 3.12
- FastAPI & Uvicorn
- LangGraph & LangChain Core
- LangSmith Client & Telemetry
- Model Context Protocol (`mcp` 2.x `MCPServer`)
- SQLAlchemy 2.x & aiosqlite
- pdfplumber & pypdf & reportlab
- Tailwind CSS & Chart.js

## Quickstart

### 1. Install Dependencies

```bash
pip install -r requirements.txt
```

### 2. Configure Environment

Copy `.env.example` to `.env`:

```bash
copy .env.example .env
```

Set `OPENAI_API_KEY` (optional, for LLM-assisted extraction and generation) and `LANGCHAIN_API_KEY` (optional, for cloud LangSmith tracing). The platform operates fully offline if keys are not provided.

### 3. Generate Sample Reports

```bash
python sample_data/generate_sample_reports.py
```

### 4. Run the Web Application

```bash
uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
```

Open `http://localhost:8000` in your browser.

### 5. Run the MCP Server

```bash
python -m app.mcp.server
```

### 6. Run Tests

```bash
python -m pytest
```
