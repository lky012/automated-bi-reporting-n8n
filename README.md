# Automated BI Pipeline with AI Anomaly Alerting (V2 Enterprise)

This repository contains the n8n automation workflow for the **Automated Weekly Business Review (WBR) & AI Insights System**.

## 🌟 V2 Enterprise Upgrade Highlights

The pipeline has been upgraded from a standard reporting script to a production-grade, fault-tolerant architecture featuring:

- **Data Quality Gate (Data Validation):** A strict JavaScript node intercepts merging Google Sheets data. It rigorously scans for null values or invalid schemas (e.g., severe outliers like a $10,000 cost typo) and routes exceptions to operations before any downstream processing occurs.
- **Autonomous Anomaly Routing:** Instead of sending standard reports containing critical alerts hidden inside, the pipeline evaluates metrics for severe anomalies (e.g., negative margins). An IF node intercepts these and dynamically fires off a highly visible 🚨 Urgent Action Alert.
- **Multi-Model AI Fallback Chain (Self-Healing):** Guaranteeing 100% reporting uptime despite API rate limits. The primary model (Gemini 2.5 Flash) features "Continue On Error" routing to a fallback model (Gemini 2.5 Flash Lite), and ultimately to a hardcoded JSON failsafe.
- **Compute-Optimized Graph Generation:** QuickChart.io API rendering is strictly decoupled from the main ETL computation. API endpoints for generating 6 unique visual charts are only called *after* the AI mathematically clears the raw data for anomalies, preventing the system from wasting compute on garbled edge cases.

## System Architecture

1. **[Ingestion]** Parallel Google Sheets pulls (Orders & Costs) triggered every Monday at 09:00.
2. **[Validation]** JS Code Data Quality Gate sanitizes inputs. 
3. **[Metrics ETL]** Computes YoY, MoM, WoW KPIs and Spoilage rates.
4. **[Exception Gate]** Anomaly router either executes Urgent Alert or passes to standard flow.
5. **[AI Narrative]** Gemini 2.5 Flash authors a strategic HTML executive summary.
6. **[Reporting]** Renders QuickChart components and delivers final HTML email.

## Setup Instructions

1. Import the `Automated BI Pipeline with AI Anomaly Alerting.json` into your n8n workspace.
2. Configure your Google Sheets credentials for the input nodes.
3. Configure your Google Gemini API credentials.
4. Configure your Gmail credentials for the output nodes.
5. Select the relevant target emails for alerting and standard reporting.

> **Note:** This workflow requires n8n version 1.0+ and access to the Langchain Gemini nodes.
